---
title: 'Reading Notes Series: Applied Predictive Modeling - Section 3 '
author: Laurent Barcelo
date: '2018-10-22'
slug: reading-notes-series-applied-predictive-modeling-section-3
categories:
  - Reading Notes
tags:
  - data pre-processing
  - Reading Notes
  - Books
description: ''
featured_image: ''
---

This is the first series of reading notes of the DataBarBlog!

This first series is related to M. Kuhn and K. Johnson book "Applied Predictive Modeling". The book pdf is available [here](https://vuquangnguyen2016.files.wordpress.com/2018/03/applied-predictive-modeling-max-kuhn-kjell-johnson_1518.pdf), but if, like me, you prefer to have a paper copy of the book, you can purchase it [here](https://www.amazon.com/Applied-Predictive-Modeling-Max-Kuhn/dp/1461468485/ref=sr_1_1?ie=UTF8&qid=1540208647&sr=8-1&keywords=applied+predictive+modeling). **This post is dedicated to section 3 on data pre-processing.**

<img src="/post/2018-10-22-reading-notes-series-applied-predictive-modeling-section-3_files/Applied Predictive Modeling Cover.png" alt="Book cover" width="400px"/>

The chapter highlights some important considerations concerning data pre-processing, i.e., the checks, transformations, etc... that should be performed prior to any modeling. These checks are not specific to types of modelings, although the authors note several times in the chapter than **some techniques are more robust than others** to non symmetry of data, outliers, etc...


The chapter starts with and example of cell segmentation on microscope images. 


**Data Tranformation**

**Data centering and scaling** that allows all predictors to share a uniform scale. The authors note that this improves the stability of calculations. The next step is related to skewness in data. Skewness can be corrected by applying mathematical transformations such as log and square root. One interesting function to correct skewness in predictors is the **Box-Cox tranformation**. This transformation as one parameter λ. Through this parameter, it is possible to switch from square transformation (λ = 2), square root transformation (λ = 0.5), log transformation(λ = 0), inverse transformation (λ = -1). One nice aspect is that some implementations of the Box-Cox model allow to determine the "best" λ value for each predictor.

Predictors transformation can also be applied to a **set of predictors rather than individual predictors** and the book defines two aspects: resolving **outliers** on the one side and **data reduction and feature extraction** on the other side. As outliers may correspond to valid part of data (like a different cluster of data), some treatments allow to "regularize" them as some modeling techniques are sensitive to their presence. One example of such treatment is the spatial sign treatment which allows to project all data points on the surface of a multidimensional sphere. An example of this transformtion is shown in the figure below.

<img src="/post/2018-10-22-reading-notes-series-applied-predictive-modeling-section-3_files/spatial sign treatment.png" alt="Spation sign transformation" width="400px"/>

The chapter continues with data reduction techniques and principal components analysis (**PCA**) in particular. One key reason that can make PCA a required pre-processing transformation is that **principal components derived from PCA are uncorrelated**. One negative aspect is that we interpretability of the model is less direct. Note that it can be important to scale and center data before applying PCA treatment, otherwise, this could lead to over-emphasizing predictors with the largest range.


**Dealing with missing values**

We should handle missing values with care. There are several "flavors" of missing values:

* **Structurally missing values** for values that cannot exist by essence;
* **Informative missingness** correspond to sets of missing values that informs on the outcome. In the case of customer ratings for instance, there is generally fewer neutral ratings as customers with more polarized opinions are more compelled to rate products.
* **Censored data**, which is not to be confused with missing values as something can be infered from their value. It is the case for instance of a measure that is below the detection limit of the apparatus. In that case, we can infer that the value is lower or equal to the detection limit and there are several strategies such as replacing these missing values with the detection limit or a random number drawn between 0 and the detection limit.

On small datasets, it can be very penalizing to remove observations containing missing values. In such cases, **missing values can be imputed**. **K-nearest neighbor model** is a popular technique to inform missing values (which requires the entire dataset to compute). Another possibility is to predict missing values from a combination of other predictors.


**Removing Predictors**

It can be usefull to remove predictors to reduce computation time and complexity. Some predictors correspond to near zero variance predictors and can be detected with the following rule of thumb:

* The fraction of unique values over the sample size is low (10% or less);
* The ratio of the frequency of the most prevalent value to the frequency of the second most prevalent value is large (20 or more)


Another reason to remove predictors or to remove the amount of predictors is related to **predictors collinearity or multicollinearity**. This is the case when a predictor is highly correlated to an other one or to a combination of other predictors. There are several possiblities to diagnose this:

* The **correlation matrix**... in particular, variables can be ordered with a hierarchical clustering;
* PCA with the loadings matrix;
* **Variance Inflation Factor or VIF** which is mostly relevant for regression techniques. Instead, the authors propose a **more heuristic approach** selectively removing predictors based on highest pairwise correlations.


**Adding Predictors**

Addition of predictors can be usefull in cases such as the coding of a categorical variable into "dummy variables" (It seems to correspond to what is sometime referred to as "One Hot Encoding"). In such cases a categorical variables that can take 3 value is replaced by 2 variables coded between 0 and 1. The 3rd values corresponds to the two variables being equal to 0.


**Binning Predictors**

Although there can be some preceived advantages to this, **binning predictors should be avoided** as it can reduce the performance of the model, increase the loss of precision of the prediction. It can also lead to a high rate of false positive.


**Usefull functions in R**

There is a **R package** that has been designed for the book. It is the [AppliedPredictiveModeling](https://cran.r-project.org/web/packages/AppliedPredictiveModeling/index.html) package. Most of the functions can be found in the [caret](https://cran.r-project.org/web/packages/caret/caret.pdf) package and other packages (corrplot, e1071, lattice).

* `skewness()` function… can be combined with apply: `apply(df, 2, skewness)`
* `caret::BoxCoxTrans()` function to perform a box cox transformation… 
* `prcomp()` function for principal components transformation for instance: `prcomp(df, center = TRUE, scale = TRUE)` 
* `caret::preProcess()` function. Can be used to pipe transformations such as: `preProcess(df, method = c(“BoxCox”, “center”, “scale”, “pca”))` used in conjonction with `predict()` to apply it
* `caret::spatialSign()` function  for the spatial sign transformation
* `caret::nearZeroVar()` function to list predictors with low variance.
* `cor()` function to get correlations
* `corrplot::corrplot()` to display nicely correlation matrices such as in `corrplot(cor(df), order = "hclust")`
* `caret::findCorrelation()` to list predictors that are pairwise correlated with others. use: `findCorrelation(df, cutoff = 0.75)`
* `caret::dummyVars()` function for one-hot-encoding







