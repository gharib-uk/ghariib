---
title: "A Comprehensive Guide to Fine-Tuning Reasoning Models Fine-Tuning DeepSeek-R1 on Medical CoT with DigitalOceans GPU Droplets"
date: 2025-02-18T13:52:11.000Z
draft: false
type: posts
categories: 
- ai-ml
---
# A Comprehensive Guide to Fine-Tuning Reasoning Models Fine-Tuning DeepSeek-R1 on Medical CoT with DigitalOceans GPU Droplets

<br/>

<br/>
[Introduction](#introduction)[](#introduction)
----------------------------------------------

Recent advances in Large Language Models (LLMs) have shown promise in systematic reasoning tasks, with open-source models like DeepSeek-R1 demonstrating impressive capabilities in breaking down complex problems into logical steps. By fine-tuning these reasoning-focused models for medical applications, we can create proof-of-concept AI assistants that could potentially support healthcare professionals in their clinical decision-making processes while maintaining transparent chains of reasoning. In this tutorial, we’ll explore how to leverage **DigitalOcean’s GPU Droplets** to fine-tune a [**distilled quantized version of DeepSeek-R1**](https://huggingface.co/unsloth/DeepSeek-R1-Distill-Llama-8B-bnb-4bit), transforming it into a specialized reasoning assistant that can help analyze patient cases, suggest potential diagnoses, and provide verified structured explanations for its recommendations.

Shoutout to this great [DataCamp tutorial](https://www.datacamp.com/tutorial/fine-tuning-deepseek-r1-reasoning-model) and the paper, [HuatuoGPT-o1, Towards Medical Complex Reasoning with LLMs](https://arxiv.org/pdf/2412.18925), for inspiring this tutorial.

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

Knowledge of these prerequisites will be helpful with following along with this tutorial:

-   Python and PyTorch
-   Deep Learning fundamentals (ex: neural networks, hyperparameters, etc.)
-   Experience working with Hugging Face models and the Transformers library

[When should we use Fine-Tuning?](#when-should-we-use-fine-tuning)[](#when-should-we-use-fine-tuning)
-----------------------------------------------------------------------------------------------------

Fine-tuning adapts a pre-trained model’s existing knowledge to perform specific tasks by training it further on a curated dataset. Fine-tuning shines in scenarios where **consistent formatting**, specific **tone** requirements, or **complex instruction following** are needed, as it can optimize the model’s behaviour for these particular use cases. This approach typically requires fewer computational resources and less time than training a model from scratch. Before proceeding with fine-tuning, however, it is good practice for developers to first consider the advantages of alternatives such as prompt engineering, Retrieval Augmented Generation (RAG), and even training a model from scratch.

Approach

When should we consider this?

Prompt Engineering

Prompt engineering involves crafting precise instructions to guide the model’s behaviour using existing capabilities. We have tutorials that **refine system prompts for specific use-cases** with DigitalOcean’s [1-click models](/products/ai-ml/1-click-models): [Getting Started with LLMs for Social Media Analytics](/community/tutorials/llm-for-social-media-analytics#part-2-prompt-engineering-for-social-media-analytics) & [How to Create an Email Newsletter Generator](/community/tutorials/newsletter-generator-with-1click-models#part-2-newsletter-generation-with-prompt-engineering)

Retrieval-Augmented Generation

In cases where the goal is to incorporate new or up-to-date information, Retrieval-Augmented Generation (RAG) is typically more appropriate. RAG allows the model to access external knowledge without modifying its underlying parameters.

Training From Scratch

Training a model from scratch can be beneficial in applications where model interpretability and explainability are desired. This approach gives you greater control over the model’s architecture, data, and decision-making process.

One can do combinations of different approaches such as fine-tuning and RAG. By combining fine-tuning to establish a robust baseline with RAG to handle dynamic updates, the system achieves both adaptability and efficiency without requiring constant re-training. It really all comes down to organizational resource constraints and desired performance.

**Monitoring whether outputs deliver to the standards of the intended utility and iterating/pivoting if not is absolutely critical.**

Once we know that fine-tuning is the approach we want to take, we need to assemble the necessary components.

[What do we need to Fine-Tune a Model?](#what-do-we-need-to-fine-tune-a-model)[](#what-do-we-need-to-fine-tune-a-model)
-----------------------------------------------------------------------------------------------------------------------

### [A pre-trained model](#a-pre-trained-model)[](#a-pre-trained-model)

A pre-trained model is a neural network that has already been trained on a large general-purpose corpus of data. Hugging Face has a plethora of [open-source models](https://huggingface.co/models) available for you to use.

**In this tutorial, we will be using a very popular reasoning model, DeepSeek-R1.** Reasoning models excel at intricate tasks like advanced problems in math or coding. We chose [“unsloth/DeepSeek-R1-Distill-Llama-8B-bnb-4bit](https://huggingface.co/unsloth/DeepSeek-R1-Distill-Llama-8B-bnb-4bit)” because it is distilled and pre-quantized, making it a more memory efficient and cost-effective model to perform experiments with. We were especially curious about its potential for complex tasks such as medical analysis. Note that using them for simpler tasks such as summarization or translation would be overkill due to the tendency reasoning models have towards being computationally expensive and verbose.

### [Dataset](#dataset)[](#dataset)

Hugging Face has a great selection of [datasets](https://huggingface.co/datasets). We will be using the [Medical O1 Reasoning Dataset](https://huggingface.co/datasets/FreedomIntelligence/medical-o1-reasoning-SFT). This dataset was generated with GPT-4o by searching for solutions to verifiable medical problems and validating them through a medical verifier.

This dataset will be used to perform **supervised fine-tuning (SFT)**, where models are trained on a dataset of instructions and responses. To minimize the difference between the generated answers and ground-truth responses, SFT adjusts the weights in the LLM.

### [GPUs](#gpus)[](#gpus)

GPUs aren’t always necessary to fine-tune a model. However, using a GPU (or multiple GPUs) can speed up the process significantly, especially for larger models or datasets like the ones used in this tutorial. In this article, we will show you how you can make use of [DigitalOcean GPU Droplets](/products/gpu-droplets).

[Tools and Frameworks](#tools-and-frameworks)[](#tools-and-frameworks)
----------------------------------------------------------------------

Before starting this tutorial, it is recommended to familiarize yourself with the following libraries and tools:

### [Unsloth](#unsloth)[](#unsloth)

[Unsloth](https://github.com/unslothai/unsloth) is all about making LLM training faster, with a particular focus on fine-tuning. The [FastLanguageModel class](https://github.com/unslothai/unsloth?tab=readme-ov-file#-documentation), part of the [Unsloth library](https://docs.unsloth.ai/), provides a simplified abstraction for fine-tuning LLMs. This class can handle loading the trained model weights, preprocessing input text, and executing inference to generate outputs.

### [Transformer Reinforcement Learning (TRL)](#transformer-reinforcement-learning-trl)[](#transformer-reinforcement-learning-trl)

The HuggingFace Library, TRL, is used to train transformer language models with Reinforcement Learning. This tutorial will utilize the [SFTTrainer](https://huggingface.co/docs/trl/en/sft_trainer) Class.

### [Transformers](#transformers)[](#transformers)

[Transformers](https://huggingface.co/docs/transformers/en/index) is also a HuggingFace Library. We will be using the [TrainingArguments](https://huggingface.co/docs/transformers/v4.48.2/en/main_classes/trainer#transformers.TrainingArguments) class to specify our desired arguments in SFTTrainer.

### [Weights and Biases](#weights-and-biases)[](#weights-and-biases)

The [W&B platform](https://wandb.ai/home) will be used for experiment tracking. Specifically, [loss curves](https://wandb.ai/mostafaibrahim17/ml-articles/reports/A-Deep-Dive-Into-Learning-Curves-in-Machine-Learning--Vmlldzo0NjA1ODY0) will be monitored.

[Part 2: Implementation](#part-2-implementation)[](#part-2-implementation)
--------------------------------------------------------------------------

### [Step 1: Set up a GPU Droplet and Launch Jupyter Labs](#step-1-set-up-a-gpu-droplet-and-launch-jupyter-labs)[](#step-1-set-up-a-gpu-droplet-and-launch-jupyter-labs)

Follow this tutorial, [“Setting Up the GPU Droplet Environment for AI/ML Coding”](/community/tutorials/jupyter-notebooks-with-gpu-droplets), to set up a GPU Droplet environment for our Jupyter Notebook.

### [Step 2: Install Dependencies](#step-2-install-dependencies)[](#step-2-install-dependencies)

```
%%capture
!pip install unsloth
!pip install --force-reinstall --no-cache-dir --no-deps git+https://github.com/unslothai/unsloth.git
!pip install --upgrade jupyter
!pip install --upgrade ipywidgets
!pip install wandb
```

### [Step 3: Configure Access Tokens](#step-3-configure-access-tokens)[](#step-3-configure-access-tokens)

HuggingFace Tokens can be obtained from the [Hugging Face Access Token page](https://huggingface.co/settings/tokens). Note that you may need to create Hugging Face account.

```
from huggingface_hub import login 
hf_token = "Replace with your actual token"
login(hf_token) 
```

Similarly, you will also need a [Weights & Biases](https://wandb.ai/home) account to get a token for this step. ![wandb](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/wandb.png)

```
import wandb
wb_token = "Replace with your actual token"

wandb.login(key=wb_token)
run = wandb.init(
    project='Medical Assistant', 
    job_type="training", 
    anonymous="allow"
)
```

```
from unsloth import FastLanguageModel
```

### [Step 4: Loading the model and tokenizer](#step-4-loading-the-model-and-tokenizer)[](#step-4-loading-the-model-and-tokenizer)

```
max_seq_length = 2048

model, tokenizer = FastLanguageModel.from_pretrained(
    model_name = "unsloth/DeepSeek-R1-Distill-Llama-8B-bnb-4bit",
    max_seq_length = max_seq_length,
    load_in_4bit = True,
    dtype = None, #if using H100, will automatically default to bfloat16
    token = hf_token,
)
```

### [Step 5: Testing Model Outputs Before Fine-Tuning](#step-5-testing-model-outputs-before-fine-tuning)[](#step-5-testing-model-outputs-before-fine-tuning)

#### Creating a System Prompt

It is good practice to verify whether model outputs match your standards for format, quality, accuracy, etc. to assess if fine-tuning is necessary. Since we are interested in reasoning, we will formulate a system prompt that elicits a chain of thought.

Instead of writing the prompt directly in our input, let’s start by writing up a prompt template that incorporates place holders.  
![socialnetworkmeme](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/social%20network%20meme.png)

In this prompt template, we will specify precisely what we are looking for.

```
prompt_template= """### Role:
You are a medical expert specializing in clinical reasoning, diagnostics, and treatment planning. Your responses should:
- Be evidence-based and clinically relevant
- Include differential diagnoses when appropriate
- Consider patient safety and standard of care
- Note any important limitations or uncertainties

### Question:
{question}

### Thinking Process:
{thinking}

### Clinical Assessment:
{response}

"""
```

Notice the {thinking} placeholder. The primary goal of this step is to instruct the LLM to explicitly articulate its reasoning process before providing the final answer. This is often what is referred to as ["chain-of-thought prompting”](https://arxiv.org/pdf/2201.11903).

#### Inference with our System Prompt (Before Fine-tuning)

Here, we format the question using the structured prompt (prompt\_template) to ensure the model follows a logical reasoning process. We will tokenize the input, return them as PyTorch tensors, and move it to the GPU (cuda) for faster inference.

```
question = "A 58-year-old woman reports a 3-year history of urine leakage when laughing, exercising, or lifting heavy objects. She denies any nighttime incontinence or feelings of urgency. On physical exam, she demonstrates urine loss with Valsalva maneuver, and a Q-tip test shows hypermobility of the urethrovesical junction with a 45-degree excursion. What would urodynamic testing most likely show regarding her post-void residual volume and detrusor muscle activity?"
FastLanguageModel.for_inference(model) #model defined in step 4
inputs = tokenizer([prompt_template.format(question,"")], return_tensors="pt").to("cuda")
```

After, we will generate a response using the model, specifying key parameters like max\_new\_tokens=1200 (limits response length).

```
outputs = model.generate(
    input_ids=inputs.input_ids,
    attention_mask=inputs.attention_mask,
    max_new_tokens=1200,
    use_cache=True,
)
```

To obtain the final readable answer, we will decode the output tokens back into text.

```
response = tokenizer.batch_decode(outputs)
print(response[0].split("### Response:")[1])
```

Feel free to experiment with different prompt formulations and see how they affect your outputs.

### [Step 6: Load the Dataset](#step-6-load-the-dataset)[](#step-6-load-the-dataset)

The dataset, FreedomIntelligence/medical-o1-reasoning-SFT, that we’re using has three columns: Question, Complex\_CoT, and Response. ![SFT dataset](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Mel/Dataset.png)

We will create a function ([formatting\_prompts\_func](https://huggingface.co/docs/trl/v0.4.7/en/sft_trainer#format-your-input-prompts)) to format the input prompts in the dataset.

```
def formatting_prompts_func(examples):
    inputs = examples["Question"]
    cots = examples["Complex_CoT"]
    outputs = examples["Response"]
    texts = []
    for input, cot, output in zip(inputs, cots, outputs):
        text = prompt_template.format(input, cot, output) + tokenizer.eos_token
        texts.append(text)
    return {
        "text": texts,
    }
```

```
from datasets import load_dataset
dataset = load_dataset("FreedomIntelligence/medical-o1-reasoning-SFT","en", split = "train[0:500]",trust_remote_code=True)
dataset = dataset.map(formatting_prompts_func, batched = True,)
dataset["text"][0]
```

### [Step 7: Prepare the Model for Parameter Efficient Fine-Tuning (PEFT)](#step-7-prepare-the-model-for-parameter-efficient-fine-tuning-peft)[](#step-7-prepare-the-model-for-parameter-efficient-fine-tuning-peft)

Instead of updating all the parameters of the model during fine-tuning, PEFT methods typically only modify a small subset of parameters, resulting in savings in computational power and time.

Here is an overview of some of the parameters and arguments we will be using the .get\_peft\_model method of Unsloth’s FastLanguageModel class.

Hyperparameter

Possible Values

**r:** [LoRA](https://arxiv.org/pdf/2106.09685) rank. This determines the number of trainable adapters.

Select any number greater than 0; recommended numbers are 8, 16, 32, 64, and 128. Note that a higher rank yields more intelligent, but slower, model outputs.

**target\_modules**: These are the modules (layers) within the transformer architecture where LoRA will be applied.

**q\_proj, k\_proj, v\_proj:** The query, key, and value projection layers in the attention mechanism. Fine-tuning these is crucial for adapting the model’s attention to the new task. **o\_proj:** This is the output projection layer in the attention mechanism. **gate\_proj, up\_proj, down\_proj**: These are the projection layers in the feed-forward network (FFN) part of the transformer block. Fine-tuning these can help the model learn task-specific representations in the FFN.

**lora\_alpha:** This is a scaling factor for the LoRA updates. It helps control the magnitude of the updates applied to the original weights. It’s related to the learning rate, and tuning it can be important for performance.

It’s often set to a multiple of r (ex: 2r or 4r).

**lora\_dropout:** This is the dropout probability applied to the LoRA updates. Dropout is a regularization technique that helps prevent overfitting.

When set to 0, no dropout is applied. You might increase this if you observe overfitting.

**bias:** The bias parameter indicates how biases, which are constants added to offset the result, are handled by the model.

Set as “none” if no bias is to be added. Other possible arguments include “all” or “lora\_only”, specifying which layers bias is added to.

**use\_gradient\_checkpointing:** Gradient checkpointing is a technique to reduce memory usage during training at the cost of some extra computation. It recomputes activations during the backward pass instead of storing them.

The “unsloth” argument can be used for an optimized implementation of gradient checkpointing for long contexts within the Unsloth library. Alternatively, this argument can be set to True for standard gradient checkpointing (to save memory at the expense of slower backward pass) or False to disable it.

**random\_state:** This sets the random seed for initializing the LoRA weights. Using a fixed random seed ensures reproducibility—you’ll get the same results if you run the code again with the same seed.

It doesn’t matter what value this is, as long as it’s consistent throughout your code.

**use\_rslora:** [rsLoRA](https://arxiv.org/abs/2312.03732) introduces a scaling factor to stabilize gradients during training, addressing the issue of gradient collapse that can occur in standard LoRA as the rank increases.

rsLoRA is applied when set to True (sets the adapter scaling factor to lora\_alpha/math.sqrt®); this is recommended for higher r values. The default value is False (default value of lora\_alpha/r).

```
model = FastLanguageModel.get_peft_model(
    model,
    r=16,  
    target_modules=[
        "q_proj",
        "k_proj",
        "v_proj",
        "o_proj",
        "gate_proj",
        "up_proj",
        "down_proj",
    ],
    lora_alpha=16,
    lora_dropout=0,  
    bias="none",  
    use_gradient_checkpointing="unsloth",  # True or "unsloth" for very long context
    random_state=522,
    use_rslora=False
    )
```

Now that we’ve evaluated model outputs, it is time to use our SFT dataset to fine-tune the pre-trained model.

### [Step 8: Model Training with SFTTrainer](#step-8-model-training-with-sfttrainer)[](#step-8-model-training-with-sfttrainer)

[Supervised Fine-tuning Trainer](https://huggingface.co/docs/trl/en/sft_trainer) is a class to develop supervised fine-tuned models from TRL.  
We will also be using

```
from trl import SFTTrainer
from transformers import TrainingArguments
from unsloth import is_bfloat16_supported
```

[Training](https://huggingface.co/docs/transformers/en/main_classes/trainer) Arguments

Hyperparameter

Possible Values

**per\_device\_train\_batch\_size:** Number of samples processed per device/GPU during training step

Typically powers of 2: 1, 2, 4, 7, 16, 32…

**gradient\_accumulation\_steps:** Number of forward passes to accumulate before performing backward pass

Higher values allow for larger effective batch sizes. (Effective batch size = per\_device\_train\_batch\_size \* gradient\_accumulation\_steps.)

**warmup\_steps:** Number of steps for the learning rate warmup phase

Non-negative integer, typically 5-10% of total training steps (max\_steps)

**max\_steps:** Total number of training steps to perform

Positive integers, depends on dataset size and training needs

**learning\_rate:** Step size used for model weight updates

Typically between 1e-5 and 1e-3 (ex: 2e-4, 3e-4, 5e-5)

**fp16:** Controls whether to use 16-bit floating point precision **bf16:** Controls whether to use brain floating point format

Enables mixed precision training (fp16 or bf16) for faster training, if supported by the hardware. Potential values include: not is\_bfloat16\_supported() or is\_bfloat16\_supported().

**logging\_steps:** How frequently to log training metrics

Positive integer value indicating interval of steps to pass before logging the training metrics. The value chosen involves striking a balance between having enough information to track training progress and keeping the overhead of logging manageable.

**optim:** Optimization algorithm for training

adamw 8-bit performs similarly to adamw (a popular, robust optimizer), but with reduced GPU memory usage, making it a recommended choice

**weight\_decay:** a regularization technique to prevent overfitting where the value corresponds to the amount of weight decay to apply.

A float value that defaults to 0.

**lr\_scheduler\_type:** Schedule for learning rate adjustments

The default and suggested value is “linear”. Other alternatives include “cosine”, “polynomial”, etc. and may be chosen to achieve faster convergence.

**seed**: Random seed for reproducibility

It doesn’t matter what value this is, as long as it’s consistent throughout your code.

**output\_dir**: Location to save training outputs

A string of the directory path

```
trainer = SFTTrainer(
    model=model,
    tokenizer=tokenizer,
    train_dataset=dataset,
    dataset_text_field="text",
    max_seq_length=max_seq_length,
    dataset_num_proc=2,
    args=TrainingArguments(
        per_device_train_batch_size=2,
        gradient_accumulation_steps=4,
        warmup_steps=5,
        max_steps=60,
        learning_rate=2e-4,
        fp16=not is_bfloat16_supported(),
        bf16=is_bfloat16_supported(),
        logging_steps=10,
        optim="adamw_8bit",
        weight_decay=0.01,
        lr_scheduler_type="linear",
        seed=522,
        output_dir="outputs", #saving in Response column
    ),
)
```

This command will start the training process.

```
trainer_stats = trainer.train()
```

### [Step 9: Monitoring Experiments](#step-9-monitoring-experiments)[](#step-9-monitoring-experiments)

Experiment tracking can be done with Weights and Biases. Essentially, we want to ensure that the training loss decreases over time to ensure model performance is improving with fine-tuning.

If model performance is degrading, it may be worth experimenting with the hyperparameter values.

### [Step 10: Model Inference After Fine-Tuning](#step-10-model-inference-after-fine-tuning)[](#step-10-model-inference-after-fine-tuning)

```
question = "A 58-year-old woman reports a 3-year history of urine leakage when laughing, exercising, or lifting heavy objects. She denies any nighttime incontinence or feelings of urgency. On physical exam, she demonstrates urine loss with Valsalva maneuver, and a Q-tip test shows hypermobility of the urethrovesical junction with a 45-degree excursion. What would urodynamic testing most likely show regarding her post-void residual volume and detrusor muscle activity?"


FastLanguageModel.for_inference(model)  
inputs = tokenizer([prompt_template.format(question, "")], return_tensors="pt").to("cuda")

outputs = model.generate(
    input_ids=inputs.input_ids,
    attention_mask=inputs.attention_mask,
    max_new_tokens=1200,
    use_cache=True,
)
response = tokenizer.batch_decode(outputs)
print(response[0].split("### Response:")[1])
```

### [Step 11: Saving the Model Locally](#step-11-saving-the-model-locally)[](#step-11-saving-the-model-locally)

```
new_model_local = "DeepSeek-R1-Medical-COT"
model.save_pretrained(new_model_local) 
tokenizer.save_pretrained(new_model_local)

model.save_pretrained_merged(new_model_local, tokenizer, save_method = "merged_16bit",)
```

### [Step 12: Pushing the Model to HuggingFace Hub](#step-12-pushing-the-model-to-huggingface-hub)[](#step-12-pushing-the-model-to-huggingface-hub)

If it is desirable to make the model accessible and beneficial to the wider AI community, we can publish the adopter, tokenizer, and model on to the Hugging Face Hub. This will allow others to easily integrate our model into their own projects and systems.

```
new_model_online = "HuggingFaceUSERNAME/DeepSeek-R1-Medical-COT"
model.push_to_hub(new_model_online)
tokenizer.push_to_hub(new_model_online)

model.push_to_hub_merged(new_model_online, tokenizer, save_method = "merged_16bit")

```

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

Fine-tuning is how smart teams transform those pre-trained models into precise, targeted tools that solve real problems. Here, we’re not reinventing the wheel, but rather aligning these wheels so that they take us where we want to go. While pre-trained models are powerful, they can be generic with outputs that may lack the structure and substance characteristic of professional-grade work.

We hope that through this tutorial, you gained an intuition around when to use and fine-tune reasoning models as well as some inspiration to better refine this technology for your use-case.

[References and Additional Resources](#references-and-additional-resources)[](#references-and-additional-resources)
-------------------------------------------------------------------------------------------------------------------

[Fine-Tuning DeepSeek R1 (Reasoning Model) | DataCamp](https://www.datacamp.com/tutorial/fine-tuning-deepseek-r1-reasoning-model)

[HuatuoGPT-o1, Towards Medical Complex Reasoning with LLMs](https://github.com/FreedomIntelligence/HuatuoGPT-o1/tree/main)

[Train your own R1 reasoning model locally (GRPO)](https://unsloth.ai/blog/r1-reasoning)

[Unslothai Llama3.1\_(8B)-GRPO.ipynb](https://colab.research.google.com/github/unslothai/notebooks/blob/main/nb/Llama3.1_\(8B\)-GRPO.ipynb#scrollTo=DkIvEkIIkEyB)

[Fine-Tuning Your Own Llama 3 Model](https://www.youtube.com/watch?v=baQH7tsIQQ8)

#### [Source](https://www.digitalocean.com/community/tutorials/fine-tuning-deepseek-medical-cot)

<br/>
---
