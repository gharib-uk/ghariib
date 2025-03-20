---
title: "Understand K-Fold Cross-ValidationA Step-by-Step Guide"
date: 2025-02-05T14:25:49.000Z
draft: false
type: posts
categories: 
- ai-ml,gpu
---
# Understand K-Fold Cross-ValidationA Step-by-Step Guide

<br/>

<br/>
[**📖 Introduction**](#introduction)[](#introduction)
-----------------------------------------------------

When a machine learning model learns about the data, it often performs well with the training data but underperforms with unseen data or the test data. This is known as model overfitting. Model overfitting occurs when the model hugs the training data well, and underfitting occurs when the model does not perform well, even with the training data.

Cross-validation is one of the techniques that ensures that the machine learning model generalizes well to unseen data. It works as follows:

1.  **Splitting Data into Folds:** Any given dataset is divided into multiple subsets, known as “folds.”
2.  **Training & Validation Cycles:** The model is trained on a subset of the data, and one fold is used for validation. This process repeats, with a different fold used each time.
3.  **Averaging Results:** The performance metrics from each validation step are averaged to provide a more reliable estimate of the model’s effectiveness.

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Shaoni/5-feb/bias-variance%20tradeoff.png)

[Image Source](https://developer.download.nvidia.com/video/gputechconf/gtc/2019/presentation/s9817-rapids-cuml-a-library-for-gpu-accelerated-machine-learning.pdf)

[📌 Prerequisites](#prerequisites)[](#prerequisites)
----------------------------------------------------

**Basic Knowledge of Machine Learning** – Understanding [model training](/community/tutorials/how-to-train-question-answering-machine-learning-models), evaluation metrics, and overfitting.  
**Python Programming Skills** – Familiarity with Python and libraries like [`scikit-learn`](/community/tutorials/python-scikit-learn-tutorial), [`numpy`](/community/tutorials/numpy-matrix-multiplication), and [`pandas`](/community/tutorials/python-pandas-module-tutorial).  
**Dataset Preparation** – A cleaned and preprocessed dataset ready for model training.  
**Scikit-Learn Installed** – Install it using `pip install scikit-learn` if not already available.  
**Understanding of Model Performance Metrics** – Knowledge of accuracy, precision, recall, RMSE, etc., depending on the task.

[🚀 Common Cross-Validation Methods](#common-cross-validation-methods)[](#common-cross-validation-methods)
----------------------------------------------------------------------------------------------------------

-   **K-Fold Cross-Validation:** The dataset is split into _k_ equal parts, and the model is trained _k_ times, each time using a different fold is used as the validation set.
-   **Stratified K-Fold:** This method ensures that each fold maintains the same proportion of classes in classification problems. It is often used when the target variable data is imbalanced, i.e., when the target variable is a categorical column, and the classes are not distributed equally.
-   **Leave-One-Out (LOO):** This method uses only one instance for validation while training on the rest, repeating for all instances.
-   **Time-Series Cross-Validation:** Used for sequential data, ensuring training data precedes validation data.

Cross-validation helps in selecting the best model and hyperparameters while preventing overfitting.

In this guide, we’ll explore:

-   What K-Fold Cross-Validation is
-   How it compares to a traditional train-test split
-   Step-by-step implementation using `scikit-learn`
-   Advanced variations like **Stratified K-Fold, Group K-Fold, and Nested K-Fold**
-   Handling imbalanced datasets

[**🤔 What is K-Fold Cross-Validation?**](#what-is-k-fold-cross-validation)[](#what-is-k-fold-cross-validation)
---------------------------------------------------------------------------------------------------------------

K-Fold Cross-Validation is a resampling technique used to evaluate machine learning models by splitting the dataset into **K** equal-sized folds. The model is trained on **K-1** folds and validated on the remaining fold, repeating the process **K** times. The final performance score is the average of all iterations.

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2024/Shaoni/5-feb/k-fold.png)

### [**Why Use K-Fold Cross-Validation?**](#why-use-k-fold-cross-validation)[](#why-use-k-fold-cross-validation)

-   Unlike a single train-test split, K-Fold uses multiple splits, reducing the variance in performance estimates. Hence, the model becomes more capable of making predictions about unseen datasets.
-   Each data point is used for training and validation, maximizing available data and leading to more robust performance evaluation.
-   Since the model is validated multiple times across different data segments, it helps detect and mitigate overfitting. This ensures that the model does not memorize specific training samples but generalizes well to new data.
-   By averaging results across multiple folds, K-Fold Cross-Validation provides a more reliable estimate of the model’s true performance, reducing bias and variance.
-   K-Fold Cross-Validation is often used in combination with grid search and randomized search to find optimal hyperparameters without overfitting to a single train-test split.

[🔍 K-Fold vs. Train-Test Split](#k-fold-vs-train-test-split)[](#k-fold-vs-train-test-split)
--------------------------------------------------------------------------------------------

Aspect

K-Fold Cross-Validation

Train-Test Split

Data Utilization

Data is divided into multiple folds, ensuring that each data point has a chance to be part of both the training and validation sets across different iterations.

Divide the data into fixed portions for training and testing.

Bias-Variance Tradeoff

It reduces variance as the model is trained multiple times on unseen data; hence, the optimal bias-variance tradeoff is achieved.

There is a chance of high variance with the plain train-test split. This often occurs because the model hugs the training data well and often fails to understand the test data.

Overfitting Risk

Low risk of overfitting as the model gets tested across different folds.

Higher risk of overfitting if the train-test split is not representative.

Performance Evaluation

Provides a more reliable and generalized performance estimate.

Performance depends on a single train-test split, which may be biased.

[🏁 Implementing K-Fold Cross-Validation in Python](#implementing-k-fold-cross-validation-in-python)[](#implementing-k-fold-cross-validation-in-python)
-------------------------------------------------------------------------------------------------------------------------------------------------------

Let’s implement K-Fold Cross-Validation using `scikit-learn`.

### [**Step 1: Import Dependencies**](#step-1-import-dependencies)[](#step-1-import-dependencies)

First, we will start by importing the necessary libraries.

```
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from sklearn.model_selection import KFold, cross_val_score, train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.preprocessing import LabelEncoder
from sklearn import linear_model, tree, ensemble
```

### [**Step 2: Load and Explore the Titanic Dataset**](#step-2-load-and-explore-the-titanic-dataset)[](#step-2-load-and-explore-the-titanic-dataset)

For this demo, we will use the Titanic dataset, a very famous dataset that will help us understand how to perform k-fold cross-validation.

```
df = pd.read_csv("https://raw.githubusercontent.com/datasciencedojo/datasets/master/titanic.csv")
print(df.head(3))
print(df.info())
```

```
  PassengerId  Survived  Pclass  \
0            1         0       3   
1            2         1       1   
2            3         1       3   

                                                Name     Sex   Age  SibSp  \
0                            Braund, Mr. Owen Harris    male  22.0      1   
1  Cumings, Mrs. John Bradley (Florence Briggs Th...  female  38.0      1   
2                             Heikkinen, Miss. Laina  female  26.0      0   

   Parch            Ticket     Fare Cabin Embarked  
0      0         A/5 21171   7.2500   NaN        S  
1      0          PC 17599  71.2833   C85        C  
2      0  STON/O2. 3101282   7.9250   NaN        S  
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 891 entries, 0 to 890
Data columns (total 12 columns):
 #   Column       Non-Null Count  Dtype  
---  ------       --------------  -----  
 0   PassengerId  891 non-null    int64  
 1   Survived     891 non-null    int64  
 2   Pclass       891 non-null    int64  
 3   Name         891 non-null    object 
 4   Sex          891 non-null    object 
 5   Age          714 non-null    float64
 6   SibSp        891 non-null    int64  
 7   Parch        891 non-null    int64  
 8   Ticket       891 non-null    object 
 9   Fare         891 non-null    float64
 10  Cabin        204 non-null    object 
 11  Embarked     889 non-null    object 
dtypes: float64(2), int64(5), object(5)
memory usage: 83.7+ KB
None
```

### [**Step 3: Data Preprocessing**](#step-3-data-preprocessing)[](#step-3-data-preprocessing)

**Now, it is a great practice to start with data processing and feature engineering before building any model.**

```
df = df[['Survived', 'Pclass', 'Sex', 'Age', 'SibSp', 'Parch', 'Fare']]  # Select relevant features
df.dropna(inplace=True)  # Remove missing values

# Encode categorical variable
label_encoder = LabelEncoder()
df['Sex'] = label_encoder.fit_transform(df['Sex'])

# Split features and target
X = df.drop(columns=['Survived'])
y = df['Survived']
df.shape
```

(714, 7)

### [**Step 4: Define the K-Fold Split**](#step-4-define-the-k-fold-split)[](#step-4-define-the-k-fold-split)

```
kf = KFold(n_splits=5, shuffle=True, random_state=42)
```

Here, we specify `n_splits=5`, meaning the data is divided into **five folds**. The `shuffle=True` ensures randomness.

### [**Step 5: Train and Evaluate the Model**](#step-5-train-and-evaluate-the-model)[](#step-5-train-and-evaluate-the-model)

```
model = RandomForestClassifier(n_estimators=100, random_state=42)
scores = cross_val_score(model, X, y, cv=kf, scoring='accuracy')
print(f'Cross-validation accuracy scores: {scores}')
print(f'Average Accuracy: {np.mean(scores):.4f}')
```

**Cross-validation accuracy scores: \[0.77622378 0.8041958 0.79020979 0.88111888 0.80985915\]** **Average Accuracy: 0.8123**

```
score = cross_val_score(tree.DecisionTreeClassifier(random_state= 42), X, y, cv= kf, scoring="accuracy")  
print(f'Scores for each fold are: {score}')  
print(f'Average score: {"{:.2f}".format(score.mean())}')
```

**Scores for each fold are: \[0.72727273 0.79020979 0.76923077 0.81818182 0.8028169\]** **Average score: 0.78**

[**⚡ Advanced Cross-Validation Techniques**](#advanced-cross-validation-techniques)[](#advanced-cross-validation-techniques)
----------------------------------------------------------------------------------------------------------------------------

### [**1\. Stratified K-Fold (For Imbalanced Datasets)**](#1-stratified-k-fold-for-imbalanced-datasets)[](#1-stratified-k-fold-for-imbalanced-datasets)

For datasets with imbalanced classes, **Stratified K-Fold** ensures each fold has the same class distribution as the full dataset. This distribution of classes makes it the ideal choice for imbalance classification problems.

```
from sklearn.model_selection import StratifiedKFold
skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(model, X, y, cv=skf, scoring='accuracy')
print(f'Average Accuracy (Stratified K-Fold): {np.mean(scores):.4f}')
```

### [**Average Accuracy (Stratified K-Fold): 0.8124**](#average-accuracy-stratified-k-fold-0-8124)[](#average-accuracy-stratified-k-fold-0-8124)

### [**2\. Repeated K-Fold Cross-Validation**](#2-repeated-k-fold-cross-validation)[](#2-repeated-k-fold-cross-validation)

**Repeated K-Fold** runs K-Fold multiple times with different splits to further reduce variance. This is usually done when the data is simple and models such as logistic regression can be fitted into the data set.

```
from sklearn.model_selection import RepeatedKFold
rkf = RepeatedKFold(n_splits=5, n_repeats=10, random_state=42)
scores = cross_val_score(model, X, y, cv=rkf, scoring='accuracy')
print(f'Average Accuracy (Repeated K-Fold): {np.mean(scores):.4f}')
```

### [**Average Accuracy (Repeated K-Fold): 0.8011**](#average-accuracy-repeated-k-fold-0-8011)[](#average-accuracy-repeated-k-fold-0-8011)

### [**3\. Nested K-Fold Cross-Validation (For Hyperparameter Tuning)**](#3-nested-k-fold-cross-validation-for-hyperparameter-tuning)[](#3-nested-k-fold-cross-validation-for-hyperparameter-tuning)

Nested K-Fold performs hyperparameter tuning within the inner loop while evaluating performance in the outer loop, reducing overfitting.

```
from sklearn.model_selection import GridSearchCV, cross_val_score
param_grid = {'n_estimators': [50, 100, 150], 'max_depth': [None, 10, 20]}
gs = GridSearchCV(model, param_grid, cv=5)
scores = cross_val_score(gs, X, y, cv=5)
print(f'Average Accuracy (Nested K-Fold): {np.mean(scores):.4f}')
```

### [**4\. Group K-Fold (For Non-Independent Samples)**](#4-group-k-fold-for-non-independent-samples)[](#4-group-k-fold-for-non-independent-samples)

If your dataset has groups (e.g., multiple images from the same patient), **Group K-Fold** ensures samples from the same group are not split across training and validation, which is useful for hierarchical data.

```
from sklearn.model_selection import GroupKFold
gkf = GroupKFold(n_splits=5)
groups = np.random.randint(0, 5, size=len(y))
scores = cross_val_score(model, X, y, cv=gkf, groups=groups, scoring='accuracy')
print(f'Average Accuracy (Group K-Fold): {np.mean(scores):.4f}')
```

[**💡 FAQs**](#faqs)[](#faqs)
-----------------------------

### [**How to run K-Fold Cross-Validation in Python?**](#how-to-run-k-fold-cross-validation-in-python)[](#how-to-run-k-fold-cross-validation-in-python)

Use `cross_val_score()` from `scikit-learn` with `KFold` as the `cv` parameter.

### [**What’s the difference between K-Fold and Stratified K-Fold?**](#what-s-the-difference-between-k-fold-and-stratified-k-fold)[](#what-s-the-difference-between-k-fold-and-stratified-k-fold)

K-Fold randomly splits data, whereas Stratified K-Fold maintains class balance in each fold.

### [**How do I choose the right number of folds?**](#how-do-i-choose-the-right-number-of-folds)[](#how-do-i-choose-the-right-number-of-folds)

-   **5- or 10-fold** is standard for most cases.
-   **Higher folds (e.g., 20)** reduce bias but increase computation time.

### [**What does the `KFold` class do in Python?**](#what-does-the-kfold-class-do-in-python)[](#what-does-the-kfold-class-do-in-python)

It divides the dataset into `n_splits` folds for training and validation.

[🔚 Conclusion](#conclusion)[](#conclusion)
-------------------------------------------

To ensure that any [machine learning model](/community/tutorials/an-introduction-to-machine-learning) that you are building performs best when provided with unseen data. Cross-validation becomes a crucial step to make the model reliable. –Fold cross-validation is one of the best ways to make sure that the model does not overfit the training data, hence maintaining the [bias-variance](/community/tutorials/pytorch-101-advanced) tradeoff. Dividing the data into different folds and training and validating the model iteratively through each phase provides a better estimate of how the model will perform when provided withan unknown dataset.

In Python, implementing K-Fold Cross-Validation is straightforward using libraries like `scikit-learn`, which offers `KFold` and `StratifiedKFold` for handling imbalanced datasets. Integrating K-Fold Cross-Validation into your workflow allows you to fine-tune hyperparameters effectively, compare models with confidence, and enhance generalization for real-world applications.

Whether building a regression, classification, or deep learning models, this validation approach is a key component for machine learning pipelines.

[References](#references)[](#references)
----------------------------------------

-   [A Gentle Introduction to k-fold Cross-Validation](https://machinelearningmastery.com/k-fold-cross-validation/)
-   [A Comprehensive Guide to K-Fold Cross Validation](https://www.datacamp.com/tutorial/k-fold-cross-validation)
-   [K-Fold Cross Validation Technique and its Essentials](https://www.analyticsvidhya.com/blog/2022/02/k-fold-cross-validation-technique-and-its-essentials/)
-   [How To Build a Deep Learning Model to Predict Employee Retention Using Keras and TensorFlow](/community/tutorials/how-to-build-a-deep-learning-model-to-predict-employee-retention-using-keras-and-tensorflow)

#### [Source](https://www.digitalocean.com/community/tutorials/k-fold-cross-validation-python)

<br/>
---
