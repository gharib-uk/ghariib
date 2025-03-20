---
title: "DeepSeek-R1 : internals made easy 🐋"
date: 2025-01-26
---

![This open source AI crushes everything - DeepSeek R1 - YouTube](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fazx69go6rieenz2glovg.jpg)  
Well, This week has been all about DeepSeek-R1 making the headlines. So, in this post, let's understand right from `What` is DeepSeek-R1 model and it's `working internals` in depth.

## Firstly, What's DeepSeek-R1?

DeepSeek-R1 is an open-source reasoning model developed by DeepSeek, a Chinese AI company which can work on tasks that require logical inference, mathematical problem-solving, and real-time decision-making.

What sets reasoning models like DeepSeek-R1 and OpenAI’s o1 apart from traditional large language models (LLMs) is their ability to show how they arrived at a conclusion.

![Batman DeepSeek](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F11q7ophb6w01b1hk4c6w.png)

As you can see in the image above, with DeepSeek-R1, you see what steps it follows for reasoning for a prompt which makes it easier to understand and if necessary, challenge its output. This capability gives reasoning models an edge in fields where outcomes need to be explainable, like research or complex decision-making.

Also, this model challenges the industry's reliance on `supervised fine-tuning` (SFT) by showing that `reinforcement learning` (RL) can improve reasoning capabilities. But again, apart from the things I mentioned above, what makes this `revolutionary?`

- _Autonomous Skill Emergence_: Unlike GPT-4 or Claude 3.5 Sonnet, which requires human-curated reasoning examples, `R1-Zero` develops skills like self-verification and multi-step planning _through pure RL_.
- _Cost_: Distilled 7B models outperform _GPT-4o at_ 1/100th the training cost.
- **Open Source**: Full release of model weights, training code.

## Technical Architecture :

### Base Model Foundation :

It's built on top of `DeepSeek-V3-Base` model which is - a 671B parameter Mixture-of-Experts model (MoE = _integrating multiple specialized models, or "experts," to solve complex problems more effectively_) with :

- _16 Expert Networks_: which are each specialized submodels for math, code, logic, etc
- _Dynamic Activation_: 37B parameters activated per token through learned routing.
- _Pre-Training_: 4.8T (yes, **Trillion**) tokens spanning 52 languages and technical domains which includes STEM papers, Github repositories.

### The R1 Variants :

| Model | Parameters | Training Approach | Key Innovation |
| --- | --- | --- | --- |
| R1-Zero | 671B MoE | Pure RL (No SFT) | Autonomous reasoning discovery |
| R1 | 671B MoE | Multi-stage SFT+RL | Human-aligned CoT generation |
| R1-Distill | 1.5B–70B | SFT on R1 outputs | Cost-efficient deployment |

## DeepSeek Internals in Depth:

![DeepSeek-R1: Affordable, Efficient, and State-of-the-Art AI Reasoning | by  LM Po | Jan, 2025 | Medium](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fmiro.medium.com%2Fv2%2Fresize%3Afit%3A1400%2F0%2AUkWMT4LF0Jh0PuJm)

### 1\. Reinforcement Learning at it's Core :

DeepSeek-R1’s most groundbreaking feature is its reliance on **reinforcement learning (RL)** to develop reasoning capabilities. Unlike traditional LLMs that depend on **supervised fine-tuning (SFT)** with human-curated examples, DeepSeek-R1 uses RL to autonomously discover reasoning patterns. Here’s how it works:

#### A. Group Relative Policy Optimization (GRPO)

This is a `critic-free` RL framework that reduces the compute costs by **40%** when used instead of Proximal Policy Optimization (PPO).  
The way that this algorithm works is :

1. **_Group Sampling_** : For each prompt, the model generates G = 16 responses using the current policy. These responses form a group which lateron is used to compute rewards and advantages.
2. **_Reward Normalization_**: Each response in the group is assigned a reward based on the accuracy, format and on is language consistency and Advantage Ai is calculated. This normalization helps stabilize training by reducing variance in group statistics.
3. **_Policy Update_** : Maximization of advantage while constraining KL divergence. (_Kullback-Leibler (KL) divergence is a statistical metric that measures the difference between two probability distributions_). In the equation below, β=0.01 controls the strength of the KL penalty, ensuring the policy doesn’t deviate too far from the reference.

![Handwritten Equations](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fp4xbblnj8l7nreuhoj7z.jpeg)

#### B. Hybrid Reward Engineering :

This is a three-tiered reward system that prevents `reward hacking`. (_Reward hacking occurs when a reinforcement learning (RL) agent exploits flaws or ambiguities in the reward function to achieve high rewards, without genuinely learning or completing the intended task. Reward hacking exists because RL environments are often imperfect, and it is fundamentally challenging to accurately specify a reward function.)_

| Reward Type | Calculation Method | Weight (λ) |
| --- | --- | --- |
| Accuracy (r\_acc) | Binary (1 if final answer correct) | 1.0 |
| Format (r\_fmt) | Cosine similarity to <**think>/<answer**\> template | 0.3 |
| Language (r\_lang) | % of tokens in target language | 0.2 |

Total Reward: r\_total = r\_acc + λ1r\_fmt + λ2r\_lang

### 2\. Cold-Start Supervised Fine-Tuning (SFT) :

Before applying RL, DeepSeek-R1 goes through a cold-start SFT phase which helps in `seeding` the model with basic reasoning patterns. Now, this phase involves:

#### A. Curated Dataset

- ~1,000 high-quality Chain-of-Thought (CoT) examples are manually curated.
- Each example follows a strict XML-style template:

![vim](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ftlbbutosbeffrfaw6qny.png)

#### B. Template Enforcement :

- The model is fine-tuned to generate responses in the `<think>`/`<answer>` format.
    
- This ensures that the reasoning process is structured and interpretable.
    

### 3\. Rejection Sampling for High-Quality Data:

After the RL process, DeepSeek-R1 generates **600K high-quality reasoning samples** through **rejection sampling**. The way that it works is :

1. _Sample Generation_ :
    
    - The RL Model generates multiple responses for each prompt.
    - Only _those_ responses that pass the **rule-based** checks are retained.
2. _Semantic Filtering_ :
    
    - Responses with low semantic coherence or incorrect reasoning are discarded.
3. _Final Dataset_ :
    
    - The filtered dataset is used for further fine-tuning and distillation.

### 4\. Distillation to Smaller Models

DeepSeek-R1’s reasoning capabilities are distilled into smaller models (1.5B–70B parameters) for cost-efficient deployment. The distillation process involves:

1. _Dataset Creation_ :
    
    - 800k samples are generated from the RL-trained model.
    - These samples include both reasoning (600k) and general tasks (200k).
2. _Fine-Tuning_ :
    
    - Smaller models (like Qwen-7B, Llama-70B) are fine-tuned on the distilled dataset.
    - No RL is applied during distillation, which makes it computationally efficient.
3. _Performance_ :
    
    - The distilled 7B model achieves **55.5% pass@1** on AIME 2024, outperforming GPT-4o (9.3%) at a fraction of the cost.

## **Performance Analysis: Benchmarks**

### Mathematical Reasoning

| Benchmark | R1 | R1-Zero | GPT-40 | Human Expert |
| --- | --- | --- | --- | --- |
| AIME 2024 (pass@1) | 79.8% | 71.0% | 9.3% | 85% |
| MATH-500 (pass@1) | 97.3% | 95.9% | 74.6% | 98% |
| IMO Problem Formalization | 81% | N/A | 22% | 89% |

Key Insight: R1 achieves near-human performance on Olympiad-level problems through:

- Step Recycling: Reusing partial solutions across similar problems
- Symbolic-Statistical Fusion: Combining neural intuition with algebraic simplifications

### Coding & Software Engineering

| Task | R1 | GPT-40 | SWE Human |
| --- | --- | --- | --- |
| LiveCodeBench (pass@1) | 65.9% | 32.9% | 72% |
| Codeforces Elo | 2029 | 759 | 2100 (95th percentile) |
| SWE-Bench Resolved | 49.2% | 38.8% | 58% |

Breakthroughs:

- Debugging Chains: Automatically generates test cases to validate code patches
- Cross-Language Transfer: Solves Python problems then ports solutions to Rust

![Understanding DeepSeek R1: How Reinforcement Learning Reshapes Language  Model Reasoning? • Tech Explorer 🚀](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F6188a0i5rygu88modu4a.jpg)

Go to Source
