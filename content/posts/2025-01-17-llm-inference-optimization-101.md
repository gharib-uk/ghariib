---
title: "LLM Inference Optimization 101"
date: 2025-01-17T12:31:44.000Z
draft: false
type: posts
categories: 
- ai-ml
---
# LLM Inference Optimization 101

<br/>

<br/>
[Fast inference makes the world go brrr](#fast-inference-makes-the-world-go-brrr)[](#fast-inference-makes-the-world-go-brrr)
----------------------------------------------------------------------------------------------------------------------------

Large Language Models (LLMs) generate coherent, natural language responses, effectively automating a multitude of tasks that were previously exclusive to humans. As many key players in the field, such as [Jensen Huang](https://youtu.be/k82RwXqZHY8?t=412) and [Ilya Sutskever](https://youtu.be/YD-9NG1Ke5Y?t=853), have recently alluded to, we’re in an era of agentic AI. This new paradigm seeks to revolutionize various aspects of our lives, from personalized medicine and education to intelligent assistants, and beyond.

However, it is important to be aware that while these models are getting increasingly powerful, widespread adoption is hindered by the massive costs to run them, frustrating wait times that render certain real-world applications impractical, as well as, of course, their growing carbon footprint. To reap the benefits of this technology, while mitigating cost and power consumption, it is critical that we continue to optimize every aspect of LLM inference.

The goal of this article is to give readers an overview of current ways in which researchers and deep learning practitioners are optimizing LLM inference.

[What is LLM inference?](#what-is-llm-inference)[](#what-is-llm-inference)
--------------------------------------------------------------------------

Similar to how one uses what they learned to solve a new problem, inference is when a trained AI model uses patterns detected during training to infer and make predictions on new data. This inference process is what enables LLMs to perform tasks like text completion, translation, summarization, and conversation.

[Text Generation Inference with 1-click Models](#text-generation-inference-with-1-click-models)[](#text-generation-inference-with-1-click-models)
-------------------------------------------------------------------------------------------------------------------------------------------------

DigitalOcean has partnered with HuggingFace to offer [1-click models](/products/ai-ml/1-click-models). This allows for the integration of GPU Droplets with state-of-the-art open-source LLMs in [Text Generation Inference](https://huggingface.co/docs/text-generation-inference/index) (TGI)-optimized container applications. This means many of the inference optimizations covered in this article (ex: tensor parallelism, quantization, flashattention, paged attention) are already taken care of and maintained by HuggingFace. For information on how to use these 1-click models, check out our article [Getting Started with LLMs](/community/tutorials/llm-for-social-media-analytics).

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

While this article includes some introductory deep learning concepts, many topics discussed are relatively advanced. Those determined to better understand inference optimization are encouraged to explore the links scattered throughout the article and in the references section.

It is advised that readers have an understanding of [neural network fundamentals](https://www.youtube.com/watch?v=aircAruvnKk), [the attention mechanism](https://www.youtube.com/watch?v=eMlx5fFNoYc), [the transformer](https://www.youtube.com/watch?v=bCz4OMemCcA), and [data types](https://newsletter.maartengrootendorst.com/i/145531349/common-data-types) before proceeding.

It would also help to be knowledgeable about the [GPU memory hierarchy](/community/tutorials/the-hidden-bottleneck-how-gpu-memory-hierarchy-affects-your-computing-experience).

The article,[Introduction to GPU Performance Optimization](/community/tutorials/an-introduction-to-gpu-optimization), provides context on how GPUs can be programmed to accelerate neural network training and inference. It also explains key terms such as latency and throughput.

[The Two Phases of LLM Inference](#the-two-phases-of-llm-inference)[](#the-two-phases-of-llm-inference)
-------------------------------------------------------------------------------------------------------

_The prefill phase can be likened to reading an entire document at once and processing all the words simultaneously to write the first word whereas the decode phase can be compared to continuing to write this response word by word, where the choice of each word depends on what was written before._

LLM inference can be divided into two phases: **prefill** and **decode**. These stages are separated due to the different computational requirements of each stage. While prefill, a highly-parallelized matrix-matrix operation that saturates GPU utilization, is compute-bound, decode, a matrix-vector operation that underutilizes the GPU compute capability, is memory-bound.

![KV caching](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/inferenceoptimization101/kvcaching.png)

Let’s explore why prefill is compute-bound and decode is memory-bound.

### [Prefill](#prefill)[](#prefill)

In the prefill stage, the LLM processes the entire input prompt at once to generate the first response token. This involves performing a full forward pass through the transformer layers for every token in the prompt simultaneously. While memory access is needed during prefill, the computational work of processing the tokens in parallel dominate the performance profile.

### [Decode](#decode)[](#decode)

In the decode stage, text is generated **autoregressively** where the next token is predicted one at time given all previous tokens. The decoding process is memory-bound due to its need to repeatedly access historical context. For each new token generated, the model must load the attention cache (key/value states, AKA KV cache) from all previous tokens, requiring frequent memory accesses that become more intensive as the sequence grows longer.

Metrics can be used to assess performance and identify areas of potential bottlenecks during these two inference stages.

[Metrics](#metrics)[](#metrics)
-------------------------------

Metric

Definition

Why do we care?

Time-to-First-Token ([TTFT](https://docs.nvidia.com/nim/benchmarking/llm/latest/metrics.html#time-to-first-token-ttft))

Time to process the prompt and generate the first token. TTFT tells us how long prefill took.

The longer the prompt, the longer the TTFT as the attention mechanism needs the whole input sequence to compute the KV cache. Inference optimization seeks to minimize TTFT.

Inter-token Latency ([ITL](https://docs.nvidia.com/nim/benchmarking/llm/latest/metrics.html#inter-token-latency-itl)) AKA Time per Output Token

Average time between consecutive tokens. ITL tells us the rate at which decoding (token generation) occurs.

Consistent ITLs are ideal as they are indicative of efficient memory management, high GPU memory bandwidth, and well-optimized attention computation.

[Optimizing Prefill and Decode](#optimizing-prefill-and-decode)[](#optimizing-prefill-and-decode)
-------------------------------------------------------------------------------------------------

### [Speculative Decoding](#speculative-decoding)[](#speculative-decoding)

[Speculative Decoding](https://research.google/blog/looking-back-at-speculative-decoding/) uses a smaller, faster model to generate multiple tokens simultaneously, and then verifies them with the larger target model. Since the generated samples come from exactly the same probability distribution as those produced by naïve decoding, speculative decoding results in speed-ups in inference, while maintaining the same quality of responses.

### [Chunked Prefills and Decode-Maximal Batching](#chunked-prefills-and-decode-maximal-batching)[](#chunked-prefills-and-decode-maximal-batching)

[SARATHI](https://arxiv.org/pdf/2308.16369) shows how chunked prefills can enable the division of large prefills into manageable chunks, which can then be batched with decode requests (decode-maximal batching) for more efficient processing.

[Batching](#batching)[](#batching)
----------------------------------

Batching groups inference requests together, with larger batch sizes corresponding to higher throughput. However, batch sizes can only be increased up to a certain extent due to limited GPU on-chip memory.

### [Batch Size](#batch-size)[](#batch-size)

To achieve maximum utilization of the hardware, one can try to find [the critical ratio](https://youtu.be/HTcnp9NEHGY?t=394) where there’s a balance between two key limiting factors:

-   The time needed to transfer weights between memory and compute units (limited by memory bandwidth)
-   The time required for actual computational operations (limited by FLOPS)

While these two times are equal, the batch size can be increased without incurring any performance penalty. Beyond this point, increasing batch size would create bottlenecks in either memory transfer or computation. To determine an optimal batch size, profiling is important.

[KV cache management](https://medium.com/@plienhar/llm-inference-series-4-kv-caching-a-deeper-look-4ba9a77746c8) plays a critical role in determining the maximum batch size and improving inference. Thus, **the rest of the article will focus on managing the KV cache.**

[KV Cache Management](#kv-cache-management)[](#kv-cache-management)
-------------------------------------------------------------------

When looking at how memory is allocated in the GPU during serving, the model weights remain fixed and the activations only utilize a fraction of the GPU’s memory resources compared to the KV cache. Therefore, freeing up space for the KV cache is critical. This can be achieved by reducing the model weight memory footprint through quantization, reducing the KV cache memory footprint with modified architectures and attention variants, as well as pooling memory from multiple GPUs with parallelism.

[Quantization](#quantization)[](#quantization)
----------------------------------------------

[Quantization](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-quantization) reduces the number of bits needed to store the model’s parameters (ex: weights, activations, and gradients). This technique reduces inference latency by exchanging memory for accuracy.

[Attention and its variants](#attention-and-its-variants)[](#attention-and-its-variants)
----------------------------------------------------------------------------------------

Review of Queries, Keys, and Values:

**Queries:** Represent the context or question.

**Keys:** Represent the information being attended to.

**Values:** Represent the information being retrieved.

Attention weights are computed by comparing queries with keys, and then used to weight values, producing the final output representation.

Query (Prompt) → Attention Weights → Relevant Information (Values)

Attention Variant

Description

Visual Representation

Scaled Dot-Product Attention ![sdpa](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/inferenceoptimization101/sdpa.png)

[Scaled Dot-Product Attention (SDPA)](https://arxiv.org/pdf/1706.03762) is a key component of the [Transformer](https://jalammar.github.io/illustrated-transformer/) architecture, and is a type of self-attention mechanism that allows the model to attend to different parts of the input sequence simultaneously and weigh their relevance.

![attention calculation](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/inferenceoptimization101/attention%20calculation.png)

Multi-Head Attention ![mha](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/inferenceoptimization101/MHA.png)

In [Multi-Head Attention (MHA)](https://arxiv.org/pdf/1706.03762), multiple SDPA heads operate in parallel resulting in richer relationships between different aspects of the input sequence to be captured.

![multi-head attention](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/inferenceoptimization101/MHA2.png)

Multi-Query Attention

[Multi-Query Attention (MQA)](https://arxiv.org/pdf/1911.02150) is a memory-efficient refinement to MHA where one key-value head is shared across multiple attention heads. **MQA reduces the size of KV-cache memory** allowing space for larger batch sizes. While MQA results in faster decoder inference than MHA, there may be quality degradation.

![MQA](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/inferenceoptimization101/MQA.png)

Grouped-Query Attention

[Grouped-Query Attention (GQA)](https://arxiv.org/pdf/2305.13245v2) modifies MQA to have groupings of queries for multiple key-value heads. The number of key-value heads is more than one as in MQA, but less than the number of queries as in MHA. **By striking a balance between MQA and MHA, GQA achieves comparable quality to MHA, while also achieving speed similar to MQA.**

![GQA](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/inferenceoptimization101/GQA.png)

![SWA](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Mel/inferenceoptimization101/SWA.png)

[Sliding Window Attention (SWA)](https://paperswithcode.com/method/sliding-window-attention) or local attention, restricts attention to a fixed-size window that slides over the sequence. While SWA is not efficient to scale to long inputs, [Character AI](https://research.character.ai/optimizing-inference/?ref=blog.character.ai#:~:text=Hybrid%20Attention%20Horizons) saw that speed and quality wasn’t impacted with long sequences when interleaving SWA and global attention, with adjacent global attention layers sharing a KV cache [(cross-layer attention)](https://arxiv.org/pdf/2405.12981).

#### Local Attention vs. Global Attention

Local and global attention mechanisms differ in key aspects. Local attention uses less computation (O(n \* w)) and memory by focusing on token windows, enabling faster inference especially for long sequences, but may miss long-range dependencies. Global attention, while computationally more expensive (O(n^2)) and memory-intensive due to processing all token pairs, is able to better capture full context and long-range dependencies at the cost of slower inference speed.

### [Paged Attention](#paged-attention)[](#paged-attention)

Inspired by virtual memory allocation, [PagedAttention](https://arxiv.org/pdf/2309.06180) proposed a framework for optimizing KV cache that takes the variation of the number of tokens across requests into consideration.

### [FlashAttention](#flashattention)[](#flashattention)

There are three variations of FlashAttention, with [FlashAttention-3](https://arxiv.org/pdf/2407.08608) being the latest release and optimized for Hopper GPUs. Each iteration of this algorithm takes a hardware-aware approach to make the attention computation as fast as possible. Past articles written on FlashAttention include: [Designing Hardware-Aware Algorithms: FlashAttention](/community/tutorials/flashattention) and [FlashAttention-2](/community/tutorials/flashattention2)

[Model Architectures: Dense Models vs. Mixture of Experts](#model-architectures-dense-models-vs-mixture-of-experts)[](#model-architectures-dense-models-vs-mixture-of-experts)
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Dense LLMs** are the standard where all parameters are actively engaged during inference.

**Mixture of Experts (MoE) LLMs** are composed of multiple specialized sub-networks with a routing mechanism. Because only relevant experts are activated for each input, improved parameter efficiency and faster inference than dense models is often observed.

[Parallelism](#parallelism)[](#parallelism)
-------------------------------------------

Larger models often require multiple GPUs to run effectively. There are a number of different parallelization strategies that allow for multi-GPU inference.

Parallelism Type

Partions

Description

Purpose

Data

Data

Splits different batches of data across devices.

Distribution of memory and computation for large datasets that wouldn’t fit on a single device

[Tensor](https://arxiv.org/pdf/1909.08053)

Weight Tensors

Splits tensors across multiple devices either row-wise or column-wise

Distribution of memory and computation for large tensors that wouldn’t fit on a single device

[Pipeline](https://arxiv.org/pdf/1811.06965)

Model Layers (vertically)

Splits different stages of the full model pipeline in parallel

Improves throughput by overlapping computation of different model stages

[Context](https://arxiv.org/pdf/2411.01783)

Input Sequences

Divides input sequences into segments across devices

Reduces memory bottleneck for long sequence inputs

[Expert](https://proceedings.mlr.press/v162/rajbhandari22a/rajbhandari22a.pdf)

MoE models

Splits experts, where each expert is a smaller model, across devices

Allows for larger models with improved performance by distributing computation across multiple experts

[Fully Sharded Data](https://engineering.fb.com/2021/07/15/open-source/fsdp/)

Data, model, optimizer, and gradients

Shards components across devices, processes data in parallel, and synchronizes after each training step. Parameters are fetched and reconstructed from shards as needed, used for computation, and then promptly discarded, reducing memory footprint.

Enables training of extremely large models that exceed the memory capacity of a single device by distributing both model parameters and activations.

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

It’s undeniable that inference is an exciting area of research and optimization. The field moves fast, and to keep up, inference needs to move faster. In addition to more agentic workflows, we’re seeing more dynamic inference strategies that allow models to “think longer” on harder problems. For example, reasoning models like OpenAI’s o1 model shows consistent performance improvements on challenging mathematical and programming tasks when more computational resources are devoted during inference.

Thanks so much for reading! This article is certainly not conclusive to all there is in inference optimization. Stay tuned for more exciting articles on this topic and adjacent ones.

[References and Other Excellent Resources](#references-and-other-excellent-resources)[](#references-and-other-excellent-resources)
----------------------------------------------------------------------------------------------------------------------------------

Blog posts:

[Mastering LLM Techniques: Inference Optimization | NVIDIA Technical Blog](https://developer.nvidia.com/blog/mastering-llm-techniques-inference-optimization/)

[LLM Inference at scale with TGI](https://huggingface.co/blog/martinigoyanes/llm-inference-at-scale-with-tgi)

[Looking back at speculative decoding](https://research.google/blog/looking-back-at-speculative-decoding/) (Google Research)

[LLM Inference Series: 4. KV caching, a deeper look | by Pierre Lienhart | Medium](https://medium.com/@plienhar/llm-inference-series-4-kv-caching-a-deeper-look-4ba9a77746c8)

[A Visual Guide to Quantization - by Maarten Grootendorst](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-quantization)

[Optimizing AI Inference at Character.AI](https://research.character.ai/optimizing-inference/)

[Optimizing AI Inference at Character.AI (Part Deux)](https://research.character.ai/optimizing-ai-inference-at-character-ai-part-deux/)

Papers:

[LLM-Inference-Bench: Inference Benchmarking of Large Language Models on AI Accelerators](https://arxiv.org/pdf/2411.00136)

[Efficient Memory Management for Large Language Model Serving with PagedAttention](https://arxiv.org/pdf/2309.06180)

[SARATHI: Efficient LLM Inference by Piggybacking Decodes with Chunked Prefills](https://arxiv.org/pdf/2308.16369)

[The LLama 3 Herd of Models](https://ai.meta.com/research/publications/the-llama-3-herd-of-models/)

[Context Parallelism for Scalable Million-Token Inference](https://arxiv.org/pdf/2411.01783)

Talks:

[Mastering LLM Inference Optimization From Theory to Cost Effective Deployment: Mark Moyou](https://www.youtube.com/watch?v=9tvJ_GYJA-o)

[NVIDIA CEO Jensen Huang Keynote at CES 2025](https://www.youtube.com/watch?v=k82RwXqZHY8)

[Building Machine Learning Systems for a Trillion Trillion Floating Point Operations :: Jane Street](https://www.janestreet.com/tech-talks/building-machine-learning-systems-for-a-trillion-trillion-floating-point-operations/)

[Dylan Patel - Inference Math, Simulation, and AI Megaclusters - Stanford CS 229S - Autumn 2024](https://youtu.be/hobvps-H38o?t=2461)

[How does batching work on modern GPUs?](https://www.youtube.com/watch?v=HTcnp9NEHGY)

GitHub Links:

[Sharan Chetlur -Nvidia/Presentation Slides - High Performance LLM Serving on Nvidia GPUs](https://github.com/mlops-discord/gpu-optimization-workshop/blob/main/Talk%202%20-%20High%20Performance%20LLM%20Serving%20on%20Nvidia%20GPUs%20-%20Sharan%20Chetlur%20-Nvidia/Presentation%20Slides%20-%20High%20Performance%20LLM%20Serving%20on%20Nvidia%20GPUs.pdf)

[GitHub - huggingface/search-and-learn: Recipes to scale inference-time compute of open models](https://github.com/huggingface/search-and-learn)

#### [Source](https://www.digitalocean.com/community/tutorials/llm-inference-optimization)

<br/>
---
