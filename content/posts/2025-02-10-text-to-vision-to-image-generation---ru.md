---
title: "Text to Vision to Image Generation - Running DeepSeeks Janus Pro on DigitalOceans GPU Droplets"
date: 2025-02-10T19:25:49.000Z
draft: false
type: posts
categories: 
- ai-ml,gpu
---
# Text to Vision to Image Generation - Running DeepSeeks Janus Pro on DigitalOceans GPU Droplets

<br/>

<br/>
DeepSeek AI, the rising star of the AI world from Hangzhou China, has been one of the hottest topics around the past few weeks. This is largely thanks to the incredible performance of their [R1 series of models](https://github.com/deepseek-ai/DeepSeek-R1), which offer comparable reasoning capabilities to [OpenAI O1](https://openai.com/o1/) at a fraction of the training cost. The popularity of DeepSeek R1 has brought open-source models surging back to the forefront of the general consciousness.

More recently, DeepSeek also released their newest version of the autoregressive framework [Janus](https://huggingface.co/deepseek-ai/Janus-1.3B), [Janus Pro](https://huggingface.co/deepseek-ai/Janus-Pro-7B). Janus-Pro is a unified understanding and generation Multimodal Large Language Model that is capable of interpreting and generating both image and text data. In it does this by “by decoupling visual encoding into separate pathways, while still utilizing a single, unified transformer architecture for processing. The decoupling not only alleviates the conflict between the visual encoder’s roles in understanding and generation, but also enhances the framework’s flexibility.”

Follow along with this article to learn how Janus Pro works, how it compares to other multimodal LLMs, and how to run Janus Pro on a [DigitalOcean GPU Droplet](/products/gpu-droplets).

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

Python: Experience with Python code is required to follow along Deep Learning: This article will explore advanced concepts in deep learning

[The Janus Pro Framework](#the-janus-pro-framework)[](#the-janus-pro-framework)
-------------------------------------------------------------------------------

The Janus model family is based on the autoregressive transformer, which determines the probabilistic correlation between elements in a sequence to infer the following element. The unique approach of Janus is the decoupling of the encoding methods to convert the raw inputs into features. These are then processed by an unified autoregressive transformer. In practice, this allows for the creation of a combined model for both visual understanding and image synthesis.

In this section, we will explore what allowed the Janus architecture and framework to achieve such great results.

### [Janus Pro Architecture](#janus-pro-architecture)[](#janus-pro-architecture)

![Janus pro architecture](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/James/Screenshot%202025-02-05%20at%203.30.49%E2%80%AFPM.png)

The core architecture of Janus Pro is the same as its predecessor, Janus. In Janus models, the defining characteristic of their processing is the decoupled visual encoding for multimodal vision and generation. The independent encoders are then used to interpret the features from the inputs. They are then processed by a unified autoregressive transformer.

For multimodal understanding, they use the SigLIP (Sigmoid Loss for Language Image Pre-Training) to extract the coarse features from the image. These features are then flattened to a 1-dimensional representation where an adaptor maps the features to the input space of the LLM.

For generation tasks, the VQ tokenizer converts the image features into discrete IDs and flattens the sequence to a single dimension. “They then use a generation adaptor to map the codebook embeddings for each ID into the input space of the LLM. They then concatenate these feature sequences to form a multimodal feature sequence, which is subsequently fed into the LLM for processing. The built-in prediction head of the LLM is utilized for text predictions in both the pure text understanding and multimodal understanding tasks, while a randomly initialized prediction head is used for image predictions in the visual generation task. The entire model adheres to an autoregressive framework without the need for specially designed attention masks” ([Source](https://arxiv.org/pdf/2410.13848)).

**Info:** Deploy DeepSeek R1, the open-source advanced reasoning model that excels at text generation, summarization, and translation tasks. As one of the most computationally efficient open-source LLMs available, you’ll get high performance while keeping infrastructure costs low with DigitalOcean’s GPU Droplets.

→[Deploy your model in just one click](https://cloud.digitalocean.com/gpus/new?region=nyc2&size=gpu-h100x8-640gb&fleetUuid=e31b0c76-06d2-49d4-b446-2b81a38007c7&appId=177155486&image=deepseek-r1-671b&type=applications)

### [Janus Pro Training Strategy](#janus-pro-training-strategy)[](#janus-pro-training-strategy)

![janus stages](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/James/Screenshot%202025-02-07%20at%2012.20.55%E2%80%AFPM.png)

To achieve these results, Janus Pro used an optimized version of the three stage training process from Janus. Stage 1 trains the adaptors and image head, stage 2 is the unified pretraining of everything but the generation and understanding encoder, and stage 3 supervised finetuning of the understanding encoder. Let’s look at these in more detail.

In Stage 1, the goal is to train a connection between the visual and textual features in the embedding space. This functionally facilitates the LLMs to understand image elements and have the beginnings of image generation capabilities. During this stage, the model is frozen with only the understanding adaptor, generation adaptor and image head being updated. In Janus Pro, this process is extended for more training steps. More training on ImageNet allowed for model pixel dependence and superior image generation capabilities on limited categories of images. ([Source](https://github.com/deepseek-ai/Janus/blob/main/janus_pro_tech_report.pdf))

In Stage 2, the LLM is unfrozen and they perform unified pretraining on a multimodal corpus to allow Janus to learn and understand pure text data, multimodal understanding data, and visual generation data ([source](https://github.com/deepseek-ai/Janus/blob/main/janus_pro_tech_report.pdf) ). In Janus Pro, they ignore ImageNet completely at this stage, and instead use text-to-image data to generate images based on dense descriptions. This improved both training efficiency and overall robustness of the image generation capabilities. ([Source](https://github.com/deepseek-ai/Janus/blob/main/janus_pro_tech_report.pdf))

In Stage 3, all parameters of the pretrained model, except the generation encoder, are fine-tuned with instruction tuning data to enhance the model’s instruction-following and dialogue capabilities. This refines its capabilities to better follow that of conventional instruction-response LLMs. To ensure consistent improvement across all modalities, the fine-tuning data consists of multimodal data, pure text data, and text to image data. In Janus Pro, they use an adjusted ratio of this data split. They found that slightly reducing the text-to-image data proportion actually improves multimodal performance without affecting generation capabilities significantly.

It is thanks to this tripartite training paradigm that Janus Pro is capable of such a wide variety of deep learning tasks. In our experiments, we found the model to be extremely capable for any of the tasks we gave it including instruction-response, multimodal understanding of image data, and text-to-image generation.

[Janus Pro running on DigitalOcean GPU Droplets](#janus-pro-running-on-digitalocean-gpu-droplets)[](#janus-pro-running-on-digitalocean-gpu-droplets)
----------------------------------------------------------------------------------------------------------------------------------------------------

<a href="https://www.youtube.com/watch?v=9R7MaiyQogU" target="\_blank">View YouTube video</a>

To get started, you will need a DigitalOcean GPU Droplet. If you haven’t created one before, we recommend following the steps shown in this [tutorial](/community/tutorials/getting-started-with-llama), the [documentation](https://docs.digitalocean.com/products/droplets/how-to/gpu/create/), or by watching the video above.

Once your GPU Droplet is set up, open the web console or SSH in using your local terminal. Then, paste the following code into the terminal window.

```
apt get install -y git-lfs pip3
git-lfs clone https://huggingface.co/spaces/deepseek-ai/Janus-Pro-7B
pip install -r requirements.txt spaces omegaconf einops timm spaces torchvision attrdict 
python app.py - -share
```

This will download the Janus Pro model into the HuggingFace cache, and then launch the web application run by Gradio. This can be accessed anywhere on any browser by using the shared, public link.

![janus explaining a meme](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/James/Screenshot%202025-02-10%20at%206.48.38%E2%80%AFPM.png)

To get started, upload an image to the GUI. Then ask the GUI a question about the image. For example, we found the model quite capable at interpreting memes and scientific equations. It’s also incredible for image captioning.

Next, tab over to the image generator and try your hand at the generation. While nowhere near the capabilities of FLUX or Stable Diffusion, we are impressed by the versatility of the model.

Overall, we found Janus Pro to be a very capable multimodal understanding LLM and with image generation capabilities.

[Closing Thoughts](#closing-thoughts)[](#closing-thoughts)
----------------------------------------------------------

In conclusion, Janus Pro is an incredibly interesting model. It is capable as both an LLM, visual understanding model, and image generator. We look forward to seeing how future efforts with autoregressive models continue to advance the field.

#### [Source](https://www.digitalocean.com/community/tutorials/deepseek-janus-pro-gpu-droplets)

<br/>
---
