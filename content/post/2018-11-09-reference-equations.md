---
title: Reference Equations
author: Laurent Barcelo
date: '2018-11-09'
slug: reference-equations
categories:
  - Posts
tags:
  - Equations
description: ''
featured_image: ''
---

(Note: this post is constantly updated...)

**Regression**

* Sample average of x: $\bar{x} = \sum_{i=1}^n\frac{x_i}{n}$  

* Sum of Squares of the x:  $\text{SXX} = \sum_{i=1}^n(x_i - \bar{x})^2$

* Sample variance of the x: $\text{SD}^2_x = \frac{\text{SXX}}{n-1}$

* Sample standard deviation of the x: $\text{SD}_x = \sqrt{\frac{\text{SXX}}{n-1}}$

* Sum of cross-products: $\text{SXY} = \sum_{i=1}^n(x_i - \bar{x})(y_i - \bar{y})$

* Sample covariance: $s_{XY} = \frac{\text{SXY}}{n-1}$

* Residual Sum of Square (RSS) or SSE: $\text{RSS} = \text{SSE} = \sum_{i=1}^n(y_i - \hat{y_i})^2$

* Variance of the regression: $\hat{\sigma}^2 = \text{MSE} = \frac{\text{RSS}}{n-2}$ where $n-2$ is the degree of freedom $\text{df}$

* Regression equation slope: $\hat{\beta_1} = \frac{\text{SXY}}{\text{SXX}}$

* Regression equation intercept: $\hat{\beta_0} = \bar{y} - \hat{\beta_1}\bar{x}$

* Variance of the regression slope:  $\text{Var}({\hat{\beta_1}|X}) = \frac{\sigma^2}{\text{SXX}}$ ... use $\hat{\sigma}$ instead of ${\sigma}$ for the estimate of the variance;

* Variance of the regression intercept: $\text{Var}({\hat{\beta_0}|X}) = \sigma^2(\frac{1}{n} + \frac{\bar{x}^2}{\text{SXX}})$  ... use $\hat{\sigma}$ instead of ${\sigma}$ for the estimate of the variance;

* standard error $\text{se}({\hat{\beta_1}|X}) = \sqrt{\hat{\text{Var}}(\hat{\beta_1}|X)}$   