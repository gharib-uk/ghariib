---
title: "How to Correctly Install ChromeDriver on Ubuntu 🐧"
date: 2025-03-19
---

**ChromeDriver** is a small software that helps other programs (like Selenium) to control Google Chrome browser automatically. If you want to automate a web browser using Selenium on Ubuntu or any other system, you need **ChromeDriver**. This small software helps Selenium control **Google Chrome** for testing websites or scraping data.

In this tutorial, we will help you to install correct ChromeDriver version that work properly with installed Google Chrome version on your Ubuntu and other Debian based systems in a very simple way.

## Step 1: Update Your System

1. Open the **Terminal** (press `Ctrl + Alt + T`).
2. Execute following command to update your system packages:
    
    ```
    sudo apt update && sudo apt upgrade -y
    ```
    

## Step 2: Install Google Chrome

Before installing ChromeDriver make sure you have already installed Google chrome browser on your system. You can also install latest **Google Chrome** using:

```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

Check if Chrome is installed:

```
google-chrome --version
```

## Step 3: Install Correct ChromeDriver Version

ChromeDriver can be downloaded form official download page. It is important that you install the same version of Chromedriver as Google chrome. So follow the below instructions to download and configure correct Chromedriver:

1. Check your Chrome version:
    
    ```
    google-chrome --version
    ```
    
2. Download the matching ChromeDriver version. Replace `133.0.6943.98` in below command with the version shows by above command:
    
    ```
    wget https://storage.googleapis.com/chrome-for-testing-public/133.0.6943.98/linux64/chromedriver-linux64.zip
    ```
    
3. Extract and configure ChromeDriver to make it globally accessible:
    
    ```
    unzip chromedriver-linux64.zip
    ```
    

## Step 4: Verify ChromeDriver Installation

Run this command to check if ChromeDriver is installed:

```
chromedriver --version
```

If the output is like this:

```text

ChromeDriver 133.0.6943.98 (da53563ceb66412e2637507c8724bd0cab05e453-refs/branch-heads/6943_85@{#7})
```

Then the installation is successful!

<figure>

![Correctly Install ChromeDriver on Ubuntu](https://tecadmin.net/wp-content/uploads/2025/02/installing-chromedriver-with-google-chrome-ubuntu.png)

<figcaption>

Correctly Installed ChromeDriver on Ubuntu

</figcaption>

</figure>

## Step 5: Test ChromeDriver with Selenium

Install **Python and Selenium**:

```
pip install selenium
```

Create a test script:

```python"

from selenium importA webdriver

driver = webdriver.Chrome()
driver.get("https://www.google.com")
print("Chrome opened successfully!")
driver.quit()
```

Run the script:

```
python test_selenium.py
```

## Conclusion

Now you have installed correct ChromeDriver version on Ubuntu without any errors. You can use it for Selenium automation, testing websites, or web scraping etc. If you face any issues, check your Chrome version and download the correct ChromeDriver version again.

The post How to Correctly Install ChromeDriver on Ubuntu 🐧 appeared first on TecAdmin.

Go to Source
