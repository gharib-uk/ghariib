---
title: "12 Days of DigitalOcean Day 9 - Automating Receipt Parsing with DigitalOceans GenAI Agent"
date: 2025-01-02T14:00:00.000Z
draft: false
type: posts
categories: 
- flask,digitalocean,python,ai-ml
---
# 12 Days of DigitalOcean Day 9 - Automating Receipt Parsing with DigitalOceans GenAI Agent

<br/>

<br/>
### [🎄 Day 9: Extracting Key Receipt Details with DigitalOcean’s GenAI 🎁](#day-9-extracting-key-receipt-details-with-digitalocean-s-genai)[](#day-9-extracting-key-receipt-details-with-digitalocean-s-genai)

Welcome to **Day 9** of the **12 Days of DigitalOcean**! Over the past few days, we’ve been building an [Email-Based Receipt Processing Service](/community/tutorials/deploying-flask-app-on-digitalocean). The goal is simple: allow users to [forward receipts or invoices to a dedicated email address](/community/tutorials/setting-up-postmark-for-receipts), extract key details like `date`, `amount`, `currency`, and `vendor`, and store them in a database, or a spreadsheet for easy review later.

Sounds handy, right? Now, think about doing this manually—digging through emails, copying details one by one, and pasting them into a spreadsheet. It’s repetitive, error-prone, and, let’s be honest, a waste of your time. That’s where AI comes in.

Instead of writing a parser for every receipt format, you’ll use [DigitalOcean’s GenAI Platform](/products/gen-ai) to handle this dynamically. By the end of this tutorial, you’ll have a Flask app that takes messy receipt emails, processes them with AI, and transforms them into clean, structured JSON.

[🤔 Why Use GenAI Agent for This?](#why-use-genai-agent-for-this)[](#why-use-genai-agent-for-this)
--------------------------------------------------------------------------------------------------

Emails aren’t standardized. Every vendor formats receipts differently, and writing custom parsers for all those variations is impractical. AI is perfect for this kind of problem. It excels at recognizing patterns in unstructured text and extracting relevant information.

With **DigitalOcean’s GenAI Platform**, you get access to pre-trained models from [Meta, Mistral, and Anthropic](https://docs.digitalocean.com/products/genai-platform/details/models/). These models are powerful and flexible, and you can even fine-tune them for specific tasks if needed. For this tutorial, we’ll use a pre-trained model to keep things simple.

[✨ What You’ll Learn](#what-you-ll-learn)[](#what-you-ll-learn)
---------------------------------------------------------------

-   How to create and configure a GenAI agent on DigitalOcean.
-   How to test your agent to ensure it’s extracting the right details.
-   How to integrate the agent with an [already-deployed Flask app](/community/tutorials/deploying-flask-app-on-digitalocean).
-   How to test the entire pipeline, from email to JSON output.

[🛠 What You’ll Need](#what-you-ll-need)[](#what-you-ll-need)
-------------------------------------------------------------

To get the most out of this tutorial, we assume the following:

1.  A Flask App Already Deployed on DigitalOcean: If you haven’t deployed a Flask app yet, you can follow the instructions in [Day 7 - Building and Deploying the Email-Based Receipt Processor](/community/tutorials/deploying-flask-app-on-digitalocean#what-you-ll-need).
2.  Postmark Configured for Email Testing: To test the email-to-receipt processing pipeline, you’ll need Postmark set up to forward emails to your Flask app. See [Day 8 - Connecting Postmark to Your Flask App](/community/tutorials/setting-up-postmark-for-receipts#step-by-step-guide) for a step-by-step guide.

**Note:** Even if you don’t have these set up, you’ll still learn how to:

-   Configure a GenAI agent on DigitalOcean.
-   Interact with the agent programmatically to complete tasks using the [OpenAI Python SDK](https://github.com/openai/openai-python).

[🧑‍🍳 Step-by-Step Guide](#step-by-step-guide)[](#step-by-step-guide)
----------------------------------------------------------------------

### [Step 1: Create a GenAI Agent](#step-1-create-a-genai-agent)[](#step-1-create-a-genai-agent)

Let’s start by creating the core component of this system: a GenAI agent.

-   **Log in to your [DigitalOcean account](https://cloud.digitalocean.com/login)** and navigate to the **[GenAI Platform](https://cloud.digitalocean.com/gen-ai/)**.
    
    ![DigitalOcean GenAI Platform dashboard showing the main interface](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_genai_platform_dashboard.png)
    
-   Click **Create Agent**, and give your agent a name like `receipt-processor-agent`.
    
    ![Configuration screen for creating a new agent with settings options](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/create_agent_configuration.png)
    
-   Add the following instructions to describe the agent’s task:
    

```
Extract the following details from emails:
- Date of transaction
- Amount
- Currency
- Vendor name

Ensure the output is in the following JSON format:
{
    "date": "<date>",
    "amount": "<amount>",
    "currency": "<currency>",
    "vendor": "<vendor>"
}
```

![Screen showing the DigitalOcean agent creation interface with instruction input](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/create_agent_digitalocean.png)

-   **Select a Model**: For this tutorial, select **Llama 3.1 Instruct (8B)**. It’s great for general-purpose instruction-following tasks.
    
    **Pro Tip**: Models can be updated later, even after the agent is created. You can also evaluate models in the [Model Playground](https://cloud.digitalocean.com/gen-ai/model-playground) before selecting one. For a complete list of supported models, refer to the [DigitalOcean documentation](https://docs.digitalocean.com/products/genai-platform/details/models/).
    

![DigitalOcean droplet creation interface showing model selection](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_droplet_creation.png)

-   **Optional: Add a Knowledge Base** You can enhance your agent by adding a knowledge base. This allows it to learn from specific documents or datasets you provide. For simplicity, we’ll skip this step in this tutorial.
    
-   **Create the Agent**: Scroll down and click **Create Agent** to complete the setup.
    

![DigitalOcean create agent screen with final configuration options](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_create_agent_screen.png)

### [Step 2: Test Your Agent](#step-2-test-your-agent)[](#step-2-test-your-agent)

Before hooking the agent into an app, let’s make sure it works. Go to the **Playground** tab in the agent settings. This is a testing area where you can interact with the agent and see how it responds.

![DigitalOcean GenAI Playground interface for testing the agent](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_playground_interface.png)

-   Paste this “sample email” text into the Playground:

```
Thank you for your purchase! Your order #12345 has been processed.  Total: $35.99. Date: December 29, 2024. Thank you for shopping at Amazon.com
```

-   The agent should return:

```
{
    "date": "Dec 29, 2024",
    "amount": "35.99",
    "currency": "USD",
    "vendor": "Amazon"
}
```

![DigitalOcean settings interface showing agent configuration](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_settings_interface.png)

-   If it doesn’t, adjust the instructions and test again. This step is about making sure the agent understands what you want.
    
    ![DigitalOcean settings interface showing instruction configuration](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_settings_interface_instructions.png)
    

### [Step 3: Grab the Endpoint and Access Key](#step-3-grab-the-endpoint-and-access-key)[](#step-3-grab-the-endpoint-and-access-key)

-   Once your agent works, it’s time to connect it to your app. Go to the **Endpoint** tab in the agent settings and copy the URL. It should look something like this:

```
https://agent-1234abcd5678efgh1234-abcde.ondigitalocean.app
```

![DigitalOcean receipt processor agent endpoint configuration](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_receipt_processor_agent_endpoint.png)

-   Next, go to the **Settings** tab and create an access key. This key lets your app securely communicate with the agent. Save the key somewhere safe—you’ll need it soon.

![DigitalOcean settings page showing endpoint access keys](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_settings_endpoint_access_keys.png)

### [Step 4: Securely Store Environment Variables in DigitalOcean](#step-4-securely-store-environment-variables-in-digitalocean)[](#step-4-securely-store-environment-variables-in-digitalocean)

Now that we have the agent endpoint and secure agent key, we need to store them securely so they’re accessible to our Flask app.

1.  **Go to App Settings**: Navigate to your app in the DigitalOcean dashboard and click **Settings**.
    
2.  **Locate Environment Variables**: Scroll to the **App-Level Environment Variables** section.
    
    ![DigitalOcean app settings page with configuration options](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_app_settings_screenshot.png)
    
3.  **Add the Following Variables**:
    
    -   `AGENT_BASE_URL`: Paste the GenAI agent’s base URL, e.g., `https://agent-1234abcd5678efgh1234-abcde.ondigitalocean.app`.
    -   `SECURE_AGENT_KEY`: Paste the secure agent key generated in the GenAI settings.
4.  **Save and Redeploy**: Save the changes, and DigitalOcean will automatically redeploy your app with the new environment variables.
    

![Environment variable editor interface showing configuration](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/environment_variable_editor_screenshot.png)

![DigitalOcean App Platform environment variables configuration page](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_app_platform_environment_variables.png)

### [Step 5: Update Your Flask App with the OpenAI SDK](#step-5-update-your-flask-app-with-the-openai-sdk)[](#step-5-update-your-flask-app-with-the-openai-sdk)

Now, we’ll update our Flask app to communicate with the GenAI agent using the [OpenAI SDK](https://github.com/openai/openai-python). Here’s how we’ll approach it:

1.  **Install Required Libraries**: Run this command to ensure you have the necessary libraries:
    
    ```
    pip install openai flask python-dotenv
    ```
    
2.  **Freeze Requirements**: Generate a `requirements.txt` file to track your dependencies:
    
    ```
    pip freeze > requirements.txt
    ```
    
3.  **Set Up the OpenAI SDK**: In the app, we’ll initialize the OpenAI SDK and configure it to use the DigitalOcean GenAI endpoint. This is what handles communication with the agent we created earlier.
    
    ```
    from openai import OpenAI
    import os
    from dotenv import load_dotenv
    
    # Load environment variables
    load_dotenv()
    
    # Secure agent key and endpoint
    SECURE_AGENT_KEY = os.getenv("SECURE_AGENT_KEY")
    AGENT_BASE_URL = os.getenv("AGENT_BASE_URL")
    AGENT_ENDPOINT = f"{AGENT_BASE_URL}/api/v1/"
    
    # Initialize the OpenAI client
    client = OpenAI(
        base_url=AGENT_ENDPOINT,
        api_key=SECURE_AGENT_KEY
    )
    ```
    
4.  **Add a Function to Process Email Content**: Now we’ll add a function to send email content to the GenAI agent, process it using a prompt, and return the result. Here’s how it works:
    
    ```
    def process_with_ai(email_content):
        """
        Process email content with GenAI.
        """
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
    
        return response.choices[0].message.content
    ```
    
5.  **Update the Flask Endpoint**: Finally, we’ll connect this function to the `/inbound` route of the Flask app to handle incoming emails. Here’s the updated endpoint:
    
    ```
    @app.route('/inbound', methods=['POST'])
    def handle_inbound_email():
        """
        Process inbound emails and return extracted JSON.
        """
        email_content = request.json.get("TextBody", "")
        if not email_content:
            return jsonify({"error": "No email content provided"}), 400
    
        extracted_data = process_with_ai(email_content)
        return jsonify({"extracted_data": extracted_data})
    ```
    

#### Complete Flask App with GenAI Integration

app.py

```
# app.py

from flask import Flask, request, jsonify
import os
from dotenv import load_dotenv
from openai import OpenAI
import logging

# Load environment variables
load_dotenv()

# Initialize Flask app
app = Flask(__name__)

# Configure logging
logging.basicConfig(level=logging.INFO)

# Secure agent key and endpoint
SECURE_AGENT_KEY = os.getenv("SECURE_AGENT_KEY")
AGENT_BASE_URL = os.getenv("AGENT_BASE_URL")
AGENT_ENDPOINT = f"{AGENT_BASE_URL}/api/v1/"

# Initialize the OpenAI client for GenAI
client = OpenAI(
    base_url=AGENT_ENDPOINT,
    api_key=SECURE_AGENT_KEY
)

def process_with_ai(email_content):
    """
    Process email content with GenAI.
    """
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

    return response.choices[0].message.content

@app.route('/inbound', methods=['POST'])
def handle_inbound_email():
    """
    Process inbound emails and return extracted JSON.
    """
    email_content = request.json.get("TextBody", "")
    if not email_content:
        logging.error("No email content provided.")
        return jsonify({"error": "No email content provided"}), 400

    extracted_data = process_with_ai(email_content)
    logging.info("Extracted Data: %s", extracted_data)  # Log the extracted data

    return jsonify({"extracted_data": extracted_data})

if __name__ == "__main__":
    app.run(port=5000)
```

### [Step 6: Deploy to DigitalOcean](#step-6-deploy-to-digitalocean)[](#step-6-deploy-to-digitalocean)

To deploy the updated Flask app, follow the steps from [Day 7](/community/tutorials/deploying-flask-app-on-digitalocean#step-4-deploy-to-digitalocean-s-app-platform). Here’s a quick summary:

1.  **Push Your Updated Code to GitHub**: After making the necessary changes to your Flask app, commit and push the updated code to GitHub. This will trigger an automatic deployment in DigitalOcean’s App Platform.

```
git add .
git commit -m "Add GenAI integration for receipt processing"
git push origin main
```

![Terminal showing git commit and push commands](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/terminal_git_commit_push.png)

2.**Monitor Deployment**: You can track the progress in the **Deployments** section of your app’s dashboard.

![DigitalOcean project dashboard showing deployment status](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_project_dashboard.png)

3.**Verify Your Deployment**: After the deployment completes, navigate to your app’s public URL and test its functionality. You can also check the **runtime logs** in the dashboard to confirm that the app started successfully.

![DigitalOcean app deployment status and configuration](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_app_deployment.png)

### [Step 7: Test the Entire Pipeline](#step-7-test-the-entire-pipeline)[](#step-7-test-the-entire-pipeline)

1.  **Send a Sample Email**: Forward a sample receipt email to the Postmark address you configured in [Day 8](/community/tutorials/setting-up-postmark-for-receipts).
    
    ![Example receipt email showing order details](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/order_receipt_email.png)
    
2.  **Monitor Postmark Activity**: Log in to Postmark and confirm that the email was forwarded to your app. Refer to the [Day 8 tutorial](/community/tutorials/setting-up-postmark-for-receipts#monitoring-activity).
    
    ![Email dashboard showing message activity](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/email_dashboard_screenshot.png)
    
    ![Postmark API response showing successful delivery](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/postmark_api_response_screenshot.png)
    
3.  **Check Runtime Logs**: In the DigitalOcean App Platform dashboard, view your app’s runtime logs. You should see the JSON output from the Flask app.
    
    **Expected Output**:
    
    ```
    2024-12-31 12:34:56,789 INFO Extracted Data: {
     "date": "Dec 29, 2024",
     "amount": "35.99",
     "currency": "USD",
     "vendor": "Amazon"
    }
    
    ```
    
    !\[DigitalOcean runtime logs showing application output\]([https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean\_runtime\_logs\_screenshot](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_runtime_logs_screenshot) \_json\_response.png)
    

[🎁 Wrap-Up](#wrap-up)[](#wrap-up)
----------------------------------

Here’s what we accomplished today:

1.  Created and configured a GenAI agent on DigitalOcean to process receipt emails.
2.  Tested the agent interactively and programmatically to ensure it extracted the right details.
3.  Updated the Flask app to integrate with the GenAI agent using the OpenAI SDK.
4.  Deployed the updated app to DigitalOcean.
5.  Tested the entire pipeline, from sending an email to viewing the extracted JSON in logs.

Here is the previous tutorial from this series on [Day 8:Connecting Postmark to Your Flask App](/community/tutorials/setting-up-postmark-for-receipts).

**Up next**: In the next tutrorial, you’ll complete the pipeline by storing the extracted JSON data into a database. This will make your Email-Based Receipt Processing Service ready for real-world use. Stay tuned—Day 10 is all about tying it all together! 🚀

#### [Source](https://www.digitalocean.com/community/tutorials/automating-receipt-parsing-with-digitalocean-genai)

<br/>
---
