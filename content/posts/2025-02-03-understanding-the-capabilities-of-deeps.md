---
title: "Understanding the Capabilities of DeepSeek R1 Large Language Models"
date: 2025-02-03T14:25:49.000Z
draft: false
type: posts
categories: 
- ai-ml,gpu
---
# Understanding the Capabilities of DeepSeek R1 Large Language Models

<br/>

<br/>
DeepSeek R1 has, for good reason, taken the AI/ML community by storm these past weeks, and has even in fact spread beyond to the wider world with major effects on both the economy and politics. This is largely because of the model suite’s open-source nature & incredibly low training price, which has shown the greater community that training SOTA AI models my not require nearly as much capital or proprietary research as previously thought.

In the first part of this series, we introduced [DeepSeek R1 and showed how to run the model using Ollama](/community/tutorials/deepseek-r1-gpu-droplets). In this follow up, we will begin with a deeper dive into what actually makes R1 so special. We will focus on analyzing model’s unique Reinforcement Learning (RL) paradigm to see how reasoning capabilities of LLMs can be incentivized purely through RL, and, afterwards, discuss how the distillation of these techniques to other models allows us to share these capabilites with existing releases. We will conclude with a short demonstration on how to setup and run DeepSeek R1 models with [GPU Droplets](/products/gpu-droplets) using [1-Click Model GPU Droplets](https://marketplace.digitalocean.com/apps/deepseek-r1-671b-8x).

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

-   Deep Learning: this article will cover intermediate to advanced topics related to neural network training and reinforcement learning
-   DigitalOcean account: We will specifically make use of DigitalOcean’s HuggingFace 1-Click Model GPU Droplets to test R1

[DeepSeek R1 Overview](#deepseek-r1-overview)[](#deepseek-r1-overview)
----------------------------------------------------------------------

The goal of the DeepSeek R1 research project was to recreate the effective reasoning capabilities shown by powerful reasoning models, namely OpenAI’s [O1](https://openai.com/o1/). To achieve this, they sought to improve their existing work, [DeepSeek-v3-Base](https://huggingface.co/deepseek-ai/DeepSeek-V3-Base), using pure reinforcement learning. This lead to the emergence of DeepSeek R1 Zero, which exhibits super performance on reasoning benchmarks, but lacks human interpretability and showed some unusual behaviors like language mixing.

To ameliorate these problems, they proposed DeepSeek R1, which incorporates a small amount of cold-start data and a multi-stage training pipeline. R1 achieved SOTA LLM readibility and utility by fine-tuning the DeepSeek-v3-Base model on thousands of cold-start data examples, then performing another round of Reinforcement Learning, followed by performing supervised fine-tuning on a reasoning dataset, and finally finishing with a final round of Reinforcement Learning. They then distilled the technique to other models by supervised fine-tuning them on data collected from R1.

Follow along for a deeper dive into these stages of development, and a discussion for how these improved the model iteratively to reach the capabilities of DeepSeek R1.

### [Training DeepSeek R1 Zero](#training-deepseek-r1-zero)[](#training-deepseek-r1-zero)

To create DeepSeek R1 Zero, the baseline model from which R1 was developed, the researchers applied RL directly to the base model without any SFT data. The chosen RL paradigm they selected is called Group Relative Policy Optimization (GRPO). This process was adapted from the [DeepSeekMath](https://arxiv.org/pdf/2402.03300) paper.

GRPO is similar to familiar, other RL systems, but differs in one significant way: it does not use a critic model. Instead, GRPO estimates the baseline from group scores instead. The reward modeling has two rules for this system that each rewards accuracy and format adherence to a template. The reward then acts as the source of the training signal, which then is used to modify the optimization direction of RL. This rule based system allows the RL process to iteratively modify and improve the model.

![template for RL training](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/James/Screenshot%202025-02-03%20at%2011.40.13%E2%80%AFAM.png)

The training template itself is a simple writing format that guides the base model to adhere to our specified instructions, as shown above. The model measures the responses to the adjusted “prompt” for each step of RL. “This is a noteworthy achievement, as it underscores the model’s ability to learn and generalize effectively through RL alone” ([Source](https://arxiv.org/pdf/2501.12948)).

This self evolution of the model leads it to develop its powerful reasoning capabilities, including self-reflection and consideration of alternative approaches. This is further enhanced by a moment during training the research team calls the model’s “Aha moment”. “During this phase, DeepSeek-R1-Zero learns to allocate more thinking time to a problem by reevaluating its initial approach. This behavior is not only a testament to the model’s growing reasoning abilities but also a captivating example of how reinforcement learning can lead to unexpected and sophisticated outcomes” ([Source](https://arxiv.org/pdf/2501.12948)).

DeepSeek R1 Zero performed extremely well across benchmarks, but suffered strongly in terms of readibility and utility compared to proper, human-adapted LLMs. The research team thus proposed DeepSeek R1 to better enhance the model for human level tasks.

### [From DeepSeek R1 Zero to DeepSeek R1](#from-deepseek-r1-zero-to-deepseek-r1)[](#from-deepseek-r1-zero-to-deepseek-r1)

To go from the relatively untamed DeepSeek R1 Zero to the much more functional DeepSeek R1, the researchers introduced several training stages.

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/James/Screenshot%202025-02-03%20at%2011.48.29%E2%80%AFAM.png)

To start, DeepSeek-v3-Base was fine-tuned on thousands of cold-start data pieces before initiating the same RL paradigm used for DeepSeek R1 Zero with an additional reward for consistent language in outputs. In practice, this phase works to enhance the model’s reasoning capabilities, particularly in reasoning-intensive tasks such as coding, mathematics, science, and logic reasoning, which involve well-defined problems with clear solutions ([Source](https://arxiv.org/pdf/2501.12948)).

When this RL stage completes, they use the resultant model to collect new data for supervised fine-tuning. “Unlike the initial cold-start data, which primarily focuses on reasoning, this stage incorporates data from other domains to enhance the model’s capabilities in writing, role-playing, and other general-purpose tasks” ([Source](https://arxiv.org/pdf/2501.12948)).

![RL training R1 zero](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/James/Screenshot%202025-02-03%20at%2011.52.50%E2%80%AFAM.png)

Next, a second RL stage is implemented to improve the model’s “helpfulness and harmlessness while simultaneously refining its reasoning capabilities” ([Source](https://arxiv.org/pdf/2501.12948)). By training the model further on diverse prompt distributions with reward signals, they are able to train a model that excels in reasoning while prioritizing helpfulness and harmlessness. This helps with the models’ “human-like” responsiveness. This helps the model to evolve the incredible reasoning capabilities it is known for. Over time, this process helps the model develop its characteristic long chains of thought and reasoning.

### [DeepSeek R1 Capabilities](#deepseek-r1-capabilities)[](#deepseek-r1-capabilities)

![metrics for R1 capabilities](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/James/Screenshot%202025-02-03%20at%2012.07.44%E2%80%AFPM.png)

Across the board, R1 demonstrates state of the art performance on reasoning benchmarks. On certain tasks, such as math, it even has shown to outperform the metrics released for O1. Overall, there is extremely high performance on stem related questions as well, which is primarily attributed to the large-scale reinforcement learning. In addition to STEM subjects, the model is highly proficient at question answering, instruction tasks, and complex reasoning. The authors argue that these improvements and enhanced capabilities are owed to the evolution of the models Chain of Thought processing through Reinforcement Learning. The long Chain of Thought data used throughout reinforcement learning and fine-tuning to encourage the model to deliver longer, more introspective outputs.

### [DeepSeek R1 Distilled models](#deepseek-r1-distilled-models)[](#deepseek-r1-distilled-models)

![R1 distilled models evaluation](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/James/Screenshot%202025-02-03%20at%202.11.49%E2%80%AFPM.png)

To extend the capabilities of DeepSeek R1 to smaller models, the authors collected 800000 samples from DeepSeek R1 and used those to fine-tune models like QWEN and LLAMA. They found that this relatively straight-forward distillation technique allows for the transfer of the R1 reasoning capabilities to these new models with a high-degree of success. They did this without any additional RL, showcasing the power of the original models responses to do model distillation.

[Launching DeepSeek R1 on GPU Droplets](#launching-deepseek-r1-on-gpu-droplets)[](#launching-deepseek-r1-on-gpu-droplets)
-------------------------------------------------------------------------------------------------------------------------

Launching DeepSeek R1 on [GPU Droplets](/products/gpu-droplets) is very straightforward if you already have a DigitalOcean account. Be sure to sign in before proceeding further.

![how to launch a DeepSeek 1-click model](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/James/gpu%20select.gif)

We provide access to R1 as a 1-Click Model GPU Droplet. To launch it, simply open up the [GPU Droplet console](https://cloud.digitalocean.com/gpus/new), navigate to the “[1-Click Models](/products/ai-ml/1-click-models)” tab in the template selection window, and start up the machine!

From there, the model will be accessible by following the HuggingFace or OpenAI methologies for communicating for the model. Use the following script to interact with your model with Python code.

```
import os
from huggingface_hub import InferenceClient

client = InferenceClient(base_url="http://localhost:8080", api_key=os.getenv("BEARER_TOKEN"))

chat_completion = client.chat.completions.create(
    messages=[
        {"role":"user","content":"What is Deep Learning?"},
    ],
    temperature=0.7,
    top_p=0.95,
    max_tokens=128,
)
## or use OpenAI formatting
#import os
#from openai import OpenAI
#
#client = OpenAI(base_url="http://localhost:8080/v1/", api_key=os.getenv("BEARER_TOKEN"))
#
#chat_completion = client.chat.completions.create(
#    model="tgi",
#    messages=[
#        {"role": "system", "content": "You are a helpful assistant."},
#        {"role": "user", "content": "What is Deep Learning?"},
#    ],
#    temperature=0.7,
#    top_p=0.95,
#    max_tokens=128,
#)
```

Alternatively, we have created a custom personal assistant that works on the same system. We recommend using the personal assistant for these tasks, as it abstracts much of the complication of directly interacting with the model by putting everything in a nice GUI window. To learn more about using the personal assistant script, please check out this [tutorial](/community/tutorials/1click-model-personal-assistant).

[Closing Thoughts](#closing-thoughts)[](#closing-thoughts)
----------------------------------------------------------

In conclusion, R1 is an incredible step forward for the LLM development community. Their process promises to save millions of dollars on training costs while offering comparable or even better performance than state of the art closed source models. We will be watching DeepSeek closely to see how they continue to grow as their model gains international recognition.

#### [Source](https://www.digitalocean.com/community/tutorials/deepseek-r1-large-language-model-capabilities)

<br/>
---
