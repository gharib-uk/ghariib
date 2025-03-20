---
title: "12 Days of DigitalOcean Day 8 - Connecting Postmark to Your Flask App"
date: 2024-12-30T14:00:00.000Z
draft: false
type: posts
categories: 
- flask,digitalocean,python
---
# 12 Days of DigitalOcean Day 8 - Connecting Postmark to Your Flask App

<br/>

<br/>
Welcome to **Day 8** of **12 Days of DigitalOcean**! Yesterday, you [deployed a Flask app on DigitalOcean](/community/tutorials/deploying-flask-app-on-digitalocean) to start building an **Email-Based Receipt Processor**. This app will let you send receipts to an email address and have them automatically processed by your app.

Today, you’ll set up [Postmark](https://postmarkapp.com) to handle incoming emails. Postmark receives emails, turns them into easy-to-handle JSON data, and sends it to your app. This means you don’t have to worry about managing email servers or decoding raw email formats—Postmark takes care of all of that for you.

By the end of this tutorial, you’ll have a setup where emails sent to a dedicated address are automatically forwarded to your Flask app, ready to be logged, stored, or analyzed. Let’s get started!

[✨ How It Works](#how-it-works)[](#how-it-works)
------------------------------------------------

Here’s how everything fits together:

1.  A user sends an email (like a receipt) to a Postmark-provided address.
2.  Postmark receives the email, processes its content into JSON.
3.  Postmark sends the structured data to your Flask app hosted on DigitalOcean using a webhook URL.
4.  Your app processes the data, extracting the information it needs, like the sender, subject, and body.

With this setup, Postmark handles the heavy lifting of [email parsing](https://postmarkapp.com/developer/user-guide/inbound/parse-an-email) so your app can focus on using the data—whether it’s storing it in a database, cleaning it up, or preparing it for analysis.

![Email processing flow diagram](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/email_processing_flow.jpeg)

[🧑‍🍳 Recipe for Day 8: Connecting Postmark to Your Flask App](#recipe-for-day-8-connecting-postmark-to-your-flask-app)[](#recipe-for-day-8-connecting-postmark-to-your-flask-app)
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

You’ll start by updating the Flask app to handle incoming emails. Then, you’ll configure Postmark to send email data to your app and test the setup to ensure it’s all working.

### [Step 1 - Update Your Flask App](#step-1-update-your-flask-app)[](#step-1-update-your-flask-app)

Your app needs a route where Postmark can send the email data. Let’s set that up.

1.  Open your `app.py` file and add this code:
    
    ```
    from flask import Flask, request, jsonify
    
    app = Flask(__name__)
    
    @app.route('/inbound', methods=['POST'])
    def inbound():
        # Parse the JSON data sent by Postmark
        email_data = request.get_json()
    
        # Extract useful information
        subject = email_data.get('Subject', 'No subject')
        from_email = email_data.get('FromFull', {}).get('Email', 'Unknown sender')
        body = email_data.get('TextBody', '')
    
        # Log the details (or process them)
        print(f"Received email from {from_email} with subject: {subject}")
        print(f"Body: {body}")
    
        return jsonify({"status": "success"}), 200
    ```
    
    This code sets up a new `/inbound` route that does three things:
    
    -   Listens for POST requests from Postmark.
    -   Extracts key details like the subject, sender email, and body from the JSON payload.
    -   Logs these details to your app’s console for now. We’ll extend this to analyze the data and store it in a database as a next step.
2.  Save the changes to `app.py`.
    
3.  Commit and push the changes to GitHub to redeploy the app on DigitalOcean:
    
    ```
    git add app.py
    git commit -m "Add inbound route for Postmark emails"
    git push origin main
    ```
    
    ![Terminal showing git commit with Postmark emails](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/terminal_git_commit_postmark_emails.png)
    
4.  Now your app is ready to receive email data from Postmark. Head over to your **DigitalOcean App Platform** dashboard and check the status of your app. Once it’s marked as **live**, grab the public URL for your app. Postmark will send email data to this URL.
    

![DigitalOcean app dashboard showing live app](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_app_dashboard.png)

**Quick Tip**: If you’d like to test locally instead, you can use [Ngrok](https://ngrok.com/) to expose your Flask app to the internet temporarily. Run:

```
ngrok http 5000
```

Ngrok will give you a URL like `https://abcd1234.ngrok.io`. You can use this as your webhook URL when setting up Postmark.

### [Step 2 - Set Up Postmark](#step-2-set-up-postmark)[](#step-2-set-up-postmark)

Postmark will handle the job of parsing emails and forwarding the structured data to your app.

1.  **Sign Up or Log In to Postmark:**
    
    -   If you already have an account, log in to [Postmark](https://postmarkapp.com/), or [sign up for free](https://postmarkapp.com/) (the free plan is perfect for testing and experimenting).
2.  **Create a Server:**
    
    -   Once logged in, go to the **Servers** tab and click **Create Server**.
        
        ![Postmark server dashboard](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/postmark_servers_dashboard.png)
        
    -   Name the server something like `Receipt Processor`
        
        ![Postmark server setup screen](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/postmark_server_setup.png)
        
3.  **Use the Default Inbound Stream:**
    
    -   Every new server includes a few built-in message streams. One of them is the Default Inbound Stream, which is what we’ll use for this setup.
    -   Click on **Default Inbound Stream** to view its details.
    
    ![Postmark receipt processor settings](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/postmark_receipt_processor_settings.png)
    
    **Quick Note**: Postmark organizes emails using **streams**, which are designed to handle different types of email traffic. The **Default Inbound Stream** is where incoming emails are processed.
    
    -   Postmark will provide you with an email address, like `123456abcdefgh@inbound.postmarkapp.com`.
    
    ![Inbound email setup details](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/postmark_inbound_email_setup.png)
    
    **Quick Note**: Save this email address! You’ll use it to send test emails later and verify your setup.
    

### [Step 3 - Connect Postmark to Your App](#step-3-connect-postmark-to-your-app)[](#step-3-connect-postmark-to-your-app)

Next, you’ll set up the webhook URL, so Postmark knows where to send email data.

1.  **Set Your Webhook URL in Postmark:**
    
    -   On the **Default Inbound Stream** settings screen, find the field labeled **Set your server’s inbound webhook URL**.
        
    -   Paste your **DigitalOcean app URL**, adding `/inbound` at the end. Postmark will use this URL to send the email data. For example:
        
        ```
        hammerhead-xyz.ondigitalocean.app/inbound
        ```
        
    
    ![Set inbound webhook URL in Posthog](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/set_inbound_webhook_url_posthog.png)
    
    -   If you’re testing locally with `Ngrok`, paste your Ngrok public URL here and add `/inbound` at the end.
    
    **Quick Tip**: If you update your webhook URL later (for example, when switching from Ngrok to a live app URL), you can return to this field in Postmark’s **Default Inbound Stream** settings and update the URL.
    

### [Step 4: Test the Setup](#step-4-test-the-setup)[](#step-4-test-the-setup)

Let’s make sure everything is working as expected.

1.  **Send a Test Email:**
    
    -   Use the Postmark-provided email address (e.g., `123456abcdefgh@inbound.postmarkapp.com`).
        
    -   Send an email with a subject and body for testing.
        
        -   For example:
            
            ```
            Subject: Receipt for Order #12345
            Body: Thank you for your purchase!
            Your order #12345 has been processed.  
            
            Total: $50.00  
            Date: December 29, 2024  
            ```
            
        
        ![Test receipt email](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/receipt_for_order_12345.png) Once the email is sent to your Postmark-provided email address, Postmark will forward the email data to your Flask app.
        
2.  **Check Postmark Activity:**
    
    -   Navigate to the **Activity** tab in your Postmark dashboard to confirm the email was received and forwarded correctly.
        
        ![Postmark activity log](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/receipt_order_12345_email.png)
        
3.  **Verify Your Runtime Logs:**
    
    -   Head to your **DigitalOcean App Platform dashboard**, select your app, and click on the **Logs** tab to access the **Runtime Logs**.
        
    -   You should see something like this:
        
        ```
        Received email from sender@example.com with subject: Receipt for Order #12345
        Body: 
        Thank you for your purchase!  
        Your order #12345 has been processed.  
        
        Total: $50.00  
        Date: December 29, 2024  
        ```
        
        ![Runtime logs displaying email receipt processing](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/email-receipt-processor/digitalocean_runtime_logs_screenshot.png)
        
        digitalocean\_runtime\_logs\_screenshot
        

[Troubleshooting](#troubleshooting)[](#troubleshooting)
-------------------------------------------------------

If something isn’t working, here are a few things to check:

1.  Make sure the `/inbound` route is set up in your Flask app and it’s running (either on DigitalOcean or locally with Ngrok)
2.  Verify that the webhook URL in Postmark includes `/inbound` and matches your app’s public URL.
3.  Double-check the email address you’re sending to—it should match the one from Postmark’s default inbound stream.
4.  Check Postmark’s **Activity Logs** to confirm emails are being received.
5.  Look at the **Runtime Logs** in DigitalOcean to see if the Flask app is logging incoming data.
6.  Ensure Flask’s print statements in the `/inbound` route are active and haven’t been commented out or removed. These logs are essential for debugging during testing.

**Tip**: If you’re testing locally with Ngrok, make sure your Ngrok URL is active and points to the correct port.

[🎁 Wrap Up](#wrap-up)[](#wrap-up)
----------------------------------

Here’s what you accomplished today:

1.  You added an inbound route in your Flask app to handle emails from Postmark.
2.  Configured Postmark’s default inbound stream, grabbed the provided email address, and connected it to your app using your DigitalOcean URL.
3.  Sent a test email and checked both the [Runtime Logs](https://docs.digitalocean.com/products/app-platform/how-to/view-logs/#:~:text=Runtime%20log%3A%20Records%20messages%20and,potential%20reason%20for%20the%20crash.) on DigitalOcean and the **Activity Logs** on Postmark to confirm everything is working as expected.

Here is the previous tutorial from this series on [Day 7: Building and Deploying the Email-Based Receipt Processor](/community/tutorials/deploying-flask-app-on-digitalocean).

Your **Email-Based Receipt Processor** is now ready to receive and process emails automatically! In the next tutorial, you’ll take this data and make it even more useful by integrating AI tools to extract and organize receipt details. See you for **Day 9**! 🚀

#### [Source](https://www.digitalocean.com/community/tutorials/setting-up-postmark-for-receipts)

<br/>
---
