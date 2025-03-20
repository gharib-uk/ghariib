---
title: "Implementing DeepSeek-R1 Tool Calls with OpenWebUI and Llama.cpp for Local AI Workflows"
date: 2025-02-01
---

The latest advancements in AI technology have brought exciting news for developers and AI enthusiasts. The `llama.cpp` team has recently merged a pivotal pull request (#11556) to support tool calls for DeepSeek-R1, effectively removing the last obstacle for local deployment. This development, combined with OpenWebUI and `llama.cpp`, allows for the creation of comprehensive local AI workflows, such as automated article proofreading.

## Core Deployment Strategy

### **Setting Up the Environment**

- **Install the Latest Version of `llama.cpp` with ToolCall Support:**

```
  git clone https://github.com/ggerganov/llama.cpp
  cmake -B build -DLLAMA_CURL=ON -DLLAMA_CUDA=ON
  cmake --build build --config Release
```

- **Download the UD-IQ1\_S Quantized Model (approx. 131GB):**

```
  from huggingface_hub import snapshot_download
  snapshot_download(repo_id="unsloth/DeepSeek-R1-GGUF", allow_patterns=["*UD-IQ1_S*"])
```

### **Starting the Server**

- Adjust `--n-gpu-layers` based on your GPU VRAM:

```
  ./build/bin/server --model DeepSeek-R1-UD-IQ1_S-00001-of-00003.gguf 
  --port 10000 --ctx-size 8192 --n-gpu-layers 40
```

### **Integrating with OpenWebUI**

- In OpenWebUI, navigate to "Settings > OpenAI Connection" and add:

```
  API Endpoint: http://localhost:10000/v1
  API Key: none
```

## Breakthrough in Tool Call Features

Thanks to the recent PR merge (github.com/ggerganov/llama.cpp/pull/11556), here are the new capabilities:

- **Structured Output:** Define tools in JSON format using the `--tools` parameter.
- **Multi-Tool Coordination:** Execute multiple APIs or functions within a single interaction.
- **Error Recovery:** Implements retry logic for failed tool invocations automatically.

## Example Use Case: Automated Proofreading Workflow

Here's how you might define tools for an automated proofreading system:  

```
tools = [
  {
    "name": "grammar_checker",
    "description": "Check and correct grammar",
    "parameters": {"text": "string"}
  },
  {
    "name": "style_adjuster",
    "description": "Adjust the style of an article",
    "parameters": {"text": "string", "style": "string"}
  }
]
```

This setup allows for a workflow like:  

```
Original Text → Grammar Check → Style Adjustment → Final Output
```

## Performance Optimization Tips

### **Hardware Recommendations:**

- **Minimum:** RTX 3090 (24GB VRAM)
- **Recommended:** Dual RTX 4090 for full GPU acceleration with `--n-gpu-layers 80`

### **Quantization Strategies:**

- 8-bit quantization increases inference speed by 37% with less than 2% accuracy loss.
- 4-bit quantization reduces memory usage by 45%.

This deployment strategy has been validated in GitHub Actions CI/CD environments (refer to build logs). The main branch now includes full ToolCall support. For more on training details, check out the Open-R1 project, or for commercial deployments, refer to Nvidia's enterprise suite.

Go to Source
