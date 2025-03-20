---
title: "How to Create an Email Newsletter Generator"
date: 2025-01-27T15:35:44.000Z
draft: false
type: posts
categories: 
- ai-ml
---
# How to Create an Email Newsletter Generator

<br/>

<br/>
[Introduction](#introduction)[](#introduction)
----------------------------------------------

Email newsletters have been a staple of online communication for decades. Unlike social media where algorithms control content visibility, email newsletters land directly in subscribers’ inboxes, allowing for greater control over content reception.

In this tutorial, we will demonstrate how DigitalOcean’s [1-click models](/products/ai-ml/1-click-models) can be configured into a [Gradio](https://www.gradio.app/) user interface to create a newsletter generator. We hope to show you how well-formulated system prompt templates can accelerate your newsletter writing workflow in a way that can meet your standards and feels authentic to you, significantly reducing the time and effort required to create high-quality and engaging content.

There are 3 parts to this tutorial.

1.  Setting up the 1-click model
2.  Understanding how to customize prompts for your newsletter use-case
3.  Implementation with a [Gradio](https://www.gradio.app/) user interface

### [1-Click Models](#1-click-models)[](#1-click-models)

DigitalOcean is committed to providing developers and innovators with the best resources and tools to bring their ideas to life. DigitalOcean has partnered with Hugging Face to offer [1-click models](/products/ai-ml/1-click-models). This allows for the integration of GPU Droplets with state-of-the-art open-source LLMs in [Text Generation Inference (TGI)](https://huggingface.co/docs/text-generation-inference/index) container applications. As opposed to closed-source models, open-source models allow you to have [greater control](https://www.youtube.com/watch?t=28&v=e9gNEAlsOvU&feature=youtu.be) over the model and your data during inference.

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

Those determined to create a newsletter generator are encouraged to follow the steps- it’s doable without extensive experience. Knowledge of Python is helpful, and tools like [Cursor](https://www.cursor.com/) are great for assisting with code generation and modification.

[Part 1 Setting up the 1-click model](#part-1-setting-up-the-1-click-model)[](#part-1-setting-up-the-1-click-model)
-------------------------------------------------------------------------------------------------------------------

This part of the tutorial can be skipped if you are already familiar with setting up 1-click models from our documentation or previous tutorials.

### [Step 1: Account](#step-1-account)[](#step-1-account)

To access these 1-click models, [sign up](https://cloud.digitalocean.com/registrations/new) for an account or [login](https://cloud.digitalocean.com/login) to an existing account.

### [Step 2: Finding the GPU droplet portal](#step-2-finding-the-gpu-droplet-portal)[](#step-2-finding-the-gpu-droplet-portal)

Navigate to the “Create GPU Droplet” page, by either clicking on [**GPU Droplets**](/products/gpu-droplets) on the left panel or in the drop-down menu from the green “Create” button on the top right. ![create gpu droplet](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/sma/step2.png)

### [Step 3: Choose a datacenter region](#step-3-choose-a-datacenter-region)[](#step-3-choose-a-datacenter-region)

![datacenter region](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/sma/step3.png)

### [Step 4: Select the 1-click model](#step-4-select-the-1-click-model)[](#step-4-select-the-1-click-model)

Where it says Choose an Image, navigate to the “1-click Models” tab and select the [1-click model](https://marketplace.digitalocean.com/solutions/ai-models) you would like to use.

![1-click model](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/sma/step4.png)

### [Step 5: Choose a GPU Plan](#step-5-choose-a-gpu-plan)[](#step-5-choose-a-gpu-plan)

Choose a GPU plan. Currently, there is only the option of using either 1 or 8 H100 GPUs. ![GPU plan](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/sma/step5.png)

### [Step 6: Volumes and Backups (additional cost)](#step-6-volumes-and-backups-additional-cost)[](#step-6-volumes-and-backups-additional-cost)

This step can be skipped if not required for your application. Select “Add Volume block storage” if additional [Volumes](https://docs.digitalocean.com/products/droplets/details/features/#volumes-block-storage)(data storage) is desired for your Droplet. ![volumes](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/sma/step6volumes.png)

If daily or weekly automated server backups are desired, those can be selected for as well.  
![backups](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/sma/step6backups.png)

### [Step 7: Add an SSH Key](#step-7-add-an-ssh-key)[](#step-7-add-an-ssh-key)

Select an existing [SSH Key](https://docs.digitalocean.com/glossary/ssh/) or click “Add a SSH Key” for [instructions](https://docs.digitalocean.com/products/droplets/how-to/add-ssh-keys/). ![ssh key](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/sma/step7.png)

### [Step 8: Select for Desired Advanced Options](#step-8-select-for-desired-advanced-options)[](#step-8-select-for-desired-advanced-options)

Advanced options are available for you to customize your GPU Droplet experience.  
![Advanced options gpu droplet](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/sma/step8.png)

### [Step 9: Launch Your Droplet](#step-9-launch-your-droplet)[](#step-9-launch-your-droplet)

After filling in the final details (name, project, tags), click the “Create a GPU Droplet” button on the right-hand side of the page to proceed.

![launch droplet](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/sma/step9create.png)

It typically takes 10-15 minutes for the Droplet to be deployed. Once deployed, you will be charged for the time it is on. Remember to destroy your Droplet when it is not being used.

![deploy droplet](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/sma/step9deploy.png)

### [Step 10: Web Console](#step-10-web-console)[](#step-10-web-console)

![gpu web console](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/sma/step10webconsole.png) Once the GPU Droplet has been successfully deployed, click the “Web Console” button to access it in a new window.

### [Step 11 : Choose between cURL or Python](#step-11-choose-between-curl-or-python)[](#step-11-choose-between-curl-or-python)

#### cURL

[cURL](https://docs.digitalocean.com/glossary/curl/) is a command-line tool for transferring data; it’s great for one-off or quick testing scenarios without setting up a full Python environment. The following cURL command can be modified and pasted into the Web Console.

```
curl http://localhost:8080/v1/chat/completions \
    -X POST \
    -d '{"messages":[{"role":"user","content":"What is Deep Learning?"}],"temperature":0.7,"top_p":0.95,"max_tokens":128}}' \
    -H 'Content-Type: application/json' \
    -H "Authorization: Bearer $BEARER_TOKEN"
```

#### cURL Breakdown

Here’s a breakdown of the cURL command to give you more context should you want to modify the request body (line that begins with -d).

**Base URL:** [http://localhost:8080/v1/chat/completions](http://localhost:8080/v1/chat/completions)

This is the endpoint URL for the chat completion

-   localhost: Indicates that the API is running on the local machine
-   8080: Specifies the [port number](https://www.techtarget.com/searchnetworking/definition/port-number) the API is listening on
-   v1/chat/completions: The endpoint for requesting text completions

**\-X POST**: The POST [HTTP method](https://www.w3schools.com/tags/ref_httpmethods.asp) sends data to the server

Request Body

-   **\-d ‘{“messages”:\[{“role”:“user”,“content”:“What is Deep Learning?”}\],“temperature”:0.7,“top\_p”:0.95,“max\_tokens”:128}}’**: [JSON](https://www.json.org/json-en.html) data that will be sent in the request body with the following parameters:
    -   **messages**
        -   **role:** the role of the sender (system or user)
        -   **content**: the text content of the message
    -   **temperature:** randomness in the response
    -   **top\_p**: controls the diversity of the generated text
    -   **max\_tokens:** the maximum number of tokens to generate in the response

Headers  
\-**H 'Content-Type: application/json**’: HTTP Header specifying to the server that the request is in JSON format

**\-H “Authorization: Bearer $BEARER\_TOKEN”**: HTTP Header includes the authorization token required to access the API.

#### Python

We will be using the TGI with Python in this tutorial for more programmatic control over requests. The Python code can be implemented in an IDE like [VS Code](https://code.visualstudio.com/download).

### [Step 12: Bearer Token](#step-12-bearer-token)[](#step-12-bearer-token)

![Bearer Token](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/sma/step12.png) The [Bearer Token](https://docs.digitalocean.com/glossary/bearer-token/) allows for requests to be sent to the public IP of the deployed GPU Droplet. To store this token as an environment variable, copy the Bearer Token from the Web Console. In the code snippet below, replace “PASTE BEARER TOKEN” with the copied token. Paste the updated code snippet into your terminal.

In Terminal:

```
export BEARER_TOKEN="PASTE BEARER TOKEN"
```

A common mistake when exporting the bearer token in your terminal is forgetting to include the quotes.

[Part 2 Newsletter Generation with Prompt Engineering](#part-2-newsletter-generation-with-prompt-engineering)[](#part-2-newsletter-generation-with-prompt-engineering)
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Considering people often receive large volumes of emails daily, email newsletters should include valuable, relevant, or engaging content to ensure subscribers don’t lose interest or unsubscribe.  
Understanding what makes a successful newsletter will help one develop effective system prompts to streamline writing a good one.

### [What makes a good newsletter?](#what-makes-a-good-newsletter)[](#what-makes-a-good-newsletter)

A successful newsletter starts with content relevant to your readers’ interests, rather than just promoting your business or product offerings. The key is maintaining a consistent balance between informative content and promotional material –we recommend following the 80/20 rule, where 80% of the content provides value and only 20% is promotional. Additionally, excellent newsletters feature attention-grabbing subject lines, clear and scannable formatting, mobile-friendly design, and a consistent sending schedule that sets reader expectations.

### [Designing System Prompts](#designing-system-prompts)[](#designing-system-prompts)

**System prompts** are instructions or context-setting messages given to the model prior to processing user interactions. A well-designed system prompt should provide the necessary context and guidelines that are to be expected from all outputs of your newsletter generator.

Before proceeding, it is critical to consider what information you will need to generate your newsletter. **The more descriptive you are in your prompt, the more control you have over the output.** In contrast, the less information you provide, the more freedom you’re giving the LLM to be creative.

[Part 3 Gradio Implementation](#part-3-gradio-implementation)[](#part-3-gradio-implementation)
----------------------------------------------------------------------------------------------

For this implementation, we will be using [Gradio](https://www.gradio.app/), an open-source Python library for building web interfaces to demo machine learning models. We will be generating newsletters for a fictional streaming service that features award-winning AI-generated movies:

Lumina is a pioneering digital streaming service that exclusively features award-winning movies and series generated entirely by artificial intelligence. Launched in 2028, the platform distinguishes itself by curating and streaming the most innovative AI-generated content from global creators.

The beauty of this newsletter generator is that by configuring it in a way that we are happy with, we can reduce the cognitive overload that comes with writing drafts. In the case of Lumina, our fictional digital streaming service, the marketing lead tells us what they are looking for:

_“Our goal is simple: blow their minds with the incredible AI-generated films and series we’re curating, while subtly showing why Lumina is the only platform they need to be watching. We’ll do 80% pure, mind-blowing content about these revolutionary films—think behind-the-scenes insights, creator spotlights, technology breakthroughs—and just a light 20% about our platform. Each newsletter needs to feel like a conversation with a film-obsessed friend who knows all the coolest, most mind-bending stuff happening in entertainment. No corporate speak, no dry technology lectures. We’re making AI filmmaking feel accessible, exciting, and absolutely must-see. When our readers finish the newsletter, they should be reaching for their devices to check out what we’re showcasing. ”_

### [Create a virtual environment and install dependencies](#create-a-virtual-environment-and-install-dependencies)[](#create-a-virtual-environment-and-install-dependencies)

In terminal, enter these line by line, hitting enter after each line:

```

pip3 install virtualenv
mkdir NewsletterGen
cd NewsletterGen
python3 -m venv env

pip3 install huggingface-hub gradio

```

### [Import statements](#import-statements)[](#import-statements)

```
import os
import gradio as gr
from huggingface_hub import InferenceClient
```

### [Establishing a Client Connection](#establishing-a-client-connection)[](#establishing-a-client-connection)

[InferenceClient](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/inference_client) is a class from the [huggingface\_hub](https://huggingface.co/docs/huggingface_hub/main/en/index) library that allows you to make API calls to a deployed model.  
For this step, you will need to include the address of your GPU droplet in the base\_url. If you haven’t already exported your bearer token in the terminal (see step 12 of Part 1), do it now.

Copy the address of your GPU Droplet from the WebConsole and paste it in the base\_url below.

```
client = InferenceClient(base_url=http://"REPLACE WITH GPU DROPLET ADDRESS:8080", api_key=os.getenv("BEARER_TOKEN"))
```

[Generate newsletter function](#generate-newsletter-function)[](#generate-newsletter-function)
----------------------------------------------------------------------------------------------

In this function, we will pass in parameters for inputs for which you’ll be able to include details you’d like to be included in your newsletter.

The parameters that will be passed into this function include: headline\_topic, featured, highlights, tech\_insight, platform\_update, call\_to\_action.

```
def generate_newsletter(
   headline_topic,
   featured,
   highlights,
   tech_insight,
   platform_update,
   call_to_action
):
```

Next, we will construct a system prompt. Notice how detailed we are.

```
  partial_message = ""
  output = client.chat.completions.create(
      messages=[
          {"role": "system", "content": """You are a newsletter writer for Lumina.
           Lumina is a pioneering digital streaming service that exclusively features award-winning movies and series generated entirely by artificial intelligence.
           Launched in 2028, the platform distinguishes itself by curating and streaming the most innovative AI-generated content from global creators.
         Your primary goal is to create engaging, informative newsletters that:
           - Provide substantive insights into AI-generated content
           - Highlight unique film innovations
           - Maintain an authoritative yet approachable tone
           - Follow an 80/20 content-to-promotion ratio
           - Excite readers about the future of AI in entertainment
           Here are some guidelines to help you craft compelling newsletters:
           - Write at a 9th-grade reading level
           - Use active voice
           - Avoid technical jargon
           - Include concrete examples
           - Maintain a sense of wonder about AI creativity"""},
          {"role": "user", "content": prompt},
      ],
      stream=True,
      max_tokens=2048,
  )

```

```
   for chunk in output:
       partial_message += chunk.choices[0].delta.content
       yield partial_message
```

### [Create the Gradio interface](#create-the-gradio-interface)[](#create-the-gradio-interface)

```
with gr.Blocks(title="") as interface:
  gr.Markdown("#Lumina Newsletter Generator")
   with gr.Row():
      with gr.Column():
          headline_topic = gr.Textbox(
              label="Topic",
              placeholder="Topic to generate a headline about. If nothing is included, headline will be generated based on contents of the article."
          )
          featured = gr.Textbox(
              label="Featured Creators",
              placeholder="Describe innovative creators to spotlight in the newsletter."
          )
          highlights = gr.Textbox(
              label="Movie/Series Highlights",
              placeholder=""
           )
          tech_insight = gr.Textbox(
              label="Technology Insight",
              placeholder="Details on AI filmmaking techniques to discuss in the newsletter."
          )
          platform_update = gr.Textbox(
              label="Platform Updates",
              placeholder="Any updates worth mentioning to Lumina's Streaming platform."
          )
          call_to_action = gr.Textbox(
              label="Call-to-Action",
              placeholder="Information to discuss/include when encouraging platform exploration."
          )

  with gr.Row():
      generate_button = gr.Button("Generate Newsletter")
      clear_button = gr.Button("Clear All")


  output_text = gr.Textbox(
      label="Generated Newsletter",
      lines=20,
      interactive=False
  )

```

### [Set up Button Actions](#set-up-button-actions)[](#set-up-button-actions)

```
   generate_button.click(
       fn=generate_newsletter,
       inputs=[
           headline_topic,
           featured,
           highlights,
           tech_insight,
           platform_update,
           call_to_action
       ],
       outputs=output_text
   )
```

```
clear_button.click(
      fn=lambda: [None] * 7,
      inputs=None,
      outputs=[
          headline_topic,
          featured,
          highlights,
          tech_insight,
          platform_update,
          call_to_action
      ]
  )
```

### [Add example inputs for user to test the platform](#add-example-inputs-for-user-to-test-the-platform)[](#add-example-inputs-for-user-to-test-the-platform)

```
r.Examples(
      examples=[
          [
              "",
              "ArtificialDreams Studio: Known for their groundbreaking work in emotional AI storytelling, they've pioneered new techniques for generating authentic character expressions",
              "New Release: 'Digital Echoes' - A sci-fi thriller about AI consciousness, featuring revolutionary real-time emotion mapping",
              "Breakthrough in AI-generated facial micro-expressions allows for more nuanced character performances",
              "New interactive story branching feature allows viewers to influence narrative directions",
              "Join our Valentine's Special: First month free for new subscribers"
          ],
          [
              "",
              "QuantumNarrative Labs: Specializing in procedurally generated worlds with dynamic storytelling engines",
              "Premiere: 'Infinite Canvas' - An episodic series where each viewer experiences a unique storyline",
              "Introduction of quantum-inspired narrative generation for infinite story possibilities",
              "Launch of multi-language AI dubbing with perfect lip-sync",
              "Experience unlimited stories - Start your journey with 50% off annual subscriptions"
          ]
      ],
      inputs=[
          headline_topic,
          featured,
          highlights,
          tech_insight,
          platform_update,
          call_to_action
      ]
  )

interface.launch()

```

### [Conclusion](#conclusion)[](#conclusion)

Congratulations on making it to this point, we covered a lot. In this tutorial, we went over how to configure DigitalOcean’s 1-click models. We then discussed the components of a good newsletter and system prompt. Finally, we demonstrated how newsletter creation workflows can be augmented by designing an LLM-powered newsletter generator that can be customized with inputs that feed into a carefully designed prompt template.

Happy experimenting!

#### [Source](https://www.digitalocean.com/community/tutorials/newsletter-generator-with-1click-models)

<br/>
---
