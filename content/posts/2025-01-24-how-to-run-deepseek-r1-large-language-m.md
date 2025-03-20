---
title: "How to Run DeepSeek R1 Large Language Models on DigitalOcean GPU Droplets"
date: 2025-01-24T16:42:49.000Z
draft: false
type: posts
categories: 
- ai-ml,gpu
---
# How to Run DeepSeek R1 Large Language Models on DigitalOcean GPU Droplets

<br/>

<br/>
Here at DigitalOcean, we have been carefully watching the closing of the gap between open-source Large Language Models (LLMs) and their commercial, closed-source counterparts. One of the most important capabilities of these models is reasoning - the action of thinking about something in a logical, sensible way.

For a long time, LLMs were very linear. When given a prompt, they provided an answer. There is no meta-logic involved, or any stage where the model might be able to self-correct if it is mistaken. This effectively hinders their ability to reason, question, or adjust to problems that may be inherent to the instruction they are responding to. For example, with low-reasoning models, complex language based mathematics problems may be too complicated to solve without explicit instructions and work on the users part.

Enter the latest generation of reasoning LLMs. Ushered in by OpenAI’s [O1 model](https://openai.com/o1/) series, reasoning models have taken the community by storm as they have effectively closed the gap between human and machine learning capabilities on a variety of logic tasks. These include coding, mathematics, and even scientific reasoning.

Like with all previous steps forward in development, the open source community has been working hard to match the closed-source models capabilities. Recently, the first open-source models to achieve this level of abstract reasoning, the [Deepseek R1](https://api-docs.deepseek.com/news/news250120) series of LLMs, was released to the public.

In the first of this 2 part article series, we will show how to run these models on [DigitalOcean’s GPU Droplets](/products/gpu-droplets) using Ollama. Readers can expect to learn how to set up the GPU Droplet, install Ollama, and begin reasoning with Deepseek R1.

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

-   DigitalOcean account: this tutorial will use DigitalOcean’s GPU droplets
-   Bash shell familiarity: we will be using the terminal to access, download, and use Ollama. The commands will be provided

[Setting up the GPU Droplet](#setting-up-the-gpu-droplet)[](#setting-up-the-gpu-droplet)
----------------------------------------------------------------------------------------

The first thing we need to do is set up our machine. To begin, create a new GPU Droplet following the procedure shown in the official DigitalOcean [documentation](https://docs.digitalocean.com/products/droplets/how-to/gpu/create/).

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/James/Screenshot%202025-01-24%20at%201.21.18%E2%80%AFPM.png)

We recommend selecting the “AI/ML Ready” OS and using a single NVIDIA H100 GPU for this project, unless you intend to use the largest, 671B parameter model.

Once your machine has started up, proceed to the next section.

[Installing Ollama & DeepSeek R1](#installing-ollama-deepseek-r1)[](#installing-ollama-deepseek-r1)
---------------------------------------------------------------------------------------------------

For this demonstration, we will take advantage of the incredible work done by the Ollama developers to bring our model online at rapid speed. Open up a web console window using the button on the top right of your [GPU Droplet details](/products/gpu-droplets) page, and navigate to the working directory of your choosing.

Once you are in the place you would like to work, paste the following command into the terminal:

```
curl -fsSL https://ollama.com/install.sh | sh
```

This will execute the installation of Ollama onto our machine. This process may take a few minutes as it installs. Once it has completed, everything is ready to go! Wasn’t that simple?

Now, all we need to do is execute the command that runs DeepSeek R1 on our machine. Ollama provides all of the available model sizes (1.5b, 7b, 8b, 14b, 32b, 70b and 671b parameters), so we recommend using the largest available to run on a single GPU, the 70b model.

```
ollama run deepseek-r1:70b
```

This will take a few minutes the first time it is run to download the roughly 40GB model onto our machine and load it, but from here, we will get an interactive window to chat with our open source reasoning model.

**Info:** Deploy DeepSeek R1, the open-source advanced reasoning model that excels at text generation, summarization, and translation tasks. As one of the most computationally efficient open-source LLMs available, you’ll get high performance while keeping infrastructure costs low with DigitalOcean’s GPU Droplets.

→[Deploy your model in just one click](https://cloud.digitalocean.com/gpus/new?region=nyc2&size=gpu-h100x8-640gb&fleetUuid=e31b0c76-06d2-49d4-b446-2b81a38007c7&appId=177155486&image=deepseek-r1-671b&type=applications)

[Reasoning with DeepSeek R1](#reasoning-with-deepseek-r1)[](#reasoning-with-deepseek-r1)
----------------------------------------------------------------------------------------

As we mentioned at the start of this article, R1 is capable of handling complex tasks related to mathematics, coding, and even scientific reasoning. A reasoning model is a framework or set of principles that guide how an AI system reasons, makes decisions, and solves problems. It outlines the algorithms, techniques, and heuristics that enable the AI to draw inferences, draw conclusions from available information, and generate solutions to complex tasks. It does this by using the iterative nature of GPT models with metalogic reasoning to better understand and infer solutions to the proposed problem.

Let’s try a sample math problem ([source](https://www.k5learning.com/free-math-worksheets/fifth-grade-5/word-problems/measurement-mass-weight)), and see how the model reasons the answer back to us:

```
## input
Answer this question: A package that is heavier than 11 lbs and 8 oz will have a label that says “heavy” on it. Gloria packed 6 flowerpots to send to her customers. Each of the flowerpots weighs 1 lb and 12 oz. The packing material weighs 5 oz. Will her package be labeled as “heavy”?
## output
<think>
First, I need to determine the total weight of Gloria's package to see if it exceeds the threshold for
being labeled as "heavy."

Gloria has packed 6 flowerpots, each weighing 1 pound and 12 ounces. Additionally, there is packing
material that weighs 5 ounces.

I'll start by calculating the total weight of all the flowerpots. Since each pot is 1 lb 12 oz, multiplying
this by 6 gives:
6 pots × (1 lb + 12 oz) = 6 lbs and 72 oz.

Next, I need to convert the 72 ounces into pounds because the threshold is given in pounds and ounces.
Knowing that 16 ounces make a pound, I divide 72 by 16:
72 oz ÷ 16 oz/lb = 4.5 lb.

So, the flowerpots weigh a total of 6 lbs + 4.5 lbs = 10.5 lbs.

Now, adding the weight of the packing material, which is 5 ounces. To maintain consistent units, I convert
5 ounces to pounds:
5 oz ÷ 16 oz/lb ≈ 0.3125 lb.

Adding this to the flowerpots' total weight:
10.5 lbs + 0.3125 lbs = 10.8125 lbs.

Finally, comparing the total package weight of approximately 10.81 lbs to the 11 lbs and 8 oz threshold,
which is equivalent to 11.5 lbs (since 8 oz = 0.5 lb). Since 10.81 lbs is less than 11.5 lbs, the package
will not be labeled as "heavy."
</think>

**Solution:**

To determine if Gloria's package will be labeled as “heavy,” we need to calculate its total weight and
compare it to the threshold of **11 lbs and 8 oz** (which is equivalent to **11.5 lbs**).

1. **Calculate the Weight of the Flowerpots:**
   
   - Each flowerpot weighs **1 lb and 12 oz**.
   - Convert **12 oz** to pounds:
\[
12\ \text{oz} = \frac{12}{16}\ \text{lbs} = 0.75\ \text{lbs}
\]

- So, each flowerpot weighs **1.75 lbs**.

- Gloria packed **6 flowerpots**, so:
\[
6 \times 1.75\ \text{lbs} = 10.5\ \text{lbs}
\]

2. **Add the Weight of Packing Material:**

- The packing material weighs **5 oz**.
- Convert **5 oz** to pounds:
\[
5\ \text{oz} = \frac{5}{16}\ \text{lbs} \approx 0.3125\ \text{lbs}
\]

- Total weight of the package:
\[
10.5\ \text{lbs} + 0.3125\ \text{lbs} = 10.8125\ \text{lbs}
\]

3. **Compare with the Threshold:**

- The threshold for a “heavy” label is **11 lbs and 8 oz**, which is **11.5 lbs**.

- Since **10.8125 lbs < 11.5 lbs**, the package does **not** meet the criteria for being labeled as
“heavy.”

**Final Answer:**

\[
\boxed{\text{No}}
\]
```

Look closely at the way the model reasoned out the response to this question. It first determined the parameters for solving the question: finding the total weight of the flower pots and packing material. It then reiterated the facts from the problem, and showed its work reasoning the total weight of the flower pots and packing materials individually. It then combined those values, and then compared the two values to get the correct answer: no.

Now, let’s look at a coding example:

```
## input
How can i write a Python function that converts wav files to mp3s?

## output
Okay, so I want to create a Python function that converts WAV files to MP3s. Hmm, where do I start? Well, I
know that WAV and MP3 are both audio formats, but they're different in terms of compression and quality.
WAV is uncompressed, which makes the files larger, while MP3 is compressed, making the files smaller but
with some loss in quality.

First, I need to figure out how to read a WAV file in Python. I've heard about libraries like `wave` or
`soundfile`, but I'm not sure if they can help me convert formats. Maybe those are more for manipulating
audio data rather than converting formats.

Then there's the conversion part. How do I actually convert a WAV file into an MP3? It seems like I might
need some external tool or library that handles the encoding. Oh, right! I think `ffmpeg` is a powerful
tool for audio and video processing. Maybe I can use it within Python.

So, perhaps I can write a function that uses `ffmpeg` to convert the WAV file to MP3. But how do I
interface with `ffmpeg` from Python? I recall there's a library called `subprocess` that allows you to run
shell commands from Python. That could work.

Wait, but is there a more direct way without using shell commands? Maybe there are pure Python libraries
for converting audio formats. I did some searching and found `pydub`. It seems like `pydub` can handle
format conversions and uses `ffmpeg` under the hood. That might be easier to use than dealing with
`subprocess` directly.

Alright, so I'll go with `pydub`. First, I need to install it using pip:

pip install pydub


But wait, does `pydub` require `ffmpeg` to be installed on my system? Yes, it does. So I need to make sure
that `ffmpeg` is installed and accessible from the command line.

Once that's set up, I can write a function that takes a WAV file path as input and outputs an MP3 file. Let
me sketch this out:

1. Import the necessary modules.
2. Define a function called `convert_wav_to_mp3`.
3. Use `pydub.AudioSegment` to read the WAV file.
4. Export the audio segment to MP3 format.

But I should also handle exceptions, like if the input file doesn't exist or if there's an error during
conversion. Maybe add some try-except blocks for better error handling.

Let me think about potential issues:
- What if the output path already exists? Should I overwrite it or raise an error?
- How do I handle different bitrates or quality settings in MP3?
- Are there any specific parameters I need to set for the conversion?

For now, I'll keep it simple and focus on a basic conversion. I can always add more features later.

So, putting it all together, here's what my function might look like:


from pydub import AudioSegment

def convert_wav_to_mp3(wav_file_path, mp3_file_path):
  try:
# Load the WAV file
    sound = AudioSegment.from_wav(wav_file_path)
 
# Export to MP3
   sound.export(mp3_file_path, format="mp3")
 
   print("Conversion successful!")
  except Exception as e:
    print(f"An error occurred: {e}")

I should test this function with a sample WAV file to see if it works. If it does, great! I can then think
about adding more features like handling multiple files or different quality settings.

Wait, but what if the user wants to convert all WAV files in a directory? Maybe I can modify the function
to accept a list of file paths or even search for WAV files recursively. That would make the function more
versatile.

Also, considering performance, converting large numbers of files might take some time. Maybe adding
progress indicators or parallel processing could improve the user experience, but that's probably beyond
the scope of this initial implementation.

In summary, to write a Python function that converts WAV files to MP3s:
1. Use `pydub` as it simplifies the conversion process using `ffmpeg`.
2. Ensure `ffmpeg` is installed and accessible.
3. Write a function that imports the WAV file and exports it in MP3 format.
4. Add error handling for potential issues like missing files or conversion errors.

I think this approach should work well for my needs, but I'll need to test it thoroughly to make sure there
are no unexpected issues.
</think>

To convert WAV files to MP3s using Python, you can utilize the `pydub` library, which simplifies audio
format conversions by leveraging `ffmpeg`. Below is a step-by-step guide and code implementation:

**Step 1: Install Required Libraries**

First, install `pydub` using pip. Note that `pydub` requires `ffmpeg` to be installed on your system.

pip install pydub

Ensure `ffmpeg` is accessible from the command line.

**Step 2: Implement the Conversion Function**

Here's a Python function that converts a WAV file to MP3:

from pydub import AudioSegment

def convert_wav_to_mp3(wav_file_path, mp3_file_path):
#Converts a WAV file to an MP3 file.

#Args:
#wav_file_path (str): Path to the input WAV file.
#mp3_file_path (str): Desired path for the output MP3 file.
#"""
  try:
# Load the WAV file
    sound = AudioSegment.from_wav(wav_file_path)
 
# Export to MP3 format
    sound.export(mp3_file_path, format="mp3")
 
print("Conversion successful!")
  except Exception as e:
    print(f"An error occurred during conversion: {e}")

**Step 3: Example Usage**

To use the function:
# Example usage
convert_wav_to_mp3("input.wav", "output.mp3")

This will convert `input.wav` to `output.mp3`.

**Additional Considerations:**

1. **Error Handling**: The function includes basic error handling. Depending on your needs, you can extend
it by handling specific exceptions.

2. **Multiple Files**: To process multiple WAV files, modify the function to accept a list of file paths or
iterate through a directory.

3. **Quality Settings**: `pydub` allows specifying parameters like bitrate. You can enhance the function to
accept such options.

4. **Performance**: For large-scale conversions, consider adding progress indicators or parallel processing
for efficiency.

This implementation provides a robust foundation for converting WAV files to MP3s using Python.
```

Again, this is a very telling example of the model’s reasoning skills. In the response, it first provided a sufficient answer. Continuing on, it reasoned that the answer may not consider all possible edge cases for the user, such as converting all files in a directory. To ameliorate this, it provided another working solution that better solves every potential problem. It then expounded upon these considerations in detail to conclude the response. Overall, this is a very impressive and comprehensive solution that nearly mimics the reasoning of a human actor.

Based on these responses, we recommend trying all sorts of challenging questions with R1. The model is incredibly robust, especially at the 60b param level and up.

[Closing Thoughts](#closing-thoughts)[](#closing-thoughts)
----------------------------------------------------------

In this article, we showed how to run DeepSeek R1 using DigitalOcean’s GPU Droplets with Ollama. As we saw above, this provides us with a fast and powerful reasoning mechanism to aid us across a variety of tasks, including programming and math. We were very impressed with these models, and will definitely be using them to facilitate projects wherever possible.

Check back soon for part two of this series, where we will dig deeper into R1’s architecture, expand on how the model training was done, and learn what makes the model’s reasoning so powerful.

#### [Source](https://www.digitalocean.com/community/tutorials/deepseek-r1-gpu-droplets)

<br/>
---
