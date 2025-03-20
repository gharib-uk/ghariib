---
title: "My take on the Memory Layer Paper by Meta (noob friendly)"
date: 2025-01-03
categories: 
  - "general"
---

Ref : https://arxiv.org/abs/2412.09764

So Meta FAIR's new paper is a banger as always coming from them lol. This one is about increasing model capabilities but with fewer FLOPs spent for the same or more number of parameters basically. What they changed is where the FLOPs (computation in GPU terms , we AI folks like to call it that way).

So in traditional architecture, you got the input converted to a vector representation and the attention heads transforms them to imbue them with context within the input and then this transformed input is passed to the MLP, this is the actual Neural net that has in its parameters the reasoning and memory both encoded. As the input vector passes through each layer, the input is transformed with a bit of reasoning and knowledge that the model learnt during the course of its learning. Then we do this for a bunch of times and in the end the vector represents the next token that should be part of the input. Pretty standard. Neat.

Now the important part here is the MLP here does both the knowledge , and the reasoning and the reasoning based transformations in an abstract sense. What these guys attempted is a separation of concerns.

Here is what they did, they took a few MLP layers out and put in what they call memory layers essentially these are key value dictionaries and during the course of the input processing after the prompt is converted to vectors, we apply dot product on these keys to find the most fitting pairs for the given query (input prompt). These values from the key value pairs that were selected is then allowed to transform the input prompt that is now a vector imbuing new information.

Now what was this new information imbued into these vectors, that was the stuff the LLM learnt during training time, the memory and some of the reasoning steps that could be in a sense learnt or raw dogged is captured in this key value representation. Its learnt and put in this dictionary, and the operation we did just before is to find the relevant bits of its memory for the given prompt and feed it as part of the input for the MLP

The MLP now does the reasoning part, well ofc since this is an AI system it also retains a bit of knowledge i assume, but much of the learning of information must have been picked up by the memory layers and what the MLP does is the harder reasoning part that requires mixing and bending and processing these information over and over again. which its really good at the MLP or dense layers.

So in a gist, we made the MLP the compute intensive part do the hard reasoning part while the memory layers which are predominantly just dot products (less compute) handle the rote learnable parts.

What these keys and values in the memory layer are ? We dont know for sure, its learnt by the network together as this layer is training along with the MLP. The network just figures out this arrangement as it minimizes the loss like all AI systems end up doing ( or the ones that we remember being successful lol )

This separation of concerns is kinda inspired from what folks did with MoME mixture of a million experts and the PEER router setup, in that approach instead of an MLP the router had a fk ton of single neurons learn a bunch of stuff. And the router used a key query dot product thing to find neurons that matter and sort of on the fly merge them to form a makeshift neural net. I can see strong influence of that in here

I was also reminiscing about Extended Mind Transformers while reading through this, tho its a different approach.

I feel like while the 2 papers i mentioned above focused on memory and then citations. this approach takes a priority on compute cost.

Okay now there is a slight chance some folks might be like why is this less compute intensive, what the fk is dense and sparse.

When the input goes through the MLP each neuron or param or node or value whatever you call it, affects the input vector. Each of them. And as you can see thats why they are dense.

The memory doesnt require each value to undergo processing with the input vector, only the most matching values found by applying dot product on the keys wrt inptu vector needs to undergo a MLP layer or something of that sort that can then again transform the input. This is a sparse operation therefore.

The performance seems to be really good, it scales better than the MoE approaches and dense approaches (check paper and graphs). But i would love to see more folks test this idea out and id like to hear from them. But i got a feeling people are gonna sweep it under the rug even tho its such a fun idea. Maybe meta is cooking something with its byte representation and LCMs with this shit.

Go to Source
