---
title: "12 Days of DigitalOcean Day 10 - Saving Attachments to DigitalOcean Spaces"
date: 2025-01-06T14:00:00.000Z
draft: false
type: posts
categories: 
- spaces,digitalocean
---
# 12 Days of DigitalOcean Day 10 - Saving Attachments to DigitalOcean Spaces

<br/>

<br/>
Welcome back to **Day 10** of the **12 Days of DigitalOcean**! Yesterday, we taught your app to [extract information from email content using DigitalOcean’s GenAI agent](/community/tutorials/automating-receipt-parsing-with-digitalocean-genai). That was a huge step, but let’s face it—receipts and invoices don’t always live in the email body. More often than not, they’re attachments.

Today, we’re going to handle that. We’ll teach your app how to extract these attachments, save them securely to DigitalOcean Spaces, and generate public URLs for each file. These URLs will eventually be stored in our database, allowing you to preview the attached files when reviewing expenses.

Let’s dive in.

[🚀 What You’ll Learn](#what-you-ll-learn)[](#what-you-ll-learn)
----------------------------------------------------------------

By the end of today’s session, you’ll know how to:

1.  [Create a DigitalOcean Space](https://docs.digitalocean.com/products/spaces/how-to/create/) to store attachments.
2.  Extract and decode Base64-encoded attachments from [Postmark emails](https://postmarkapp.com/blog/attachments-finally-here).
3.  Upload attachments to DigitalOcean Spaces using [boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html).
4.  Generate unique file names with [uuid](https://docs.python.org/3/library/uuid.html) to prevent overwrites.
5.  Orchestrate the entire workflow to handle multiple attachments seamlessly.

[🛠 What You’ll Need](#what-you-ll-need)[](#what-you-ll-need)
-------------------------------------------------------------

To get the most out of this tutorial, we assume the following:

1.  **A Flask App Already Deployed on DigitalOcean:** If you haven’t deployed a Flask app yet, you can follow the instructions in [Day 7 - Building and Deploying the Email-Based Receipt Processor](/community/tutorials/deploying-flask-app-on-digitalocean#what-you-ll-need).
2.  **Postmark Configured for Email Testing:** To test the email-to-receipt processing pipeline, you’ll need Postmark set up to forward emails to your Flask app. See [Day 8 - Connecting Postmark to Your Flask App](/community/tutorials/setting-up-postmark-for-receipts#step-by-step-guide) for a step-by-step guide.
3.  **DigitalOcean Spaces Setup**: We’ll store processed attachments in a DigitalOcean Space. If you don’t have a Space yet, we’ll guide you through creating one in this tutorial.

<$> \[info\] **Note:** Even if you don’t have everything set up, you’ll still learn how to:

-   Create a DigitalOcean Space to store attachments.
    
-   Decode Base64-encoded attachments programmatically.
    
-   Upload files to DigitalOcean Spaces using `boto3`.
    
-   Seamlessly integrate attachment handling into your Flask app.
    
    <$>
    

[Step 1: Create a DigitalOcean Space](#step-1-create-a-digitalocean-space)[](#step-1-create-a-digitalocean-space)
-----------------------------------------------------------------------------------------------------------------

First, we need a place to store our attachments. DigitalOcean Spaces is an object storage service, perfect for securely handling files like receipts and invoices. It’s scalable, secure, and integrates seamlessly with our app.

### [Create the Space](#create-the-space)[](#create-the-space)

1.  Log in to the [DigitalOcean dashboard](https://cloud.digitalocean.com/spaces), and click on **Spaces Object Storage**.
    
2.  Then, click **Create Bucket**.
    
    ![DigitalOcean Spaces Object Storage interface displaying file management options](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_spaces_object_storage_interface.png)
    
3.  Choose a **Region** close to your users (e.g., `nyc3` for New York).
    
4.  Name your Space (e.g., `email-receipts`)
    
    ![Guide to creating a Spaces bucket on DigitalOcean, highlighting configuration options](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/create_spaces_bucket_guide.png)
    

This will create your bucket named `email-receipts` available at a URL like `https://email-receipts.nyc3.digitaloceanspaces.com`

### [Generate Access Keys](#generate-access-keys)[](#generate-access-keys)

To interact with your Space programmatically (e.g., via `boto3`), you’ll need an Access Key and Secret Key.

1.  Open your Space, click **Settings**, and scroll to **Access Keys**.
    
2.  Click **Create Access Key**.
    
    ![Settings page in DigitalOcean showing how to create and manage access keys](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_settings_access_key.png)
    
3.  Set **Permissions** to **All Permissions**, so our app can read, write, and delete files.
    
4.  Name the key (or use the default) and click **Create Access Key**.
    
    ![DigitalOcean interface for creating a new Spaces access key with permissions configuration](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_spaces_access_key_creation.png)
    
5.  Save the Access Key and Secret Key—this is the only time you’ll see the Secret Key!
    
    ![DigitalOcean Spaces settings page showing bucket configurations and access key options](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_spaces_settings.png)
    

### [Update Environment Variables](#update-environment-variables)[](#update-environment-variables)

In the DigitalOcean App Platform dashboard:

1.  Go to **Settings > Environment Variables**.
    
2.  Add the following:
    
    -   `SPACES_ACCESS_KEY`: Your Spaces Access Key ID.
        
    -   `SPACES_SECRET_KEY`: Your Spaces Secret Key.
        
    -   `SPACES_BUCKET_NAME`: The name of your Space (e.g., `email-receipts`).
        
    -   `SPACES_REGION`: The region of your Space (e.g., `nyc3`).
        

![DigitalOcean App Platform environment variables configuration page](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_app_environment_variables.png)

[Step 2: Process and Upload Attachments to DigitalOcean Spaces](#step-2-process-and-upload-attachments-to-digitalocean-spaces)[](#step-2-process-and-upload-attachments-to-digitalocean-spaces)
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

To handle attachments in your app, we’ll update our `app.py` and write a few new functions. Each function serves a specific purpose, from decoding attachments to uploading them to DigitalOcean Spaces. Let’s walk through these one by one.

**Note:** If you don’t already have the app set up, follow the instructions in [Day 7 - Building and Deploying the Email-Based Receipt Processor](/community/tutorials/deploying-flask-app-on-digitalocean) to create and deploy it to DigitalOcean’s App Platform.

### [Decode and Save Attachments](#decode-and-save-attachments)[](#decode-and-save-attachments)

Postmark sends attachments as Base64-encoded data inside the JSON payload. The first step is decoding this data and saving it locally using Python’s `base64` library. This function ensures each file gets a unique name with the help of the `uuid` library.

**What is Base64?** It’s like a translator for binary files (like PDFs). It converts them into a plain text format that’s safe to send over the web. Once we decode it back into binary, we can handle it just like any regular file.

**Where Do Files Get Saved?**: We’ll temporarily save the decoded files in `/tmp`. It’s a short-term storage directory available on most systems. Think of it like a scratchpad—it’s perfect for short-term use, and everything gets cleared once the app stops running.

Here’s the function to decode the attachment, ensure the filename is unique (thanks to `uuid`), and save it in `/tmp`.

```
import os
import base64
import uuid

def decode_and_save_attachment(attachment):
    """Decode base64-encoded attachment and save it locally with a unique name."""
    file_name = attachment.get("Name")
    encoded_content = attachment.get("Content")

    if not file_name or not encoded_content:
        logging.warning("Invalid attachment, skipping.")
        return None

    unique_file_name = f"{uuid.uuid4()}_{file_name}"
    file_path = os.path.join("/tmp", unique_file_name)

    try:
        with open(file_path, "wb") as file:
            file.write(base64.b64decode(encoded_content))
        logging.info(f"Attachment saved locally: {file_path}")
        return file_path
    except Exception as e:
        logging.error(f"Failed to decode and save attachment {file_name}: {e}")
        return None
```

### [Upload Attachments to DigitalOcean Spaces](#upload-attachments-to-digitalocean-spaces)[](#upload-attachments-to-digitalocean-spaces)

Now that we’ve decoded and saved the files, the next step is uploading them to DigitalOcean Spaces. We’ll use `boto3`, a powerful Python SDK for working with AWS-compatible APIs, to handle the upload. Spaces works just like an S3 bucket, so it’s a perfect fit.

This function uploads the file to your Space and returns a public URL.

```
import boto3

def upload_attachment_to_spaces(file_path):
    """Upload a file to DigitalOcean Spaces and return its public URL."""
    file_name = os.path.basename(file_path)
    object_name = f"email-receipt-processor/{file_name}"
    try:
        s3_client.upload_file(file_path, SPACES_BUCKET, object_name, ExtraArgs={"ACL": "public-read"})
        file_url = f"https://{SPACES_BUCKET}.{SPACES_REGION}.cdn.digitaloceanspaces.com/{object_name}"
        logging.info(f"Attachment uploaded to Spaces: {file_url}")
        return file_url
    except Exception as e:
        logging.error(f"Failed to upload attachment {file_name} to Spaces: {e}")
        return None
```

### [Process Multiple Attachments](#process-multiple-attachments)[](#process-multiple-attachments)

Let’s bring it all together. This function orchestrates everything:

1.  Decodes each attachment.
2.  Uploads it to Spaces.
3.  Collects the URLs for the uploaded files.

```
def process_attachments(attachments):
    """Process all attachments and return their URLs."""
    attachment_urls = []
    for attachment in attachments:
        file_path = decode_and_save_attachment(attachment)
        if file_path:
            file_url = upload_attachment_to_spaces(file_path)
            if file_url:
                attachment_urls.append({"file_name": os.path.basename(file_path), "url": file_url})
            os.remove(file_path)  # Clean up local file
    return attachment_urls
```

### [Update the `/inbound` Route](#update-the-inbound-route)[](#update-the-inbound-route)

Finally, update the `/inbound` route to include attachment handling. This route will now handle email content processing, attachment decoding and uploading, and returning the final response.

```
@app.route('/inbound', methods=['POST'])
def handle_inbound_email():
    """Process inbound emails and return extracted JSON."""
    logging.info("Received inbound email request.")
    data = request.json

    email_content = data.get("TextBody", "")
    attachments = data.get("Attachments", [])

    if not email_content:
        logging.error("No email content provided.")
        return jsonify({"error": "No email content provided"}), 400

    extracted_data = extract_text_from_email(email_content)
    attachment_urls = process_attachments(attachments)

    response_data = {
        "extracted_data": extracted_data,
        "attachments": attachment_urls
    }

    # Log the final combined data
    logging.info("Final Response Data: %s", response_data)

    return jsonify(response_data)
```

### [Final Complete Code](#final-complete-code)[](#final-complete-code)

Here’s the full `app.py` file with all the updates:

```
from flask import Flask, request, jsonify
import os
import base64
import uuid
import boto3
from dotenv import load_dotenv
from openai import OpenAI
import logging

# Load environment variables
load_dotenv()

# Initialize Flask app
app = Flask(__name__)

# Configure logging
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

# Initialize DigitalOcean GenAI client
SECURE_AGENT_KEY = os.getenv("SECURE_AGENT_KEY")
AGENT_BASE_URL = os.getenv("AGENT_BASE_URL")
AGENT_ENDPOINT = f"{AGENT_BASE_URL}/api/v1/"
client = OpenAI(base_url=AGENT_ENDPOINT, api_key=SECURE_AGENT_KEY)

# DigitalOcean Spaces credentials
SPACES_ACCESS_KEY = os.getenv("SPACES_ACCESS_KEY")
SPACES_SECRET_KEY = os.getenv("SPACES_SECRET_KEY")
SPACES_BUCKET = os.getenv("SPACES_BUCKET_NAME")
SPACES_REGION = os.getenv("SPACES_REGION")
SPACES_ENDPOINT = f"https://{SPACES_BUCKET}.{SPACES_REGION}.digitaloceanspaces.com"

# Initialize DigitalOcean Spaces client
session = boto3.session.Session()
s3_client = session.client(
    's3',
    region_name=SPACES_REGION,
    endpoint_url=SPACES_ENDPOINT,
    aws_access_key_id=SPACES_ACCESS_KEY,
    aws_secret_access_key=SPACES_SECRET_KEY
)

def extract_text_from_email(email_content):
    """Extract relevant details from the email content using DigitalOcean GenAI."""
    logging.debug("Extracting details from email content.")
    prompt = (
        "Extract the following details from the email:\n"
        "- Date of transaction\n"
        "- Amount\n"
        "- Currency\n"
        "- Vendor name\n\n"
        f"Email content:\n{email_content}\n\n"
        "Ensure the output is in JSON format with keys: date, amount, currency, vendor."
    )
    response = client.chat.completions.create(
        model="your-model-id",  # Replace with your GenAI model ID
        messages=[{"role": "user", "content": prompt}]
    )
    logging.debug("GenAI processing completed.")
    return response.choices[0].message.content

def decode_and_save_attachment(attachment):
    """Decode base64-encoded attachment and save it locally with a unique name."""
    file_name = attachment.get("Name")
    encoded_content = attachment.get("Content")

    if not file_name or not encoded_content:
        logging.warning("Invalid attachment, skipping.")
        return None

    unique_file_name = f"{uuid.uuid4()}_{file_name}"
    file_path = os.path.join("/tmp", unique_file_name)

    try:
        with open(file_path, "wb") as file:
            file.write(base64.b64decode(encoded_content))
        logging.info(f"Attachment saved locally: {file_path}")
        return file_path
    except Exception as e:
        logging.error(f"Failed to decode and save attachment {file_name}: {e}")
        return None

def upload_attachment_to_spaces(file_path):
    """Upload a file to DigitalOcean Spaces and return its public URL."""
    file_name = os.path.basename(file_path)
    object_name = f"email-receipt-processor/{file_name}"
    try:
        s3_client.upload_file(file_path, SPACES_BUCKET, object_name, ExtraArgs={"ACL": "public-read"})
        file_url = f"https://{SPACES_BUCKET}.{SPACES_REGION}.cdn.digitaloceanspaces.com/{object_name}"
        logging.info(f"Attachment uploaded to Spaces: {file_url}")
        return file_url
    except Exception as e:
        logging.error(f"Failed to upload attachment {file_name} to Spaces: {e}")
        return None

def process_attachments(attachments):
    """Process all attachments and return their URLs."""
    attachment_urls = []
    for attachment in attachments:
        file_path = decode_and_save_attachment(attachment)
        if file_path:
            file_url = upload_attachment_to_spaces(file_path)
            if file_url:
                attachment_urls.append({"file_name": os.path.basename(file_path), "url": file_url})
            os.remove(file_path)  # Clean up local file
    return attachment_urls

@app.route('/inbound', methods=['POST'])
def handle_inbound_email():
    """Process inbound emails and return extracted JSON."""
    logging.info("Received inbound email request.")
    data = request.json

    email_content = data.get("TextBody", "")
    attachments = data.get("Attachments", [])

    if not email_content:
        logging.error("No email content provided.")
        return jsonify({"error": "No email content provided"}), 400

    extracted_data = extract_text_from_email(email_content)
    attachment_urls = process_attachments(attachments)

    response_data = {
        "extracted_data": extracted_data,
        "attachments": attachment_urls
    }

    # Log the final combined data
    logging.info("Final Response Data: %s", response_data)

    return jsonify(response_data)

if __name__ == "__main__":
    logging.info("Starting Flask application.")
    app.run(port=5000)

```

[Step 3: Deploy to DigitalOcean](#step-3-deploy-to-digitalocean)[](#step-3-deploy-to-digitalocean)
--------------------------------------------------------------------------------------------------

To deploy the updated Flask app, follow the steps from [Day 7](/community/tutorials/deploying-flask-app-on-digitalocean#step-4-deploy-to-digitalocean-s-app-platform). Here’s a quick summary:

1.  **Push Your Updated Code to GitHub**: After making the necessary changes to your Flask app, commit and push the updated code to GitHub. This will trigger an automatic deployment in DigitalOcean’s App Platform.
    
    ```
    git add .
    git commit -m "Add attachment processing with DigitalOcean Spaces"
    git push origin main
    ```
    
2.  **Monitor Deployment**: You can track the progress in the **Deployments** section of your app’s dashboard.
    
    ![DigitalOcean project dashboard showing deployment status](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_project_dashboard.png)
    
3.  **Verify Your Deployment**: After the deployment completes, navigate to your app’s public URL and test its functionality. You can also check the **runtime logs** in the dashboard to confirm that the app started successfully.
    
    ![DigitalOcean app deployment status and configuration](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_app_deployment.png)
    

[Step 4: Test the Entire Workflow](#step-4-test-the-entire-workflow)[](#step-4-test-the-entire-workflow)
--------------------------------------------------------------------------------------------------------

Now that your app is fully configured and ready, it’s time to test the entire workflow. We’ll ensure that the email body is processed, attachments are decoded and uploaded to DigitalOcean  
Spaces, and the final output includes everything we need.

Here’s how you can test step by step:

1.  **Send a Test Email**: Send an email to Postmark with a text body and an attachment. If you’re unsure how to configure Postmark, check [Day 8: Connecting Postmark to Your Flask App](/community/tutorials/setting-up-postmark-for-receipts) where we walked through setting up Postmark to forward emails to your app.
    
    ![Example of an email invoice receipt with highlighted payment details and attachments](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/email_invoice_receipt_screenshot.png)
    
2.  **Check Postmark Activity JSON**: In the Postmark dashboard, navigate to the **Activity** tab. Locate the email you sent, and ensure that the JSON payload includes the text body and Base64-encoded attachment data. This confirms Postmark is correctly forwarding the email data to your app.
    
    ![Postmark JSON response showing encoded email attachments with metadata](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/postmark_json_encoded_attachments.png)
    
3.  **Monitor the Logs**: Check the runtime logs in your DigitalOcean App Platform dashboard to ensure the app processes the JSON payload. We covered how to access runtime logs in [Day 9](/community/tutorials/automating-receipt-parsing-with-digitalocean-genai).
    
    ![DigitalOcean runtime logs interface displaying application log entries](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_runtime_logs_screenshot_2.png)
    
4.  **Verify Spaces Upload**: Visit your DigitalOcean Space to confirm that the files were uploaded successfully. You should see the attachments in your bucket.
    
    ![DigitalOcean Spaces interface showing uploaded files with names and metadata](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_files_interface.png)
    
5.  **Check the Final Output**: The app should log the extracted data and the attachment URLs. These logs will include:
    
    -   Details extracted from the email body.
    -   Public URLs for the uploaded attachments.  
        Refer to [Day 9](/community/tutorials/automating-receipt-parsing-with-digitalocean-genai) for tips on inspecting runtime logs.

By the end of these steps, your workflow will be ready to save data to a database, which we’ll tackle next.

[🎁 Wrap-Up](#wrap-up)[](#wrap-up)
----------------------------------

Today, we taught your app to handle attachments like a pro. Here’s what we did:

-   Created a DigitalOcean Space for secure, scalable storage.
-   Decoded Base64-encoded attachments from Postmark JSON.
-   Ensured unique filenames with `uuid`.
-   Uploaded attachments to DigitalOcean Spaces using `boto3`.
-   Generated public URLs for each file, ready to be used in your receipt processor.

**Up next,** we’ll integrate this data into a database. This will allow you to store extracted email details and attachment URLs for long-term use, making your receipt processor even more powerful. Stay tuned!

#### [Source](https://www.digitalocean.com/community/tutorials/configuring-digitalocean-spaces)

<br/>
---
