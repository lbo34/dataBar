---
title: Reading Notes Series - Applied Predictive Modeling - Section 5 (measuring performance
  in regression models)
author: Laurent Barcelo
date: '2018-10-24'
slug: reading-notes-series-applied-predictive-modeling-section-5-measuring-performance-in-regression-models
categories:
  - Reading Notes
tags:
  - regression
  - Books
  - RMSE
  - MSE
  - Variance-Bias Tradeoff
description: ''
featured_image: ''
---

This first series is related to M. Kuhn and K. Johnson book "Applied Predictive Modeling". The book pdf is available [here](https://vuquangnguyen2016.files.wordpress.com/2018/03/applied-predictive-modeling-max-kuhn-kjell-johnson_1518.pdf), but if, like me, you prefer to have a paper copy of the book, you can purchase it [here](https://www.amazon.com/Applied-Predictive-Modeling-Max-Kuhn/dp/1461468485/ref=sr_1_1?ie=UTF8&qid=1540208647&sr=8-1&keywords=applied+predictive+modeling). **This post is dedicated to section 5 on measuring performance in regression models.**

<img src="/post/2018-10-22-reading-notes-series-applied-predictive-modeling-section-3_files/Applied Predictive Modeling Cover.png" alt="Book cover" width="400px"/>

The chapter is a short chapter. It is the first chapter of part II on regression models and is an introduction for the other chapters in this section.

As an introduction, the authors explain that models accuracy can be assessed in different ways, each way bringing its own nuance.

**Quantitative measure of performances**

For regressions, when the outcome is a number, the **Root Mean Square Error (RMSE)** is a common measure of model performance. The RMSE is the square root of MSE and MSE is the average of the square of the errors, the errors being the difference between the expected value and the predicted value.

The **coefficient of determination R^2** is also a very common measure of performance. There are several versions, but the simplest one is the square of the correlation coefficient. It corresponds the the proportion of the overall variation in the data that is explained by the model... It is therefore related to the overall variance of the data, so it is very dependent on the range. For a given RMSE, the R^2 will be larger if the range (or variance) in data is larger.


**The variance-bias tradeoff**

It can be shown that the Expected value for the MSE, E(MSE), is:

E(MSE) = σ^2 + (model bias)^2 + model variance

where: σ^2 is the variance of the noise. The bias of the model is minimum when the model fits correctly the data. The variance of the model is maximum when the model overfits data

One good illustration of the concept (not taken from the book) is the following:

<img src="/post/2018-10-24-reading-notes-series-applied-predictive-modeling-section-5-measuring-performance-in-regression-models_files/bias variance tradeoff.png" alt="bias variance tradeoff" width="400px"/>
[source](https://towardsdatascience.com/understanding-the-bias-variance-tradeoff-165e6942b229)


**Usefull functions in R**

There is a **R package** that has been designed for the book. It is the [AppliedPredictiveModeling](https://cran.r-project.org/web/packages/AppliedPredictiveModeling/index.html) package. Most of the functions can be found in the [caret](https://cran.r-project.org/web/packages/caret/caret.pdf) package and other packages (corrplot, e1071, lattice).

* `caret::R2()` calculates the coefficient of determination;
* `caret::RMSE()` calculates the RMSE
* `cor()` in base R calculates the correlation coefficient. Ex: `cor(y, x)`. Can also be used with a method argument `cor(y, x, method = "spearman") `