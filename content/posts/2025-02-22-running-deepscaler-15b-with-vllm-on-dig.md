---
title: "Running DeepScaleR-15B with vLLM on DigitalOcean"
date: 2025-02-22T00:41:07.000Z
draft: false
type: posts
categories: 
- ai-ml
---
# Running DeepScaleR-15B with vLLM on DigitalOcean

<br/>

<br/>
[Introduction](#introduction)[](#introduction)
----------------------------------------------

DeepSeek-R1, an open-source-model, achieved comparable performance to OpenA1’s o1 model on reasoning tasks. If this wasn’t already impressive, newer open-source models like [DeepScaleR-1.5B-Preview](https://huggingface.co/agentica-org/DeepScaleR-1.5B-Preview) by [Agentica](https://agentica-project.com/), part of the [Berkeley AI Research](https://bair.berkeley.edu/) and Sky Computing lab, surpass o1 performance in benchmarks like [AIME2024](https://artofproblemsolving.com/wiki/index.php/2024_AIME_I?srsltid=AfmBOopahQ34ZX1kHhIYxJIUBVUd-LcafHZlL2eQlv8bG1Mgve5juieZ). This level of performance on AIME2024 is especially significant because mathematical reasoning has traditionally been a weakness for language models.

Trained on 40,000 math problems over 3,800 A100 GPU hours, DeepScaleR uses a novel iterative context lengthening scheme. The training process involves curating a high-quality dataset, using an Outcome Reward Model (ORM) instead of a Process Reward Model (PRM), and progressively increasing the context window from 8K to 24K tokens. This approach allows the model to learn effective reasoning patterns at shorter contexts before scaling to longer ones, significantly improving efficiency. The authors have open-sourced their [dataset](https://huggingface.co/datasets/agentica-org/DeepScaleR-Preview-Dataset), [code](https://github.com/agentica-project/deepscaler), and [training logs](https://wandb.ai/mluo/deepscaler-1.5b) to further research in scaling intelligence with RL.

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

There are two sections of this article. (1) An overview of the model and (2) its implementation. The overview requires some familiarity with LLM training and evaluation. Knowledge of reinforcement learning algorithms like [Proximal Policy Optimization (PPO)](https://arxiv.org/pdf/1707.06347) machine learning concepts like loss functions would be helpful. The implementation, where we run the model on DigitalOcean’s GPU droplets, is very straightforward and requires minimal coding experience. Feel free to skip the overview section if you’re simply interested in running this model.

[Model Performance](#model-performance)[](#model-performance)
-------------------------------------------------------------

Here’s a quote outlining [DeepScaleR’s](https://huggingface.co/agentica-org/DeepScaleR-1.5B-Preview) performance from Agentica’s blog post, “[DeepScaleR: Surpassing O1-Preview with a 1.5B Model by Scaling RL](https://pretty-radio-b75.notion.site/DeepScaleR-Surpassing-O1-Preview-with-a-1-5B-Model-by-Scaling-RL-19681902c1468005bed8ca303013a4e2)”: _“Our results confirm this: RL scaling improved AIME accuracy from **28.9% to 43.1%**! These findings suggest that neither SFT nor RL alone is sufficient. Instead, by combining high-quality SFT distillation with RL scaling, we can truly unlock the reasoning potential of LLMs.”_

**What’s the significance of 43.1% Pass[@1](/community/users/1) accuracy on AIME2024 (+14.3% improvement over the base model)?**  
[AIME2024](https://artofproblemsolving.com/wiki/index.php/2024_AIME_I?srsltid=AfmBOopahQ34ZX1kHhIYxJIUBVUd-LcafHZlL2eQlv8bG1Mgve5juieZ) (American Invitational Mathematics Examination) is a highly regarded benchmark derived from a prestigious high school mathematics competition. You’ll see this benchmark come up a lot when evaluating reasoning models looking at problem solving abilities. A 43.1% Pass[@1](/community/users/1) accuracy means that in a single attempt, the model correctly solves 43.1% of the AIME problems it encounters. The 14.3% improvement over the base model (from 28.9% to 43.1%) demonstrates that **combining SFT distillation with RL scaling is a promising approach**.

[Training Recipe](#training-recipe)[](#training-recipe)
-------------------------------------------------------

### [Group Relative Policy Optimization (GRPO)](#group-relative-policy-optimization-grpo)[](#group-relative-policy-optimization-grpo)

[GRPO](https://arxiv.org/pdf/2402.03300) builds upon PPO (Proximal Policy Optimization), an extensively used reinforcement learning algorithm, and introduces **two significant changes.** The first key addition involves **normalizing the advantage function** across all samples generated from the same prompt. The advantage function quantifies **how much better a particular action is compared to the expected value.** By normalizing these advantages across samples from the same prompt, GRPO creates a **more consistent scale for comparing the relative benefits of different actions**, which is particularly valuable when handling multiple samples from the same starting point.

The second major addition **incorporates KL (Kullback-Leibler) divergence regularization on top of PPO’s surrogate loss function**. KL divergence measures the difference between two probability distributions. By adding this as a regularization term to PPO’s existing loss function, GRPO helps prevent the new policy from deviating from the old one (policy drift).

### [Reward Function](#reward-function)[](#reward-function)

The reward function involves a binary approach to evaluating responses where a reward value of 1 is assigned for answers that successfully pass both [LaTeX](https://www.latex-project.org/about/) and [Sympy](https://www.sympy.org/en/index.html) validation checks, indicating correct mathematical formatting and content.

Conversely, it assigns a reward value of 0 for any answers that are either incorrect or fail to meet the proper formatting requirements.The system deliberately excludes partial reward mechanisms (PRMs) and does not provide intermediate feedback during the evaluation process.

### [Iterative Context Lengthening](#iterative-context-lengthening)[](#iterative-context-lengthening)

Training large reasoning models using reinforcement learning **requires significant computational resources**. To address this challenge, an adaptive training strategy was used where they started with shorter contexts and gradually increased the length as the model’s performance improves. **This optimization reduces both the financial costs and overall training duration.**

[Implementation](#implementation)[](#implementation)
----------------------------------------------------

DeepScaleR can be served with high-performance inference systems such as vLLM, Hugging Face Text Generation Inference (TGI), SGLang, and TensorRT-LLM. In this tutorial, we will show you how you can run [DeepScaleR-1.5B](https://pretty-radio-b75.notion.site/DeepScaleR-Surpassing-O1-Preview-with-a-1-5B-Model-by-Scaling-RL-19681902c1468005bed8ca303013a4e2) with the [vLLM](https://docs.vllm.ai/en/latest/getting_started/quickstart.html) inference engine on DigitalOcean GPU Droplets.

### [Step 1: Set up a GPU Droplet in a Jupyter Notebook environment](#step-1-set-up-a-gpu-droplet-in-a-jupyter-notebook-environment)[](#step-1-set-up-a-gpu-droplet-in-a-jupyter-notebook-environment)

To set up a GPU Droplet in a Jupyter Notebook environment, follow this tutorial: [“Setting Up the GPU Droplet Environment for AI/ML Coding - Jupyter Labs”](/community/tutorials/jupyter-notebooks-with-gpu-droplets).

### [Step 2: Import the Model](#step-2-import-the-model)[](#step-2-import-the-model)

Initialize the language model by creating an LLM object, specifying the model path as “agentica-org/DeepScaleR-1.5B-Preview”.

```
from vllm import LLM, SamplingParams
llm = LLM(model= "agentica-org/DeepScaleR-1.5B-Preview")
```

### [Step 3: Format the Prompt and Sampling Parameters](#step-3-format-the-prompt-and-sampling-parameters)[](#step-3-format-the-prompt-and-sampling-parameters)

Define your input prompt (replace “\[enter prompt here\]” with your actual prompt text). Set the [sampling parameters](https://docs.vllm.ai/en/v0.6.4/dev/sampling_params.html) in a SamplingParams object, including arguments like temperature, top\_p, and max\_tokens.

```
prompt = [enter prompt here]
sampling_params = SamplingParams(temperature=0.8, top_p=0.95, max_tokens = 512)
```

### [Step 4: Generate Your Results](#step-4-generate-your-results)[](#step-4-generate-your-results)

To generate text using the llm.generate() function, start by calling the function and passing in the parameters we defined in the last step (prompt and sampling\_params). Then, iterate through each output produced by the generation process. From each output, extract the original prompt and the corresponding generated text. Finally, print both the prompt and the generated text for you to evaluate.

```
outputs = llm.generate(prompt,sampling_params)
for output in outputs:
  prompt = output.prompt
  generated_text = output.outputs[0].text
  print(f"Prompt:{prompt!r}, Generated text: {generated_text!r}")
```

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

And there you have it. We hope this article gives you the background you need to understand the innovation behind DeepScaleR and some exposure to the current paradigm of reasoning models. We encourage you to experiment with this reasoning model and others for your desired use-case.

Check out this tutorial for inspiration: [A Comprehensive Guide to Fine-Tuning Reasoning Models: Fine-Tuning DeepSeek-R1 on Medical CoT with DigitalOcean’s GPU Droplets](/community/tutorials/fine-tuning-deepseek-medical-cot)

[References](#references)[](#references)
----------------------------------------

[https://pretty-radio-b75.notion.site/DeepScaleR-Surpassing-O1-Preview-with-a-1-5B-Model-by-Scaling-RL-19681902c1468005bed8ca303013a4e2](https://pretty-radio-b75.notion.site/DeepScaleR-Surpassing-O1-Preview-with-a-1-5B-Model-by-Scaling-RL-19681902c1468005bed8ca303013a4e2)  
[agentica-org/DeepScaleR-1.5B-Preview · Hugging Face](https://huggingface.co/agentica-org/DeepScaleR-1.5B-Preview) [GitHub - agentica-project/deepscaler: Democratizing Reinforcement Learning for LLMs](https://github.com/agentica-project/deepscaler) [DeepSeekMath: Pushing the Limits of Mathematical Reasoning in Open Language Models](https://arxiv.org/pdf/2402.03300) [Quickstart — vLLM](https://docs.vllm.ai/en/latest/getting_started/quickstart.html)

#### [Source](https://www.digitalocean.com/community/tutorials/deepscaler)

<br/>
---
