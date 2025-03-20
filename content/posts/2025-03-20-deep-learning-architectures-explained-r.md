---
title: "Deep Learning Architectures Explained ResNet InceptionV3 SqueezeNet"
date: 2025-03-20T09:33:57.000Z
draft: false
type: posts
categories: 
- deep-learning,neural-network,ai-ml
---
# Deep Learning Architectures Explained ResNet InceptionV3 SqueezeNet

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

Deep learning has completely transformed the way computers view and recognize images. Deep learning architectures help machines classify images, predict objects in an image, and even generate realistic visuals. The base of these powerful deep learning models lies in dense neural network architectures, which are designed for optimal performance, accuracy, and efficiency. In this article, we will explore the three most widely used [Convolutional Neural Networks](/community/tutorials/writing-cnns-from-scratch-in-pytorch): ResNet, InceptionV3, and SqueezeNet. We will learn how they work, their key innovations, and their impact on deep learning applications.

ResNet: ResNet, also known as Residual Networks, was proposed by Microsoft in 2015 and brought a breakthrough to address the vanishing/exploding gradient issue in deep learning networks. Traditional deep learning models struggled with training as they became increasingly dense due to gradient degradation, making it extremely difficult to learn effectively as the number of layers increased. ResNet introduced skip connections (residual connections) that allow the gradient to bypass multiple layers and form residual blocks, making it possible to train extremely deep networks like ResNet-50 and ResNet-101 without significant performance loss. InceptionV3: Google developed [Inceptionv3](https://www.mathworks.com/help/deeplearning/ref/inceptionv3.html), an improved version of the Inception architecture, in 2016. The architecture is designed to optimize computation using parallel convolutional filters of different sizes.

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

-   **Basic Knowledge of Neural Networks**: Understand the fundamentals of [convolutional neural networks (CNNs)](/community/tutorials/writing-cnns-from-scratch-in-pytorch) and deep learning principles.
-   **Familiarity with Backpropagation**: Know how gradient descent and backpropagation work for training neural networks.
-   **Understanding of Model Layers**: Be aware of different types of layers (e.g., convolutional, pooling, fully connected) used in CNN architectures.
-   **Experience with Deep Learning Frameworks**: Have hands-on experience with frameworks like [PyTorch](/community/tutorials/pytorch-101-understanding-graphs-and-automatic-differentiation) or [TensorFlow](/community/tutorials/pytorch-vs-tensorflow) to implement and experiment with these architectures.

[ResNet (2015)](#resnet-2015)[](#resnet-2015)
---------------------------------------------

As deep neural networks are both time-consuming to train and prone to overfitting, a team at Microsoft introduced a residual learning framework to improve the training of networks that are substantially deeper than those used previously. This research was published in the [Deep Residual Learning for Image Recognition](https://arxiv.org/abs/1512.03385) 2015 paper. And so, the famous [ResNet](/community/tutorials/popular-deep-learning-architectures-resnet-inceptionv3-squeezenet) (short for “Residual Network”) was born. When training deep networks, there comes a point where an increase in depth causes accuracy to saturate and then degrade rapidly. This is called the “degradation problem.” It highlights that not all neural network architectures are equally easy to optimize. ResNet uses a technique called “residual mapping” to combat this issue. Instead of hoping that every few stacked layers directly fit a desired underlying mapping, the Residual Network explicitly lets these layers fit a residual mapping. Below is the building block of a Residual network.

![image](https://lh5.googleusercontent.com/VngdxOkON3G8JD7PJSyMx_sdfMu_1ejmtumaxLYZt1wIgQFRYRAvxJWehXCX2a9b6Gky5R8uMGH2b-WFBiA_GXWJQIR1F1yJWhOHU0ignk0PWhFJgrsyVd1_ElHs3-KwoqFSYQkF)

The formulation of F(x)+x can be realized by feedforward neural networks with shortcut connections.

ResNets can address many problems. They are easy to optimize and achieve higher accuracy when the depth of the network increases, producing better results than previous networks. ResNet was first trained and tested on ImageNet’s over 1.2 million training images belonging to 1000 different classes.

[ResNet Architecture](#resnet-architecture)[](#resnet-architecture)
-------------------------------------------------------------------

Compared to the conventional neural network architectures, ResNets are relatively easy to understand. Below is the image of a VGG network, a plain 34-layer neural network, and a 34-layer residual neural network. In the plain network, the layers have the same number of filters for the same output feature map. If the size of output features is halved, the number of filters is doubled, making the training process more complex.

![image](https://lh5.googleusercontent.com/fIXDrntrxU-YJewW148x4VsJICzisvWOj6voUUq0eU2bdxv54e2OWJEIrgyjC4K3c2Y_zHOrpT7AQl5UP-laUEo_U0HXdAtSanORH6JudtCwwGK6oUjhSH8Coj3d2mqovYBhEe0s)

Meanwhile, in the Residual Neural Network, as we can see, there are far fewer filters and lower complexity during the training with respect to [VGG](/community/tutorials/vgg-from-scratch-pytorch). A shortcut connection is added that turns the network into its counterpart residual version. This shortcut connection performs identity mapping, with extra zero entries padded for increasing dimensions. This option introduces no additional parameter. The projection shortcut is mathematically represented as F(x{W}+x), which is used to match dimensions computed by 1×1 convolutions.

The table below shows the layers and parameters of the different ResNet architectures.

![image](https://lh3.googleusercontent.com/NPuDMZDi_u7AtBmXUJEgYJQTVEkC6fBGi-lMJqQMIZeuExuxLRS2rXGFStBp_n_OeoBn9_ds413Yk-GDl3JvRb7N_FG5JI7JX11xt5zPaXysSYlb5NyQLCzqzp_jolPn_bBExmEs)

Each ResNet block is either two layers deep (used in small networks like ResNet 18 or 34) or 3 layers deep (ResNet 50, 101, or 152).

[ResNet Training and Results](#resnet-training-and-results)[](#resnet-training-and-results)
-------------------------------------------------------------------------------------------

The samples from the ImageNet dataset are re-scaled to 224 × 224 and are normalized by a per-pixel mean subtraction. Stochastic gradient descent is used for optimization with a mini-batch size of 256. The learning rate starts from 0.1 and is divided by 10 when the error increases, and the models are trained up to 60 × 104 iterations. The weight decay and momentum are set to 0.0001 and 0.9, respectively. Dropout layers are not used.

ResNet performs extremely well with deeper architectures. Below is an image showing the error rate of two 18- and 34-layer neural networks. The graph on the left shows plain networks, while the graph on the right shows their ResNet equivalents. The thin red curve in the image represents the training error, and the bold curve represents the validation error.

![image](https://lh4.googleusercontent.com/QOAWjplvdruwvPArIi3OSO22EVIklPiSHz2t-hduDO5Zr3JQb582K4hCjsxvkosZcNGq4dnnqN47LPAbGtDDjvH3o_F-f6bSMcnzpUHDi01DHsqrG0-TnjSk8G3SvgRIBgqzORVz)

Below is the table showing the Top-1 error (%, 10-crop testing) on ImageNet validation.

![image](https://lh5.googleusercontent.com/6OSwTeBil0F_2JUNSISRrlOJOvQ2SsIecqYAJVnDQ7gRWfQBJdvg_XKpoNj8joB05Nc4zoi2u1ZK29OHIYwUz9tDSOIjuIdnQ5Sh2CNoD3DSIc7ey1YHmdY0HsGN0qFpoNvSAe3v)

ResNet has played a significant role in defining the field of deep learning as we know it today.

Below are a few important links if you’re interested in implementing a ResNet yourself:

1.  [PyTorch ResNet Implementation](https://github.com/pytorch/vision/blob/master/torchvision/models/resnet.py)
2.  [Link to the Original Research Paper](https://arxiv.org/pdf/1512.03385.pdf)

[Wide ResNet (2016)](#wide-resnet-2016)[](#wide-resnet-2016)
------------------------------------------------------------

The [Wide Residual Network](/community/tutorials/writing-resnet-from-scratch-in-pytorch) recently improved the original Deep Residual Networks. Rather than relying on increasing the depth of a network to improve its accuracy, it was shown that a network could be made shallower and wider without compromising its performance. This ideology was presented in the paper [Wide Residual Networks](https://arxiv.org/abs/1605.07146), which was published in 2016 (and updated in 2017 by Sergey Zagoruyko and Nikos Komodakis.

Deep Residual Networks have shown remarkable accuracies that have aided in performing tasks like image recognition, among many others, with relative ease. Nonetheless, a deep network still faces the challenges of network degradation and exploding or vanishing gradients. A deep residual network doesn’t guarantee the usage of all residual blocks either; a few could be skipped, or only a few make it to the larger contributing blocks (i.e., extract useful information). This problem could be solved by disabling random blocks–this is a [dropout](https://www.geeksforgeeks.org/dropout-in-neural-networks/) in a broader sense. Taking a cue from this idea, the authors of Wide ResNet have proved that a wide residual network can perform even better than a deep one.

[Wide ResNet Architecture](#wide-resnet-architecture)[](#wide-resnet-architecture)
----------------------------------------------------------------------------------

A Wide ResNet has a group of ResNet blocks stacked together, each following the BatchNormalization-ReLU-Conv structure. This structure is depicted as follows:

![image](https://lh5.googleusercontent.com/b0m2-4GdfQb-tDOxSO2XuRgC3jCMPJWkNZkGBOCRUWynjhzhA6Ke50eWYjyUXhBhsMxz98lASR3vGCrVrMH3jN6gZ1uKY6lNhnX22mr3qmLa8kKuNaLx01nluEOMjmMx4ezGXkj3)

Five groups comprise a wide ResNet. The block here refers to the residual block B(3, 3). Conv1 remains intact in any network, whereas conv2, conv3, and conv4 vary according to k, which defines the width. An average-pool layer and a classification layer succeed the convolutional layers.

The following variable measures could be tweaked to come up with various representations of the residual blocks: Convolution type: B(3, 3) shown above has 3 × 3 convolutional layers in the residual block. Other possibilities, such as integrating 3 × 3 convolutional layers with 1 × 1 convolutions, could also be explored.

-   Convolutions per block: The depth of the block has to be determined by estimating this metric’s dependency on the model’s performance.
-   Width of residual blocks: The width and depth of residual blocks must be checked consistently to monitor computational complexity and performance.
-   Dropout: A dropout layer should be added between convolutions in every residual block. This prevents overfitting.

### [Wide ResNet Training and Results](#wide-resnet-training-and-results)[](#wide-resnet-training-and-results)

Wide ResNet was trained on [CIFAR-10](https://www.cs.toronto.edu/~kriz/cifar.html). The following metrics resulted in the lowest error rates:

-   Convolution type: B(3, 3)
-   Convolution layers per residual block: 2. Thus B(3,3) was the preferred dimension (rather than B(3, 3, 3) or B(3, 3, 3, 3), for instance).
-   Width of residual blocks: A depth of 28 and a width of 10 seemed to be less error-prone.
-   Dropout: When dropout was included the error rate was further reduced. The comparative results are shown in the figure below.

![image](https://lh3.googleusercontent.com/RSTdtNPcmsezmogPbP66GkQ8ulpsmnNPnUwu66tiJOZwUiJ88TzR-kVwgLZZugWiUaPpqUsTc1iTN0UVz7KLKpbzUkBTixu--lq9IONhMw-DT5QL-5ruEloJe_zjNfXhfdquzdML)

The following table compares the complexity and performance of Wide ResNet with several other models, including the original ResNet, on both CIFAR-10 and CIFAR-100:

![image](https://lh4.googleusercontent.com/jxjyVQ5pJQSDe7qJx_chpk27lpV0aVs4c4wvHh_y_cZWd5K4VQXHf8BI92u4Sk60NURBNfhSclXxbmIfDsEQT7v4eWX5HFw6aN6duNu0N1f2h3A9U6fk8WalSS0c0vSlNE-jbWge)

Below are a few important links for implementing Wide ResNet yourself:

1.  [Link to the Original Paper](https://arxiv.org/pdf/1605.07146.pdf)
2.  [PyTorch Implementation of Wide ResNet](https://pytorch.org/hub/pytorch_vision_wide_resnet/)
3.  [Tensorflow Implementation of Wide ResNet](https://github.com/dalgu90/wrn-tensorflow)

[Inception v3 (2015)](#inception-v3-2015)[](#inception-v3-2015)
---------------------------------------------------------------

Inception v3 mainly focuses on burning less computational power by modifying the previous Inception architectures. This idea was proposed in the paper _[Rethinking the Inception Architecture for Computer Vision](https://arxiv.org/abs/1512.00567)_, published in 2015. It was co-authored by Christian Szegedy, Vincent Vanhoucke, Sergey Ioffe, and Jonathon Shlens.

In comparison to VGGNet, Inception Networks (GoogLeNet/Inception v1) have proved to be more computationally efficient, both in terms of the number of parameters generated by the network and the economical cost incurred (memory and other resources). If any changes are to be made to an Inception Network, care needs to be taken to make sure that the computational advantages aren’t lost. Thus, the adaptation of an Inception network for different use cases turns out to be a problem due to the uncertainty of the new network’s efficiency. In an Inception v3 model, several techniques for optimizing the network have been put suggested to loosen the constraints for easier model adaptation. The techniques include factorized convolutions, regularization, dimension reduction, and parallelized computations.

### [Inception v3 Architecture](#inception-v3-architecture)[](#inception-v3-architecture)

The architecture of an Inception v3 network is progressively built, step-by-step, as explained below:

1.  Factorized Convolutions: This helps to reduce computational efficiency by reducing the number of parameters involved in a network. It also checks network efficiency.
2.  Smaller convolutions: replacing bigger convolutions with smaller convolutions leads to faster training. Say a 5 × 5 filter has 25 parameters; two 3 × 3 filters replacing a 5 × 5 convolution has only 18 (3_3 + 3_3) parameters instead.

![image](https://lh4.googleusercontent.com/5kvo-MJ98DY8oLlSaUr-1L-cLAjz_rv6uOTxDD77ezXUJ9Brbf_4-wdKE-FztMeH-oYr2Y_-zV49i2Ty1dk33qOJuj8PSXiUvvJrCVA6dHeV1HW_Xb9iQyh5oPiUAd1Y-z01jCWp)

In the middle we see a 3x3 convolution, and below a fully-connected layer. Since both 3x3 convolutions can share weights among themselves, the number of computations can be reduced. 3. Asymmetric convolutions: A 3 × 3 convolution could be replaced by a 1 × 3 convolution followed by a 3 × 1 convolution. If a 3 × 3 convolution is replaced by a 2 × 2 convolution, the parameters would be slightly higher than the asymmetric convolution proposed.

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/content/images/2020/03/image-8.png) 4. Auxiliary classifier: An auxiliary classifier is a small CNN inserted between layers during training, and the loss incurred is added to the main network loss. In GoogLeNet, auxiliary classifiers were used for a deeper network, whereas in Inception v3, an auxiliary classifier acts as a regularizer.

![image](https://lh5.googleusercontent.com/_CDdEfLjUSsJZT71fGeYgLGnVzpmjMirbfVDkjpfwOX4wjCXEZc2YNmjrs8o24nzCmhO_OlejGpbEEzE9AlyZsJyc03iIfyHkSbvjbnAJMk1Q4cUqzy9KGCYyE8qyqq-Y8Z7xr_w) 5. Grid size reduction: Grid size reduction is usually done by pooling operations. However, to combat the bottlenecks of computational cost, a more efficient technique is proposed:

![image](https://lh6.googleusercontent.com/rHBAmNtz7QWCrCWHMCqdi4PPKaYV43T4mm0yS6tu1XqjM0wWfqtplEe7GSKkPBAQ0L8wZmGd9nVmYxSrn4uDAAO0WhqVjUUYEJGUiz68y-A52RsMWcy8Y58IIywrJbmcSFgwn7YU)

All the above concepts are consolidated into the final architecture.

![image](https://lh3.googleusercontent.com/bA_Rkj4a0sA3NZ1wjUYIO5_eq0hUmiBNbagOFb84C8Y9GxeedGUYNd-LIbhAlpW-1o8xSeNypMnbD6p-XsrAQvup3FeWXrAoZig7l7Y9WIK3uDHooEMEKiNNQ2qt0PfA4Zfsyltn)

[Inception v3 Training and Results](#inception-v3-training-and-results)[](#inception-v3-training-and-results)
-------------------------------------------------------------------------------------------------------------

Inception v3 was trained on ImageNet and compared with other contemporary models, as shown below.

![image](https://lh4.googleusercontent.com/5NiSkThJ0D41zX-aY7GF27r8Iu-d0DmhAWKR_b_MCRP0mN9hx-oEGuxrlJup9W2NpoyofrcRgTEFG_DYamlNtXeEL5lHibW7N9NQXd4uP7ql-PLgtn1fmaJJ98bBTUEqxGYLf9bZ)

As shown in the table, when augmented with an auxiliary classifier, factorization of convolutions, RMSProp, and Label Smoothing, Inception v3 can achieve the lowest error rates compared to its contemporaries.

Below are a few relevant links if you’re looking to implement Inception v3 yourself:

1.  [Link to the Original Research Paper](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Szegedy_Rethinking_the_Inception_CVPR_2016_paper.pdf)
2.  [PyTorch Implementation of Inception v3](https://pytorch.org/hub/pytorch_vision_inception_v3/)

[SqueezeNet (2016)](#squeezenet-2016)[](#squeezenet-2016)
---------------------------------------------------------

[SqueezeNet](https://arxiv.org/abs/1602.07360) is a smaller network designed as a more compact replacement for [AlexNet](https://paperswithcode.com/method/alexnet). It has almost 50x fewer parameters than AlexNet, yet it performs 3x faster. Researchers at DeepScale, The University of California, Berkeley, and Stanford University in 2016 proposed this architecture. It was first published in their paper titled [SqueezeNet](https://arxiv.org/abs/1602.07360): AlexNet-level accuracy with 50x fewer parameters and <0.5MB model size.

Below are the key ideas behind SqueezeNet:

-   Strategy One: use 1 × 1 filters instead of 3 × 3.
-   Strategy Two: decrease the number of input channels to 3 × 3 filters.
-   Strategy Three: Downsample late in the network so that convolution layers have large activation maps.

### [SqueezeNet Architecture and Results](#squeezenet-architecture-and-results)[](#squeezenet-architecture-and-results)

The SqueezeNet architecture comprises “squeeze” and “expand” layers. A squeeze convolutional layer has only a 1 × 1 filter. As shown below, these are fed into an expand layer with a mix of 1 × 1 and 3 × 3 convolution filters.

![image](https://lh3.googleusercontent.com/Lxq1Zbm2x3SI_fxB4N4clK9vo9OJwC4tnRkB1Me9dBxgVsi2PRX_DFIA0b_iWMg38lQPgK6bon7vNwj9AYjaL5pC5Ob9nSN11yibHpRgFQfrc_JNjqPoDW8iRr7gJzjQ8GOBdws9)

### [A “Fire Module”](#a-fire-module)[](#a-fire-module)

The paper’s authors use the term “fire module” to describe a combination of a squeeze and expand layers.

An input image is first sent into a standalone convolutional layer. This layer is followed by 8 “fire modules” named “fire2-9”, according to Strategy One above. The illustration of the resulting SqueezeNet is shown below.

![image](https://lh3.googleusercontent.com/VhO6tJF6bytSPbnRlAT6KyyRrCx8Urs3nx9tiu4m9pDMYlVlLRSaK0-6MFDbHTDP1s35-g9dlrSVHQrbrXmisGEF8URaEljP3cpB729TzCBmNM-T3J4pvViMC1LFPmUHgfsUw_wU)

From left to right: SqueezeNet, SqueezeNet with simple bypass, and SqueezeNet with complex bypass.

Following Strategy Two, the filters per fire module are increased with a “simple bypass.” Lastly, SqueezeNet performs max-pooling with a stride of 2 after layers conv1, fire4, fire8, and conv10. According to Strategy Three, pooling is given a relatively late placement, resulting in SqueezeNet with a “complex bypass” (the rightmost architecture in the image above.)

Below is an image showing how SqueezeNet compares with the original AlexNet.

![image](https://lh6.googleusercontent.com/t5mofgN7bvSINXxaYTlboz6MA24QNZgHQ456CdZXcrnbsLxD94bASk1jX8RWT_PRYd7JDCWZWJIloo9ofYw9et7_-a_C9-WUr2IQL4-BRQ8fnkvkfaY4W1stQ_b0Yz8hg1gbrpPB)

As we can observe, the weights of the compressed model for AlexNet were 240MB and achieved 80.3% accuracy. Meanwhile, a Deep Compression SqueezeNet consumes 0.47MB of memory and achieves the same performance.

Below are the details of other parameters used in the network:

-   The ReLU activation is applied between all the squeeze and expand layers inside the fire module.
-   Dropout layers are added to reduce overfitting, with a probability of 0.5 after the fire9 module.
-   There are no fully connected layers used in the network. This design choice was inspired by the [Network In Netowork (NIN)](https://arxiv.org/abs/1312.4400) architecture proposed by (Lin et al, 2013).
-   SqueezeNet was trained with a learning rate of 0.04, which is linearly decreased throughout the training process.
-   The batch size for training is 32, and the network used an Adam Optimizer.

SqueezeNet makes the deployment process easier due to its small size. Initially this network was implemented in Caffe, but the model has since gained in popularity and has been adopted to many different platforms.

Below are a few relevant links for implementing SqueezeNet on your own, or further investigating the original implementation:

1.  [Link to the Original Implementation of SqueezeNet](https://github.com/forresti/SqueezeNet)
2.  [Link to the Research Paper](https://arxiv.org/pdf/1602.07360.pdf)
3.  [SqueezeNet in Tensorflow](https://github.com/vonclites/squeezenet)
4.  [SqueezeNet in PyTorch](https://github.com/pytorch/vision/blob/master/torchvision/models/squeezenet.py)

#### [Source](https://www.digitalocean.com/community/tutorials/popular-deep-learning-architectures-resnet-inceptionv3-squeezenet)

<br/>
---
