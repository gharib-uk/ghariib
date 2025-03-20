---
title: "12 Days of DigitalOcean Day 11 - Save Receipt Data and Attachments in Google Sheets"
date: 2025-01-07T14:00:00.000Z
draft: false
type: posts
categories: 
- flask,spaces,digitalocean
---
# 12 Days of DigitalOcean Day 11 - Save Receipt Data and Attachments in Google Sheets

<br/>

<br/>
[Introduction](#introduction)[](#introduction)
----------------------------------------------

Welcome to **Day 11** of the “12 Days of DigitalOcean” series! Yesterday, we [set up DigitalOcean Spaces to securely store attachments](/community/tutorials/configuring-digitalocean-spaces) from inbound emails. Today, we’ll take it a step further by integrating **Google Sheets** to store both the extracted receipt details and the URLs of uploaded attachments.

Google Sheets is simple, familiar, and ideal for managing and tracking receipt data. By the end of this tutorial, your app will seamlessly store extracted receipt details and attachment URLs in a Google Sheet for easy organization. Let’s get started!

**Note:** While we’re using Google Sheets for this project, you can use any database, such as PostgreSQL or MongoDB, for a more robust storage solution. If you prefer PostgreSQL, check out these tutorials from the Birthday Reminder series to get started:

-   [Setting Up a PostgreSQL Database on DigitalOcean](/community/tutorials/setting-up-postgresql-database-for-birthday-reminders)
-   [Connecting to PostgreSQL with Python](/community/tutorials/connecting-to-postgresql-database-with-python)

[**What You’ll Learn**](#what-you-ll-learn)[](#what-you-ll-learn)
-----------------------------------------------------------------

1.  Configure a Google Cloud Project and enable the Sheets API.
2.  Create a [service account](https://docs.gspread.org/en/v6.1.3/oauth2.html#for-bots-using-service-account) to securely access Google Sheets.
3.  Store Google credentials [safely in DigitalOcean App Platform](https://docs.digitalocean.com/products/app-platform/how-to/use-environment-variables/).
4.  Use [gspread](https://docs.gspread.org/en/v6.1.3/) to programmatically update Google Sheets with receipt data and attachment URLs.

[**🛠 What You’ll Need**](#what-you-ll-need)[](#what-you-ll-need)
-----------------------------------------------------------------

To get the most out of this tutorial, we assume the following:

1.  **A Flask App Already Deployed on DigitalOcean:** If you haven’t deployed a Flask app yet, you can follow the instructions in [Day 7: Building and Deploying the Email-Based Receipt Processor](/community/tutorials/deploying-flask-app-on-digitalocean#what-you-ll-need).
2.  **Postmark Configured for Email Testing:** To test the email-to-receipt processing pipeline, you’ll need Postmark set up to forward emails to your Flask app. See [Day 8: Connecting Postmark to Your Flask App](/community/tutorials/setting-up-postmark-for-receipts#step-by-step-guide) for a step-by-step guide.
3.  **DigitalOcean Spaces Setup:** Processed attachments will be stored in a DigitalOcean Space. If you don’t have a Space yet, you can follow the instructions in [Day 10: Storing Attachments in DigitalOcean Spaces](/community/tutorials/configuring-digitalocean-spaces).
4.  **Access to Google Cloud Console:** You’ll need access to Google Cloud Console to set up APIs, create a service account, and generate credentials for Google Sheets.
5.  **A Google Sheet Named `Receipts`:** Ensure you have a blank Google Sheet titled `Receipts` (or any other name) ready to store the extracted data. You’ll share this sheet with the service account created during this tutorial.

**Note:** Even if you don’t have everything set up yet, you’ll still learn how to:

-   Configure your Google Cloud Project to interact with Google Sheets.
-   Create a service account for secure access to Google Sheets.
-   Use the `gspread` library to update Google Sheets programmatically.
-   Securely store sensitive credentials in DigitalOcean App Platform.

[Step 1: Set Up Google Sheets API](#step-1-set-up-google-sheets-api)[](#step-1-set-up-google-sheets-api)
--------------------------------------------------------------------------------------------------------

Before we can save anything to Google Sheets, we need to make sure our app has permission to interact with it. This involves creating a [Google Cloud Project](https://console.cloud.google.com/projectselector2/apis/dashboard?inv=1&invt=AbmNnw&supportedpurview=project), enabling the necessary APIs, and setting up a service account to handle the heavy lifting.

### [1.1 **Create a Google Cloud Project**](#1-1-create-a-google-cloud-project)[](#1-1-create-a-google-cloud-project)

A Google Cloud Project is the foundation for everything you’ll do with Google APIs. It acts as your app’s home base for managing resources and configurations.

1.  Go to the Google Cloud Console and click **New Project**.
    
    ![Google Cloud New Project](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/google_cloud_new_project.png)
    
2.  Name your project (e.g., `Receipt Processor`) and click **Create**.
    
    ![Google Cloud New Project Setup](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/google_cloud_new_project_setup.png)
    

### [1.2 **Enable APIs**](#1-2-enable-apis)[](#1-2-enable-apis)

The **Google Sheets API** lets your app write to Google Sheets, and the **Google Drive API** gives your app access to files stored in Google Drive.

1.  Navigate to the new project. Then go to **APIs & Services > Enabled APIs & Services**.
2.  Search for **Google Sheets API** and **Google Drive API**.
3.  Click **Enable** for both.

![Google Cloud Console APIs Services](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/google_cloud_console_apis_services.png)

### [1.3 **Create a Service Account**](#1-3-create-a-service-account)[](#1-3-create-a-service-account)

Your app can’t log in to Google Sheets the same way you do. Instead, it needs a **service account**—a special bot that handles authentication and permissions for your app. This ensures your app can securely access Google Sheets without requiring manual logins or user interaction.

1.  Go to **APIs & Services > Credentials** and click **\+ Create Credentials**.
    
    ![Google Cloud Credentials Page](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/google_cloud_credentials_page.png)
    
2.  Select **Service Account**
    
    ![Google Cloud Service Account Setup](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/google_cloud_service_account_setup.png)
    
3.  Give your service account a name (e.g., `Receipt Bot`) and a description like _“Handles communication with Google Sheets for receipt tracking.”_
    
    <$> \[info\] **Note:** Your service account will generate an email address (e.g., `something@project-id.iam.gserviceaccount.com`). You’ll need this later to share access to your Google Sheet. <$>
    
    ![Create Service Account Google Cloud](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/create_service_account_google_cloud.png)
    
4.  Click **Create and Continue** until you’re back at the credentials screen. You can skip assigning roles if you don’t need extra permissions.
    

### [1.4 **Download Credentials**](#1-4-download-credentials)[](#1-4-download-credentials)

The service account needs credentials to prove its identity. These come in the form of a JSON file.

1.  In the **Credentials** screen, locate your service account and click the pencil icon to edit it.
    
2.  Go to the **Keys** tab and click **Add Key > Create New Key**.
    
3.  Select **JSON** and download the file.
    
    ![Google Cloud Create Private Key](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/google_cloud_create_private_key.png)
    

<$> \[info\] **Note:** This file contains everything your app needs to authenticate with Google Sheets. Treat it like a password—don’t share it or commit it to Git. <$>

### [1.5 **Share the Google Sheet**](#1-5-share-the-google-sheet)[](#1-5-share-the-google-sheet)

Finally, give the service account permission to access your Google Sheet.

1.  Get the **client\_email** from the JSON file you downloaded
    
    ![Service Account JSON Example](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/service_account_json_example.png)
    
2.  Open your Google Sheet. Click **Share**, paste the email address, and give it **Editor** access.
    
    ![Google Sheets Share Dialog](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/google_sheets_share_dialog.png)
    

Now your app can read from and write to the Google Sheet securely.

[**Step 2: Store Your Credentials Securely**](#step-2-store-your-credentials-securely)[](#step-2-store-your-credentials-securely)
---------------------------------------------------------------------------------------------------------------------------------

We don’t want sensitive credentials like the service account JSON lying around in our codebase—it’s a security risk. Instead, we’ll store the credentials as an environment variable in DigitalOcean App Platform, where they’ll be safe and accessible only at runtime.

The tricky part is that Google’s credentials JSON file is formatted for readability, but environment variables need a single-line string. Let’s fix that first.

### [2.1 **Format the JSON**](#2-1-format-the-json)[](#2-1-format-the-json)

Google’s credentials JSON is pretty, but we need it compact for storage as an environment variable. Here’s how to convert it into a single-line string:

1.  Run this Python snippet on your local machine:
    
    ```
    import json
    with open("path/to/service-account.json", "r") as file:
        creds = json.load(file)
    print(json.dumps(creds))
    ```
    

This will output the JSON as a single line. Copy the result.

### [2.2 **Add to DigitalOcean**](#2-2-add-to-digitalocean)[](#2-2-add-to-digitalocean)

Now, let’s store the single-line JSON securely in DigitalOcean App Platform:

1.  Go to your app’s **Settings > Environment Variables**.
    
2.  Add a new variable:
    
    1.  Click on Raw Editor, and paste the single-line JSON string.
    2.  Paste the single-line JSON string as the value:
    
    ```
    GOOGLE_CREDS={"type": "service_account", "project_id": "receipt-processor-45a6b", ......}
    ```
    
    ![Environment Variable Editor Screenshot](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/environment_variable_editor_screenshot_2.png)
    
3.  Check the **Encrypt** option to keep the credentials safe. Click Save.
    
4.  This action will trigger an automatic redeployment of your app.
    
    ![Bulk Editor Settings](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/bulk_editor_settings.png)
    
    That’s it! Your credentials are now securely stored, and your app is ready to use them at runtime.
    

[**Step 3: Update the App**](#step-3-update-the-app)[](#step-3-update-the-app)
------------------------------------------------------------------------------

Now that your credentials are securely stored, it’s time to connect your app to Google Sheets and update it to handle receipt data and attachments.

Before diving into the code, let’s make sure you’ve installed the necessary dependencies and updated your `requirements.txt` file. This ensures your app has all the libraries it needs to run seamlessly.

### [**3.1 Install Dependencies**](#3-1-install-dependencies)[](#3-1-install-dependencies)

Run the following command to install all required Python libraries:

```
pip install flask boto3 python-dotenv gspread oauth2client openai
```

Next, freeze your dependencies into a `requirements.txt` file:

```
pip freeze > requirements.txt
```

This step captures all your app’s dependencies, making it easier to deploy and manage in DigitalOcean App Platform.

### [3.2 **Connect to Google Sheets**](#3-2-connect-to-google-sheets)[](#3-2-connect-to-google-sheets)

This step allows your app to authenticate with Google Sheets using the credentials stored in `GOOGLE_CREDS`. Once authenticated, the app can read and write to the sheet programmatically.

Add this code to your app:

```
import gspread
from oauth2client.service_account import ServiceAccountCredentials
import os
import json

# Load credentials from environment variables
# GOOGLE_CREDS is the single-line JSON string that contains the service account credentials.
creds_json = os.getenv("GOOGLE_CREDS")
creds_dict = json.loads(creds_json)  # Convert the JSON string back into a dictionary.

# Define the required scopes for accessing Google Sheets and Google Drive.
# The "spreadsheets" scope allows the app to read/write Sheets, and the "drive" scope allows access to Sheets stored in Drive.
scope = ["https://spreadsheets.google.com/feeds", "https://www.googleapis.com/auth/drive"]

# Authenticate using the service account credentials.
credentials = ServiceAccountCredentials.from_json_keyfile_dict(creds_dict, scope)

# Create a gspread client to interact with Google Sheets.
sheets_client = gspread.authorize(credentials)

# Open the Google Sheet by name. Replace "Receipts" with the name of your Sheet.
# This provides access to the first worksheet in the Sheet, which you can use to read/write rows.
sheet = sheets_client.open("Receipts").sheet1
```

### [3.3 **Save Receipt Data and Attachments**](#3-3-save-receipt-data-and-attachments)[](#3-3-save-receipt-data-and-attachments)

Your app processes email data and attachments—this function ensures both are saved to Google Sheets. Each row includes receipt details (e.g., vendor, amount, currency) and attachment URLs.

Add this function to your app:

```
def save_to_google_sheets(extracted_data, attachment_urls):
    """
    Save extracted receipt data and attachment URLs to Google Sheets.
    """
    try:
        # Combine all attachment URLs into a single string, separated by commas.
        # This ensures all URLs are stored in one cell in the Sheet.
        attachments_str = ", ".join([attachment["url"] for attachment in attachment_urls])

        # Append a new row with extracted data and attachment URLs to the Google Sheet.
        # Each element in the list corresponds to a column in the Sheet.
        sheet.append_row([
            extracted_data.get("vendor", ""),
            extracted_data.get("amount", ""),
            extracted_data.get("currency", ""),
            extracted_data.get("date", ""),
            attachments_str  # Store all attachment URLs in a single column
        ])
        # Log a success message to confirm data was saved.
        logging.info("Data and attachments saved to Google Sheets.")
    except Exception as e:
        # Log an error if something goes wrong while saving to the Sheet.
        logging.error(f"Failed to save data to Google Sheets: {e}")
```

### [3.4 Final Code](#3-4-final-code)[](#3-4-final-code)

Here’s the complete code for your app, consolidating all the pieces we’ve worked on so far.

```
from flask import Flask, request, jsonify
import os
import base64
import uuid
import boto3
from dotenv import load_dotenv
from openai import OpenAI
import logging
import json
import gspread
from oauth2client.service_account import ServiceAccountCredentials

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

# Google Sheets API setup
creds_json = os.getenv("GOOGLE_CREDS")
if not creds_json:
    raise ValueError("Google credentials not found in environment variables.")
creds_dict = json.loads(creds_json)

scope = ["https://spreadsheets.google.com/feeds", "https://www.googleapis.com/auth/drive"]
credentials = ServiceAccountCredentials.from_json_keyfile_dict(creds_dict, scope)
sheets_client = gspread.authorize(credentials)
sheet = sheets_client.open("Receipts").sheet1  # Replace "Receipts" with your sheet name

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
    return json.loads(response.choices[0].message.content)

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

def save_to_google_sheets(extracted_data, attachment_urls):
    """Save extracted receipt data and attachment URLs to Google Sheets."""
    try:
        # Combine all attachment URLs into a single string (comma-separated)
        attachments_str = ", ".join([attachment["url"] for attachment in attachment_urls])

        # Append a new row with extracted data and attachment URLs
        sheet.append_row([
            extracted_data.get("vendor", ""),
            extracted_data.get("amount", ""),
            extracted_data.get("currency", ""),
            extracted_data.get("date", ""),
            attachments_str  # Store all attachment URLs in a single column
        ])
        logging.info("Data and attachments saved to Google Sheets.")
    except Exception as e:
        logging.error(f"Failed to save data to Google Sheets: {e}")

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

    # Save extracted data and attachment URLs to Google Sheets
    save_to_google_sheets(extracted_data, attachment_urls)

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

[Step 4: Deploy to DigitalOcean](#step-4-deploy-to-digitalocean)[](#step-4-deploy-to-digitalocean)
--------------------------------------------------------------------------------------------------

To deploy the updated Flask app, follow the steps from [Day 7: Building and Deploying the Email-Based Receipt Processor](/community/tutorials/deploying-flask-app-on-digitalocean#step-4-deploy-to-digitalocean-s-app-platform). Here’s a quick summary:

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
    

[Step 5: Test the Entire Workflow](#step-5-test-the-entire-workflow)[](#step-5-test-the-entire-workflow)
--------------------------------------------------------------------------------------------------------

Now that your app is fully configured and ready, it’s time to test the entire workflow. We’ll ensure that the email body is processed, attachments are decoded and uploaded to DigitalOcean Spaces, and the final output includes receipt details and attachment URLs, **all saved in Google Sheets.**

Here’s how you can test step by step:

1.  **Send a Test Email**: Send an email to Postmark with a text body and an attachment. If you’re unsure how to configure Postmark, check [Day 8: Connecting Postmark to Your Flask App](/community/tutorials/setting-up-postmark-for-receipts) where we walked through setting up Postmark to forward emails to your app.
    
    ![Example of an email invoice receipt with highlighted payment details and attachments](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/email_invoice_receipt_screenshot.png)
    
2.  **Check Postmark Activity JSON**: In the Postmark dashboard, navigate to the **Activity** tab. Locate the email you sent, and ensure that the JSON payload includes the text body and Base64-encoded attachment data. This confirms Postmark is correctly forwarding the email data to your app.
    
    ![Postmark JSON response showing encoded email attachments with metadata](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/postmark_json_encoded_attachments.png)
    
3.  **Monitor the Logs**: Check the runtime logs in your DigitalOcean App Platform dashboard to ensure the app processes the JSON payload. We covered how to access runtime logs in [Day 9: Automating Receipt Parsing with DigitalOcean’s GenAI Agent](/community/tutorials/automating-receipt-parsing-with-digitalocean-genai).
    
    ![DigitalOcean runtime logs interface displaying application log entries](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_runtime_logs_screenshot_2.png)
    
4.  **Verify Spaces Upload**: Visit your DigitalOcean Space to confirm that the files were uploaded successfully. You should see the attachments in your bucket.
    
    ![DigitalOcean Spaces interface showing uploaded files with names and metadata](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_files_interface.png)
    
5.  **Check Google Sheets**: Open your Google Sheet and confirm that receipt details and attachment URLs are saved as a new row. The details should include:
    
    -   Vendor, amount, currency, and date extracted from the email body.
    -   Comma-separated URLs for the uploaded attachments in the last column.
    
    ![Google Sheets Receipts](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/google_sheets_receipts.png)
    

By the end of these steps, your app will have successfully completed the full email-to-Google Sheets workflow, setting the stage for further automation.

[**🎁 Wrap-Up**](#wrap-up)[](#wrap-up)
--------------------------------------

Amazing work! Today, you’ve learned how to seamlessly integrate **Google Sheets** into your app to manage receipt data. Specifically, you:

-   Set up a Google Cloud Project and enabled the Sheets API.
    
-   Created a service account and securely stored its credentials in DigitalOcean App Platform.
    
-   Used `gspread` to programmatically update Google Sheets with receipt details and attachment URLs.
    

**Up Next:** In the final tutorial, we’ll complete the automation by enabling your app to send confirmation emails whenever a receipt is processed successfully. These emails will include the extracted receipt details and a direct link to the Google Sheet for easy access and verification.

See you in the last chapter of the 12 Days of DigitalOcean series! 🚀

#### [Source](https://www.digitalocean.com/community/tutorials/storing-receipt-details-in-google-sheets)

<br/>
---
