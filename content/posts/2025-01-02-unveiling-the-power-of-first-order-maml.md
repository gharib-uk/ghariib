---
title: "Unveiling the Power of First-Order MAML Algorithm in Meta-Learning"
date: 2025-01-02T14:23:10.123Z
draft: false
type: posts
categories: 
- ai-ml
---
# Unveiling the Power of First-Order MAML Algorithm in Meta-Learning

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

In the field of machine learning, the ability to quickly adapt to new tasks with limited data is a highly sought-after skill. We call it meta-learning or learning to learn and its goal is to train models on similar tasks so they can quickly get the hang of new ones. Meta-learning uses a meta-learner to swiftly adapt algorithms without having to do as much manual transferring. One popular meta-learning approach is First-Order MAML (FOMAML).

It’s based on Model-Agnostic Meta-Learning (MAML). The aim is to enable fast adaptation of deep networks to new tasks. FOMAML achieves this by omitting second derivatives. From a deep learning perspective, meta-learning rocks for three reasons. First, you can learn from just a few examples. Second you can get up to speed on new tasks quickly. And third, you can build systems that generalize better.

In this post, we’ll check out how FOMAML works, its pluses and how to implement it in PyTorch on the MNIST dataset.

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

-   **Basic Knowledge of Machine Learning:** Familiarity with supervised learning and optimization concepts.
-   **Descent Basics:** Understanding how optimization works.
-   **Mathematical Notation:** Comfort with notations like partial derivatives and matrix operations.

[Model-Agnostic Meta-Learning (MAML) algorithm](#model-agnostic-meta-learning-maml-algorithm)[](#model-agnostic-meta-learning-maml-algorithm)
---------------------------------------------------------------------------------------------------------------------------------------------

Meta-learning algorithms like MAML are super interesting! Basically, you’ve got a model that’s defined by parameters - θ. When you want the model to adapt to some new task (T i), it updates its parameters to θ’i and this updated version is found by taking a step or more on the loss for that particular task Ti. So, with just one step, the new parameters are θ’i = θ - α∇θℒTi(fθ), where α is the learning rate, and ∇θℒTi(fθ) represents the gradient of the loss ℒTi computed on task Ti with respect to the model’s parameters θ.

Meta-optimization works by optimizing the model parameters θ, while the objective is computed using the updated model parameters θ’. MAML tries to set up the initialization parameters, so that just a couple adjustments on a new task will provide good performance.

The MAML algorithm has been widely studied and applied in various domains. It’s performed when you must quickly get used to new stuff without much labeled data. By learning some initialization parameters that make quick adaption easier, MAML lets models adapt to new tasks without a ton of fine-tuning.

[Understanding First-Order MAML](#understanding-first-order-maml)[](#understanding-first-order-maml)
----------------------------------------------------------------------------------------------------

In meta-learning, the Model-Agnostic Meta-Learning (MAML) method has proved to be effective. However, computing second derivatives in the original MAML approach can be time-consuming and resource-intensive. To overcome this shortcoming, improvements have led to the creation of the First-Order MAML (FOMAML) algorithm. Without the need for the second derivative terms, the computation is simplified in FOMAML, leading to a cheaper and more efficient implementation. FOMAML is faster and more efficient than the original MAML method since it disregards second derivatives and focuses only on first-order s.

Let’s consider a typical few-shot learning scenario where there are two levels of training. The outer loop consists of meta-training, while the inner loop involves task-specific training. In the outer loop, the model is trained on a set of tasks T, where each task contains a small support set S and query set Q. The objective is to help the model to adapt quickly to new tasks in the inner loop by updating its initial parameters θ.

Using the support set S for each task t∈T, the model updates its parameters. Afterwards, it measures its performance using the query set Q. This process is repeated across all t∈T.

Now, let’s dive into the mathematics of First-Order MAML:

Initialization:

Set the initial parameter vector as θ.

Outer loop:

For each task t in T:

-   Sample a support set S\_t and a query set Q\_t for task t.
-   Compute the loss L\_t(θ) on support set S\_t using the current parameters θ.
-   Update the model’s parameters using descent: θ’ = θ - α \* ∂L\_t(θ)/∂θ, where α is the learning rate.
-   Compute the loss L’\_t(θ’) on query set Q\_t using the updated parameters θ’.
    -   Accumulate the losses L’\_t(θ’) across all tasks.

Inner loop:

Compute the of the accumulated losses with respect to the initial parameters θ

-   ∇θ = ∂(Σ\_t L’\_t(θ’))/∂θ.

Update the initial parameters

-   θ ← θ - β \* ∇θ, where β is the meta- step size.

Repeat steps 2-4 for multiple outer loop iterations until convergence

In summary, First-Order MAML optimizes the model’s initial parameters through an iterative process of updating the parameters based on the losses computed on the support sets and query sets of multiple tasks. This enables the model to quickly adapt to new tasks with minimal training examples.

[Difference between MAML and First-Order MAML](#difference-between-maml-and-first-order-maml)[](#difference-between-maml-and-first-order-maml)
----------------------------------------------------------------------------------------------------------------------------------------------

1.MAML: We have to perform two steps during the adaptation process in MAML. In the first stage, we take the derivative of the loss function respectively with respect to model parameters (∇\_θ L(D\_train, θ)). Then, we change model parameters by updating them with the help of descent algorithm at a learning rate β to get adapted parameters (θ’ = θ - β \* ∇\_θ L(D\_train, θ)). The second step involves computing loss function’s derivative on test set for adapted parameters (∇\_θ’ L(D\_test, θ’)). Lastly, we update original model parameters using 2nd order information (∇\_θ L(D\_test, θ’)). 2.First-Order MAML: First Order-MAML just takes one step during adaptation. We compute the of loss function on training set w.r.t. model parameter (∇\_θ L(D\_train, θ)) and directly update model’s parameter using descent algorithm where learning rate is β (θ’ = θ - β \* ∇\_θ L(D\_train, θ)). There is no second-order computation in First-Order MAML.

[FOMAML with PyTorch and the MNIST dataset](#fomaml-with-pytorch-and-the-mnist-dataset)[](#fomaml-with-pytorch-and-the-mnist-dataset)
-------------------------------------------------------------------------------------------------------------------------------------

Here are the steps that may be taken to show how FOMAML can be used with PyTorch and the MNIST dataset:

[Load the MNIST dataset](#load-the-mnist-dataset)[](#load-the-mnist-dataset)
----------------------------------------------------------------------------

PyTorch’s datasets.MNIST class offers a simple means of loading the MNIST dataset. This class is in charge of importing the dataset and carrying out any necessary preprocessing, such as transforming the images into tensors.

```
import torch
from torchvision import datasets, transforms

# Define the transformation to apply to the images
transform = transforms.Compose([
    transforms.ToTensor(),
    transforms.Normalize((0.5,), (0.5,))
])

# Load the MNIST dataset
train_dataset = datasets.MNIST(root='./data', train=True, transform=transform, download=True)
test_dataset = datasets.MNIST(root='./data', train=False, transform=transform)
```

-   First, we imported PyTorch and the datasets and transforms modules from torchvision. These provide tools for loading data and preprocessing it.
-   Then we defined a function to normalize the image data that I would pass into the model. By normalizing, We put all the data on the same scale so it’s easier for the model to learn. We used the ToTensor() method to convert the images to tensors and Normalize() method to standardize the values.
-   After that, we loaded the MNIST dataset using the datasets module. This contains thousands of examples of handwritten digits. We split it into training and test sets, and applied the normalization function to each.

[Define the model architecture](#define-the-model-architecture)[](#define-the-model-architecture)
-------------------------------------------------------------------------------------------------

For this example, let’s use a simple multi-layer perceptron (MLP) as the base model. We can define the model using the `torch.nn` module.

```
import torch.nn as nn

class MLP(nn.Module):
    def __init__(self):
        super(MLP, self).__init__()
        self.fc1 = nn.Linear(784, 256)
        self.fc2 = nn.Linear(256, 10)

    def forward(self, x):
        x = x.view(x.size(0), -1)
        x = self.fc1(x)
        x = nn.functional.relu(x)
        x = self.fc2(x)
        return x

model = MLP()
```

-   In the above code, we used a simple multilayer perceptron with two linear layers. The first layer converts the image pixels into a 256-dimensional representation. The second layer then maps that to the final 10 output classes.
-   In the forward pass, we flattened the image tensor, passed it through each linear layer, and applied the ReLU activation function after the first one. This introduces nonlinearity into the model.
-   Finally, we instantiated the MLP model class.

[Define the FOMAML training loop](#define-the-fomaml-training-loop)[](#define-the-fomaml-training-loop)
-------------------------------------------------------------------------------------------------------

The example below is a basic example to demonstrate the concept of meta-learning using the MNIST dataset. You may need to modify and adapt this code to suit your specific requirements or experiment with different architectures, hyperparameters, or techniques for better performance.

```
import torch.optim as optim

# Define the FOMAML training loop
def fomaml_train(model, train_dataset, num_iterations, num_inner_updates, inner_lr, meta_lr):
    optimizer = optim.SGD(model.parameters(), lr=meta_lr)  # Initialize the optimizer with the meta-learning rate
    loss_fn = nn.CrossEntropyLoss()  # Define the loss function as CrossEntropyLoss

    for iteration in range(num_iterations):
        model_copy = MLP()  # Create a copy of the model for each iteration
        model_copy.load_state_dict(model.state_dict())  # Initialize the copy with the current model parameters

        for task in train_dataset:
            task_inputs, task_labels = task ## The task-specific inputs and labels are retrieved from the task
            task_labels = torch.tensor(task_labels, dtype=torch.long)  # Convert task_labels to a tensor with the appropriate data type

            # Fine-tune the model on the task using  descent
            task_optimizer = optim.SGD(model_copy.parameters(), lr=inner_lr)  # Initialize the task-specific optimizer with the inner learning rate
            for inner_update in range(num_inner_updates):
                task_optimizer.zero_grad()
                task_outputs = model_copy(task_inputs)
                task_labels = task_labels.view(-1)  # Reshape the labels to have the appropriate shape
                loss = loss_fn(task_outputs, task_labels)  # Calculate the loss between task_outputs and task_labels
                loss.backward()  # Backpropagate the s
                task_optimizer.step()  # Update the model parameters using the task-specific optimizer

        # Update the meta-learner using the s from the fine-tuned models
        optimizer.zero_grad()
        meta_outputs = model_copy(task_inputs)
        meta_loss = loss_fn(meta_outputs, task_labels)  # Calculate the loss between meta_outputs and task_labels
        meta_loss.backward()  # Backpropagate the s
        optimizer.step()  # Update the meta-learner model parameters using the optimizer

        if (iteration + 1) % 10 == 0:
            print(f"Iteration {iteration + 1}: Meta Loss = {meta_loss.item()}")

# Run the FOMAML training loop
fomaml_train(model, train_dataset, num_iterations=100, num_inner_updates=5, inner_lr=0.01, meta_lr=0.001)

```

-   In the above code, we use PyTorch to train a neural network model with a meta-learning algorithm called FOMAML. We start by importing the PyTorch optimizer and defining the FOMAML training loop function.
-   The function takes in the model, training data, number of iterations, inner updates inner learning rate, and meta learning rate. It initializes an optimizer using the meta learning rate.
-   Then it loops through the number of iterations. For each iteration, it makes a copy of the model to train and it loads the current model weights into the copy.
-   Inside this loop, it loops through each task in the training data. It gets the inputs and labels for the task. The labels are converted to tensors to match the model.
-   Then it fine-tunes the copy of the model on the task using descent. It initializes a task-specific optimizer with the inner learning rate.
-   It loops through the number of inner updates performing a forward pass, computing the loss, backpropagating, and updating the model copy’s weights.

After fine-tuning on the task the updated model copy weights are used to improve the original model. So it’s learning on lower-level tasks to improve performance on higher-level tasks.

The code shows the overall FOMAML algorithm flow using PyTorch to handle much of the calculations and weight updating. It’s a common technique for fast adaptation in meta-learning. FOMAML can take the knowledge from seeing all those MNIST examples and really quickly adapt to new digits with just a few labeled samples to learn from.

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

In this article, we delved into the fascinating realm of meta-learning, a coveted skill in the field of machine learning that enables models to swiftly adapt to new tasks with limited data. Specifically, we looked at the First-Order MAML (FOMAML) algorithm which is a fine-tuned version of Model-Agnostic Meta-Learning (MAML). FOMAML is unique by simplifying computations, avoiding second derivatives and having a more efficient implementation than the original MAML method.

The article contained an extensive exposition on MAML algorithm highlighting its importance in cases where quick adaptation to new tasks is essential especially when there are very few labels available. We elucidated the mechanics of FOMAM, showing that its efficiency can be traced by first-order s only. Even though information may be misleadingly low, experiments have shown that FOMAML can rival MAML in performance making it viable especially when minimizing costs in computing is a priority.

In order to make these theories practical enough, the article presented a step by step guide on implementing FOMAML using MNIST dataset within PyTorch. The code demonstrates how to load the MNIST dataset and define a simple multi-layer perceptron (MLP) model as well as the FOMAML training loop. This example helped readers understand how FOMAML uses prior knowledge from similar tasks to quickly adapt to new challenges thereby displaying its potential as meta-learning application.

#### [Source](https://www.digitalocean.com/community/tutorials/first-order-maml-algorithm-in-meta-learning)

<br/>
---
