
<!-- README.md is generated from README.Rmd. Please edit that file -->
ssc
===

Provides a collection of self-labeled techniques for semi-supervised classification. In semi-supervised classification, both labeled and unlabeled data are used to train a classifier. This learning paradigm has obtained promising results, specifically in the presence of a reduced set of labeled examples. This package implements a collection of self-labeled techniques to construct a classification model. These techniques enlarges the original labeled set using the most confident predictions to classify unlabeled data. The techniques implemented can be applied to classification problems in several domains by the specification of a suitable supervised base classifier. At low ratios of labeled data, it can be shown to perform better than classical supervised classifiers.

Installation
------------

You can install ssc from github with:

``` r
# install.packages("devtools")
devtools::install_github("mabelc/SSC")
```

Example
-------

This is a basic example which shows you how to solve a common problem:

``` r
library(ssc)

## Load Iris data set
data(iris)

x <- iris[, -5] # instances without classes
x <- as.matrix(x)
y <- iris$Species

## Prepare data
set.seed(1)
# Use 50% of instances for training
tra.idx <- sample(x = length(y), size = ceiling(length(y) * 0.5))
xtrain <- x[tra.idx,] # training instances
ytrain <- y[tra.idx]  # classes of training instances
# Use 70% of train instances as unlabeled set
tra.na.idx <- sample(x = length(tra.idx), size = ceiling(length(tra.idx) * 0.7))
ytrain[tra.na.idx] <- NA # remove class information of unlabeled instances

# Use the other 50% of instances for inductive testing
tst.idx <- setdiff(1:length(y), tra.idx)
xitest <- x[tst.idx,] # testing instances
yitest <- y[tst.idx] # classes of testing instances

## Example: Training from a set of instances with 1-NN as base classifier.
library(caret)
#> Loading required package: lattice
#> Loading required package: ggplot2
m <- selfTraining(x = xtrain, y = ytrain, learner = knn3, learner.pars = list(k = 1))
pred <- predict(m, xitest)
table(pred, yitest)
#>             yitest
#> pred         setosa versicolor virginica
#>   setosa         24          0         0
#>   versicolor      0         22         9
#>   virginica       0          0        20
```

Overview of the package
-----------------------

The ssc package implements the following self-labeled techniques:

-   coBC
-   democratic
-   selfTraining
-   setred
-   snnrce
-   triTraining

selfTraining, setred and snnrce are single-classifier based. coBC, democratic and triTraining are multi-classifier based.
