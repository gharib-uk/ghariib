---
title: "Image Processing Using Llama 32 with Hugging Face Transformers"
date: 2025-03-19T00:00:00.000Z
draft: false
type: posts
categories: 
- gen-ai,ai-ml,solution-engg
---
# Image Processing Using Llama 32 with Hugging Face Transformers

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

Extracting insights from images has long been a challenge across industries like finance, healthcare, and law. Traditional methods, such as [Optical Character Recognition (OCR)](https://en.wikipedia.org/wiki/Optical_character_recognition), have struggled with complex layouts and contextual understanding.

[Llama 3.2 Vision](https://marketplace.digitalocean.com/apps/meta-llama-llama-3-2-90b-vision-instruct-8x), an advanced AI model, enhances image processing capabilities like Visual Question Answering and OCR. By integrating this model with [DigitalOcean’s cloud infrastructure](https://cloud.digitalocean.com/), this tutorial provides a scalable and efficient way to implement [AI-powered image processing](/community/tutorials/arithmetic-bitwise-and-masking-python-opencv).

In this tutorial, you will learn to set up Llama 3.2 Vision with DigitalOcean’s cloud infrastructure, and demonstrate how to use it for AI-powered image processing for extracting employee IDs and names from images. We will cover the installation and configuration steps, as well as provide examples of how to use the model for Visual Question Answering and OCR. By the end of this tutorial, you will have a solid understanding of how to leverage Llama 3.2 Vision for your image processing needs.

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

Before proceeding, ensure you have:

-   A [DigitalOcean GPU Droplet](https://docs.digitalocean.com/products/droplets/how-to/gpu/) with [Python 3.10+](/community/tutorials/how-to-install-python-3-and-set-up-a-programming-environment-on-an-ubuntu-20-04-server) installed.
-   A [DigitalOcean Spaces account](https://docs.digitalocean.com/products/spaces/how-to/create/) with an access key and secret key.
-   A [DigitalOcean Managed MySQL](https://docs.digitalocean.com/products/databases/mysql/) database.
-   A [HuggingFace Token](https://huggingface.co/docs/hugs/en/how-to/cloud/digital-ocean).

[Step 1 - Set Up the Environment](#step-1-set-up-the-environment)[](#step-1-set-up-the-environment)
---------------------------------------------------------------------------------------------------

### [SSH into Your GPU Droplet](#ssh-into-your-gpu-droplet)[](#ssh-into-your-gpu-droplet)

Connect to your server via SSH:

```
ssh root@your_server_ip
```

### [Install Python & Create a Virtual Environment](#install-python-create-a-virtual-environment)[](#install-python-create-a-virtual-environment)

Run the following commands to set up a Python virtual environment:

```
apt install python3.10-venv -y
python3.10 -m venv llama-env
```

### [Activate the Virtual Environment](#activate-the-virtual-environment)[](#activate-the-virtual-environment)

```
source llama-env/bin/activate
```

[Step 2 - Install Required Dependencies](#step-2-install-required-dependencies)[](#step-2-install-required-dependencies)
------------------------------------------------------------------------------------------------------------------------

### [Install PyTorch & Hugging Face CLI](#install-pytorch-hugging-face-cli)[](#install-pytorch-hugging-face-cli)

```
pip install torch torchvision torchaudio
pip install -U "huggingface_hub[cli]"
huggingface-cli login
```

### [Install the Transformers Library](#install-the-transformers-library)[](#install-the-transformers-library)

```
pip install --upgrade transformers
```

### [Install Flask & AWS SDK (Boto3)](#install-flask-aws-sdk-boto3)[](#install-flask-aws-sdk-boto3)

[Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) is required to interact with **[DigitalOcean Spaces](https://docs.digitalocean.com/products/spaces/how-to/create/)**, which is S3-compatible.

```
pip install flask boto3
```

### [Install MySQL Connector for Python](#install-mysql-connector-for-python)[](#install-mysql-connector-for-python)

```
pip install mysql-connector-python
```

[Step 3 - Install & Configure Nginx](#step-3-install-configure-nginx)[](#step-3-install-configure-nginx)
--------------------------------------------------------------------------------------------------------

Install **[Nginx](https://nginx.org/)** to serve your Flask application:

```
sudo apt install nginx -y
```

[Step 4 - Set Up the Flask Web Application](#step-4-set-up-the-flask-web-application)[](#step-4-set-up-the-flask-web-application)
---------------------------------------------------------------------------------------------------------------------------------

### [Application Folder Structure](#application-folder-structure)[](#application-folder-structure)

Organize your project as follows:

```
llama-webapp/
├── app.py                # Main Flask app file
├── static/
│   └── styles.css        # Optional: CSS file for styling
└── templates/
    └── index.html        # HTML template for the web page
```

### [Python Code for the Application](#python-code-for-the-application)[](#python-code-for-the-application)

Below is the Flask app (`app.py`) that loads the **[Llama 3.2 model](https://marketplace.digitalocean.com/apps/meta-llama-llama-3-2-90b-vision-instruct-8x)**, processes uploaded images, and extracts employee details.

```
import os
import json
import requests
from PIL import Image
from flask import Flask, request, render_template, session
from transformers import MllamaForConditionalGeneration, AutoProcessor
import boto3
import torch
import mysql.connector
import re

# Initialize Flask app
app = Flask(__name__)
app.secret_key = "your_secret_key"  # Replace with a strong secret key

# Load the Llama 3.2 model and processor
model_id = "meta-llama/Llama-3.2-11B-Vision-Instruct"
model = MllamaForConditionalGeneration.from_pretrained(
    model_id, torch_dtype=torch.bfloat16, device_map="auto"
)
processor = AutoProcessor.from_pretrained(model_id)

# DigitalOcean Spaces setup
SPACE_NAME = "your_space_name"
SPACE_REGION = "your_region"
ACCESS_KEY = "your_access_key"
SECRET_KEY = "your_secret_key"

s3 = boto3.client(
    "s3",
    region_name=SPACE_REGION,
    endpoint_url=f"https://{SPACE_REGION}.digitaloceanspaces.com",
    aws_access_key_id=ACCESS_KEY,
    aws_secret_access_key=SECRET_KEY
)

# DigitalOcean Managed MySQL setup
DB_HOST = "your_mysql_host"
DB_PORT = 25060
DB_NAME = "your_database_name"
DB_USER = "your_username"
DB_PASSWORD = "your_password"

# Function to establish a database connection
def get_db_connection():
    try:
        conn = mysql.connector.connect(
            host=DB_HOST, port=DB_PORT, database=DB_NAME, user=DB_USER, password=DB_PASSWORD
        )
        print("✅ Database connection successful!")
        return conn
    except Exception as e:
        print(f"❌ Error connecting to the database: {e}")
        return None

# Function to extract Employee ID & Name from an image
def extract_employee_details(image_path):
    """Extracts Employee Name and ID using Llama 3.2 Vision AI."""
    try:
        image = Image.open(image_path)
        prompt = (
            "Extract the Employee ID and Name from the given image. "
            "Provide output in valid JSON format with keys: 'employee_id' and 'employee_name'."
        )
        messages = [{"role": "user", "content": [{"type": "image"}, {"type": "text", "text": prompt}]}]

        input_text = processor.apply_chat_template(messages, add_generation_prompt=True)
        inputs = processor(image, input_text, return_tensors="pt").to(model.device)

        output = model.generate(**inputs, max_new_tokens=1024)
        raw_result = processor.decode(output[0])

        json_match = re.search(r"\{.*?\}", raw_result, re.DOTALL)
        extracted_data = json.loads(json_match.group(0)) if json_match else {}
        employee_id = extracted_data.get("employee_id", "").strip()
        employee_name = extracted_data.get("employee_name", "").strip()

        return employee_id, employee_name
    except Exception as e:
        return None, None

# Flask Routes
@app.route("/", methods=["GET", "POST"])
def index():
    """Handles image uploads, extracts Employee Name & ID, and stores it in MySQL."""
    result = None
    image_url = session.get("image_url")

    if request.method == "POST":
        image_file = request.files.get("image")
        if not image_file:
            return "Error: Please upload an image.", 400
        filename = image_file.filename
        image_path = os.path.join("/tmp", filename)
        image_file.save(image_path)

        employee_id, employee_name = extract_employee_details(image_path)
        return render_template("index.html", result=result, image_url=image_url)

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

[**Step 5: Run & Access the Application**](#step-5-run-access-the-application)[](#step-5-run-access-the-application)
--------------------------------------------------------------------------------------------------------------------

1.  Start the Flask application:
    
    ```
    python app.py
    ```
    
2.  Open your browser and visit:
    
    ```
    http://your_server_ip:5000
    ```
    
3.  Upload an image, extract employee details, and verify data storage in the database.
    

[FAQs](#faqs)[](#faqs)
----------------------

### [1\. What is Llama 3.2, and how does it differ from previous versions?](#1-what-is-llama-3-2-and-how-does-it-differ-from-previous-versions)[](#1-what-is-llama-3-2-and-how-does-it-differ-from-previous-versions)

[Llama 3.2](https://marketplace.digitalocean.com/apps/meta-llama-llama-3-2-90b-vision-instruct-8x) is a state-of-the-art AI model developed by [Meta (Facebook)](https://www.meta.com/) that builds upon its predecessor, [Llama 3](https://ai.meta.com/blog/meta-llama-3/). It offers improved natural language understanding, better performance in multimodal tasks (including image processing), and enhanced efficiency when integrated with [Hugging Face Transformers](https://huggingface.co/).

### [2\. Can Llama 3.2 process images directly?](#2-can-llama-3-2-process-images-directly)[](#2-can-llama-3-2-process-images-directly)

Yes, Llama 3.2 introduces vision models (11B and 90B) that enable it to process and understand images directly, allowing for tasks like image captioning, object recognition, and scene interpretation

### [3\. What are some common use cases of Llama 3.2 in image processing?](#3-what-are-some-common-use-cases-of-llama-3-2-in-image-processing)[](#3-what-are-some-common-use-cases-of-llama-3-2-in-image-processing)

Llama 3.2 can assist in image processing tasks such as:

-   **Image Captioning:** Generating descriptive text from images.
-   **Object Recognition:** Identifying objects within an image when combined with a vision model.
-   **Text Extraction (OCR):** Helping interpret extracted text from an image.
-   **Style Transfer & Image Editing:** Assisting in AI-powered image generation and modification.

### [4\. How do I set up Hugging Face Transformers to work with Llama 3.2?](#4-how-do-i-set-up-hugging-face-transformers-to-work-with-llama-3-2)[](#4-how-do-i-set-up-hugging-face-transformers-to-work-with-llama-3-2)

You can install the required libraries and load the model using the following steps:

```
pip install transformers torch torchvision
```

Then, load the model with:

```
from transformers import AutoModelForCausalLM, AutoTokenizer

model_name = "meta-llama/llama-3-2"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForCausalLM.from_pretrained(model_name)
```

If working with images, you may also need transformers’s vision models like [CLIP](https://huggingface.co/docs/transformers/en/model_doc/clip):

```
from transformers import CLIPProcessor, CLIPModel

clip_model = CLIPModel.from_pretrained("openai/clip-vit-base-patch32")
processor = CLIPProcessor.from_pretrained("openai/clip-vit-base-patch32")
```

### [5\. How can I use Llama 3.2 for image captioning?](#5-how-can-i-use-llama-3-2-for-image-captioning)[](#5-how-can-i-use-llama-3-2-for-image-captioning)

Llama 3.2’s vision model can generate high-quality captions for images:

```
from transformers import AutoProcessor, AutoModelForVision2Seq
from PIL import Image

model_name = "meta-llama/llama-3.2-vision"
processor = AutoProcessor.from_pretrained(model_name)
model = AutoModelForVision2Seq.from_pretrained(model_name)

image = Image.open("your_image.jpg")
inputs = processor(images=image, return_tensors="pt")
output = model.generate(**inputs)

caption = processor.batch_decode(output, skip_special_tokens=True)[0]
print("Generated Caption:", caption)
```

### [6\. Can I fine-tune Llama 3.2 Vision on my own dataset?](#6-can-i-fine-tune-llama-3-2-vision-on-my-own-dataset)[](#6-can-i-fine-tune-llama-3-2-vision-on-my-own-dataset)

Yes, you can fine-tune Llama 3.2 Vision using Hugging Face’s transformers library with LoRA (Low-Rank Adaptation).

Example fine-tuning setup:

```
from peft import LoraConfig, get_peft_model

config = LoraConfig(
    r=8, lora_alpha=32, target_modules=["q_proj", "v_proj"]
)
fine_tuned_model = get_peft_model(model, config)
```

This allows efficient fine-tuning without retraining the entire model.

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

In this tutorial, you learned how to extract employee IDs and names from images using the [Llama 3.2 Vision](https://marketplace.digitalocean.com/apps/meta-llama-llama-3-2-90b-vision-instruct-8x) model. We integrated [DigitalOcean Spaces](/products/spaces) for storing images and used a managed [MySQL database](https://docs.digitalocean.com/products/databases/mysql/) for structured data storage. This solution provides an automated way to process and manage employee verification data with AI-powered efficiency.

#### [Source](https://www.digitalocean.com/community/tutorials/image-processing-using-llama-huggingface)

<br/>
---
