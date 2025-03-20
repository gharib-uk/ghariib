---
title: "Wan 21 The Latest in Video Generative Models"
date: 2025-03-11T16:46:46.000Z
draft: false
type: posts
categories: 
- ai-ml,gpu
---
# Wan 21 The Latest in Video Generative Models

<br/>

<br/>
[Introduction](#introduction)[](#introduction)
----------------------------------------------

Video generative models have made tremendous strides recently. Although advancements in language modeling are impressive, with their capacity to tackle more intricate tasks, generating realistic videos poses a unique challenge. As humans, our brains have evolved over millions of years to instinctively detect even the slightest visual inconsistencies, making realistic video generation a remarkably complex task. In a previous article, we discussed [HunyuanVideo](/community/tutorials/hunyuanvideo-gpu-droplets), a leading open-source vision language model that has caught up to impressive closed source models like [Sora](https://openai.com/sora/) and [Veo 2](https://deepmind.google/technologies/veo/veo-2/).

Beyond the typical use case of entertainment, areas of increased research interest for video generation models include [predicting protein folding dynamics](https://phys.org/news/2025-01-video-generative-molecular-world.html) and modeling [real-world environments](https://openai.com/index/video-generation-models-as-world-simulators/) for embodied intelligence (ex: robotics, self-driving cars). Advances in video generation models could be instrumental for scientific research and our ability to develop complex physical systems.

[Introducing Wan 2.1](#introducing-wan-2-1)[](#introducing-wan-2-1)
-------------------------------------------------------------------

On February 26th, 2025, a collection of open-source video foundation models Wan 2.1 were released. The series consists of four distinct models divided into two categories: text-to-video and image-to-video. The text-to-video category includes [T2V-14B](https://huggingface.co/Wan-AI/Wan2.1-T2V-14B) and [T2V-1.3B](https://huggingface.co/Wan-AI/Wan2.1-T2V-1.3B), while the image-to-video category features [I2V-14B-720P](https://huggingface.co/Wan-AI/Wan2.1-I2V-14B-720P) and [I2V-14B-480P](https://huggingface.co/Wan-AI/Wan2.1-I2V-14B-480P). These models range in size from 1.3 billion to 14 billion parameters. The larger 14B model particularly excels in scenarios requiring high motion, producing videos at 720p resolution with realistic physics. Meanwhile, the smaller 1.3B model offers an excellent compromise between quality and efficiency, allowing users to generate 480p videos on standard hardware in approximately four minutes.

On February 27th, 2025, Wan2.1 was [integrated](https://comfyanonymous.github.io/ComfyUI_examples/wan/) into [ComfyUI](https://www.comfy.org/), an open source node-based interface for creating images, videos, and audio with GenAI, and on March 3rd, 2025, Wan2.1’s [T2V](https://github.com/huggingface/diffusers/blob/main/src/diffusers/pipelines/wan/pipeline_wan.py) and [I2V](https://github.com/huggingface/diffusers/blob/main/src/diffusers/pipelines/wan/pipeline_wan_i2v.py) were integrated into [Diffusers](https://huggingface.co/docs/diffusers/main/en/api/pipelines/wan), a popular Python library developed by Hugging Face that provides tools and implementations for diffusion models.

![peak signal-to-noise ratio (PSNR) vs. efficiency](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/p2svsefficiency.png) In this figure, we can see that with fewer parameters, Wan-VAE achieves a higher efficiency (frame/latency) and a comparable peak signal-to-noise ratio (PSNR) as Hunyuan video.

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

There are two parts to this tutorial. (1) An overview covering the model architecture and training methodology and (2) an implementation where we run the model. Note that the overview section of this article might receive an update once Wan 2.1’s full technical report is released. For the first part of the tutorial, an understanding of deep learning fundamentals is critical for following along with the theory. Some exposure to concepts discussed in this tutorial may be helpful (ex: autoencoders, diffusion transformers, flow matching). To complete the second part of this tutorial, a GPU is required. If you don’t have access to a GPU, consider [signing up](https://cloud.digitalocean.com/registrations/new) for a DigitalOcean account to utilize a GPU Droplet. Feel free to skip the overview section if you’re only interested in implementing Wan 2.1.

[Overview](#overview)[](#overview)
----------------------------------

### [A Refresher on Autoencoders](#a-refresher-on-autoencoders)[](#a-refresher-on-autoencoders)

An [Autoencoder](https://www.deeplearningbook.org/contents/autoencoders.html) is a neural network designed to replicate its input as its output. For instance, an autoencoder can convert a handwritten digit image into a compact, lower-dimensional representation known as a **latent representation**, then reconstruct the original image. Through this process, it **learns** to compress data efficiently while minimizing errors in reconstructing an image. [Variational Autoencoders (VAEs)](https://arxiv.org/pdf/1906.02691), on the other hand, encode data into a continuous, probabilistic latent space rather than a fixed, discrete representation as with traditional autoencoders. This allows for the generation of new, diverse data samples and smooth interpolation between them, critical for tasks like image and video generation.

### [A Refresher on Causal Convolutions](#a-refresher-on-causal-convolutions)[](#a-refresher-on-causal-convolutions)

[Causal convolutions](https://paperswithcode.com/method/causal-convolution) are a type of convolution specifically designed for temporal data, ensuring that the model’s predictions at any given timestep t are only dependent on past timesteps (t-1, t-2, …) and not on any future timesteps (t+1, t+2, …).

![Standard vs Causal Convolution](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/standard%20vs%20causal%20convolution.png) [(Source)](https://www.researchgate.net/publication/350311379_MoViNets_Mobile_Video_Networks_for_Efficient_Video_Recognition) Standard (left) vs. Causal (right) Convolutions

Causal convolutions are applied in different dimensions to various data types.

-   1D: Audio
-   2D: Image
-   3D: Video

### [Wan-VAE: A 3D Causal Variational Autoencoder](#wan-vae-a-3d-causal-variational-autoencoder)[](#wan-vae-a-3d-causal-variational-autoencoder)

A 3D Causal Variational Autoencoder, as implemented with Wan 2.1, is an advanced type of VAE that incorporates 3D causal convolutions, allowing it to handle **both spatial and temporal dimensions** in **video sequences**.

This novel 3D causal VAE architecture, termed **Wan-VAE**, can efficiently encode and decode 1080P videos of unlimited length while preserving historical temporal information, making it suitable for video generation tasks.

![WAN-VAE](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/wan%20vae.png)

### [Feature Cache and Chunking](#feature-cache-and-chunking)[](#feature-cache-and-chunking)

Processing long videos in one pass can lead to GPU memory overflow due to high-resolution frame data and temporal dependencies. Thus, the causal convolution module has a feature cache mechanism to provide historical data without retaining full video in memory. Here, video sequence frames are structured in a “1 + T” input format (1 initial frame + T subsequent frames), dividing the video into “1 + T/4” chunks.

For example: A 17-frame video (T=16) becomes 1 + 16/4 = 5 chunks.

Each encoding/decoding operation processes a single video chunk at a time, which corresponds to a single latent representation. To reduce the risk of GPU memory overflow, the number of frames in each processing chunk is limited to a maximum of 4. This frame limit is determined by the temporal compression ratio, which measures the compression of the time dimension.

### [Text-to-Video (T2V) Architecture](#text-to-video-t2v-architecture)[](#text-to-video-t2v-architecture)

The T2V models generate videos from text prompts.

Component

Description

Diffusion Transformers (DiT) + Flow Matching ![dit](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/dit.png)

Wan AI 2.1 is built on the principles of mainstream Diffusion Transformers, incorporating the Flow Matching framework to achieve advanced capabilities. Diffusion Transformers (DiTs) specifically refer to transformer architectures applied to diffusion models, which are models that generate data by adding and then removing noise from training data. [Flow Matching](https://arxiv.org/pdf/2210.02747) is where a neural network is directly trained to predict smooth, continuous transformations between simple and complex data distributions. This results in more stable training, faster inference, and improved performance compared to conventional diffusion-based approaches.

T5 Encoder and Cross-Attention for Text Processing ![T5 and cross attention](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/cross%20attention.png)

Wan2.1 employs the [T5 (Text-To-Text Transfer Transformer)](https://huggingface.co/docs/transformers/model_doc/t5) Encoder ([UMT5](https://huggingface.co/docs/transformers/v4.34.0/model_doc/umt5)) to embed text into the vision model. Within each transformer, a [cross-attention](https://medium.com/@sachinsoni600517/cross-attention-in-transformer-f37ce7129d78) mechanism is utilized. This attention variant enhances the model’s ability to process multilingual text inputs (English and Chinese) and align the text prompts with visual outputs.

Time Embeddings ![Time embeddings](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/timestep.png)

Time embeddings in Wan 2.1 serve as numerical representations that capture temporal patterns in data. These embeddings are processed through a shared MLP (consisting of a Linear layer and SiLU activation) used across all transformer blocks. The shared MLP reduces parameter count and boosts efficiency, while individual transformer blocks develop unique biases for specialized processing. This dual approach allows for both parameter efficiency and specialized processing, as each block can focus on different aspects of the input data, enhancing the model’s overall performance.

### [Image-2-Video (I2V) Architecture](#image-2-video-i2v-architecture)[](#image-2-video-i2v-architecture)

The I2V models generate videos from images using text prompts.

Component

Description

**Condition Image**

Video synthesis is controlled with a condition image as the first frame

**Guidance Frames**

Frames filled with zeros (guidance frames) are concatenated with the previously generated condition image along the temporal axis

**Condition Latent Representation**

A 3D VAE is used to compress the guidance frames into a condition latent representation

**Binary Mask**

A binary mask is added (1 for preserved frames, 0 for frames to generate). This mask, spatially aligned with the condition latent representation, extends temporally to match the target video’s length

**Mask Rearrangement**

The binary mask is then reshaped to align with the VAE’s temporal stride, ensuring seamless integration with the latent representation

**DiT Model Input**

The noise latent representation, condition latent representation, and the rearranged binary mask are combined by concatenating them along the channel axis. This combined input is then fed into the DiT model

**Channel Projection**

Due to the increased channel count compared to T2V models, a supplementary projection layer, initialized with zeros, is implemented to adapt the input for the I2V DiT model

**CLIP Image Encoder**

A CLIP image encoder extracts feature representations from the condition image, capturing its visual essence

**Global Context MLP**

These extracted features are projected by a three-layer MLP, generating a global context that encapsulates the image’s overall information

**Decoupled Cross-Attention**

This global context is then injected into the DiT model via decoupled cross-attention, allowing the model to leverage the condition image’s features throughout the video generation process

[Implementation](#implementation)[](#implementation)
----------------------------------------------------

Wan 2.1 offers flexible implementation [options](https://github.com/Wan-Video/Wan2.1?tab=readme-ov-file#quickstart). In this tutorial, we’ll utilize Comfy UI to showcase a seamless way to run the Wan 2.1 I2V model. Before following along with this tutorial, set up a [GPU Droplet](/products/gpu-droplets) and find a picture you’d like to convert into a video.

For optimal performance, we recommend selecting an “AI/ML Ready” OS and utilizing a single NVIDIA H100 GPU for this project. ![choose an image - ai/ml](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/ai-ml.png)

#### Step 0: Install Python and Pip

```
apt install python3-pip
```

#### Step 1: Install ComfyUI

```
pip install comfy-cli
comfy install
```

Select nvidia when prompted “What GPU do you have?”

#### Step 2: Download the necessary models

```
cd comfy/ComfyUI/models
wget -P diffusion_models https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/diffusion_models/wan2.1_i2v_480p_14B_fp8_e4m3fn.safetensors
wget -P text_encoders https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/text_encoders/umt5_xxl_fp8_e4m3fn_scaled.safetensors
wget -P clip_vision https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/clip_vision/clip_vision_h.safetensors
wget -P vae https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/vae/wan_2.1_vae.safetensors
```

#### Step 3: Launch ComfyUI

```
comfy launch
```

You’ll see a URL in the console output. You’ll need this URL in a later step to access the GUI. ![gui link](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/gui%20link.png)

#### Step 4: Open VSCode

In VSCode, click on “Connect to…” in the Start menu. ![connect](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/connect%20to.png)

Choose “Connect to Host…”. ![host](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/host.png)

#### Step 5: Connect to your GPU Droplet

Click “Add New SSH Host…” and enter the SSH command to connect to your droplet. This command is usually in the format ssh root@\[your\_droplet\_ip\_address\]. Press Enter to confirm, and a new VSCode window will open, connected to your droplet.

You can find your droplet’s IP address on the GPU droplet page. ![gpu droplet IP](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/ip.png)

#### Step 6: Access the ComfyUI GUI

In the new VSCode window connected to your droplet, type >sim and select “Simple Browser: Show”. ![show simple browser](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/show%20simple.png)

![paste simple browser](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/simple%20browser.png)

Copy the ComfyUI GUI URL from your web console (from Step 3) and paste it into the Simple Browser. Press Enter, and the ComfyUI GUI will open. ![GUI window](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/gui%20window.png)

#### Step 7: Update the ComfyUI Manager

Click the Manager button in the top right corner. In the menu that pops up, click Update ComfyUI. ![update manager](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/update%20comfy%20ui.png) You’ll be prompted to restart ComfyUI. Click “Restart” and refresh your browser if needed.

#### Step 8: Load a Workflow

Download the [workflow](https://comfyanonymous.github.io/ComfyUI_examples/wan/) of your choice in Json format (here, we’re using the I2V workflow). ![workflow](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/Workflow.png)

#### Step 9: Install Missing Nodes

If you’re working with a workflow that requires additional nodes, you might encounter a “Missing Node Types” error. Go to “Manager” > “Install missing custom nodes” and install the latest verisions of the required nodes. ![missing nodes](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/install%20missing%20custom%20nodes.png) You’ll be prompted to restart ComfyUI. Click “Restart” and refresh your browser if needed.

#### Step 10: Upload an Image

![upload image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/upload%20image.png)

#### Step 11: Add Prompts

![prompt](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/prompts.png)

**Positive Prompt vs. Negative Prompt** A “positive prompt” tells a model what to include in its generated output, essentially guiding it towards a specific desired element, while a “negative prompt” instructs the model what to exclude or avoid, acting as a filter to refine the content by removing unwanted aspects.

We will be using the following prompts to get our character to wave: Positive Prompt _“A portrait of a seated man, his gaze engaging the viewer with a gentle smile. One hand rests on a wide-brimmed hat in his lap, while the other lifts in a gesture of greeting.”_

Negative Prompt _“No blurry face, no distorted hands, no extra limbs, no missing limbs, no floating hat”_

#### Step 12: Run the Workflow

To run the workflow, select Queue. If you run into errors, ensure the correct files are passed into the nodes. ![queue](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/queue.png)

Would you look at that - we got our character to wave at us. ![wave](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wan2.1/wave.png)

Feel free to play around with the different parameters to see how performance is altered.

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

Great work! In this tutorial, we explored the model architecture of Wan 2.1, a cutting-edge collection of video generative models. We also successfully implemented Wan 2.1’s image-to-video model using ComfyUI. This achievement underscores the rapid advancement of open-source video generation models, foreshadowing a future where AI-generated video becomes an integral tool in various industries, including media production, scientific research, and digital prototyping.

[References](#references)[](#references)
----------------------------------------

[https://www.wan-ai.org/about](https://www.wan-ai.org/about)

[https://github.com/Wan-Video/Wan2.1](https://github.com/Wan-Video/Wan2.1)

[https://stable-diffusion-art.com/wan-2-1/](https://stable-diffusion-art.com/wan-2-1/)

#### Additional Resources

[Flow Matching for Generative Modeling (Paper Explained)](https://www.youtube.com/watch?v=7NNxK3CqaDk)

[\[2210.02747\] Flow Matching for Generative Modeling](https://arxiv.org/abs/2210.02747)

#### [Source](https://www.digitalocean.com/community/tutorials/wan-video-foundation-models)

<br/>
---
