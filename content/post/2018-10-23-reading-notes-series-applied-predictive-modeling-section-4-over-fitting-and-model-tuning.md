---
title: Reading Notes Series - Applied Predictive Modeling - Section 4 (over-fitting
  and model tuning)
author: Laurent Barcelo
date: '2018-10-23'
slug: reading-notes-series-applied-predictive-modeling-section-4-over-fitting-and-model-tuning
categories:
  - Reading Notes
tags:
  - over fitting
  - Books
  - Reading Notes
  - model selection
  - resampling methods
  - bootstrap
  - k-fold cross validation
description: ''
featured_image: ''
---

This first series is related to M. Kuhn and K. Johnson book "Applied Predictive Modeling". The book pdf is available [here](https://vuquangnguyen2016.files.wordpress.com/2018/03/applied-predictive-modeling-max-kuhn-kjell-johnson_1518.pdf), but if, like me, you prefer to have a paper copy of the book, you can purchase it [here](https://www.amazon.com/Applied-Predictive-Modeling-Max-Kuhn/dp/1461468485/ref=sr_1_1?ie=UTF8&qid=1540208647&sr=8-1&keywords=applied+predictive+modeling). **This post is dedicated to section 4 on overfitting and model tuning.**

<img src="/post/2018-10-22-reading-notes-series-applied-predictive-modeling-section-3_files/Applied Predictive Modeling Cover.png" alt="Book cover" width="400px"/>

The chapter is a great overview of the challenge of overfitting and of strategies to avoid it (resampling strategies mostly). It is written in a very didactic way.


**The problem of overfitting**

Overfitting is a problem that occurs in virtually every field where modeling is performed. A model overfits the data when it starts to fit features that are specific to the sample (noise) and that are not applicable to a new sample or to unseen data. Therfore, a model that overfits data may have a good performance on the training data, but this performance generalizes poorly on other data.

The following example taken from the book illustrates two examples of classification using a type of model called support vector machine. One of the tuning parameter for this type of model is generally called the "cost" parameter. When the cost parameter increases, the model tries to correctly predict every point in the training set. The figure on the left was generated with a high "cost" value. It is likely to fit features that are specific to the sample and is likely to overfit data. The figure on the right was generated with a low "cost" value. Although much simpler, it is more likely to generalize to new data. With this example, the authors do a good job at illustrating the trade-offs the modeler has to optimize.

<img src="/post/2018-10-23-reading-notes-series-applied-predictive-modeling-section-4-over-fitting-and-model-tuning_files/classification overfitting example.png" alt="Classification over-fitting example" width="400px"/>


**Model Tuning**

The authors propose a generic approach for model tuning that is based on resampling strategies. This approach is summarized in the following figure. When applied to the previous example, the support vector machine model can be evaluated with a range of "cost" values. For each interation, the model performance can be carefully characterized using resampling strategies and finally the performance of the different models can be compared.

<img src="/post/2018-10-23-reading-notes-series-applied-predictive-modeling-section-4-over-fitting-and-model-tuning_files/The model tuning process.png" alt="the Model Tuning Process" width="400px"/>


**Data Splitting**

When the sample size is very large, there is no real problem to assign a subset of the dataset to the test data and to use only the other part of the data to build the models. However, when the sample size is not that large, every sample in the dataset may be necessary to build a relevant model. In this case, the best strategy is to use resampling methods.

In most cases (although there are examples where it is not the case), **random sampling** of data is better. In cases where there is an imbalance between classes, **stratified random sampling** can be used in order to sample data from each class.

Another technique called **dissimilarity sampling** allows to sample data based on the predictor values.


**Resampling Techniques**

This is the "meat" of the chapter. The authors do a very good job at explaining the differences between the different techniques and detailing when one is more suitable than another one.

In the **k-Fold Cross-Validation** (see figure below), the dataset is randomly split into k-folds. At each iteration, one fold is held-out (starting by the first one) as the test sample. The model is calculated on the remaining (k-1) folds and its performance is assessed on the test sample. At the end, after the k iterations, we are left with k performance assessment that can be averaged to give an estimation of the model performance. 

<img src="/post/2018-10-23-reading-notes-series-applied-predictive-modeling-section-4-over-fitting-and-model-tuning_files/k-fold cross-validation.png" alt="k-Fold Cross-Validation principle" width="400px"/>

The choice of k is generally 5 or 10. When k is larger, the bias of estimation of the model performance decreases (at the expense of a larger computing time).

The k-Fold Cross-validation can be repeated... so for instance, we can use a k-Fold Cross-validation with k = 10 and 5 repeats, which will correspond to 50 estimates of the model performance.

A specific case of k-Fold Cross-Validation is called **LOOCV for "leave-one-out cross-validation"** for which k is equal to the sample size. At each iteration, only one obervation is held-out to characterize the model performance.


In the **Repeated Training/Test Splits** (see figure below), the process is to randomly subset a test set, to build the model on the remaining data and assess its performance on the test set. This is reproduced several times with each time a new resampling of the test set. When applying this strategy, there are 2 parameters to define: the proportion of the original dataset to use for each test set (a good rule of thumb is 20-25%) and the number of repetitions. To get stable estimate of performance, 50 to 200 repetitions are suggested. This strategy is sometimes called "leave-group-out cross-validation" or "Monte Carlo cross-validation".

<img src="/post/2018-10-23-reading-notes-series-applied-predictive-modeling-section-4-over-fitting-and-model-tuning_files/Repeated training - test splits.png" alt="Repeated training - test splits" width="400px"/>

In the **boostrap** (see figure below), the process is slightly different. At each iteration, a training sample having the same size as the original dataset is taken. The subtility is that this is a sampling with replacement which means that a specific observation occuring once in the original dataset may be appearing multiple times in the training dataset. At each iteration, the model is built on the training dataset and evaluated on the test dataset which is made of the observations that have not been selected in the training set for each iteration. Consequently, the different test sets have different sizes. The variability of the performance tends to be lower when using the boostrap technique. However, there is a larger bias in the evaluation of this performance. 

Some modifications to the boostrap have been proposed to try correcting the bias in performance estimation. The authors list the "632 method" and the "632+ method".

<img src="/post/2018-10-23-reading-notes-series-applied-predictive-modeling-section-4-over-fitting-and-model-tuning_files/Boostrap.png" alt="Boostrap" width="400px"/>


**Choosing final tuning parameters**

The authors propose a case study using a German credit data set containing 1,000 samples labelled "good" or "bad" for credit. Stratified random sampling is used to create a subset of 800 samples. Several support vector machine models are built with a "cost" parameter ranging from 0.25 to 128 using a k-Fold Cross-validation with k = 10 and 5 repeats. The results are available on the figure below. We see clearly that the performance estimated on the training data (apparent performance, red curve) keeps increasing to very high values when the "cost" parameter increases. However, the model performance average on test data (Cross-validated, black curve) has a very different pattern and level. It first increases rapidly. At a "cost" value of 2^0, it reaches a kind of plateau and starts decreasing for cost values larger than 2^4. The error bars represent +/- 2 standard deviations.

<img src="/post/2018-10-23-reading-notes-series-applied-predictive-modeling-section-4-over-fitting-and-model-tuning_files/Case Study Support Vector Machine.png" alt="Case Study SVM" width="400px"/>

The performance of the models is also summarized by the authors in the table below. Based on this table, there are several strategies to select a suitable model:

* the **"pick the best"** method leads to select the model that has the higher cross-validated performance. In this case, it would lead to select a model with a cost of 8. Although this us numerically the best model, simpler models (with lower "cost" values) have similar performances, so other strategies could lead to a better choice.
* the **"one standard error"** method leads to choose the simplest model which is in the range of the "pick the best" performance minus 1 standard deviation. In that case, it would lead to select a model with a cost of 2.
* the **"tolerance"** method for which we can define an acceptable percentage of performance decrease to favor a simpler model. If, we would accept a 4% loss in accuracy for example, a model with a "cost" of 1 would be suitable.

<img src="/post/2018-10-23-reading-notes-series-applied-predictive-modeling-section-4-over-fitting-and-model-tuning_files/Table for Case study SVM.png" alt="Models performance" width="400px"/>


**Data Splitting recommendations**

Based on the previous case study, the authors have compared several of the data splitting strategies mentionned above. The results are highlighted in the figure below. All strategies (except Boostrap with 632 method) lead to the same pattern described above: rapid increase of performance followed by plateau and then decrease of the performance. The main difference is in the variability of the predictions. It can be seen that the error bars vary widely (and also the computing time).

* **Bootstrap gets the least variable predictions**. However, the value of the prediction is slightly lower than what is given by the cross-validation strategies. Even if the data is resampled 50 times, it is less variable than k-fold cross validation with k=10 which is resampled 100 times. The authors mention that this difference in variability decreases significantly when sample size increases. The authors conclude that bootstrap is suitable in cases we are more interested in model selection than in model performance. In that case, the performance bias is less of an issue.

* **k-fold Cross-Validation is less biased**. It has a faster computing time. It has however a larger performance variability. However, this becomes less significant for larger samples, so it would be the way to go for large samples. For smaller sample sizes, the authors recommended multiple iterations of k-fold cross-validation.

<img src="/post/2018-10-23-reading-notes-series-applied-predictive-modeling-section-4-over-fitting-and-model-tuning_files/Data splitting comparisons.png" alt="Data splitting comparisons" width="400px"/>


**Choosing between models**

In this section, the authors propose a generic template to choose between different type of models:

1. Start testing models that are least interpretable (more "black-box") but more adaptable (boosted trees, support vector machine) as these models have a high likelihood to provide predictions with good precision;
2. Investigate simpler, less "black-box" model such as multivariate adaptive regression splines (MARS), partial least squares, generalized additive models, or naïve Bayes models;
3. Consider using the simplest model that reasonably approximates the per- formance of the more complex methods.


**Usefull functions in R**

There is a **R package** that has been designed for the book. It is the [AppliedPredictiveModeling](https://cran.r-project.org/web/packages/AppliedPredictiveModeling/index.html) package. Most of the functions can be found in the [caret](https://cran.r-project.org/web/packages/caret/caret.pdf) package and other packages (corrplot, e1071, lattice).

* `caret::createDataPartition()` to partition data. Ex: `trainingRows <- createDataPartition(classes, p = .80, list= FALSE)`. Can be used with repetitions: Ex: `repeatedSplits <- createDataPartition(trainClasses, p = .80, times = 3)`;
* `caret::maxdissim()` to sample according to maximum dissimilarity
* `caret::createResamples()` for bootstrapping.
* `caret::createFolds()` for k-fold cross-validation. Ex: `cvSplits <- createFolds(trainClasses, k = 10, returnTrain = TRUE)`
* `caret::createMultiFolds()` for repeated cross-validation.
* `caret::train()` function to train models. Can be used with many different arguments. Ex: to run the German bank support vector machine case study:

> set.seed(1056)  
> svmFit <- train(Class ~ ., data = GermanCreditTrain, method = "svmRadial")  

to specify pre-processing transformations:
  
> set.seed(1056)  
> svmFit <- train(Class ~ ., data = GermanCreditTrain, method = "svmRadial",  
>                 preProc = c("center", "scale"))

to test several tuning parameters:

> set.seed(1056)  
> svmFit <- train(Class ~ ., data = GermanCreditTrain, method = "svmRadial", preProc = c("center", "scale"),  
>                 tuneLength = 10)

Note: by default, the basic bootstrapt procedure is applied. In order to specify a 10-fold cross-validation:

> set.seed(1056)  
> svmFit <- train(Class ~ ., data = GermanCreditTrain, method = "svmRadial", preProc = c("center", "scale"), tuneLength = 10,  
>                 trControl = trainControl(method = "repeatedcv", repeats = 5))

* `caret::resamples()` can be used to compare models that have been assessed using the **same seed**. Ex: `resamp <- resamples(list(SVM = svmFit, Logistic = logisticReg))`, then: `summary(resamp)`

to compare models: `modelDifferences <- diff(resamp)` then: `summary(modelDifferences)`
