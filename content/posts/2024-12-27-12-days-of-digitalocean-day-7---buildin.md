---
title: "12 Days of DigitalOcean Day 7 - Building and Deploying the Email-Based Receipt Processor"
date: 2024-12-27T14:00:00.000Z
draft: false
type: posts
categories: 
- flask,digitalocean,python
---
# 12 Days of DigitalOcean Day 7 - Building and Deploying the Email-Based Receipt Processor

<br/>

<br/>
Welcome to **Day 7 of the 12 Days of DigitalOcean**! Today, we’re kicking off the second project in this series: an **Email-Based Receipt Processing Service**. This app is designed to make your life a little easier by organizing your emailed receipts into a database. No more endless scrolling through your inbox to find that one receipt you need.

If you enjoyed the [Birthday Reminder Service](/community/tutorials/setting-up-postgresql-database-for-birthday-reminders), you’ll love this next step. It’s another practical, production-ready app that highlights how DigitalOcean can help you turn ideas into real, working solutions. By the end of this tutorial, your Flask app will be live on the web and ready to process receipt data like a pro.

[✨ Why This App?](#why-this-app)[](#why-this-app)
-------------------------------------------------

We’ve all been there—scrambling to find a specific receipt for tax season or reimbursements. It’s not fun. This app aims to solve that by automating the whole process.

[**How the App Works**](#how-the-app-works)[](#how-the-app-works)
-----------------------------------------------------------------

1.  **You forward a receipt to a specific email address.**
2.  **The app extracts details** like the vendor, amount, and date using smart tools.
3.  **Those details are stored in a database**, like [PostgreSQL](/community/tutorials/setting-up-postgresql-database-for-birthday-reminders), where they’re neatly organized.
4.  **You get a confirmation**—via email or SMS—once the receipt is processed.

It’s simple, efficient, and takes a tedious task off your plate. **Today**, we’re focusing on building the backend system with Flask, setting it up for production with [Gunicorn](https://gunicorn.org), and deploying it to [DigitalOcean’s App Platform](https://docs.digitalocean.com/products/app-platform/).

[🚀 What You’ll Learn](#what-you-ll-learn)[](#what-you-ll-learn)
----------------------------------------------------------------

Here’s what we’ll cover today:

1.  **Set up a Flask app** to serve as the foundation of your receipt processor.
2.  **Configure Gunicorn** to handle production workloads smoothly.
3.  **Use GitHub** for version control and collaboration.
4.  **Deploy the app to [DigitalOcean’s App Platform](https://cloud.digitalocean.com/apps/new)** so it’s live and ready to use.

This tutorial will give you hands-on experience with deploying a real-world Flask app while keeping things manageable and fun.

[🛠 What You’ll Need](#what-you-ll-need)[](#what-you-ll-need)
-------------------------------------------------------------

Before diving in, make sure you have:

-   A **DigitalOcean account**. If you don’t have one, you can [sign up here](https://cloud.digitalocean.com/registrations/new).
-   **Python 3.8+** installed on your local machine.
-   **Git** installed and ready to use.

[🧑‍🍳 Recipe for Day 7: Building and Deploying a Flask App](#recipe-for-day-7-building-and-deploying-a-flask-app)[](#recipe-for-day-7-building-and-deploying-a-flask-app)
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### [Step 1: Set Up Your Flask App](#step-1-set-up-your-flask-app)[](#step-1-set-up-your-flask-app)

Flask will serve as the backbone of this project, handling all incoming requests and processing the receipt data. Let’s get started by setting up the basics.

1.  **Create Your Project Directory**:  
    Keeping everything in a dedicated folder makes managing your project easier as it grows. Run the following commands to set up the directory:
    
    ```
    mkdir email-receipt-processor
    cd email-receipt-processor
    ```
    
2.  **Set Up a Virtual Environment**:  
    A virtual environment is like a sandbox for your project. It keeps your dependencies isolated, so you don’t accidentally break other Python projects or your system’s Python setup.
    
    ```
    pip install virtualenv
    virtualenv -p python3 venv
    ```
    
3.  **Activate the Virtual Environment**: Activate your virtual environment to install dependencies locally for the project:
    
    ```
    source venv/bin/activate
    ```
    
4.  **Install Flask and Gunicorn**:  
    Flask will be your web framework, and Gunicorn will handle production server duties.
    
    ```
    pip install Flask gunicorn
    ```
    
5.  **Save Your Dependencies**:  
    A `requirements.txt` file ensures everyone (and every machine) running your project has the same setup.
    
    ```
    pip freeze > requirements.txt
    ```
    
6.  **Write Your Flask App**:  
    Let’s start with a simple Flask app. Create a file called `app.py` and add the following code:
    
    ```
    from flask import Flask
    
    app = Flask(__name__)
    
    @app.route('/')
    def home():
        return "Welcome to the Email-Based Receipt Processor!"
    
    if __name__ == "__main__":
        app.run()
    ```
    
7.  **Test It Locally**:  
    Fire up the app and check it in your browser:
    
    ```
    python app.py
    ```
    
    ![Email Receipt Processor Code Screenshot](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/email_receipt_processor_code_screenshot.png)
    
    Open your browser and navigate to `http://localhost:5000`. You should see: **“Welcome to the Email-Based Receipt Processor!”**
    
    ![Welcome Screen](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/email_based_receipt_processor_welcome_screen.png)
    

### [Step 2: Add Gunicorn Configuration](#step-2-add-gunicorn-configuration)[](#step-2-add-gunicorn-configuration)

Flask’s built-in server is great for development, but it’s not designed to handle the traffic and performance needs of a production app. That’s where Gunicorn comes in.

1.  **Create a Gunicorn Config File**:  
    Gunicorn needs a configuration file to know how to run your app. Create one with:
    
    ```
    touch gunicorn_config.py
    ```
    
2.  **Configure Gunicorn**:  
    Open `gunicorn_config.py` and add:
    
    ```
    bind = "0.0.0.0:8080"
    workers = 2
    ```
    
    -   `bind`: Makes the app accessible over the web on port 8080
    -   `workers`: Sets the number of simultaneous requests the app can handle. Two is a good starting point for small apps.

### [Step 3: Push to GitHub](#step-3-push-to-github)[](#step-3-push-to-github)

GitHub is where the magic happens—it’s how we’ll version control the app and integrate it with DigitalOcean.

1.  **Initialize a Git Repository**:  
    A `.gitignore` file prevents unnecessary files (like your virtual environment) from being tracked by Git. Run:
    
    ```
    git init
    echo "venv/" > .gitignore
    echo "*.pyc" >> .gitignore
    git add .
    git commit -m "Initial Flask app with Gunicorn"
    ```
    
    ![Terminal Flask App Setup](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/terminal_flask_app_setup.png)
    
    Here’s what your project should look like in VS Code, for example:
    
    ![VS Code Workspace Setup](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/visual_studio_code_workspace_setup.png)
    
2.  **Create a GitHub Repository**:  
    Head to GitHub, create a new repo called `email-receipt-processor`, and follow the steps to set it up.
    
    ![GitHub Repository Setup Instructions](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/github_repository_setup_instructions.png)
    
3.  **Push Your Code to GitHub**:
    
    Back in your terminal, type:
    
    ```
    git branch -M main
    git remote add origin https://github.com/<your-github-username>/email-receipt-processor.git
    git push -u origin main
    ```
    

Replace `<your-github-username>` with your **GitHub username**

![Terminal Git Commands for Email Receipt Processor](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/terminal_git_commands_email_receipt_processor.png)

### [Step 4: Deploy to DigitalOcean’s App Platform](#step-4-deploy-to-digitalocean-s-app-platform)[](#step-4-deploy-to-digitalocean-s-app-platform)

Now it’s time to deploy your app! DigitalOcean makes this easy with its App Platform.

#### **Create a New App on DigitalOcean**

1.  Log in to your **DigitalOcean dashboard** and go to [Apps](https://cloud.digitalocean.com/apps).
    
2.  Click **Create App**, select **GitHub**, and connect your `email-receipt-processor` repo.
    
    ![DigitalOcean GitHub Connection](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/digitalocean_create_app_github_connection.png)
    
3.  **Click “Connect GitHub Account”**: You will be redirected to GitHub to authorize DigitalOcean. If prompted, log into GitHub and grant DigitalOcean access to your repositories.
    
4.  **Select the Repository**: Choose the `email-receipt-processor` repository you created earlier from the list of available repositories. <$>\[note\] **Tip:** Ensure that you have selected the correct branch (e.g., `main`) for deployment. <$>
    
    ![GitHub Repository Permissions](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/github_repository_permissions.png)
    
5.  **Click Save and Continue**: This redirects you back to the DigitalOcean App Platform configuration screen.
    

#### **Configure Deployment Settings**

1.  **Select the Repository from the Dropdown**: Once back in the App Platform, you’ll see a dropdown listing your connected repositories. Select `email-receipt-processor`.
    
    ![Create App Resources from Source](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/create_app_resources_from_source.png)
    
2.  **Choose Resources**: You can adjust the resource allocations (RAM, CPU, and bandwidth) for your app.
    
    -   Click **Edit** to modify these settings. For this tutorial, we recommend the following configuration:
        -   **RAM**: 512 MB
        -   **CPU**: 1vCPU
        -   **Bandwidth**: 50 GB
    
    ![Create App Resources Page](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/create_app_resources_page.png)
    
    ![Create App Email Receipt Processor Settings](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/create_app_email_receipt_processor_settings.png)
    

#### Set the Run Command

1.  **Scroll Down to the Run Command Section**: This step ensures DigitalOcean knows how to start your app in the production environment.
    
2.  **Click “Edit”**: Replace the default command with the following:
    
    ```
    gunicorn --worker-tmp-dir /dev/shm -c gunicorn_config.py app:app
    ```
    
    -   This command uses Gunicorn to serve your Flask app.
        
    -   The `--worker-tmp-dir /dev/shm` flag optimizes performance in containerized environments.
        
    
    ![DigitalOcean App Deployment Settings](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/digitalocean_app_deployment_settings.png)
    
3.  **Review Configuration**: On the final screen, review your app’s settings. Ensure the resource allocations and run command are correct. Then click **Back** to get back to Resources.
    
    ![Email Receipt Processor Settings](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/email_receipt_processor_settings.png)
    

#### Finalize Deployment Settings

1.  **Environment Variables**: On the next screen, you’ll see an option to set environment variables. For now, leave everything as default and click **Next**.
    
    ![Create App Environment Variables](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/create_app_environment_variables.png)
    
2.  **App Region**: Choose a deployment region close to your target audience or leave it as the default.
    
    ![Create App Info Screen](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/create_app_info_screen.png)
    
3.  **Create Resources**: On the final screen, review your app’s settings. Then click on **Create Resources**. This starts the deployment process. Sit back while DigitalOcean sets everything up!
    
    ![Create App Review Screen](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/create_app_review_screen.png)
    

#### Verify the Deployment

1.  **Deployment Status**: Once your app is deployed, you’ll see a confirmation screen with a link to your app’s public URL.
    
    ![Hammerhead App Deployment Status](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/hammerhead_app_deployment_status.png)
    
2.  **Test Your App**: Visit the URL in your browser. You should see the message: **“Welcome to the Email-Based Receipt Processor!”**
    
    ![Email-Based Receipt Processor Screenshot](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/12-Days-of-DO/Postgressql-birthday/email_based_receipt_processor_screenshot.png)
    

[🎁 Wrap-Up](#wrap-up)[](#wrap-up)
----------------------------------

You did it! Here’s what you accomplished today:

1.  Built a Flask app to kickstart your receipt processor.
2.  Configured Gunicorn to make it production-ready.
3.  Deployed your app to [DigitalOcean’s App Platform](https://cloud.digitalocean.com/apps/new), making it live and ready to use.

Your app is now up and running—an important milestone in this series. In the next tutorial, you’ll take things further by integrating [Postmark](https://postmarkapp.com) to handle email forwarding. Get ready to level up your app—it will be awesome! 🚀

Here is the next tutorial from this series on [Day 8: Connecting Postmark to Your Flask App](/community/tutorials/setting-up-postmark-for-receipts).

#### [Source](https://www.digitalocean.com/community/tutorials/deploying-flask-app)

<br/>
---
