---
title: "Automating a Podcast Script using HuggingFace 1-Click Models"
date: 2025-01-21T15:48:49.000Z
draft: false
type: posts
categories: 
- ai-ml,gpu
---
# Automating a Podcast Script using HuggingFace 1-Click Models

<br/>

<br/>
Podcasting has become one of the most popular tools for creating and disseminating news and entertainment over the past few decades. This is for numerous reasons, but the ease of access, largely free nature, and huge variety of topics covered on podcasts are some of the biggest. Starting a new podcast is also a very popular creative endeavor for these reasons.

One of the biggest challenges aspiring podcast writers face is the staggering amount of content one needs to cover when writing a lengthy podcast. Podcasts vary in length, but, according to Google [Gemini](https://gemini.google.com/), the average podcast length is anywhere from 20 to 40 minutes per episode. Consequently, many new podcast writers may have a difficult time finding subject matter to fill the time, especially after they have covered the initial materials that inspired the podcast itself.

In this article, we want to share a potential solution for podcast writers: writing with [HuggingFace 1-Click Models](/products/ai-ml/1-click-models) running on DigitalOcean’s [GPU Droplets](/products/gpu-droplets). This tool brings the fastest way to access HuggingFace models on powerful NVIDIA GPUs on the web, and can provide the perfect assistant tool for our writing procedure.Follow along for a guide to setting up & executing a podcast content creation system for any subject!

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

-   Python code: we will use Python code in this tutorial to access the model
-   Terminal navigation: this tutorial will require simple navigation of a terminal window, though the code will be provided

[Setting up the 1-Click HuggingFace Model GPU Droplet](#setting-up-the-1-click-huggingface-model-gpu-droplet)[](#setting-up-the-1-click-huggingface-model-gpu-droplet)
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

The first thing we need to do is set up our machine to generate podcast scripts. We are going to follow the same setup procedure found in our guide to creating a personal assistant with the HuggingFace 1-click Models. ([link](/community/tutorials/1click-model-personal-assistant)). We recommend reading this tutorial’s first few sections, where we cover the models available and their access methodology, before continuing this one.

To begin, create a new GPU Droplet following the procedure shown in the official HuggingFace [documentation](https://huggingface.co/docs/hugs/en/how-to/cloud/digital-ocean).

Watch the following video for a full step-by-step guide to creating a 1-Click Model GPU Droplet, and check out this article for more details on launching a new instance. We suggest researching each of the available models and their associated costs before continuing using the resources available in this [link](/products/ai-ml/1-click-models).

<a href="https://www.youtube.com/watch?v=05t0JjX\_plQ" target="\_blank">View YouTube video</a>

Once your machine has started up, proceed to the next section.

[Starting up the Personal Assistant](#starting-up-the-personal-assistant)[](#starting-up-the-personal-assistant)
----------------------------------------------------------------------------------------------------------------

To proceed, we are going to start up the same personal assistant Gradio application we developed for this [article](/community/tutorials/1click-model-personal-assistant). This will provide us with a functional playground for developing our script. Alternatively, we can use the UNIX and Python scripts shown in the HuggingFace 1-click Models documentation, but we will not show that in this tutorial.

Open up a terminal window for your GPU Droplet, and paste the following script in:

```
cd ../home
apt-get install python3-pip
pip install gradio tts huggingface_hub transformers datasets scipy torch torchaudio accelerate
touch personalAssistant.py
vim personalAssistant.py
```

This will install the required packages for the demo, and then create a script for us to use to run the personal assistant. Next, it will create and open a vim text editor. Paste the following script into the window, and then type the escape key followed by `:wq` to save the file with changes.

```
import gradio as gr
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer, StoppingCriteria, StoppingCriteriaList, TextIteratorStreamer
from threading import Thread
import os
from huggingface_hub import InferenceClient
import gradio as gr
import random
import time
from TTS.api import TTS
import torch
from transformers import AutoModelForSpeechSeq2Seq, AutoProcessor, pipeline
from datasets import load_dataset
import scipy.io.wavfile as wavfile
import numpy as np


device = "cuda:0" if torch.cuda.is_available() else "cpu"
torch_dtype = torch.float16 if torch.cuda.is_available() else torch.float32

model_id_w = "openai/whisper-large-v3"

model_w = AutoModelForSpeechSeq2Seq.from_pretrained(
    model_id_w, torch_dtype=torch_dtype, low_cpu_mem_usage=True, use_safetensors=True
)
model_w.to(device)

processor = AutoProcessor.from_pretrained(model_id_w)

pipe_w = pipeline(
    "automatic-speech-recognition",
    model=model_w,
    tokenizer=processor.tokenizer,
    feature_extractor=processor.feature_extractor,
    torch_dtype=torch_dtype,
    device=device,
)

client = InferenceClient(base_url="http://localhost:8080", api_key=os.getenv("BEARER_TOKEN"))

# Example voice cloning with YourTTS in English, French and Portuguese
# tts = TTS("tts_models/multilingual/multi-dataset/bark", gpu=True)

# get v2.0.2
tts = TTS(model_name="xtts_v2.0.2", gpu=True)

with gr.Blocks() as demo:
    chatbot = gr.Chatbot(type="messages")
    with gr.Row():
        msg = gr.Textbox(label = 'Prompt')
        audi = gr.Audio(label = 'Transcribe audio')
    with gr.Row():
        submit = gr.Button('Submit')
        submit_audio = gr.Button('Submit Audio')
        read_audio = gr.Button('Transcribe Text to Audio')
        clear = gr.ClearButton([msg, chatbot])
    with gr.Row():
        token_val = gr.Slider(label = 'Max new tokens', value = 512, minimum = 128, maximum = 1024, step = 8, interactive=True)
        temperature_ = gr.Slider(label = 'Temperature', value = .7, minimum = 0, maximum =1, step = .1, interactive=True)
        top_p_ = gr.Slider(label = 'Top P', value = .95, minimum = 0, maximum =1, step = .05, interactive=True)

    def respond(message, chat_history, token_val, temperature_, top_p_):
        bot_message = client.chat.completions.create(messages=[{"role":"user","content":f"{message}"},],temperature=temperature_,top_p=top_p_,max_tokens=token_val,).choices[0]['message']['content']
        chat_history.append({"role": "user", "content": message})
        chat_history.append({"role": "assistant", "content": bot_message})
        # tts.tts_to_file(bot_message, speaker_wav="output.wav", language="en", file_path="output.wav")

        return "", chat_history, #"output.wav"
    
    def respond_audio(audi, chat_history, token_val, temperature_, top_p_):  
        wavfile.write("output.wav", 44100, audi[1]) 
        result = pipe_w('output.wav')
        message = result["text"]
        print(message)
        bot_message = client.chat.completions.create(messages=[{"role":"user","content":f"{message}"},],temperature=temperature_,top_p=top_p_,max_tokens=token_val,).choices[0]['message']['content']
        chat_history.append({"role": "user", "content": message})
        chat_history.append({"role": "assistant", "content": bot_message})
        tts.tts_to_file(bot_message, speaker_wav="output.wav", language="en", file_path="output2.wav")
        tts.tts_to_file(bot_message,
                file_path="output.wav",
                speaker_wav="output.wav",
                language="en")
        return "", chat_history, #"output.wav"
    def read_text(chat_history):
        print(chat_history)
        print(type(chat_history))
        tts.tts_to_file(chat_history[-1]['content'],
                file_path="output.wav",
                speaker_wav="output.wav",
                language="en")
        return 'output.wav'


    msg.submit(respond, [msg, chatbot, token_val, temperature_, top_p_], [msg, chatbot])
    submit.click(respond, [msg, chatbot, token_val, temperature_, top_p_], [msg, chatbot])
    submit_audio.click(respond_audio, [audi, chatbot, token_val, temperature_, top_p_], [msg, chatbot])
    read_audio.click(read_text, [chatbot], [audi])
demo.launch(share = True)
```

We can then run the application by typing the following command in:

```
python personalAssistant.py
```

Open the shared, public link output next to access the application in your local browser.

[Creating the Podcast Script](#creating-the-podcast-script)[](#creating-the-podcast-script)
-------------------------------------------------------------------------------------------

<a href="https://www.youtube.com/watch?v=nbOcXcY9R7M" target="\_blank">View YouTube video</a>

After we have launched and opened the personal assistant, we can get started.

First, we need to decide our topic. The LLMs powering the 1-click Model GPU Droplets are incredibly versatile and able to cover nearly any subject we provide. In our experiments, they each have fairly recent knowledge bases demonstrating recent knowledge from the last year. For this example, we are going to create a tech news podcast that focuses on discussing the disruptive effects of new technologies.

When crafting the input prompt, it’s critical to make the model consider each of the points you wish to touch on.

Sample input:

```
Write a podcast script where two people, George and Maya, discuss the implications of a theoretical new competitor to Apple's Iphone. Have the two discuss in detail the consequences of the release of this new phone, considering the political, economic, and social implications of the end of the dominance of the Iphone.
```

Output:

```
Episode Title: "The Rise of a New Challenger: Implications of a Theoretical iPhone Rival"
Intro Music
George: Welcome to "Tech Talk", the podcast where we dive into the latest trends and innovations in the world of technology. I'm your host, George.
Maya: And I'm Maya. Today, we're going to explore a hypothetical scenario that's been making waves in the tech industry: the release of a new competitor to Apple's iPhone.
George: That's right. Imagine a phone that's not only on par with the iPhone in terms of hardware and software, but also offers a unique user experience that could potentially disrupt the market.
Maya: Exactly. So, let's dive into the implications of such a scenario. George, if this new phone were to become a reality, what do you think would be the immediate consequences?
Breaking Point 1: Initial Reaction
George: Well, I think the biggest impact would be on Apple's stock price. If a new competitor is offering a similar product at a lower price point, Apple's sales would likely take a hit, which would reflect in their stock price.
Maya: That's a great point. And what about the impact on consumers? Would they be willing to switch to a new brand?
Breaking Point 2: Consumer Perspective
George: I think some consumers might be interested in trying out a new brand, especially if it offers a unique feature or user experience that the iPhone doesn't. But, at the same time, there's a lot of loyalty built into the iPhone ecosystem, so it would be interesting to see how consumers respond.
Maya: That's true. And what about the impact on the ecosystem itself? Would this new competitor disrupt the entire app market?
Breaking Point 3: Ecosystem Disruption
George: Ah, that's a great question. If a new phone were to gain significant market share, it could potentially disrupt the entire app market. Developers would need to create apps for multiple platforms, which could lead to a fragmentation of the market.
Maya: Exactly. And what about the impact on the economy? Would this new competitor create new jobs or disrupt existing ones?
Breaking Point 4: Economic Impact
George: Well, it's likely that the release of a new competitor would create new jobs in the manufacturing and development sectors, but it could also lead to job losses in industries that are closely tied to the iPhone ecosystem.
Maya: That's a good point. And what about the impact on politics? Would this new competitor have any implications for national security or trade policies?
Breaking Point 5: Political Implications
George: Ah, that's an interesting question. If a new competitor were to gain significant market share, it could potentially create new tensions in the trade space, especially if the new phone were to be manufactured in a country with which the US has a trade agreement.
Maya: Exactly. And finally, what about the social implications of this new competitor? Would it lead to a more diverse and inclusive tech industry?
Breaking Point 6: Social Impact
George: Well, I think it's possible that a new competitor could lead to a more diverse and inclusive tech industry, especially if the new phone were to be designed with accessibility features or other inclusive technologies.
Maya: That's a great point. And what about the potential for social change? Would this new competitor be able to challenge the dominant narratives around tech and society?
Breaking Point 7: Social Change
George: Ah, that's a great question. If a new competitor were to gain significant market share, it could potentially challenge the dominant narratives around tech and society, especially if the new phone were to be designed with a focus on social responsibility.
Maya: Exactly. Well, that's all the time we have for today. Thanks for joining us on this thought experiment, George.
Outro Music
George: Thanks for tuning in to "Tech Talk". Join us next time for more discussions on the latest trends and innovations in the world of technology.
Maya: And don't forget to subscribe to our podcast for more in-depth analysis and expert insights.
Outro Music Continues
Note: The breaking points are indicated where the conversation can be improvised, allowing for a more dynamic and engaging discussion.
```

Take a look at how this has been formatted automatically by the model. The numerous breaking points let us offer spaces to fill in content further using the model. We could add an additional prompt for each breaking point, further expanding our new skeleton into a composited script. For example, we could enter the full segment for “Breaking Point 4: Economic Impact” with additional instructions to flesh the section out. Here is what we received when we tried:

```
## Input
Continue this conversation. Focus entirely on the political economic implications of a new competitor for the iPhone
"Breaking Point 4: Economic Impact
George: Well, it's likely that the release of a new competitor would create new jobs in the manufacturing and development sectors, but it could also lead to job losses in industries that are closely tied to the iPhone ecosystem.
Maya: That's a good point. And what about the impact on politics? Would this new competitor have any implications for national security or trade policies?"
## Output
George: That's a great question, Maya. The implications for national security and trade policies could be significant. The iPhone is a symbol of American innovation and technological prowess, and any disruption to the market could have broader implications for the US economy and its position in the world.
Maya: I see what you mean. But what about the competitor's country of origin? If it's a company from a country like China, for example, would that change the dynamics of trade policies and national security concerns?
George: Exactly. If the competitor is a Chinese company, it could lead to increased scrutiny of Chinese trade practices and potentially even calls for tariffs or other trade restrictions. This could have a ripple effect on the global economy and potentially lead to a trade war.
Maya: That's a scary prospect. And what about the impact on the US dollar? If the competitor is able to gain significant market share, could that lead to a decrease in the value of the dollar?
George: It's possible. A decrease in the value of the dollar could make American exports more expensive and potentially lead to inflation. This could have a negative impact on the US economy and potentially even lead to a recession.
Maya: I see what you mean. But what about the impact on Apple's stock price? If the competitor is able to gain significant market share, could that lead to a decline in Apple's stock price?
George: That's a good question. If the competitor is able to gain significant market share, it could lead to a decline in Apple's stock price, potentially by 10-20% or more. This could have a significant impact on the company's valuation and potentially even lead to a change in leadership.
Maya: Wow, that's a lot to consider. But what about the impact on consumers? Would a new competitor lead to lower prices and more innovation in the smartphone market?
George: Absolutely. A new competitor could lead to increased competition, which could drive down prices and lead to more innovation in the smartphone market. This could be a win for consumers, who could see better quality and more features at lower prices.
Maya: That's a great point. But what about the impact on Apple's ecosystem? If the competitor is able to gain significant market share, could that lead to a decline in the value of Apple's ecosystem, including its App Store and other services?
George: That's a good question. If the competitor is able to gain significant market share, it could lead to a decline in the value of Apple's ecosystem
```

As we can see, the model has some salient thoughts about the idea that provide us with relevant conversation topics. The authors can then take what we have created, and expand upon it by adding detail, statistics, and informed research to update and refine the existing output.

[Closing Thoughts](#closing-thoughts)[](#closing-thoughts)
----------------------------------------------------------

The versatility of the 1-Click HuggingFace models cannot be understated. In our experiments so far, we have created article localizations, [personal assistants](/community/tutorials/1click-model-personal-assistant) that rival closed source competition, and now full podcast scripts. In the future, we plan to experiment with fully automating the vocalization of the script using premiere Text-to-Speech modeling techniques, and we can see a pipeline for completely AI generated podcasting!

#### [Source](https://www.digitalocean.com/community/tutorials/podcast-script-automation-huggingface-1click-models)

<br/>
---
