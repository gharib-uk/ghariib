---
title: "How to build your first model using DSS"
date: 2025-01-15
tags: 
  - "beginners"
  - "technology"
---

# Introduction

GenAI is everywhere, and it’s changing how we approach technology. If you’ve ever wanted to dive into the world of large language models (LLMs) but felt intimidated, there’s good news! Hugging Face recently launched a self-paced course that’s perfect for beginners and more experienced enthusiasts alike. It’s hands-on, approachable, and designed to work on standard hardware thanks to the small footprint of the models.  
  
As soon as I heard the news, I decided to give it a go using Canonical’s Data Science Stack (DSS).  

In this blog, I’ll guide you through setting up DSS and running the first notebook of Hugging Face’s course. This notebook focuses on supervised fine-tuning, a methodology to adapt pre-trained language models to specific tasks or domains. By the end of this post, you’ll see just how simple and accessible GenAI can be – a perfect new skill to kick off the New Year.

# Setting up your environment

Before we dive into the course, let’s set up the foundation. DSS makes configuring your data science environment straightforward and quick. Installing DSS and its dependencies will take you two minutes by following this guide (just install it, do not launch a notebook just yet). Go ahead, I’ll wait for you here!

**NOTE**: DSS supports both Intel and NVIDIA GPUs, so you don’t need to worry about compatibility. If you do have a GPU, read the guide to enable GPU support.

.

.

.

Ok! Now that you have DSS ready on your machine, let’s go ahead and create a new notebook:

```
dss create hf-smol-course --image=jupyter/minimal-notebook
```

This command will create a new notebook named `hf-smol-course`. I am using a minimal image from the Jupyter registry so that we can start from a bare environment and install the Smol Course dependencies without any conflict. 

Now log into your notebook by navigating to the url you see with `dss list`.

Once into the notebook, open a terminal and clone the Hugging Face Smol Course repository.

**NOTE**: It’s important to store your work inside the `shared` folder, so that it will persist across reboots.

```
cd shared
git clone https://github.com/huggingface/smol-course
cd smol-course
pip install -r requirements.txt
pip install ipywidgets
```

Installing the requirements might take a little bit of time due to the various cuda dependencies.

**NOTE**: `ipywidget` is necessary to log into Hugging Face with your notebook. 

Refresh your browser to make sure that the widgets library is properly loaded.

Notice how quick and easy it was to set up a GPU-enabled Python environment from scratch. Typically, you would have gone through many more steps to install Python, virtual environments, juggle with CUDA drivers, and then maybe have a working environment.

## Get your Hugging Face token

Before getting started, make sure you have a Hugging Face token ready to use. If you don’t, head over to huggingface.co, click on your profile image, then click _Access Tokens_. Create a new `write` token for your notebooks. You will use this token at the beginning of the course notebooks to log into the HF API.

This is a self-paced course, and at this point you have your environment setup and can continue exploring all the notebooks. Let’s dive into one of them, just for fun.

# Exploring Supervised Fine-Tuning

Let’s get our hands dirty!

Enter the `1_instruction_tuning` folder, you can take a look at the README which explains the concept of chat templates and supervised fine-tuning. Good background in case you are not familiar with these techniques (do go over the chat template notebook first if you are not familiar with chat templates).  
Open the `sft_finetuning_example` notebook and run through the first few cells of the notebooks. After defining a few imports, the notebook tests the base model before pre-training, to show you what useless a base model actually is:

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1600,h_946/https://ubuntu.com/wp-content/uploads/c688/screenshot-haiku.png)

The base model’s output is non-sensical, we need to fine tune it. Consequently, the notebook walks you through the dataset preparation and then training.  
Unless you have a beefy GPU, I suggest you lower the training parameters to experiment quickly. You can always go back later and set higher training parameters to compare results.

In my case I am setting `max_steps=300` and `eval_steps=30`. Training the model took me about 20 minutes:

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_840,h_723/https://ubuntu.com/wp-content/uploads/f080/Screenshot-2025-01-09-at-17.32.18.png)

Uploading the model is optional. You can complete the following step of validating the fine tunes model again on the same prompt by adding the following snippet after the `TODO` comment:

```
model = AutoModelForCausalLM.from_pretrained(
    pretrained_model_name_or_path=f"./{finetune_name}"
).to(device)
outputs = model.generate(**inputs, max_new_tokens=100)
print("After training:")
print(tokenizer.decode(outputs[0], skip_special_tokens=True))
```

My output was the following:

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_972,h_995/https://ubuntu.com/wp-content/uploads/b578/Screenshot-2025-01-09-at-17.32.34.png)

The model started producing some sense, but it obviously would need some more training. What was your result?

# Why DSS Makes It Easier

As I went through the notebook, one thing stood out: DSS removes all the usual setup headaches. With pre-installed libraries, GPU support, and a seamless interface, I didn’t have to troubleshoot driver issues or spend hours configuring dependencies. Instead, I could focus entirely on learning and experimenting.

Also, whenever you are done with your experimentation, you can easily clean up by simply removing the notebook with

```
dss remove <notebook name>
```

Without leaving any trace behind.

This experience was a reminder of why tools like DSS are game-changers. They lower the barriers to entry, allowing more people to explore fields like data science and GenAI without getting bogged down by technical challenges.

# Next Steps

So for this new year, give yourself the gift of learning. With DSS and Hugging Face’s course, you’re just a few steps away from mastering the basics of GenAI. Who knows? This might be the start of your journey into a fascinating new field.

I will write soon about other aspects of DSS, such as how you can leverage MLFlow to take your machine learning experiments to the next level. For now, enjoy the beginning of 2025 and have fun learning more about LLMs and GenAI!

Explore more about Data Science Stack: https://ubuntu.com/ai/data-science

Go to Source
