---
title: "Building a Real-Time Weather Data Collection System with Python and AWS"
date: 2025-01-12
---

In the era of data-driven decision-making, weather data has become an invaluable resource for businesses and individuals. Whether it’s for logistics, agriculture, or travel planning, a real-time weather data collection system can provide actionable insights. In this blog, we’ll walk through building a Weather Data Collection System using Python, the OpenWeather API, and AWS S3 for storage.

## Project Overview

This project demonstrates how to:

- Fetch weather data using the OpenWeather API.
- Display the weather data when running a Python script.
- Store the data in an AWS S3 bucket for historical tracking and analysis.

By the end of this guide, you’ll have a fully functional system showcasing key aspects of DevOps principles, including automation, cloud integration, and scalability.

## AWS Services Overview

**Amazon S3 (Simple Storage Service)**

**Function:** Amazon S3 is a highly scalable and secure object storage service. In this project, it stores historical weather data for analysis.

**Key Features:**

- **Scalability:** Handles growing datasets effortlessly.
- **Durability:** Ensures data is not lost with multiple redundancies.
- **Integration:** Works seamlessly with other AWS services like Lambda, Glue, and Athena.

In our system, the S3 bucket serves as the repository for all weather data fetched from the OpenWeather API.

## Step-by-Step Implementation

**Step 1: Prerequisites**

Before we start coding, ensure you have the following:

**1\. AWS Account:** Create an AWS S3 bucket where the weather data will be stored.

**2\. OpenWeather API Key:** Sign up at OpenWeather and obtain your API key.

**3\. Python Installed:** Ensure Python 3.x is installed on your system. For this project, we’ll use VSCode as our IDE.

**Install Dependencies:** Create a `requirements.txt` file with the following content:  

```
boto3==1.26.137
python-dotenv==1.0.0
requests==2.28.2

```

Run the following command to install the dependencies:

`pip install -r requirements.txt`

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F3ncm7pg04k7gm9abad22.png)

**Step 2: Set Up Your Environment**

Create a Project Directory:  

```
mkdir weather-data-collector
cd weather-data-collector
```

**Create a .env File:**

Store sensitive information like API keys securely in a .env file:  

```
OPENWEATHER_API_KEY=your_openweather_api_key
AWS_ACCESS_KEY_ID=your_aws_access_key_id
AWS_SECRET_ACCESS_KEY=your_aws_secret_access_key
S3_BUCKET_NAME=your_s3_bucket_name
```

**Step 3: Fetch Weather Data**

Create a Python script to fetch weather data using the OpenWeather API. Store Data in AWS S3 by using the boto3 library to upload weather data to an S3 bucket.

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fi3ip5bwc1hl0ticm54kz.png)

**fetch\_weather.py:**  

```
import os
import json
import boto3
import requests
from datetime import datetime
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

class WeatherDashboard:
    def __init__(self):
        self.api_key = os.getenv('OPENWEATHER_API_KEY')
        self.bucket_name = os.getenv('AWS_BUCKET_NAME')
        self.s3_client = boto3.client('s3')

    def create_bucket_if_not_exists(self):
        """Create S3 bucket if it doesn't exist"""
        try:
            self.s3_client.head_bucket(Bucket=self.bucket_name)
            print(f"Bucket {self.bucket_name} exists")
        except:
            print(f"Creating bucket {self.bucket_name}")
        try:
            # Simpler creation for us-east-1
            self.s3_client.create_bucket(Bucket=self.bucket_name)
            print(f"Successfully created bucket {self.bucket_name}")
        except Exception as e:
            print(f"Error creating bucket: {e}")

    def fetch_weather(self, city):
        """Fetch weather data from OpenWeather API"""
        base_url = "http://api.openweathermap.org/data/2.5/weather"
        params = {
            "q": city,
            "appid": self.api_key,
            "units": "imperial"
        }

        try:
            response = requests.get(base_url, params=params)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f"Error fetching weather data: {e}")
            return None

    def save_to_s3(self, weather_data, city):
        """Save weather data to S3 bucket"""
        if not weather_data:
            return False

        timestamp = datetime.now().strftime('%Y%m%d-%H%M%S')
        file_name = f"weather-data/{city}-{timestamp}.json"

        try:
            weather_data['timestamp'] = timestamp
            self.s3_client.put_object(
                Bucket=self.bucket_name,
                Key=file_name,
                Body=json.dumps(weather_data),
                ContentType='application/json'
            )
            print(f"Successfully saved data for {city} to S3")
            return True
        except Exception as e:
            print(f"Error saving to S3: {e}")
            return False

def main():
    dashboard = WeatherDashboard()

    # Create bucket if needed
    dashboard.create_bucket_if_not_exists()

    cities = ["Philadelphia", "Seattle", "New York"]

    for city in cities:
        print(f"nFetching weather for {city}...")
        weather_data = dashboard.fetch_weather(city)
        if weather_data:
            temp = weather_data['main']['temp']
            feels_like = weather_data['main']['feels_like']
            humidity = weather_data['main']['humidity']
            description = weather_data['weather'][0]['description']

            print(f"Temperature: {temp}°F")
            print(f"Feels like: {feels_like}°F")
            print(f"Humidity: {humidity}%")
            print(f"Conditions: {description}")

            # Save to S3
            success = dashboard.save_to_s3(weather_data, city)
            if success:
                print(f"Weather data for {city} saved to S3!")
        else:
            print(f"Failed to fetch weather data for {city}")

if __name__ == "__main__":
    main()

```

**Step 4: Run the System**

Fetch and display weather data:

`python fetch_weather.py`

Fetch, display, and upload to S3

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F4nmvok3hrle0i30y0mut.png)

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fsely0p41wuwly0fs3kda.png)

## Key Features of the System

- **Real-Time Data:** Fetches live weather data from OpenWeather API.
- **Scalable Storage:** AWS S3 ensures scalability and durability for your data.
- **Automation:** Automates data collection and storage with minimal user intervention.

## Best Practices

- **Secure API Keys:** Use environment variables to protect sensitive data.
- **Error Handling:** Handle API request errors and S3 upload issues gracefully.
- **Modular Design:** Keep your scripts modular for reusability and scalability.

## Future Enhancements

- **Scheduled Data Collection:** Use cron jobs or Python libraries like APScheduler to automate periodic data fetching.
- **Data Visualization:** Build dashboards using tools like Grafana or Tableau.
- **Advanced Analytics:** Analyze historical weather data for trends and insights.

## Conclusion

With just a few lines of Python and the power of cloud services like AWS, we’ve built a scalable and functional Weather Data Collection System. This project is a great starting point for exploring more advanced DevOps practices and cloud integrations.

Happy Coding! ☁️☂️

Go to Source
