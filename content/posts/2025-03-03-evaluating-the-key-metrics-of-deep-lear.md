---
title: "Evaluating The Key Metrics of Deep Learning Models"
date: 2025-03-03T17:15:00.000Z
draft: false
type: posts
categories: 
- machine-learning,deep-learning,ai-ml,data-science
---
# Evaluating The Key Metrics of Deep Learning Models

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

In computer vision, object detection is the problem of locating one or more objects in an image. Besides the traditional object detection techniques, advanced deep learning models like [R-CNN](/community/tutorials/mask-r-cnn-in-tensorflow-2-0) and [YOLO](/community/tutorials/yolo-nas) can achieve impressive detection over different types of objects. These models accept an image as the input and return the bounding box coordinates around each detected object.

This tutorial discusses the confusion matrix and how precision, recall, and accuracy are calculated. The mAP will be discussed in another tutorial. Specifically, we’ll cover:

1.  Confusion Matrix for Binary Classification
2.  Confusion Matrix for Multi-Class Classification
3.  Calculating the Confusion Matrix with Scikit-learn
4.  Accuracy, Precision, and Recall
5.  Precision or Recall?
6.  Conclusion

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

In order to follow along with this article, you will need experience with [Python](/community/tutorial-series/how-to-code-in-python-3) code and a basic understanding of Deep Learning. We will assume that all readers have access to sufficiently powerful machines so they can run the code provided. If you do not have access to a GPU, we suggest using [DigitalOcean GPU Droplets](/products/gpu-droplets). For instructions on getting started with Python code, we recommend the [Python guide for beginners](/community/tutorial-series/how-to-code-in-python-3). This guide will help you set up your system and prepare to run beginner tutorials.

[Confusion Matrix for Binary Classification](#confusion-matrix-for-binary-classification)[](#confusion-matrix-for-binary-classification)
----------------------------------------------------------------------------------------------------------------------------------------

In binary classification, each input sample is assigned to one of two classes. Generally, these two classes are assigned labels like 1 and 0 or positive and negative. More specifically, the two class labels might be something like malignant or benign (e.g., if the problem concerns cancer classification) or success or failure (e.g., if it concerns classifying student test scores). Assume there is a binary classification problem with the classes positive and negative. Here are the labels for seven samples used to train the model. These are called the sample’s ground-truth labels.

```
positive, negative, negative, positive, positive, positive, negative
```

Note that the class labels help us humans differentiate between the different classes. The thing that is of high importance to the model is a numeric score. When feeding a single sample to the model, the model does not necessarily return a class label but rather a score. For instance, when these seven samples are fed to the model, their class scores could be:

```
0.6, 0.2, 0.55, 0.9, 0.4, 0.8, 0.5
```

Based on the scores, each sample is given a class label. How do we convert these scores into labels? We do this by using a threshold. This threshold is a hyperparameter of the model and can be defined by the user. For example, the threshold could be 0.5–then any sample above or equal to 0.5 is given the positive label. Otherwise, it is negative. Here are the predicted labels for the samples:

```
positive (0.6), negative (0.2), positive (0.55), positive (0.9), negative (0.4), positive (0.8), positive (0.5)
```

For comparison, here are both the ground truth and predicted labels. At first glance, we can see 4 correct and 3 incorrect predictions. Note that changing the threshold might give different results. For example, setting the threshold to 0.6 leaves only two incorrect predictions.

```
Ground-Truth: positive, negative, negative, positive, positive, positive, negative
Predicted   : positive, negative, positive, positive, negative, positive, positive
```

A confusion matrix is used to extract more information about model performance. It helps us visualize whether the model is “confused” in discriminating between the two classes. As seen in the next figure, it is a 2×2 matrix. The labels of the two rows and columns are Positive and Negative to reflect the two class labels. In this example, the row labels represent the ground-truth labels, while the column labels represent the predicted labels. This could be changed.

![Fig 01](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/content/images/2020/09/Fig01.jpg)

The 4 elements of the matrix (the items in red and green) represent the 4 metrics that count the number of correct and incorrect predictions the model made. Each element is given a label that consists of two words:

1.  True or False
2.  Positive or Negative

It is True when the prediction is correct (i.e., a match between the predicted and ground-truth labels) and False when there is a mismatch between the predicted and ground-truth labels. Positive or Negative refers to the predicted label.

In summary, the first word is False whenever the prediction is wrong. Otherwise, it is True. The goal is to maximize the metrics with the word True (True Positive and True Negative), and minimize the other two metrics (False Positive and False Negative). The four metrics in the confusion matrix are thus:

1.  Top-Left (True Positive): How many times did the model correctly classify a Positive sample as Positive?
2.  Top-Right (False Negative): How often did the model incorrectly classify a Positive sample as Negative?
3.  Bottom-Left (False Positive): How many times did the model incorrectly classify a Negative sample as Positive?
4.  Bottom-Right (True Negative): How many times did the model correctly classify a Negative sample as Negative?

We can calculate these four metrics for the seven predictions we saw previously. The resulting confusion matrix is given in the next figure.

![Fig 02](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/content/images/2020/09/Fig02.jpg)

This is how the confusion matrix is calculated for a binary classification problem. Now let’s see how it would be calculated for a multi-class problem.

[Confusion Matrix for Multi-Class Classification](#confusion-matrix-for-multi-class-classification)[](#confusion-matrix-for-multi-class-classification)
-------------------------------------------------------------------------------------------------------------------------------------------------------

What if we have more than two classes? How do we calculate these four metrics in the confusion matrix for a multi-class classification problem? Simple!

Assume there are 9 samples, where each sample belongs to one of three classes: _White_, _Black_, or _Red_. Here is the ground-truth data for the 9 samples.

```
Red, Black, Red, White, White, Red, Black, Red, White
```

When the samples are fed into a model, here are the predicted labels.

```
Red, White, Black, White, Red, Red, Black, White, Red
```

For easier comparison, here they are side-by-side.

```
Ground-Truth: Red, Black, Red,   White, White, Red, Black, Red,   White
Predicted:    Red, White, Black, White, Red,   Red, Black, White, Red
```

Before calculating the confusion matrix a target class must be specified. Let’s set the _Red_ class as the target. This class is marked as _Positive_, and all other classes are marked as _Negative_.

```
Positive, Negative, Positive, Negative, Negative, Positive, Negative, Positive, Negative
Positive, Negative, Negative, Negative, Positive, Positive, Negative, Negative, Positive
```

There are now only two classes again (Positive and Negative). Thus, the confusion matrix can be calculated as in the previous section. Note that this matrix is just for the _Red_ class.

![Fig 03](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/content/images/2020/09/Fig03.jpg)

For the _White_ class, replace each of its occurrences as _Positive_ and all other class labels as _Negative_. After replacement, here are the ground-truth and predicted labels. The next figure shows the confusion matrix for the _White_ class.

```
Negative, Negative, Negative, Positive, Positive, Negative, Negative, Negative, Positive
Negative, Positive, Negative, Positive, Negative, Negative, Negative, Positive, Negative
```

![Fig 04](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/content/images/2020/09/Fig04.jpg)

Similarly, here is the confusion matrix for the _Black_ class.

![Fig 05](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/content/images/2020/09/Fig05.jpg)

[Calculating the Confusion Matrix with Scikit-Learn](#calculating-the-confusion-matrix-with-scikit-learn)[](#calculating-the-confusion-matrix-with-scikit-learn)
----------------------------------------------------------------------------------------------------------------------------------------------------------------

The popular Scikit-learn library in Python has a module called [`metrics`](https://scikit-learn.org/stable/api/sklearn.metrics.html) that can calculate the metrics in the confusion matrix.

For binary-class problems the `confusion_matrix()` function is used. Among its accepted parameters, we use these two:

1.  `y_true`: The ground-truth labels.
2.  `y_pred`: The predicted labels.

The following code calculates the confusion matrix for the previously discussed binary classification example.

```
import sklearn.metrics

y_true = ["positive", "negative", "negative", "positive", "positive", "positive", "negative"]
y_pred = ["positive", "negative", "positive", "positive", "negative", "positive", "positive"]

r = sklearn.metrics.confusion_matrix(y_true, y_pred)
print(r)

array([[1, 2],
      [1, 3]], dtype=int64)
```

Note that the order of the metrics differ from that discussed previously. For example, the _True Positive_ metric is at the bottom-right corner while _True Negative_ is at the top-left corner. To fix that, we can flip the matrix.

```
import numpy

r = numpy.flip(r)
print(r)

array([[3, 1],
       [2, 1]], dtype=int64)
```

The multilabel\_confusion\_matrix () function is used to calculate the confusion matrix for a multi-class classification problem, as shown below. In addition to the y\_true and y\_pred parameters, a third parameter named labels accepts a list of the class labels.

```
import sklearn.metrics
import numpy

y_true = ["Red", "Black", "Red",   "White", "White", "Red", "Black", "Red",   "White"]
y_pred = ["Red", "White", "Black", "White", "Red",   "Red", "Black", "White", "Red"]

r = sklearn.metrics.multilabel_confusion_matrix(y_true, y_pred, labels=["White", "Black", "Red"])
print(r)

array([
      [[4 2]
      [2 1]]

       [[6 1]
        [1 1]]

        [[3 2]
         [2 2]]], dtype=int64)
```

The function calculates the confusion matrix for each class and returns all the matrices. The order of the matrices match the order of the labels in the `labels` parameter. To adjust the order of the metrics in the matrices, we’ll use the `numpy.flip()` function, as before.

```
print(numpy.flip(r[0])) # White class confusion matrix
print(numpy.flip(r[1])) # Black class confusion matrix
print(numpy.flip(r[2])) # Red class confusion matrix

# White class confusion matrix
[[1 2]
  [2 4]]

# Black class confusion matrix
[[1 1]
  [1 6]]

# Red class confusion matrix
[[2 2]
  [2 3]]
```

In the rest of this tutorial, we’ll focus on just two classes. The next section discusses three key metrics calculated using the confusion matrix.

[Accuracy, Precision, and Recall](#accuracy-precision-and-recall)[](#accuracy-precision-and-recall)
---------------------------------------------------------------------------------------------------

The confusion matrix offers four different and individual metrics, as we’ve already seen. Based on these four metrics, other metrics can be calculated which offer more information about how the model behaves:

1.  Accuracy
2.  Precision
3.  Recall

The next subsections discuss each of these three metrics.

### [Accuracy](#accuracy)[](#accuracy)

Accuracy is a metric that generally describes how the model performs across all classes. It is useful when all classes are of equal importance. It is calculated as the ratio between the number of correct predictions and the total number of predictions.

![Fig 06](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/content/images/2020/09/Fig06.jpg)

Here is how to calculate the accuracy using [Scikit-learn](/community/tutorials/python-scikit-learn-tutorial), based on the confusion matrix previously calculated. The variable _acc_ holds the result of dividing the sum of _True Positives_ and _True Negatives_ over the sum of all values in the matrix. The result is _0.5714_, which means the model is _57.14%_ accurate in making a correct prediction.

```
import numpy
import sklearn.metrics

y_true = ["positive", "negative", "negative", "positive", "positive", "positive", "negative"]
y_pred = ["positive", "negative", "positive", "positive", "negative", "positive", "positive"]

r = sklearn.metrics.confusion_matrix(y_true, y_pred)

r = numpy.flip(r)

acc = (r[0][0] + r[-1][-1]) / numpy.sum(r)
print(acc)
```

0.571

The _`sklearn.metrics`_ module has a function called _`accuracy_score()`_ that can also calculate the accuracy. It accepts the ground-truth and predicted labels as arguments.

```
acc = sklearn.metrics.accuracy_score(y_true, y_pred)
```

Note that the accuracy may be deceptive. One case is when the data is imbalanced. Assume there are 600 samples, where 550 belong to the Positive class and just 50 to the Negative class. Since most of the samples belong to one class, the accuracy for that class will be higher than for the other.

If the model made 530/550 correct predictions for the Positive class, compared to just 5/50 for the Negative class, then the total accuracy is (530 + 5) / 600 = 0.8917. This means the model is 89.17% accurate. With that in mind, for any sample (regardless of its class), the model will likely make a correct prediction 89.17% of the time. This is not valid, especially when considering the Negative class for which the model performed badly.

### [Precision](#precision)[](#precision)

The precision is calculated as the ratio between the number of _Positive_ samples correctly classified to the total number of samples classified as _Positive_ (either correctly or incorrectly). The precision measures the model’s accuracy in classifying a sample as positive.

![Fig 07](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/content/images/2020/09/Fig07.jpg)

When the model makes many incorrect Positive or few correct Positive classifications, this increases the denominator and makes the precision small. On the other hand, the precision is high when:

1.  The model makes many correct Positive classifications (maximize True Positive).
2.  The model makes fewer incorrect Positive classifications (minimize False Positive).

Imagine a man trusted by others; when he predicts something, others believe him. The precision is like this man. When the precision is high, you can trust the model when it predicts a sample as Positive. Thus, precision helps to know how accurate the model is when it says a sample is positive.

Now, let us understand what Precision is.

> **The precision reflects how reliable the model is in classifying samples as Positive**.

In the next figure, the green mark means a sample is classified as _Positive_ and a red mark means the sample is _Negative_. The model correctly classified two _Positive_ samples, but incorrectly classified one _Negative_ sample as _Positive_. Thus, the _True Positive_ rate is 2 and the _False Positive_ rate is 1, and the precision is `2/(2+1)=0.667`. In other words, the trustiness percentage of the model when it says that a sample is _Positive_ is 66.7%.

![Fig 08](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/content/images/2020/09/Fig08.jpg)

**The goal of the precision is to classify all the _Positive_ samples as _Positive_, and not misclassify a negative sample as _Positive_.** According to the next figure, if all the three _Positive_ samples are correctly classified but one _Negative_ sample is incorrectly classified, the precision is `3/(3+1)=0.75`. Thus, the model is 75% accurate when it says that a sample is positive.

![Fig 09](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/content/images/2020/09/Fig09.jpg)

The only way to get 100% precision is to classify all the _Positive_ samples as _Positive_, in addition to not misclassifying a _Negative_ sample as _Positive_.

![Fig 10](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/content/images/2020/09/Fig10.jpg)

In Scikit-learn, the `sklearn.metrics` module has a function named `precision_score()` which accepts the ground-truth and predicted labels and returns the precision. The `pos_label` parameter accepts the label of the _Positive_ class. It defaults to `1`.

```
import sklearn.metrics

y_true = ["positive", "positive", "positive", "negative", "negative", "negative"]
y_pred = ["positive", "positive", "negative", "positive", "negative", "negative"]

precision = sklearn.metrics.precision_score(y_true, y_pred, pos_label="positive")
print(precision)
```

0.6666666666666666

### [Recall](#recall)[](#recall)

The recall is calculated as the ratio between the number of _Positive_ samples correctly classified as _Positive_ to the total number of _Positive_ samples. The recall measures the model’s ability to detect _Positive_ samples. The higher the recall, the more positive samples detected.

![Fig 11](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/content/images/2020/09/Fig11.jpg)

The recall cares only about how the positive samples are classified. This is independent of how the negative samples are classified, e.g. for the precision. When the model classifies all the positive samples as _Positive_, then the recall will be 100% even if all the negative samples were incorrectly classified as _Positive_. Let’s look at some examples.

In the next figure, there are 4 different cases (A to D) and all have the same recall which is `0.667`. Each case differs only in how the negative samples are classified. For example, case A has all the negative samples correctly classified as Negative, but case D misclassifies all the negative samples as Positive. Independently of how the negative samples are classified, the recall only cares about the positive samples.

![Fig 12](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/content/images/2020/09/Fig12.jpg)

Out of the 4 cases shown above, only 2 positive samples are classified correctly as positive. Thus, the _True Positive_ rate is 2. The _False Negative_ rate is 1 because just a single positive sample is classified as negative. As a result, the recall is `2/(2+1)=2/3=0.667`.

Because it does not matter whether the negative samples are classified as positive or negative, it is better to neglect the negative samples altogether as shown in the next figure. You only need to consider the positive samples when calculating the recall.

![Fig 13](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/content/images/2020/09/Fig13.jpg)

What does it mean when the recall is high or low? When the recall is high, it means the model can classify all the positive samples correctly as _Positive_. Thus, the model can be trusted in its ability to detect positive samples.

In the next figure the recall is 1.0 because all the positive samples were correctly classified as _Positive_. The _True Positive_ rate is 3, and the _False Negative_ rate is 0. Thus, the recall is equal to `3/(3+0)=1`. This means the model detected _all_ the positive samples. Because the recall neglects how the negative samples are classified, there could still be many negative samples classified as positive (i.e. a high _False Positive_ rate). The recall doesn’t take this into account.

![Fig 14](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/content/images/2020/09/Fig14.jpg)

On the other hand, the recall is 0.0 when it fails to detect any positive sample. In the next figure all the positive samples are **incorrectly** classified as _Negative_. This means the model detected 0% of the positive samples. The _True Positive_ rate is 0, and the _False Negative_ rate is 3. Thus, the recall is equal to `0/(0+3)=0`.

![Fig 15](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/content/images/2020/09/Fig15.jpg)

When the recall has a value between 0.0 and 1.0, this value reflects the percentage of positive samples the model correctly classified as _Positive_. For example, if there are 10 positive samples and the recall is 0.6, this means the model correctly classified 60% of the positive samples (i.e. `0.6*10=6` positive samples are correctly classified).

Similar to the `precision_score()` function, the `recall_score()` function in the `sklearn.metrics` module calculates the recall. The next block of code shows an example.

```
import sklearn.metrics

y_true = ["positive", "positive", "positive", "negative", "negative", "negative"]
y_pred = ["positive", "positive", "negative", "positive", "negative", "negative"]

recall = sklearn.metrics.recall_score(y_true, y_pred, pos_label="positive")
print(recall)
```

0.6666666666666666

After defining both the precision and the recall, let’s have a quick recap:

1.  The precision measures the model’s trustworthiness in classifying positive samples, and the recall measures how many positive samples the model correctly classified.
2.  The precision considers how the positive and negative samples were classified, but the recall only considers the positive samples in its calculations. In other words, the precision depends on both the negative and positive samples, but the recall is dependent only on the positive samples (and independent of the negative samples).
3.  The precision considers when a sample is classified as Positive, but it does not consider correctly classifying all positive samples. The recall cares about correctly classifying all positive samples, but it does not care if a negative sample is classified as positive.
4.  When a model has high recall but low precision, it classifies most of the positive samples correctly but has many false positives (i.e., it classifies many Negative samples as Positive). When a model has high precision but low recall, it is accurate when it classifies a sample as Positive but can only classify a few positive samples.

Here are some questions to test your understanding:

1.  If the recall is 1.0 and the dataset has 5 positive samples, how many positive samples were correctly classified by the model? (5)
2.  Given that the recall is 0.3 when the dataset has 30 positive samples, how many positive samples were correctly classified by the model? (0.3\*30=9 samples)
3.  If the recall is 0.0 and the dataset has 14 positive samples, how many positive samples were correctly classified by the model? (0)

[Precision or Recall?](#precision-or-recall)[](#precision-or-recall)
--------------------------------------------------------------------

The decision of whether to use precision or recall depends on the type of problem being solved. If the goal is to detect all the positive samples (without caring whether negative samples would be misclassified as positive), then use recall. Use precision if the problem is sensitive to classifying a sample as Positive in general, i.e., including Negative samples that were falsely classified as Positive.

Imagine being given an image and asked to detect all the cars within it. Which metric do you use? The goal is to detect all the cars and use recall. This may misclassify some objects as cars, but it eventually will work towards detecting all the target objects.

Now, say you’re given a mammography image, and you are asked to detect whether there is cancer or not. Which metric do you use? Because it is sensitive to incorrectly identifying an image as cancerous, we must be sure when classifying an image as Positive (i.e., has cancer). Thus, precision is the preferred metric.

[FAQ’s](#faq-s)[](#faq-s)
-------------------------

**1\. What is a confusion matrix in deep learning?** A confusion matrix is a table that summarizes the performance of a classification model by comparing predicted and actual labels.

**2\. How is accuracy calculated in a classification model?** Accuracy = (True Positives + True Negatives) / (Total Samples).

\***3\. What is the difference between precision and recall?** Precision measures correctness among positive predictions, while recall measures how many actual positives were identified.

**4\. When should I prioritize precision over recall?** Prioritize precision when false positives are costly, such as in medical diagnosis or fraud detection.

**5\. How do I calculate the confusion matrix in Python using Scikit-learn?** Use from sklearn.metrics import confusion\_matrix; confusion\_matrix(y\_true, y\_pred).

**6\. How does the confusion matrix apply to multi-class classification?** It extends to multi-class problems by creating an NxN matrix where N is the number of classes.

**7.What are true positives, false positives, true negatives, and false negatives?**

-   True Positive (TP): Correctly predicted positive cases.
-   False Positive (FP): Incorrectly predicted positive cases.
-   True Negative (TN): Correctly predicted negative cases.
-   False Negative (FN): Incorrectly predicted negative cases.

**8\. How do accuracy, precision, and recall impact model performance?** They determine how well a model balances correct predictions, minimizing false positives and false negatives.

**9\. Why is accuracy sometimes misleading as an evaluation metric?** Accuracy can be misleading in imbalanced datasets where one class dominates predictions.

**10\. How do I interpret a confusion matrix for an object detection model?** It helps assess false positives, false negatives, and correct detections, influencing IoU-based performance metrics.

**11\. What are common challenges when evaluating deep learning models?** Handling class imbalance, overfitting, data quality issues, and selecting the right evaluation metrics.

**12\. How can I improve the precision and recall of my model?** Use better data preprocessing, adjust classification thresholds, tune hyperparameters, and apply data augmentation.

**13\. What is the role of a confusion matrix in object detection tasks?** It helps analyze detection errors, including misclassifications, missed detections, and false alarms.

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

This tutorial discussed the confusion matrix and how to calculate its 4 metrics (true/false positive/negative) in both binary and multiclass classification problems. Using the metrics module in Scikit-learn, we saw how to calculate the confusion matrix in Python. Based on these four metrics, we discussed accuracy, precision, and recall. Each metric is defined based on several examples, and the sklearn.metrics module calculates each one.

[References](#references)[](#references)
----------------------------------------

1.  Understanding Model Evaluation Metrics - [Scikit-learn Documentation](https://scikit-learn.org/stable/modules/model_evaluation.html)
2.  Accuracy, Precision, and Recall Explained - [DigitalOcean Blog](/community/tutorials/deep-learning-metrics-precision-recall-accuracy)
3.  Confusion Matrix for Model Performance Analysis - [Towards DataScience Blog](https://towardsdatascience.com/)
4.  Optimizing Deep Learning Model Performance - [DigitalOcean Blog](/community/conceptual-articles/optimizing-deep-learning-pipelines)
5.  Hyperparameter Tuning for Better Model Metrics - [DigitalOcean Blog](/community/tutorials/hyperparameter-optimization-with-keras-tuner)
6.  Measuring Object Detection Model Performance - [Papers With Code](https://paperswithcode.com/)

#### [Source](https://www.digitalocean.com/community/tutorials/deep-learning-metrics-precision-recall-accuracy)

<br/>
---
