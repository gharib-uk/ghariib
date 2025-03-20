---
title: "Multiple Linear Regression in Python A Comprehensive Guide"
date: 2025-01-21T00:00:00.000Z
draft: false
type: posts
categories: 
- machine-learning,python,ai-ml
---
# Multiple Linear Regression in Python A Comprehensive Guide

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

Multiple Linear Regression is a fundamental statistical technique used to model the relationship between one dependent variable and multiple independent variables. In Python, tools like `scikit-learn` and `statsmodels` provide robust implementations for regression analysis. This tutorial will walk you through implementing, interpreting, and evaluating multiple linear regression models using Python.

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

Before diving into the implementation, ensure you have the following:

-   Basic understanding of Python. You can refer to [Python Tutorial for Beginners](/community/tutorials/python-tutorial).
-   Familiarity with scikit-learn for machine learning tasks. You can refer to [Python scikit-learn Tutorial](/community/tutorials/python-scikit-learn-tutorial).
-   Understanding of data visualization concepts in Python. You can refer to [How To Plot Data in Python 3 Using matplotlib](/community/tutorials/how-to-plot-data-in-python-3-using-matplotlib) and [Data Analysis and Visualization with pandas and Jupyter Notebook in Python 3](/community/tutorials/data-analysis-and-visualization-with-pandas-and-jupyter-notebook-in-python-3).
-   Python 3.x installed with the following libraries `numpy`, `pandas`, `matplotlib`, `seaborn`, `scikit-learn`, and `statsmodels` installed.

[What is Multiple Linear Regression?](#what-is-multiple-linear-regression)[](#what-is-multiple-linear-regression)
-----------------------------------------------------------------------------------------------------------------

Multiple Linear Regression (MLR) is a statistical method that models the relationship between a dependent variable and two or more independent variables. It is an extension of simple linear regression, which models the relationship between a dependent variable and a single independent variable. In MLR, the relationship is modeled using the formula:

![MLR Equation](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Multiple-Linear-Regression-in-Python/CodeCogsEqn%20(1).png)

Where:

![Code equation](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Multiple-Linear-Regression-in-Python/CodeCogsEqn%20(2).png)

Example: Predicting the price of a house based on its size, number of bedrooms, and location. In this case, there are three independent variables, i.e., size, number of bedrooms, and location, and one dependent variable, i.e., price, that is the value to be predicted.

[Assumptions of Multiple Linear Regression](#assumptions-of-multiple-linear-regression)[](#assumptions-of-multiple-linear-regression)
-------------------------------------------------------------------------------------------------------------------------------------

Before implementing multiple linear regression, it is essential to ensure that the following assumptions are met:

1.  **Linearity**: The relationship between the dependent variable and independent variables is linear.
    
2.  **Independence of Errors**: Residuals (errors) are independent of each other. This is often verified using the [Durbin-Watson test](https://corporatefinanceinstitute.com/resources/data-science/durbin-watson-statistic/).
    
3.  **Homoscedasticity**: The variance of residuals is constant across all levels of the independent variables. A residual plot can help verify this.
    
4.  **No Multicollinearity**: Independent variables are not highly correlated. [Variance Inflation Factor (VIF)](https://www.investopedia.com/terms/v/variance-inflation-factor.asp) is commonly used to detect multicollinearity.
    
5.  **Normality of Residuals**: Residuals should follow a normal distribution. This can be checked using a [Q-Q plot](https://library.virginia.edu/data/articles/understanding-q-q-plots).
    
6.  **Outlier Influence**: Outliers or high-leverage points should not disproportionately influence the model.
    

These assumptions ensure that the regression model is valid and the results are reliable. Failing to meet these assumptions may lead to biased or misleading results.

[Preprocess the Data](#preprocess-the-data)[](#preprocess-the-data)
-------------------------------------------------------------------

In this section, you will learn to use the Multiple Linear Regression model in Python to predict house prices based on features from the [California Housing Dataset](https://www.kaggle.com/datasets/camnugent/california-housing-prices). You’ll learn how to preprocess data, fit a regression model, and evaluate its performance while addressing common challenges like multicollinearity, outliers, and feature selection.

### [Step 1 - Load the Dataset](#step-1-load-the-dataset)[](#step-1-load-the-dataset)

You will use the [California Housing Dataset](https://www.kaggle.com/datasets/camnugent/california-housing-prices), a popular dataset for regression tasks. This dataset contains 13 features about houses in Boston suburbs and their corresponding median house price.

First, let’s install the necessary packages:

```
pip install numpy pandas matplotlib seaborn scikit-learn statsmodels
```

```
from sklearn.datasets import fetch_california_housing  # Import the fetch_california_housing function from sklearn.datasets to load the California Housing dataset.
import pandas as pd  # Import pandas for data manipulation and analysis.
import numpy as np  # Import numpy for numerical computing.

# Load the California Housing dataset using the fetch_california_housing function.
housing = fetch_california_housing()

# Convert the dataset's data into a pandas DataFrame, using the feature names as column headers.
housing_df = pd.DataFrame(housing.data, columns=housing.feature_names)

# Add the target variable 'MedHouseValue' to the DataFrame, using the dataset's target values.
housing_df['MedHouseValue'] = housing.target

# Display the first few rows of the DataFrame to get an overview of the dataset.
print(housing_df.head())
```

You should observe the following output of the dataset:

```
 MedInc  HouseAge  AveRooms  AveBedrms  Population  AveOccup  Latitude  Longitude  MedHouseValue
0  8.3252      41.0  6.984127   1.023810       322.0  2.555556     37.88    -122.23          4.526
1  8.3014      21.0  6.238137   0.971880      2401.0  2.109842     37.86    -122.22          3.585
2  7.2574      52.0  8.288136   1.073446       496.0  2.802260     37.85    -122.24          3.521
3  5.6431      52.0  5.817352   1.073059       558.0  2.547945     37.85    -122.25          3.413
4  3.8462      52.0  6.281853   1.081081       565.0  2.181467     37.85    -122.25          3.422
```

Here is what each of the attributes mean:

Variable

Description

MedInc

Median income in block

HouseAge

Median house age in block

AveRooms

Average number of rooms

AveBedrms

Average number of bedrooms

Population

Block population

AveOccup

Average house occupancy

Latitude

House block latitude

Longitude

House block longitude

### [Step 2 - Preprocess the Data](#step-2-preprocess-the-data)[](#step-2-preprocess-the-data)

#### Check for Missing Values

Ensures there are no missing values in the dataset which might affect the analysis.

```
print(housing_df.isnull().sum())
```

Output:

```
MedInc           0
HouseAge         0
AveRooms         0
AveBedrms        0
Population       0
AveOccup         0
Latitude         0
Longitude        0
MedHouseValue    0
dtype: int64
```

#### Feature Selection

Let’s first create a correlation matrix to understand the dependencies between the variables.

```
correlation_matrix = housing_df.corr()
print(correlation_matrix['MedHouseValue'])
```

Output:

```
MedInc           0.688075
HouseAge         0.105623
AveRooms         0.151948
AveBedrms       -0.046701
Population      -0.024650
AveOccup        -0.023737
Latitude        -0.144160
Longitude       -0.045967
MedHouseValue    1.000000
```

You can analyze the above correlation matrix to select the dependent and independent variables for our regression model. The correlation matrix provides insights into the relationships between each pair of variables in the dataset.

In the given correlation matrix, `MedHouseValue` is the dependent variable, as it is the variable we are trying to predict. The independent variables have a significant correlation with `MedHouseValue`.

Based on the correlation matrix, you can identify the following independent variables that have a significant correlation with `MedHouseValue`:

-   `MedInc`: This variable has a strong positive correlation (0.688075) with `MedHouseValue`, indicating that as median income increases, median house value also tends to increase.
-   `AveRooms`: This variable has a moderate positive correlation (0.151948) with `MedHouseValue`, suggesting that as the average number of rooms per household increases, median house value also tends to increase.
-   `AveOccup`: This variable has a weak negative correlation (-0.023737) with `MedHouseValue`, indicating that as the average occupancy per household increases, median house value tends to decrease, but the effect is relatively small.

By selecting these independent variables, you can build a regression model that captures the relationships between these variables and `MedHouseValue`, allowing us to make predictions about median house value based on median income, average number of rooms, and average occupancy.

You can also plot the correlation matrix in Python using the below:

```
import seaborn as sns
import matplotlib.pyplot as plt

# Assuming 'housing_df' is the DataFrame containing the data
# Plotting the correlation matrix
plt.figure(figsize=(10, 8))
sns.heatmap(housing_df.corr(), annot=True, cmap='coolwarm')
plt.title('Correlation Matrix')
plt.show()
```

![Correlation Matrix](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Multiple-Linear-Regression-in-Python/correlation_matrix.png)

You’ll focus on a few key features for simplicity based on the above, such as `MedInc` (median income), `AveRooms` (average rooms per household), and `AveOccup` (average occupancy per household).

```
selected_features = ['MedInc', 'AveRooms', 'AveOccup']
X = housing_df[selected_features]
y = housing_df['MedHouseValue']
```

The above code block selects specific features from the `housing_df` data frame for analysis. The selected features are `MedInc`, `AveRooms`, and `AveOccup`, which are stored in the `selected_features` list.

The DataFrame `housing_df` is then subset to include only these selected features and the result is stored in `X` list.

The target variable `MedHouseValue` is extracted from `housing_df` and stored in the `y` list.

#### Scaling Features

You will use Standardization to ensure all features are on the same scale, improving model performance and comparability.

Standardization is a preprocessing technique that scales numerical features to have a mean of 0 and a standard deviation of 1. This process ensures that all features are on the same scale, which is essential for machine learning models sensitive to the input features’ scale. By standardizing the features, you can improve model performance and comparability by reducing the effect of features with large ranges dominating the model.

```
from sklearn.preprocessing import StandardScaler

# Initialize the StandardScaler object
scaler = StandardScaler()

# Fit the scaler to the data and transform it
X_scaled = scaler.fit_transform(X)

# Print the scaled data
print(X_scaled)
```

Output:

```
[[ 2.34476576  0.62855945 -0.04959654]
[ 2.33223796  0.32704136 -0.09251223]
[ 1.7826994   1.15562047 -0.02584253]
...
[-1.14259331 -0.09031802 -0.0717345 ]
[-1.05458292 -0.04021111 -0.09122515]
[-0.78012947 -0.07044252 -0.04368215]]
```

The output represents the scaled values of the features `MedInc`, `AveRooms`, and `AveOccup` after applying the StandardScaler. The values are now centered around 0 with a standard deviation of 1, ensuring all features are on the same scale.

The first row `[ 2.34476576 0.62855945 -0.04959654]` indicates that for the first data point, the scaled `MedInc` value is 2.34476576, `AveRooms` is 0.62855945, and `AveOccup` is -0.04959654. Similarly, the second row `[ 2.33223796 0.32704136 -0.09251223]` represents the scaled values for the second data point, and so on.

The scaled values range from approximately -1.14259331 to 2.34476576, indicating that the features are now normalized and comparable. This is essential for machine learning models that are sensitive to the scale of input features, as it prevents features with large ranges from dominating the model.

[Implement Multiple Linear Regression](#implement-multiple-linear-regression)[](#implement-multiple-linear-regression)
----------------------------------------------------------------------------------------------------------------------

Now that you are done with data preprocessing let’s implement multiple linear regression in python.

```
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score
import matplotlib.pyplot as plt
import seaborn as sns

X_train, X_test, y_train, y_test = train_test_split(X_scaled, y, test_size=0.2, random_state=42)

# The 'LinearRegression' model is initialized and fitted to the training data.
model = LinearRegression()
model.fit(X_train, y_train)

# The model is used to predict the target variable for the test set.
y_pred = model.predict(X_test)


print("Mean Squared Error:", mean_squared_error(y_test, y_pred))
print("R-squared:", r2_score(y_test, y_pred))
```

The `train_test_split` function is used to split the data into training and testing sets. Here, 80% of the data is used for training and 20% for testing.

The model is evaluated using Mean Squared Error and R-squared. Mean Squared Error (MSE) measures the average of the squares of the errors or deviations.

R-squared (R2) is a statistical measure that represents the proportion of the variance for a dependent variable that’s explained by an independent variable or variables in a regression model.

Output:

```
Mean Squared Error: 0.7006855912225249
R-squared: 0.4652924370503557
```

The output above provides two key metrics to evaluate the performance of the multiple linear regression model:

**Mean Squared Error (MSE):** `0.7006855912225249` The [MSE](https://en.wikipedia.org/wiki/Mean_squared_error) measures the average squared difference between the predicted and actual values of the target variable. A lower MSE indicates better model performance, as it means the model is making more accurate predictions. In this case, the MSE is `0.7006855912225249`, indicating that the model is not perfect but has a reasonable level of accuracy. The MSE values typically should be closer to 0, with lower values indicating better performance.

**R-squared (R2):** `0.4652924370503557` [R-squared](https://corporatefinanceinstitute.com/resources/data-science/r-squared/) measures the proportion of the variance in the dependent variable that is predictable from the independent variables. It ranges from 0 to 1, where 1 is perfect prediction and 0 indicates no linear relationship. In this case, the R-squared value is `0.4652924370503557`, indicating that about 46.53% of the variance in the target variable can be explained by the independent variables used in the model. This suggests that the model is able to capture a significant portion of the relationships between the variables but not all of it.

Let’s check out some important plots:

```
# Residual Plot
residuals = y_test - y_pred
plt.scatter(y_pred, residuals, alpha=0.5)
plt.xlabel('Predicted Values')
plt.ylabel('Residuals')
plt.title('Residual Plot')
plt.axhline(y=0, color='red', linestyle='--')
plt.show()

# Predicted vs Actual Plot
plt.scatter(y_test, y_pred, alpha=0.5)
plt.xlabel('Actual Values')
plt.ylabel('Predicted Values')
plt.title('Predicted vs Actual Values')
plt.plot([y_test.min(), y_test.max()], [y_test.min(), y_test.max()], 'r--', lw=4)
plt.show()
```

![Residual Plot](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Multiple-Linear-Regression-in-Python/download.png)

![Predicted vs Actual Values](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Multiple-Linear-Regression-in-Python/download%20(1).png)

### [Using `statsmodels`](#using-statsmodels)[](#using-statsmodels)

The [Statsmodels](https://www.statsmodels.org/stable/index.html) library in Python is a powerful tool for statistical analysis. It provides a wide range of statistical models and tests, including linear regression, time series analysis, and nonparametric methods.

In the context of multiple linear regression, `statsmodels` can be used to fit a linear model to the data, and then perform various statistical tests and analyses on the model. This can be particularly useful for understanding the relationships between the independent and dependent variables, and for making predictions based on the model.

```
import statsmodels.api as sm

# Add a constant to the model
X_train_sm = sm.add_constant(X_train)
model_sm = sm.OLS(y_train, X_train_sm).fit()
print(model_sm.summary())

# Q-Q Plot for residuals
sm.qqplot(model_sm.resid, line='s')
plt.title('Q-Q Plot of Residuals')
plt.show()
```

Output:

```
==============================================================================
Dep. Variable:          MedHouseValue   R-squared:                       0.485
Model:                            OLS   Adj. R-squared:                  0.484
Method:                 Least Squares   F-statistic:                     5173.
Date:                Fri, 17 Jan 2025   Prob (F-statistic):               0.00
Time:                        09:40:54   Log-Likelihood:                -20354.
No. Observations:               16512   AIC:                         4.072e+04
Df Residuals:                   16508   BIC:                         4.075e+04
Df Model:                           3                                        
Covariance Type:            nonrobust                                        
==============================================================================
                coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------
const          2.0679      0.006    320.074      0.000       2.055       2.081
x1             0.8300      0.007    121.245      0.000       0.817       0.843
x2            -0.1000      0.007    -14.070      0.000      -0.114      -0.086
x3            -0.0397      0.006     -6.855      0.000      -0.051      -0.028
==============================================================================
Omnibus:                     3981.290   Durbin-Watson:                   1.983
Prob(Omnibus):                  0.000   Jarque-Bera (JB):            11583.284
Skew:                           1.260   Prob(JB):                         0.00
Kurtosis:                       6.239   Cond. No.                         1.42
==============================================================================
```

Here is the summary of the above table:

#### Model Summary

The model is an [Ordinary Least Squares](https://www.xlstat.com/en/solutions/features/ordinary-least-squares-regression-ols) regression model, which is a type of linear regression model. The dependent variable is `MedHouseValue`, and the model has an R-squared value of 0.485, indicating that about 48.5% of the variation in `MedHouseValue` can be explained by the independent variables. The adjusted R-squared value is 0.484, which is a modified version of R-squared that penalizes the model for including additional independent variables.

#### Model Fit

The model was fit using the Least Squares method, and the F-statistic is 5173, indicating that the model is a good fit. The probability of observing an F-statistic at least as extreme as the one observed, assuming that the null hypothesis is true, is approximately 0. This suggests that the model is statistically significant.

#### Model Coefficients

The model coefficients are as follows:

-   The constant term is 2.0679, indicating that when all independent variables are 0, the predicted `MedHouseValue` is approximately 2.0679.
-   The coefficient for `x1`(In this case `MedInc`) is 0.8300, indicating that for every unit increase in `MedInc`, the predicted `MedHouseValue` increases by approximately 0.83 units, assuming all other independent variables are held constant.
-   The coefficient for `x2`(In this case `AveRooms`) is -0.1000, indicating that for every unit increase in `x2`, the predicted `MedHouseValue` decreases by approximately 0.10 units, assuming all other independent variables are held constant.
-   The coefficient for `x3`(In this case `AveOccup`) is -0.0397, indicating that for every unit increase in `x3`, the predicted `MedHouseValue` decreases by approximately 0.04 units, assuming all other independent variables are held constant.

#### Model Diagnostics

The model diagnostics are as follows:

-   The Omnibus test statistic is 3981.290, indicating that the residuals are not normally distributed.
-   The Durbin-Watson statistic is 1.983, indicating that there is no significant autocorrelation in the residuals.
-   The Jarque-Bera test statistic is 11583.284, indicating that the residuals are not normally distributed.
-   The skewness of the residuals is 1.260, indicating that the residuals are skewed to the right.
-   The kurtosis of the residuals is 6.239, indicating that the residuals are leptokurtic (i.e., they have a higher peak and heavier tails than a normal distribution).
-   The condition number is 1.42, indicating that the model is not sensitive to small changes in the data.

![Plot of Residuals](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Multiple-Linear-Regression-in-Python/download%20(2).png)

[Handling Multicollinearity](#handling-multicollinearity)[](#handling-multicollinearity)
----------------------------------------------------------------------------------------

[Multicollinearity](https://www.xlstat.com/en/solutions/features/multicolinearity-statistics) is a common issue in multiple linear regression, where two or more independent variables are highly correlated with each other. This can lead to unstable and unreliable estimates of the coefficients.

To detect and handle multicollinearity, you can use the [Variance Inflation Factor](https://www.investopedia.com/terms/v/variance-inflation-factor.asp). The VIF measures how much the variance of an estimated regression coefficient increases if your predictors are correlated. A VIF of 1 means that there is no correlation between a given predictor and the other predictors. VIF values exceeding 5 or 10 indicate a problematic amount of collinearity.

In the code block below, let’s calculate the VIF for each independent variable in our model. If any VIF value is above 5, you should consider removing the variable from the model.

```
from statsmodels.stats.outliers_influence import variance_inflation_factor

vif_data = pd.DataFrame()
vif_data['Feature'] = selected_features
vif_data['VIF'] = [variance_inflation_factor(X_scaled, i) for i in range(X_scaled.shape[1])]
print(vif_data)

# Bar Plot for VIF Values
vif_data.plot(kind='bar', x='Feature', y='VIF', legend=False)
plt.title('Variance Inflation Factor (VIF) by Feature')
plt.ylabel('VIF Value')
plt.show()
```

Output:

```
   Feature       VIF
0    MedInc  1.120166
1  AveRooms  1.119797
2  AveOccup  1.000488
```

The VIF values for each feature are as follows:

-   `MedInc`: The VIF value is 1.120166, indicating a very low correlation with other independent variables. This suggests that `MedInc` is not highly correlated with other independent variables in the model.
-   `AveRooms`: The VIF value is 1.119797, indicating a very low correlation with other independent variables. This suggests that `AveRooms` is not highly correlated with other independent variables in the model.
-   `AveOccup`: The VIF value is 1.000488, indicating no correlation with other independent variables. This suggests that `AveOccup` is not correlated with other independent variables in the model.

In general, these VIF values are all below 5, indicating that there is no significant multicollinearity between the independent variables in the model. This suggests that the model is stable and reliable, and that the coefficients of the independent variables are not significantly affected by multicollinearity.

![VIF](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Multiple-Linear-Regression-in-Python/download%20(3).png)

[Cross-Validation Techniques](#cross-validation-techniques)[](#cross-validation-techniques)
-------------------------------------------------------------------------------------------

[Cross-validation](https://scikit-learn.org/stable/modules/cross_validation.html) is a technique used to evaluate the performance of a machine learning model. It is a resampling procedure used to evaluate a model if we have a limited data sample. The procedure has a single parameter called `k` that refers to the number of groups that a given data sample is to be split into. As such, the procedure is often called k-fold cross-validation.

```
from sklearn.model_selection import cross_val_score
scores = cross_val_score(model, X_scaled, y, cv=5, scoring='r2')
print("Cross-Validation Scores:", scores)
print("Mean CV R^2:", scores.mean())

# Line Plot for Cross-Validation Scores
plt.plot(range(1, 6), scores, marker='o', linestyle='--')
plt.xlabel('Fold')
plt.ylabel('R-squared')
plt.title('Cross-Validation R-squared Scores')
plt.show()
```

Output:

```
Cross-Validation Scores: [0.42854821 0.37096545 0.46910866 0.31191043 0.51269138]
Mean CV R^2: 0.41864482644003276
```

The cross-validation scores indicate how well the model performs on unseen data. The scores range from 0.31191043 to 0.51269138, indicating that the model’s performance varies across different folds. A higher score indicates better performance.

The mean CV R^2 score is 0.41864482644003276, which suggests that, on average, the model explains about 41.86% of the variance in the target variable. This is a moderate level of explanation, indicating that the model is somewhat effective in predicting the target variable but may benefit from further improvement or refinement.

These scores can be used to evaluate the model’s generalizability and identify potential areas for improvement.

![Cross Validation](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Multiple-Linear-Regression-in-Python/download%20(4).png)

[Feature selection methods](#feature-selection-methods)[](#feature-selection-methods)
-------------------------------------------------------------------------------------

The [Recursive Feature Elimination](https://scikit-learn.org/dev/modules/generated/sklearn.feature_selection.RFE.html) method is a feature selection technique that recursively eliminates the least important features until a specified number of features is reached. This method is particularly useful when dealing with a large number of features and the goal is to select a subset of the most informative features.

In the provided code, you first import the `RFE` class from `sklearn.feature_selection`. Then create an instance of `RFE` with a specified estimator (in this case, `LinearRegression`) and set `n_features_to_select` to 2, indicating that we want to select the top 2 features.

Next, we fit the `RFE` object to our scaled features `X_scaled` and target variable `y`. The `support_` attribute of the `RFE` object returns a boolean mask indicating which features were selected.

To visualize the ranking of features, you create a DataFrame with the feature names and their corresponding rankings. The `ranking_` attribute of the `RFE` object returns the ranking of each feature, with lower values indicating more important features. You then plot a bar chart of the feature rankings, sorted by their ranking values. This plot helps us understand the relative importance of each feature in the model.

```
from sklearn.feature_selection import RFE
rfe = RFE(estimator=LinearRegression(), n_features_to_select=3)
rfe.fit(X_scaled, y)
print("Selected Features:", rfe.support_)

# Bar Plot of Feature Rankings
feature_ranking = pd.DataFrame({
   'Feature': selected_features,
   'Ranking': rfe.ranking_
})
feature_ranking.sort_values(by='Ranking').plot(kind='bar', x='Feature', y='Ranking', legend=False)
plt.title('Feature Ranking (Lower is Better)')
plt.ylabel('Ranking')
plt.show()
```

Output:

```
Selected Features: [ True  True False]
```

![RFE](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Multiple-Linear-Regression-in-Python/download%20(5).png)

Based on the above chart, the 2 most suitable features are `MedInc` and `AveRooms`. This can also be verified by the model’s output above as dependent variable `MedHouseValue`, is mostly dependent on `MedInc` and `AveRooms`.

[FAQs](#faqs)[](#faqs)
----------------------

### [How do you implement multiple linear regression in Python?](#how-do-you-implement-multiple-linear-regression-in-python)[](#how-do-you-implement-multiple-linear-regression-in-python)

To implement multiple linear regression in Python, you can use libraries like `statsmodels` or `scikit-learn`. Here’s a quick overview using `scikit-learn`:

```
from sklearn.linear_model import LinearRegression
import numpy as np

# Example data
X = np.array([[1, 2], [2, 3], [3, 4], [4, 5]])  # Predictor variables
y = np.array([5, 7, 9, 11])  # Target variable

# Create and fit the model
model = LinearRegression()
model.fit(X, y)

# Get coefficients and intercept
print("Coefficients:", model.coef_)
print("Intercept:", model.intercept_)

# Make predictions
predictions = model.predict(X)
print("Predictions:", predictions)
```

This demonstrates how to fit the model, obtain the coefficients, and make predictions.

### [What are the assumptions of multiple linear regression in Python?](#what-are-the-assumptions-of-multiple-linear-regression-in-python)[](#what-are-the-assumptions-of-multiple-linear-regression-in-python)

Multiple linear regression relies on several assumptions to ensure valid results:

1.  **Linearity**: The relationship between predictors and the target variable is linear.
2.  **Independence**: Observations are independent of each other.
3.  **Homoscedasticity**: The variance of residuals (errors) is constant across all levels of the independent variables.
4.  **Normality of Residuals**: Residuals are normally distributed.
5.  **No Multicollinearity**: Independent variables are not highly correlated with each other.

You can test these assumptions using tools like residual plots, Variance Inflation Factor (VIF), or statistical tests.

### [How do you interpret multiple regression results in Python?](#how-do-you-interpret-multiple-regression-results-in-python)[](#how-do-you-interpret-multiple-regression-results-in-python)

Key metrics from regression results include:

1.  **Coefficients (coef\_)**: Indicate the change in the target variable for a unit change in the corresponding predictor, keeping other variables constant.

Example: A coefficient of 2 for X1 means the target variable increases by 2 for every 1-unit increase in X1, holding other variables constant.

2.**Intercept (intercept\_)**: Represents the target variable’s predicted value when all predictors are zero.

3.**R-squared**: Explains the proportion of the variance in the target variable explained by the predictors.

Example: An R^2 of 0.85 means 85% of the variability in the target variable is explained by the model.

4.**P-values** (in `statsmodels`): Assess the statistical significance of predictors. A p-value < 0.05 typically indicates a predictor is significant.

### [What is the difference between simple and multiple linear regression in Python?](#what-is-the-difference-between-simple-and-multiple-linear-regression-in-python)[](#what-is-the-difference-between-simple-and-multiple-linear-regression-in-python)

**Feature**

**Simple Linear Regression**

**Multiple Linear Regression**

**Number of Independent Variables**

One

More than one

**Model Equation**

y = β0 + β1x + ε

y = β0 + β1x1 + β2x2 + … + βnxn + ε

**Assumptions**

Same as multiple linear regression, but with a single independent variable

Same as simple linear regression, but with additional assumptions for multiple independent variables

**Interpretation of Coefficients**

The change in the target variable for a unit change in the independent variable, while holding all other variables constant (not applicable in simple linear regression)

The change in the target variable for a unit change in one independent variable, while holding all other independent variables constant

**Model Complexity**

Less complex

More complex

**Model Flexibility**

Less flexible

More flexible

**Overfitting Risk**

Lower

Higher

**Interpretability**

Easier to interpret

More challenging to interpret

**Applicability**

Suitable for simple relationships

Suitable for complex relationships with multiple factors

**Example**

Predicting house prices based on the number of bedrooms

Predicting house prices based on the number of bedrooms, square footage, and location

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

In this comprehensive tutorial, you learned to implement Multiple Linear Regression using the California Housing Dataset. You tackled crucial aspects such as multicollinearity, cross-validation, feature selection, and regularization, providing a thorough understanding of each concept. You also learned to incorporate visualizations to illustrate residuals, feature importance, and overall model performance. You can now easily construct robust regression models in Python and apply these skills to real-world problems.

#### [Source](https://www.digitalocean.com/community/tutorials/multiple-linear-regression-python)

<br/>
---
