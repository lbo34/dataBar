---
title: Reading Notes Series - Applied Linear Regression - Chapter 2 (Linear Regression)
author: Laurent Barcelo
date: '2018-11-13'
slug: reading-notes-series-applied-linear-regression-chapter-2-linear-regression
categories:
  - Reading Notes
tags:
  - Books
  - regression
description: ''
featured_image: ''
---

This post is about the "Applied Regression Book" by Sanford Weiser and in particular on its **chapter 2 on Linear Regression**. The book is a suggested reading in my course on "statistical analysis and inference". If you are interested by the book, you can find it [here](https://www.wiley.com/en-us/Applied+Linear+Regression%2C+4th+Edition-p-9781118386088). You can also find a pdf copy of the book [here](https://www.academia.edu/34664988/Applied_Linear_Regression_4th_Edition_by_Sanford_Weisberg).

<img src="/Reading-Notes/2018-11-13-reading-notes-series-applied-linear-regression-chatper-2-linear-regression_files/Applied Linear Regression.png" alt="Applied Linear Regression by Sanford Weisberg" width="400px"/>


**Introduction**

Simple linear regression is defined bythe following **means and variance functions**:

* $E(Y | X=x) = \beta_0 + \beta_1x$
* $V(Y) = \sigma^2$

The mean function has **2 parameters: the interecept** $\beta_0$ **and the slope** $\beta_1$. Also, the variance function is **assumed to be constant** with a generally unknown value $\sigma^2$.

The non null variance function makes that $y_i$ typically differs from $E(Y | X=x_i)$ and the **error term** $e$ is introduced as their difference: $e_i = y_i - E(Y | X=x_i)$ or $y_i = \beta_0 + \beta_1x_i + e_i$. There are 2 important assumptions concerning these errors:

* $E(e_i | X=x_i) = 0$
* the errors are all independant

The **hypothesis of normal distribution is NOT strictly required** here. It is required when calculating tests and confidence intervals on small samples.
  
  
**Ordinary Least Square Estimation**
  
There are many methods available to **estimate** the parameters of the models. The ordinary least squares methods or **OLS** is one of them. The **estimates of the parameters** are then chosen to **minimize the sum of the squares of the residuals**.

The parameters $\beta_0$ and $\beta_1$ are **unknown, but can be estimated** as $\hat{\beta_0}$ and $\hat{\beta_1}$ and $\hat{y_i} = \hat{E}(Y | X=x_i) =\hat{\beta_0} + \hat{\beta_1}x_i$ and $\hat{e}_i = y_i - \hat{E}(Y | X=x_i) = y_i - \hat{y_i}$ or $y_i = \hat{\beta}_0 + \hat{\beta}_1x_i + \hat{e}_i$


**Least Squares criterion**

As the optimization is chosen to **minimize the sum of the squares of the residuals** (called RSS in this book but also referred as SSE) (i.e. the vertical distance between the observed data and the fitted line), there is an **inherent assymetry in the roles of the response and of the predictors**.

OLS estimator minimizes the RSS function: $$\text{RSS} = \text{SSE} = \sum_{i=1}^n(y_i - \hat{y_i})^2$$ 
which corresponds to: $$\hat{\beta}_1 = \frac{\text{SXY}}{\text{SXX}}$$ and $$\hat{\beta}_0 = \bar{y}-\hat{\beta}_1\bar{x}$$


**Estimation of the variance**

If the errors are independant with a 0 mean (see above), an unbiased estimate of the variance $\sigma^2$ is given by: $$\hat{\sigma^2} = \frac{\text{RSS}}{\text{df}} = \frac{\text{RSS}}{n-2}$$ where $\text{df}$ is the degrees of freedom of the RSS.

The squared root of $\hat{\sigma^2}$ is called the **standard error of regression** (also **root mean square error**)


**Properties of least square estimates**

1. It is interesting to note that the regression line only depends on $\text{SXY}$, $\text{SXX}$, $\bar{x}$ and $\bar{y}$. Therefore, **all datasets that lead to an identical value of these quantities lead to the same regression line**, even when linear regression should not be applied. This has been illustrated by Ascombe (see figure below). <img src="/Reading-Notes/2018-11-13-reading-notes-series-applied-linear-regression-chatper-2-linear-regression_files/Ascombe.png" alt="Ascombe, 1973" width="400px"/>
2. The fitted value at $x = \bar{x}$ is $\hat{\beta}_0 + \hat{\beta}_1\bar{x}$ which equals to $\bar{y}$. This means that the **fitted line passes through the point** ($\bar{x}$,$\bar{y}$).
3. If the errors have a 0 mean and the mean function is correct, $\hat{\beta}_0$ and $\hat{\beta}_1$ are **unbiased estimates** of $\beta_0$ and $\beta_1$. These estimates are also the best linear unbiased estimates (they have the **smallest variance** of unbiased linear estimates).
4. $\hat{\beta}_0$ and $\hat{\beta}_1$ are **correlated**;
5. When errors are independant and normally distributed with 0 mean ($\text{NID}(0, \sigma^2)$), the OLS estimates correspond **maximum likelihood estimates**.


**Confidence Intervals and t-tests**

When errors are $\text{NID}(0, \sigma^2)$, the parameters estimates $\hat{\beta}_0$ and $\hat{\beta}_1$, the fitted values $\hat{y}_i = \hat{E}(Y|X = x_i)$ and the predictions **are also normally distributed**. Confidence intervals and tests can be based on a t-distribution but using $\hat{\sigma^2}$ as an estimator of $\sigma^2$.

The confidence intervals for $\beta_0$ and $\beta_1$ are given by:

$$\hat{\beta}_0 +/- t({\alpha}/2, n-2)se(\hat{\beta}_0|X)$$
$$\hat{\beta}_1 +/- t({\alpha}/2, n-2)se(\hat{\beta}_1|X)$$

When using the regression for prediction, **assuming that the data used for the prediction of the mean value is relevant to the new cases**, then the variance of the prediction $\widetilde{y}_o = \hat{\beta}_0 + \hat{\beta}_1x_o$ is given by:

$$\text{Var}(\widetilde{y}_o |x_o) = \sigma^2 + \sigma^2(\frac{1}{n}+\frac{(x_o - \bar{x})^2}{\text{SXX}})$$
$$\text{sepred}(\widetilde{y}_o |x_o) = \sqrt{\text{Var}(\widetilde{y}_o |x_o)}$$

The first $\sigma^2$ refers to the variability due to the error $e_o$. The other part is related to the variability in the estimate of thr regression parameters. A confidence interval around a point prediction can be built in the same was as for the regression parameters.

A similar exercice can ve done for the fitted value:
$$\text{sefit}(\hat{y} | x_o) = \sigma\sqrt{\frac{1}{n}+\frac{(x_o - \bar{x})^2}{\text{SXX}}}$$
The difference for the confidence interval is that we need to use a F distribution as we are doing a simultaneous inference on two parameters.

<img src="/Reading-Notes/2018-11-13-reading-notes-series-applied-linear-regression-chatper-2-linear-regression_files/fit CI.png" alt="confidence interval for fit" width="200px"/>


**Coefficient of determination*,** $R^2$

In the absence of other prediction, the best prediction for $y$ would be its average $\bar{y}$... and the total variation could be calculated by the total sum of square $\text{SYY} = \sum_{i=n}^{n}(y_i-\bar{y})^2$.

When including a regression with a predictor, the unexplained variation is given by RSS (or MSE). We can then calculate the variation accounted for by the regression: SSReg = SYY - RSS.

The coefficient of determination $R^2$ is **the proportion of the total varation (SYY) explained by the regression: SSReg / SYY**. It turn out that:

$$R^2 = \frac{\text{SSreg}}{\text(SYY)} = 1 - \frac{\text{RSS}}{\text(SYY)}$$

This coefficient generalizes to multiple regression. This coefficient cannot be used however to compare models (it increases when we add a predictor), therefore an **adjusted** value is given by:

$$R^2 = 1 - \frac{\text{RSS}/\text(df)}{\text(SYY)/(n-1)}$$


**The residuals**
**The plot of residuals other quantities is often used to detect failure of assumptions**. For instance, the residuals vs fitted values should be a **null plot**.