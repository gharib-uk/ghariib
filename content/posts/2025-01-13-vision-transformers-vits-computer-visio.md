---
title: "Vision Transformers ViTs Computer Vision with Transformer Models"
date: 2025-01-13T15:49:08.123Z
draft: false
type: posts
categories: 
- python,ai-ml,data-science,gpu,droplets
---
# Vision Transformers ViTs Computer Vision with Transformer Models

<br/>

<br/>
Over the past few years, tranformers have transformed the [NLP](/community/tutorials/nlp-models-using-backtracking) domain in machine learning. Models like GPT and [BERT](/community/tutorials/how-to-train-question-answering-machine-learning-models) have set new benchmarks in understanding and generating human language. Now the same principle is been applied to computer vision domain. A recent development in the field of computer vision are vision transformers or ViTs. As detailed in the paper “[An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale](https://arxiv.org/pdf/2010.11929)”, ViTs and transformer-based models are designed to replace convolutional neural networks (CNNs). Vision Transformers are a fresh take on solving problems in computer vision. Instead of relying on traditional convolutional neural networks (CNNs), which have been the backbone of image-related tasks for decades, ViTs use the transformer architecture to process images. They treat image patches like words in a sentence, allowing the model to learn the relationships between these patches, just like it learns the context in a paragraph of text.

Unlike CNNs, ViTs divide input images into patches, serialize them into vectors, and reduce their dimensionality using matrix multiplication. A transformer encoder then processes these vectors as token embeddings. In this article, we’ll explore vision transformers and their main differences from convolutional neural networks. What makes them particularly interesting is their ability to understand global patterns in an image, which is something CNNs can struggle with.

[**Prerequisites**](#prerequisites)[](#prerequisites)
-----------------------------------------------------

1.  **Basics of Neural Networks**: Understanding of how neural networks process data.
2.  **Convolutional Neural Networks (CNNs)**: Familiarity with CNNs and their role in computer vision.
3.  **Transformer Architecture**: Knowledge of transformers, particularly their use in NLP.
4.  **Image Processing**: Understanding basic concepts like image representation, channels, and pixel arrays.
5.  **Attention Mechanism**: Understanding self-attention and its ability to model relationships across inputs.

[What are vision transformers?](#what-are-vision-transformers)[](#what-are-vision-transformers)
-----------------------------------------------------------------------------------------------

Vision transformers use the concept of attention and transformers to process images—this is similar to transformers in a natural language processing (NLP) context. However, instead of using tokens, the image is split into patches and provided as a sequence of linear embedded. These patches are treated the same way tokens or words are treated in NLP.

Instead of looking at the whole picture simultaneously, a ViT cuts the image into small pieces like a jigsaw puzzle. Each piece is turned into a list of numbers (a vector) that describes its features, and then the model looks at all the pieces and figures out how they relate to each other using a [**transformer mechanism**](/community/tutorials/transformers-for-language-translation-classification).

Unlike CNNs, ViTs works by applying specific filters or kernels over an image to detect specific features, such as edge patterns. This is the convolution process which is very similar to a printer scanning an image. These filters slide through the entire image and highlight significant features. The network then stacks up multiple layers of these filters, gradually identifying more complex patterns.  
With CNNs, pooling layers reduce the size of the feature maps. These layers analyze the extracted features to make predictions useful for image recognition, object detection, etc. However, CNNs have a fixed receptive field, thereby limiting the ability to model long-range dependencies.

**How CNN views images?** ![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Shaoni/vision_transformers/cnn_view_images.png)

ViTs, despite having more parameters, use self-attention mechanisms for better feature representation and reduce the need for deeper layers. CNNs require significantly deeper architecture to achieve a similar representational power, which leads to increased computational cost.

Additionally, CNNs cannot capture global-level image patterns because their filters focus on local regions of an image. To understand the entire image or distant relationships, CNNs rely on stacking many layers and pooling, expanding the field of view. However, this process can lose global information as it aggregates details step-by-step.

ViTs, on the other hand, divide the image into patches that are treated as individual input tokens. Using self-attention, ViTs compare all patches simultaneously and learn how they relate. This allows them to capture patterns and dependencies across the whole image without building them up layer by layer.

[What is Inductive Bias?](#what-is-inductive-bias)[](#what-is-inductive-bias)
-----------------------------------------------------------------------------

Before going further, it’s important to understand the concept of [inductive bias](https://en.wikipedia.org/wiki/Inductive_bias). Inductive bias refers to the assumption a model makes about data structure; during training, this helps the model be more generalized and reduce bias. In CNNs, inductive biases include:

1.  **Locality**: Features in images (like edges or textures) are localized within small regions.
2.  **Two-dimensional neighborhood structure**: Nearby pixels are more likely to be related, so filters operate on spatially adjacent regions.
3.  **Translation equivariance**: Features detected in one part of the image, like an edge, retain the same meaning if they appear in another part.

These biases make CNNs highly efficient for image tasks, as they are inherently designed to exploit images’ spatial and structural properties.

Vision Transformers (ViTs) have significantly less image-specific inductive bias than CNNs. In ViTs:

-   **Global processing**: Self-attention layers operate on the entire image, making the model capture global relationships and dependencies without being restricted by local regions.
-   **Minimal 2D structure**: The 2D structure of the image is used only at the beginning (when the image is divided into patches) and during fine-tuning (to adjust positional embeddings for different resolutions). Unlike CNNs, ViTs do not assume that nearby pixels are necessarily related.
-   **Learned spatial relations**: Positional embeddings in ViTs do not encode specific 2D spatial relationships at initialization. Instead, the model learns all spatial relationships from the data during training.

[How Vision Transformers Work](#how-vision-transformers-work)[](#how-vision-transformers-work)
----------------------------------------------------------------------------------------------

![iamge](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Shaoni/vision_transformers/vision_transformers.png)

Vision Transformers uses the standard Transformer architecture developed for 1D text sequences. To process the 2D images, they are divided into smaller patches of fixed size, such as P P pixels, which are flattened into vectors. If the image has dimensions H W with C channels, the total number of patches is N = H W / P P the effective input sequence length for the Transformer. These flattened patches are then linearly projected into a fixed-dimensional space D, called the [**patch embeddings**](https://medium.com/@fernandopalominocobo/demystifying-visual-transformers-with-pytorch-understanding-patch-embeddings-part-1-3-ba380f2aa37f).

A special learnable token, similar to the \[CLS\] token in BERT, is prepended to the sequence of patch embeddings. This token learns a global image representation that is later used for classification. Additionally, positional embeddings are added to the patch embeddings to encode positional information, helping the model understand the spatial structure of the image.

The sequence of embeddings is passed through the Transformer encoder, which alternates between two main operations: [**Multi-Headed Self-Attention (MSA)**](/community/tutorials/attention-mechanisms-in-computer-vision-cbam) and a feedforward neural network, also called an MLP block. Each layer includes [**Layer Normalization**](https://paperswithcode.com/method/layer-normalization) **(LN)** applied before these operations and residual connections added afterward to stabilize training. The output of the Transformer encoder, specifically the state of the \[CLS\] token, is used as the image’s representation.

A simple head is added to the final \[CLS\] token for classification tasks. During pretraining, this head is a small multi-layer perceptron (MLP), while in fine-tuning, it is typically a single linear layer. This architecture allows ViTs to effectively model global relationships between patches and utilize the full power of self-attention for image understanding.

In a hybrid Vision Transformer model, instead of directly dividing raw images into patches, the input sequence is derived from feature maps generated by a CNN. The CNN processes the image first, extracting meaningful spatial features, which are then used to create patches. These patches are flattened and projected into a fixed-dimensional space using the same trainable linear projection as in standard Vision Transformers. A special case of this approach is using patches of size 1×1, where each patch corresponds to a single spatial location in the CNN’s feature map.

In this case, the spatial dimensions of the feature map are flattened, and the resulting sequence is projected into the Transformer’s input dimension. As with the standard ViT, a classification token and positional embeddings are added to retain positional information and to enable global image understanding. This hybrid approach leverages the local feature extraction strengths of CNNs while combining them with the global modeling capabilities of Transformers.

[Code Demo](#code-demo)[](#code-demo)
-------------------------------------

Here is the code block on how to use the vision transformers on images.

```
# Install the necessary libraries  
pip install -q transformers
```

```
from transformers import ViTForImageClassification  
from PIL import Image  
from transformers import ViTImageProcessor
```

```
import requests  
import torch
```

```
# Load the model and move it to ‘GPU’  
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')

model = ViTForImageClassification.from_pretrained('google/vit-base-patch16-224')  
model.to(device)
```

```
# Load the image to perform predictions  
url = 'link to your image'  
image = Image.open(requests.get(url, stream=True).raw)

processor = ViTImageProcessor.from_pretrained('google/vit-base-patch16-224')  
inputs = processor(images=image, return_tensors="pt").to(device)  
pixel_values = inputs.pixel_values  
# print(pixel_values.shape)
```

The ViT model processes the image. It comprises a BERT-like encoder and a linear classification head situated on top of the final hidden state of the \[CLS\] token.

```
with torch.no_grad():  
  outputs = model(pixel_values)  
logits = outputs.logits

# logits.shape

prediction = logits.argmax(-1)  
print("Predicted class:", model.config.id2label[prediction.item()])
```

Here’s a basic Vision Transformer (ViT) implementation using PyTorch. This code includes the core components: patch embedding, positional encoding, and the Transformer encoder.This can be used for simple classification tasks.

```
import torch
import torch.nn as nn
import torch.nn.functional as F

class VisionTransformer(nn.Module):
    def __init__(self, img_size=224, patch_size=16, num_classes=1000, dim=768, depth=12, heads=12, mlp_dim=3072, dropout=0.1):
        super(VisionTransformer, self).__init__()
        
        # Image and patch dimensions
        assert img_size % patch_size == 0, "Image size must be divisible by patch size"
        self.num_patches = (img_size // patch_size) ** 2
        self.patch_dim = (3 * patch_size ** 2)  # Assuming 3 channels (RGB)
        
        # Layers
        self.patch_embeddings = nn.Linear(self.patch_dim, dim)
        self.position_embeddings = nn.Parameter(torch.randn(1, self.num_patches + 1, dim))
        self.cls_token = nn.Parameter(torch.randn(1, 1, dim))
        self.dropout = nn.Dropout(dropout)
        
        # Transformer Encoder
        self.transformer = nn.TransformerEncoder(
            nn.TransformerEncoderLayer(d_model=dim, nhead=heads, dim_feedforward=mlp_dim, dropout=dropout),
            num_layers=depth
        )
        
        # MLP Head for classification
        self.mlp_head = nn.Sequential(
            nn.LayerNorm(dim),
            nn.Linear(dim, num_classes)
        )
    
    def forward(self, x):
        # Flatten patches and embed
        batch_size, channels, height, width = x.shape
        patch_size = height // int(self.num_patches ** 0.5)

        x = x.unfold(2, patch_size, patch_size).unfold(3, patch_size, patch_size)
        x = x.contiguous().view(batch_size, 3, patch_size, patch_size, -1)
        x = x.permute(0, 4, 1, 2, 3).flatten(2).permute(0, 2, 1)
        x = self.patch_embeddings(x)
        
        # Add positional embeddings
        cls_tokens = self.cls_token.expand(batch_size, -1, -1)
        x = torch.cat((cls_tokens, x), dim=1)
        x = x + self.position_embeddings
        x = self.dropout(x)
        
        # Transformer Encoder
        x = self.transformer(x)
        
        # Classification Head
        x = x[:, 0]  # CLS token
        return self.mlp_head(x)

# Example usage
if __name__ == "__main__":
    model = VisionTransformer(img_size=224, patch_size=16, num_classes=10, dim=768, depth=12, heads=12, mlp_dim=3072)
    print(model)
    
    dummy_img = torch.randn(8, 3, 224, 224)  # Batch of 8 images, 3 channels, 224x224 size
    preds = model(dummy_img)
    print(preds.shape)  # Output: [8, 10] (Batch size, Number of classes)
```

[**Key Components:**](#key-components)[](#key-components)
---------------------------------------------------------

1.  **Patch Embedding**: Images are divided into smaller patches, flattened, and linearly transformed into embeddings.
2.  **Positional Encoding**: Positional information is added to the patch embeddings, as Transformers are position-agnostic.
3.  **Transformer Encoder**: Applies self-attention and feed-forward layers to learn relationships between patches.
4.  **Classification Head**: Outputs the class probabilities using the CLS token.

You can train this model on any image dataset using an optimizer like Adam and a loss function like cross-entropy. For better performance, consider pretraining on a large dataset before fine-tuning.

[Popular Follow-up Work](#popular-follow-up-work)[](#popular-follow-up-work)
----------------------------------------------------------------------------

-   **DeiT ([Data-efficient Image Transformers](https://paperswithcode.com/method/deit))** by Facebook AI: These are vision transformers trained efficiently with knowledge distillation. DeiT offers four variants: _deit-tiny_, _deit-small_, and two _deit-base_ models. Use `DeiTImageProcessor` to prepare images.
    
-   **BEiT ([BERT pre-training of Image Transformers](/community/tutorials/how-to-train-question-answering-machine-learning-models))** by Microsoft Research: Inspired by BERT, BEiT uses self-supervised masked image modeling and outperforms supervised ViTs. It relies on VQ-VAE for training.
    
-   **DINO ([Self-supervised Vision Transformer Training](/community/tutorials/grounding-dino-1-5-open-set-object-detection))** by Facebook AI: DINO-trained ViTs can segment objects without explicit training. Checkpoints are available online.
    
-   **MAE ([Masked Autoencoders](https://www.geeksforgeeks.org/masked-autoencoders-in-deep-learning/))** by Facebook pre-train ViTs by reconstructing masked patches (75%). When fine-tuned, this simple method surpasses supervised pre-training.
    

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

In conclusion, ViTs are an excellent alternative for CNNs as they apply transformers to image recognition, minimize inductive bias, and treat images as sequence patches. This simple yet scalable approach has demonstrated state-of-the-art performance on many image classification benchmarks, especially when paired with pre-training on large datasets. However, potential challenges remain, which include extending ViTs to tasks like object detection and segmentation, further improving self-supervised pre-training methods, and exploring the potential of scaling ViTs for even better performance.

[**Additional Resources**](#additional-resources)[](#additional-resources)
--------------------------------------------------------------------------

-   [Vision Transformer (ViT)](https://huggingface.co/docs/transformers/en/model_doc/vit#using-scaled-dot-product-attention-sdpa)
-   [AN IMAGE IS WORTH 16X16 WORDS: TRANSFORMERS FOR IMAGE RECOGNITION AT SCALE](https://arxiv.org/pdf/2010.11929v2)
-   [Writing CNNs from Scratch in PyTorch](/community/tutorials/writing-cnns-from-scratch-in-pytorch)

#### [Source](https://www.digitalocean.com/community/tutorials/vision-transformer-for-computer-vision)

<br/>
---
