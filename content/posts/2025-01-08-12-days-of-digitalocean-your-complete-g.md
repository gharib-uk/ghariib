---
title: "12 Days of DigitalOcean Your Complete Guide"
date: 2025-01-08T00:00:00.000Z
draft: false
type: posts
categories: 
- flask,serverless,spaces,object-storage,postgresql,logging,python
---
# 12 Days of DigitalOcean Your Complete Guide

<br/>

<br/>
Last month, I joined [DigitalOcean](/) and decided to dive in by building something fun and practical. That’s how the **12 Days of DigitalOcean** series came to life—a step-by-step journey to create **two real-world apps** while exploring DigitalOcean’s ecosystem.

Here’s what we built:

1.  **Birthday Reminder Service**: A serverless app that sends SMS reminders for upcoming birthdays.
2.  **Email Receipt Processor**: A tool that processes emailed receipts and organizes their details in a database.

These apps aren’t just examples—**they’re tools I now use daily**, and they’re a perfect starting point for anyone looking to build something useful. Along the way, you’ll learn how to:

-   Work with [managed databases](/products/managed-databases) like PostgreSQL.
-   Deploy [serverless functions](/products/functions) for lightweight, scalable apps.
-   Securely [store files](/products/spaces) with DigitalOcean Spaces.
-   Monitor [runtime logs](https://docs.digitalocean.com/products/app-platform/how-to/view-logs/) using tools like Papertrail.
-   Integrate APIs like [Twilio SMS](https://www.twilio.com/en-us/messaging/channels/sms), [Postmark](https://postmarkapp.com/inbound-email), and [Resend](https://resend.com).
-   Use [DigitalOcean’s GenAI](/products/gen-ai) to enhance your apps with intelligent data extraction and organization features.

This series is a great place to start if you’ve been looking for a way to get hands-on with [DigitalOcean](https://cloud.digitalocean.com/).

[🎂 Days 1–6: Build a Birthday Reminder Service](#days-1-6-build-a-birthday-reminder-service)[](#days-1-6-build-a-birthday-reminder-service)
--------------------------------------------------------------------------------------------------------------------------------------------

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

[📧 Days 7–12: Build an Email Receipt Processor](#days-7-12-build-an-email-receipt-processor)[](#days-7-12-build-an-email-receipt-processor)
--------------------------------------------------------------------------------------------------------------------------------------------

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
-   **[Day 12: Send Confirmation Emails](/community/tutorials/sending-confirmation-emails-with-resend)** Notify users about successfully processed receipts.

By Day 12, you’ve built a complete tool that handles receipts end-to-end.

[Start Building Today](#start-building-today)[](#start-building-today)
----------------------------------------------------------------------

This series is about more than just tutorials—it’s about creating something real while building your skills. You’ll have two practical apps and hands-on experience with key tools and technologies by the end. Whether new to [DigitalOcean](/) or looking to grow your skills, this is a great way to start.

Start with [Day 1: Set Up PostgreSQL](/community/tutorials/setting-up-postgresql-database-for-birthday-reminders), or pick any day that interests you. The best way to learn is by building—and these apps are the perfect place to begin.

Happy building! And if you follow along, [I’d love to hear](https://twitter.com/amit) what you create—share your progress or feedback!

#### [Source](https://www.digitalocean.com/community/tutorials/12-days-of-digitalocean-recap)

<br/>
---
