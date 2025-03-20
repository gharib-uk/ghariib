---
title: "12 Days of DigitalOcean Day 12 - Sending Confirmation Emails with Resend"
date: 2025-01-08T14:00:00.000Z
draft: false
type: posts
categories: 
- flask,email,python
---
# 12 Days of DigitalOcean Day 12 - Sending Confirmation Emails with Resend

<br/>

<br/>
Welcome to the final day of our **12 Days of DigitalOcean** series! We’ve come a long way, building an [Email-Based Receipt Processing Service](/community/tutorials/deploying-flask-app) that extracts receipt details from [Postmark](/community/tutorials/setting-up-postmark-for-receipts) using [DigitalOcean’s GenAI Agent](/community/tutorials/automating-receipt-parsing-with-digitalocean-genai), securely [stores attachments in DigitalOcean Spaces](/community/tutorials/configuring-digitalocean-spaces), and [saves the extracted data in Google Sheets](/community/tutorials/storing-receipt-details-in-google-sheets).

Today, we’ll add the final touch—sending confirmation emails back to the sender with the receipt details, attachment links, and a link to the Google Spreadsheet. This final step ties everything together, ensuring that users get immediate feedback that their receipts have been successfully processed.

[**🚀 What You’ll Learn**](#what-you-ll-learn)[](#what-you-ll-learn)
--------------------------------------------------------------------

By the end of this tutorial, you’ll know how to:

1.  Use the Resend API to send confirmation emails programmatically.
2.  Securely manage sensitive credentials using environment variables.
3.  Format and send transactional emails with receipt details, attachment links, and spreadsheet URLs.
4.  Test and troubleshoot a complete email processing workflow.

[**🛠 What You’ll Need**](#what-you-ll-need)[](#what-you-ll-need)
-----------------------------------------------------------------

If you’d like to build along, we assume that you’ve followed [Day 11: Save Receipt Data and Attachments in Google Sheets](/community/tutorials/storing-receipt-details-in-google-sheets) and already have:

-   A [deployed Flask app](/community/tutorials/deploying-flask-app) for processing receipt emails.
-   [Google Sheets](/community/tutorials/storing-receipt-details-in-google-sheets) and [DigitalOcean Spaces integration](/community/tutorials/configuring-digitalocean-spaces) set up.

If you’re just interested in learning how to integrate **Resend** for sending confirmation emails, you’ll need:

-   A Resend account: Sign up at [Resend](https://resend.com/).
-   An API key: Generate it from your Resend dashboard.

[Step 1: Create a Resend Account and Get the API Key](#step-1-create-a-resend-account-and-get-the-api-key)[](#step-1-create-a-resend-account-and-get-the-api-key)
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

To send emails programmatically, we’ll use **Resend**, a developer-friendly API for sending transactional emails. It simplifies sending emails so you don’t have to wrestle with setting up an email server, managing SMTP configurations, or worrying about spam filters.

1.  First, go to [Resend](https://resend.com/) and sign up for a free account. Once logged in, navigate to the **API Keys** section of the dashboard and generate a new API key.
    
    ![API Keys Dashboard Screenshot](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/api_keys_dashboard_screenshot.png)
    
2.  Give your API key a **descriptive name**, like `Receipt Processor App`, and set its **permission** to `Full Access`.
    
    ![Add API Key Receipt Processor](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/add_api_key_receipt_processor.png)
    
3.  **Copy the API Key:** Your API key will be shown only once—copy it and keep it safe. You’ll need it in the next step to authenticate your app with Resend.
    
    ![View Resend API Key Warning](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/view_api_key_warning.png)
    

[Step 2: Update Your Environment Variables](#step-2-update-your-environment-variables)[](#step-2-update-your-environment-variables)
-----------------------------------------------------------------------------------------------------------------------------------

Now that we have the Resend API key, let’s save it as an environment variable in DigitalOcean, just like we’ve been doing throughout this series.

For the Resend integration, we need to save two environment variables:

-   `RESEND_API_KEY`: The API key you generated in Step 1, which authenticates your app with Resend.
-   `RESEND_EMAIL_FROM`: The sender email address you’ll use to send confirmation emails. This should be an address verified in your Resend account.

To add these variables, follow these steps:

1.  Head over to your DigitalOcean App Platform dashboard, find your Flask app, and navigate to the **Settings** tab. Under **Environment Variables**, add the two variables:
    
    -   Key: `RESEND_API_KEY`
        
        -   Value: Paste the API key you generated in Step 1.
    -   Key: `RESEND_EMAIL_FROM`
        
        -   Value: Enter a verified sender email address from your Resend account.
2.  Save your changes to make the Resend API key available to your Flask app, which we will update next.
    
    ![DigitalOcean Project Settings](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_project_settings.png)
    

[Step 3: Install the Resend Python Library](#step-3-install-the-resend-python-library)[](#step-3-install-the-resend-python-library)
-----------------------------------------------------------------------------------------------------------------------------------

Next, we’ll install the Resend Python library to handle the API for us. It keeps your code clean and avoids dealing with raw HTTP requests. Run this in your terminal:

```
pip install resend
```

[Step 4: Update `requirements.txt`](#step-4-update-requirements-txt)[](#step-4-update-requirements-txt)
-------------------------------------------------------------------------------------------------------

Instead of editing `requirements.txt` by hand, use `pip freeze` to list all installed dependencies with exact versions. Run this:

```
pip freeze > requirements.txt
```

This updates `requirements.txt` with everything your app needs, including `resend`.

[Step 5: Write the Function to Send Emails](#step-5-write-the-function-to-send-emails)[](#step-5-write-the-function-to-send-emails)
-----------------------------------------------------------------------------------------------------------------------------------

Now it’s time to add the logic for sending confirmation emails. Think of it like emailing a friend to let them know their package has arrived—only here, it’s for receipts.

We’ll write a `send_confirmation_email` function that takes the recipient’s email, receipt details, attachment links, and Google Spreadsheet URL. Using Resend, it will format this into an email and send it. Here’s the function:

```
def send_confirmation_email(to_email, receipt_data, attachment_urls, spreadsheet_url):
    """
    Send a confirmation email with receipt details and attachment URLs.
    """
    email_from = os.getenv('RESEND_EMAIL_FROM')  # Set this in your environment variables
    subject = "Receipt Processed Successfully"
    email_body = f"""
    <h1>Receipt Confirmation</h1>
    <p>Your receipt has been successfully processed. Here are the details:</p>
    <ul>
        <li><strong>Vendor:</strong> {receipt_data.get('vendor', 'N/A')}</li>
        <li><strong>Amount:</strong> {receipt_data.get('amount', 'N/A')}</li>
        <li><strong>Currency:</strong> {receipt_data.get('currency', 'N/A')}</li>
        <li><strong>Date:</strong> {receipt_data.get('date', 'N/A')}</li>
    </ul>
    <p><strong>Attachments:</strong></p>
    <ul>
        {''.join(f'<li><a href="{url["url"]}">{url["file_name"]}</a></li>' for url in attachment_urls)}
    </ul>
    <p>You can view the processed data in the spreadsheet: <a href="{spreadsheet_url}">Google Spreadsheet</a></p>
    """
    try:
        resend.Emails.send({
            "from": email_from,
            "to": to_email,
            "subject": subject,
            "html": email_body
        })
        logging.info(f"Confirmation email sent to {to_email}.")
    except Exception as e:
        logging.error(f"Failed to send confirmation email: {e}")
```

[Step 5: Deploy to DigitalOcean](#step-5-deploy-to-digitalocean)[](#step-5-deploy-to-digitalocean)
--------------------------------------------------------------------------------------------------

To deploy the updated Flask app, follow the steps from [Day 7: Building and Deploying the Email-Based Receipt Processor](/community/tutorials/deploying-flask-app-on-digitalocean#step-4-deploy-to-digitalocean-s-app-platform). Here’s a quick summary:

1.  **Push Your Updated Code to GitHub**: After making the necessary changes to your Flask app, commit and push the updated code to GitHub. This will trigger an automatic deployment in DigitalOcean’s App Platform.
    
    ```
    git add .
    git commit -m "Add Resend integration for confirmation emails"
    git push origin main
    ```
    
2.  **Monitor Deployment**: You can track the progress in the **Deployments** section of your app’s dashboard.
    
    ![DigitalOcean project dashboard showing deployment status](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_project_dashboard.png)
    
3.  **Verify Your Deployment**: After the deployment completes, navigate to your app’s public URL and test its functionality. You can also check the **runtime logs** in the dashboard to confirm that the app started successfully.
    
    ![DigitalOcean app deployment status and configuration](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_app_deployment.png)
    
4.  **Check Runtime Logs**: If something isn’t working as expected, use the **Runtime Logs** tab in the App Platform dashboard to debug runtime issues. Look for any errors related to the Resend API or other app components.
    

### [Step 5: Test the Entire Workflow](#step-5-test-the-entire-workflow)[](#step-5-test-the-entire-workflow)

Now that your app is fully configured and ready, it’s time to test the entire workflow. We’ll ensure that the **email body is processed**, attachments are decoded and **uploaded to DigitalOcean Spaces**, receipt details and attachment URLs are **saved in Google Sheets**, and a **confirmation email is sent** to the sender.

Here’s how you can test step by step:

1.  **Send a Test Email**: Send an email to Postmark with a text body and an attachment. If you’re unsure how to configure Postmark, check [Day 8: Connecting Postmark to Your Flask App](/community/tutorials/setting-up-postmark-for-receipts), where we walked through setting up Postmark to forward emails to your app.
    
    ![Example of an email invoice receipt with highlighted payment details and attachments](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/email_invoice_receipt_screenshot.png)
    
2.  **Check Postmark Activity JSON**: In the Postmark dashboard, navigate to the **Activity** tab. Locate the email you sent and ensure that the JSON payload includes the text body and Base64-encoded attachment data. This confirms Postmark is correctly forwarding the email data to your app, as we set up in [Day 8](/community/tutorials/setting-up-postmark-for-receipts).
    
    ![Postmark JSON response showing encoded email attachments with metadata](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/postmark_json_encoded_attachments.png)
    
3.  **Monitor the Logs**: Check the runtime logs in your DigitalOcean App Platform dashboard to ensure the app processes the JSON payload. You should see logs showing that receipt details were extracted and attachments were uploaded to DigitalOcean Spaces. You can access the runtime logs in the **Logs** tab of the DigitalOcean App Platform dashboard. If you’re not familiar with DigitalOcean logs, we explored this during [Day 9: Automating Receipt Parsing withDigitalOcean’s GenAI Agent](/community/tutorials/automating-receipt-parsing-with-digitalocean-genai).
    
    ![DigitalOcean runtime logs interface displaying application log entries](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_runtime_logs_screenshot_2.png)
    
4.  **Verify Spaces Upload**: Visit your DigitalOcean Space to confirm that the files were uploaded successfully. You should see the attachments in your bucket as configured in [Day 10: Storing Attachments in DigitalOcean Spaces](/community/tutorials/configuring-digitalocean-spaces). If everything went as expected, your attachment URLs will be accessible.
    
    ![DigitalOcean Spaces interface showing uploaded files with names and metadata](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_files_interface.png)
    
5.  **Check Google Sheets**: Open your Google Sheet and confirm that a new row with receipt details and attachment URLs has been added, as we set up on [Day 11: Saving Receipt Details in Google Sheets](/community/tutorials/storing-receipt-details-in-google-sheets). The row should include:
    
    -   Vendor, amount, currency, and date extracted from the email body.
    -   Comma-separated URLs for the uploaded attachments in the last column.
    
    ![Google Sheets Receipts](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/google_sheets_receipts.png)
    
6.  **Verify the Confirmation Email**: Finally, check the inbox of the sender’s email address to ensure the confirmation email was received. This email should contain:
    
    -   The extracted receipt details (vendor, amount, currency, and date).
    -   Links to the uploaded attachments in DigitalOcean Spaces.
    -   A link to the Google Spreadsheet where the receipt data is logged.
    
    ![Receipt Confirmation Email](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/receipt_confirmation_email.png)
    

### [Troubleshooting](#troubleshooting)[](#troubleshooting)

If the workflow doesn’t work as expected, here are a few troubleshooting steps to follow:

1.  **Check the Resend Emails Dashboard for Errors**: Visit the Resend dashboard to see if any errors occurred while sending the confirmation email.
    
2.  **Verify Environment Variables**: Make sure the API key (`RESEND_API_KEY`) and sender email (`RESEND_EMAIL_FROM`) are correctly configured in your environment variables on the DigitalOcean App Platform dashboard.
    
3.  **Inspect DigitalOcean Runtime Logs**: Open the **Runtime Logs** tab in your DigitalOcean App Platform dashboard to check for errors while processing the email or uploading attachments. These logs can provide helpful insights, especially for interactions with Postmark or Resend.
    
4.  **Review Postmark Activity**: In Postmark’s **Activity** tab, confirm that the test email was properly forwarded to your Flask app. If there are any issues, Postmark will display errors related to forwarding or configuration problems.
    

[**🎁 Wrap-Up**](#wrap-up)[](#wrap-up)
--------------------------------------

Congratulations! You’ve successfully completed the **12 Days of DigitalOcean** series and built a fully functional **Email-Based Receipt Processing Service**.

Today, you:

-   Integrated the Resend API for sending transactional emails.
-   Configured environment variables to securely manage sensitive credentials.
-   Sent confirmation emails with receipt details, attachment links, and a spreadsheet URL.
-   Tested the full workflow from email submission to final confirmation.

By adding confirmation emails, you’ve wrapped up a project that processes emails, extracts details, stores attachments, and keeps everything organized in Google Sheets. It’s user-friendly, practical, and ready to solve real-world problems.

[**📚 The 12 Days of DigitalOcean**](#the-12-days-of-digitalocean)[](#the-12-days-of-digitalocean)
--------------------------------------------------------------------------------------------------

This marks the end of the **[12 Days of DigitalOcean](/community/tutorials/12-days-of-digitalocean-recap)** series. Over the past 12 days, we’ve built two real-world applications, one step at a time. Along the way, you’ve used tools like DigitalOcean’s **Serverless Functions**, **App Platform**, **Spaces Object Storage**, **PostgreSQL**, **DigitalOcean GenAI**, **Twilio**, **Google Sheets API**, **Postmark**, **PaperTrail**, and **Resend**. Each piece came together to form something greater than the sum of its parts.

Here’s a quick recap of what you’ve built:

### [🎂 Days 1–6: Build a Birthday Reminder Service](#days-1-6-build-a-birthday-reminder-service)[](#days-1-6-build-a-birthday-reminder-service)

This app tracks birthdays and sends SMS reminders automatically. It’s lightweight, serverless, and easy to maintain.

-   **[Day 1: Set Up a PostgreSQL Database](/community/tutorials/setting-up-postgresql-database-for-birthday-reminders)**  
    Create a database to store contact details.
-   **[Day 2: Connect to PostgreSQL with Python](/community/tutorials/connecting-to-postgresql-database-with-python)**  
    Securely connect to your database and fetch data.
-   **[Day 3: Check Birthdays and Send SMS Notifications](/community/tutorials/12-days-of-digitalocean-checking-birthdays-and-sending-sms-notifications)**  
    Use Twilio to notify users about upcoming birthdays.
-   **[Day 4: Deploy to DigitalOcean Functions](/community/tutorials/deploying-birthday-notifications-with-digitalocean-functions)**  
    Deploy your app to the cloud with DigitalOcean Functions.
-   **[Day 5: Automate Daily Reminders with Triggers](/community/tutorials/automating-birthday-reminders-with-triggers)**  
    Schedule reminders to run every day automatically.
-   **[Day 6: Set Up External Logging](/community/tutorials/setting-up-external-logging-for-function-based-birthday-reminder)**  
    Monitor and troubleshoot your app with Papertrail.

By Day 6, you have a fully automated service running in the cloud. It just works.

### [📧 Days 7–12: Build an Email Receipt Processor](#days-7-12-build-an-email-receipt-processor)[](#days-7-12-build-an-email-receipt-processor)

This app handles emailed receipts, extracts the needed details, and organizes everything in a database.

-   **[Day 7: Build and Deploy a Flask App](/community/tutorials/deploying-flask-app-on-digitalocean)**  
    Set up a lightweight app to process receipt emails.
-   **[Day 8: Integrate Postmark for Email Processing](/community/tutorials/setting-up-postmark-for-receipts)**  
    Forward emails to your app for processing.
-   **[Day 9: Extract and Clean Data with DigitalOcean’s GenAI](/community/tutorials/automating-receipt-parsing-with-digitalocean-genai)**  
    Use GenAI to extract structured data from email content.
-   **[Day 10: Configure DigitalOcean Spaces for Secure Storage](/community/tutorials/configuring-digitalocean-spaces)**  
    Store email attachments securely with object storage.
-   **[Day 11: Save Receipt Data to Google Sheets](/community/tutorials/storing-receipt-details-in-google-sheets)**  
    Organize structured data in a spreadsheet for easy access.
-   **[Day 12: Send Confirmation Emails](/community/tutorials/sending-confirmation-emails-with-resend)**  
    Notify users about successfully processed receipts.

By Day 12, you’ve built a complete tool that handles receipts end-to-end.

### [**What You’ve Learned**](#what-you-ve-learned)[](#what-you-ve-learned)

1.  **Storing and Managing Data**: You used [PostgreSQL](/community/tutorials/setting-up-postgresql-database-for-birthday-reminders) for structured data storage and [Google Sheets](/community/tutorials/storing-receipt-details-in-google-sheets) for easy, sharable data logging.
2.  **Automating Workflows**: With DigitalOcean [Functions](/community/tutorials/deploying-birthday-notifications-with-digitalocean-functions) and [scheduling triggers](/community/tutorials/automating-birthday-reminders-with-triggers), you automated processes and made your apps run like clockwork.
3.  **Adding Intelligence to Your Apps**: By integrating [DigitalOcean’s GenAI](/products/gen-ai), you brought intelligent data extraction and organization into your workflows, making your apps smarter and more capable.
4.  **Handling Files Securely**: You worked with DigitalOcean [Spaces](/community/tutorials/configuring-digitalocean-spaces) to store and manage files in a reliable, scalable way.
5.  **Enhancing Apps with APIs**: APIs like [Twilio](/community/tutorials/12-days-of-digitalocean-checking-birthdays-and-sending-sms-notifications), [Postmark](/community/tutorials/setting-up-postmark-for-receipts), and Resend brought functionality like SMS notifications, email forwarding, and confirmation emails to your apps.
6.  **Debugging and Monitoring**: Using tools like Papertrail, you learned to [debug and monitor](/community/tutorials/setting-up-external-logging-for-function-based-birthday-reminder) your apps effectively, keeping them running smoothly.

[What’s next](#what-s-next)[](#what-s-next)
-------------------------------------------

This is just the beginning—what you’ve learned here can be applied to countless other projects. Here are a few ways to keep going:

-   Join the conversation on [DigitalOcean’s Discord](https://discord.gg/digitalocean) to connect with other developers, share what you’ve built, and get inspired.
-   Explore more in our [tutorial library](/community/tutorials) for more ideas and projects.

If you follow along, I’d love to see what you create—feel free to share your progress or feedback with me on [Twitter](https://twitter.com/amit).

Keep it simple. Build something useful. Happy building! 🚀

#### [Source](https://www.digitalocean.com/community/tutorials/sending-confirmation-emails-with-resend)

<br/>
---
