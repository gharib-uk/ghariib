---
title: "HunyuanVideo on GPU Droplets"
date: 2025-01-17T18:50:49.000Z
draft: false
type: posts
categories: 
- ai-ml,gpu
---
# HunyuanVideo on GPU Droplets

<br/>

<br/>
The advent of text-to-video models has been one of the many AI miracles that came from the past year. From [SORA](https://openai.com/sora/) to [VEO-2](https://deepmind.google/technologies/veo/veo-2/), we have seen some truly incredible models hit the closed source market. These models are capable of generating videos of all kinds, including photorealism, animation, professional looking effects, and much more. Like everything else seemingly follows in Deep Learning, the open-source development community has followed the success of these closed sourced models closely & open-source models are always trying to achieve the same video quality and prompt fidelity.

Recently, we have seen the release of two notable AI text-to-video models that are making waves like Stable Diffusion once did. These are specifically the [LTX](https://github.com/Lightricks/LTX-Video) and [HunyuanVideo](https://github.com/Tencent/HunyuanVideo) text-to-video models. LTX’s low RAM requirements and HunYuan’s versatility and trainability have surged the popularity of text-to-video models to levels higher than ever.

In this series of articles, we will discuss how to use these incredible models on DigitalOcean’s NVIDIA GPU enabled [GPU Droplets](/products/gpu-droplets); first, by taking a deeper look at HunyuanVideo. Readers can expect to leave this first article with a firmer understanding of how HunyuanVideo and related next-generation text-to-video models work under the hood. After covering the underlying theory, we will provide a demo showing how to get started running the model.

Follow along to learn how to create your own incredible videos with HunyuanVideo and DigitalOcean.

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

-   Python: this demo will contain intermediate level Python code. Anyone will be able to copy and paste the code in to follow along, but understanding and manipulation of the scripts will require Python
-   Deep Learning: we will cover the underlying theory behind the model in the first section of this article, and terminology used will require experience with Deep Learning concepts DigitalOcean account: We are going to create a GPU Droplet on DigitalOcean, which may require the user to create an account if they have not already

[HunyuanVideo](#hunyuanvideo)[](#hunyuanvideo)
----------------------------------------------

HunyuanVideo is, arguably, the first open-source model to rival competitive closed source models for text-to-video image generation. To achieve this success, HunyuanVideo’s research team made several considerations with regard to its data organization and the pipeline architecture.

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/james/Screenshot%202025-01-08%20at%203.02.02%E2%80%AFPM.png)

The data itself was carefully curated and refined in order to only use the most informative training videos with extremely dense descriptions in text. First, the video data was aggregated from several sources. Then, this data was parsed using a series of hierarchical refinements for each resolution, 256p, 360p, 540p, and 720p. These filtration steps focused on removing any data from the original source that had traits that were undesirable, and finished with a final step of manual selection. After selecting the video data manually, the researchers developed a proprietary VLM to handle the task of creating descriptions for each video for each of the following categories: a short description, a dense description, and descriptions of the background, style, shot-type, lighting, and atmosphere of each video. These structured captions provide the textual basis for training and inference.

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/james/Screenshot%202025-01-08%20at%202.55.28%E2%80%AFPM.png)

Let’s now look at the model architecture. HunyuanVideo is a powerful video generative model with over 13 billion parameters, one of the largest available to the open-source community. The model was trained on a spatial-temporally compressed latent space, which was compressed using a [Causal 3D VAE](https://github.com/IsaacGuan/3D-VAE). The text prompts were then encoded using a large language model, and used as the condition. To generate the image, the Gaussian noise and condition are taken as input, and the model generates an output latent, which is decoded into images or videos through the 3D VAE decoder. ([Source](https://arxiv.org/abs/2412.03603))

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/james/Screenshot%202025-01-08%20at%203.08.02%E2%80%AFPM.png)

Looking a bit deeper, we can see the Transformer design in HunyuanVideo above. It employs a unified Full Attention mechanism for superior performance compared to divided spatiotemporal attention, it supports unified generation for both images and videos, and it leverages existing LLM-related acceleration capabilities more effectively, enhancing both training and inference efficiency. ([Source](https://arxiv.org/abs/2412.03603))

"To integrate textual and visual information effectively, they follow the strategy of a “Dual-stream to Single-stream” hybrid model design for video generation. In the dual-stream phase of this methodology, video and text tokens are processed independently through multiple Transformer blocks, enabling each modality to learn its own appropriate modulation mechanisms without interference. In the single-stream phase, it concatenates the video and text tokens and feed them into subsequent Transformer blocks for effective multimodal information fusion. This design captures complex interactions between visual and semantic information, enhancing overall model performance.” ([Source](https://arxiv.org/abs/2412.03603))

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/james/Screenshot%202025-01-08%20at%203.14.12%E2%80%AFPM.png)

For the text encoder, they “utilize a pre-trained Multimodal Large Language Model ([MLLM](https://arxiv.org/abs/2302.10035)) with a Decoder-Only structure \[\], which has following advantages: (i) Compared with T5, MLLM after visual instruction finetuning has better image-text alignment in the feature space, which alleviates the difficulty of instruction following in diffusion models; (ii) Compared with CLIP, MLLM has been demonstrated superior ability in image detail description and complex reasoning; (iii) MLLM can play as a zero-shot learner by following system instructions prepended to user prompts, helping text features pay more attention to key information.” ([Source](https://arxiv.org/abs/2412.03603))

Put together, we have a pipeline for creating novel videos or images from just text inputs.

[HunyuanVideo Code demo](#hunyuanvideo-code-demo)[](#hunyuanvideo-code-demo)
----------------------------------------------------------------------------

### [GPU Selection](#gpu-selection)[](#gpu-selection)

To run HunyuanVideo, we first recommend that users have adequate computing power to run the model. We recommend at least 40GB of VRAM, ideally 80. For this, we prefer to use DigitalOcean’s Cloud GPU Droplet offerings. For more details, check out this [link to get started on a GPU Droplet](/products/gpu-droplets).

Once you have chosen a GPU on a good cloud platform & started it up, we can move on to the next step.

### [Python code](#python-code)[](#python-code)

First, we are going to show how to run HunyuanVideo with Python code and Gradio. To get started, paste the following into the terminal.

```
Git clone https://github.com/Tencent/HunyuanVideo
Cd HunyuanVideo/
Pip install -r requirements.txt
python -m pip install ninja
python -m pip install git+https://github.com/Dao-AILab/flash-attention.git@v2.6.3
python -m pip install xfuser==0.4.0
python -m pip install "huggingface_hub[cli]"
Huggingface-cli login
```

You will then be prompted to log in to HuggingFace, which is required to access the models. To actually download them, after doing the HuggingFace login, paste the following into the terminal.

```
huggingface-cli download tencent/HunyuanVideo --local-dir ./ckpts
```

Once the downloads are complete, we can launch the web application with this final command:

```
python3 gradio_server.py --flow-reverse --share
```

This will create a publicly accessible and shareable link that we can now open in our local machine’s browser.

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/james/Screenshot%202025-01-09%20at%203.18.35%E2%80%AFPM.png)

From here, we can take advantage of our powerful GPU to start generating videos. Enter in a descriptive and detailed prompt into your text input first. Then, we suggest starting with a low resolution (540p), to more quickly generate the initial video. Use the default settings with this change to generate videos to start, until you find a video you like. Then, using the advanced options, set a repeatable seed, so that you can recreate an upscaled version of the same video at a higher resolution. We can also increase the number of inference steps, which we found to have a greater effect on the video quality than the nature of the output.

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/James/ComfyUI_00001_.webp)

The model is incredibly versatile and easy to use. In our testing, we found that it was capable of creating videos in a wide variety of styles including realism, fantasy, moving artwork, animation in both 2d and 3d, and much more. We were especially impressed by the realism the model could produce for human figures. We even found some success doing basic effects work with realistic characters. In particular, attention should be on how exceptional HunyuanVideo is at generating all aspects of the human body and face. It does seem to struggle with hands, but that is still the case for most diffusion based image synthesis models and to be expected. Additionally, it’s worth noting that the model is extremely detailed in the foreground, while being somewhat lacking in details in the background; a fuzz seems to cover much of the background even at higher step counts. Overall, we found the model to be very effective, and well worth the cost of using the GPU.

<a href="https://www.youtube.com/watch?v=qOJhKeXy7v8" target="\_blank">View YouTube video</a>

Here is a sample video we made by compositing 5 HunyuanVideo samples with a MusicGen sample audio track. As you can see, the possibilities are really endless as more development and fine-tunes come out for this awesome model.

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

HunyuanVideo is a really impressive first attempt at closing the gap between the open and closed source video generation models. While it does not seem to quite match the high visual levels touted by models like VEO-2 and SORA, HunyuanVideo does an admirable job of matching the diversity of subjects covered by these models during training. In the near future, we can expect to see more rapid steps forward for video models now that open-sourcing has hit this particular sector of development, especially from players like TenCent.

Look out for part two of this series where we will cover Image-to-Video generation with LTX Video!

#### [Source](https://www.digitalocean.com/community/tutorials/hunyuanvideo-gpu-droplets)

<br/>
---
