# [Regression Tree and Random Forest on Crime Rates](https://alfred-kctang.github.io/rt-rf-crime/)

## Table of Contents

* [Introduction](#introduction)
* [Data Source](#data-source)
* [Methodology](#methodology)
* [Conclusion](#conclusion)
* [Keywords](#keywords)
* [Coding Style](#coding-style)
* [License](#license)

## Introduction

The objective of this project is to demonstrate the workings of Regression Tree and Random Forest. In real-world applications, Regression Tree is seldom used, but the explanation for its mechanism sheds light on the workings of Random Forest, which is comprised of classification/regression trees. Random Forest is the de facto machine learning algorithm to try initially, even before developing a deeper understanding of the data at hand, because it is (1) versatile (it can perform both regression and classification tasks), (2) parallelizble (for clusters of machines to run), (3) quick (for training), (4) robust to outliers and non-linear data, (6) able to handle both unbalanced and high-dimensional data, (7) with low bias and moderate variance.

## Data Source

The data set is obtained from the [StatSci.org](http://www.statsci.org/data/general/uscrime.html).

## Methodology

The rpart function from the rpart package is used for building the Regression Tree model. The word "rpart" stands for recursive partitioning, which is a greedy algorithm in looking for a reasonable groupings that can be represented by a tree. The first question for the model is: how to choose a variable and its cut-off value? The answer from this function is: choose the one whose split leads to the least Residual Sum of Squares (RSS), which is calculated by the sum of the squared differences between data points in one group and its mean as well as the same sum of squared differences for the other (for there are two groups per split). The variable and its cut-off value with the minimum RSS are chosen, which is similar to the idea of fitting the best regression line with least square errors. The second question for the model is: when to stop the node splitting? It is determined by how much the reduction in RSS is in comparision with Total Sum of Squares, which is calculated by the sum of squared differences between data points and the sample mean, given a split. It is noted that the whole tree can also have its own RSS, and R-squared can be calculated from such RSS so that it can be used for comparison with a linear model.

For Random Forest, it is built using the randomForest function from the randomForest package. The algorithm generates a new set of observations by means of bootstrap, i.e. resampling uniformly at random with replacement, for the purpose of contructing each of the trees. There are, however, two key differences in the construction of trees between rpart and randomForest functions. First, a number of variables (as specified by the mtry parameter) are selected at random to divide data points into groups during each split, whereas the variable and its cut-off value with the lease RSS is chosen. Second, randomForest constructs trees in full size without pruning. To be precise, each tree is grown until the number of data points in each terminal node is no more than the size of terminal nodes (as specified by the nodesize parameter). Each of the trees would overfit the data, yet each of them overfit the data differently, and the “overreaction” to the random effects is averaged out by taking the average of the predictions made by all the trees.

As usual, I use the hold-out method so as to have an unbiased estimate of the chosen model's performance; to put it simply, a test set is chosen from the data that is only used to estimate the performance of the best model.

Leave-one-out cross validation is adopted for model comparison, between (1) one using only the first principal component, (2) the other using both first and second principal components, as well as (3) the model selected via backward elimination from my previous project.

## Conclusion

The following visualization graphically shows the partitions and predictions of the Regression Tree:

<p align="center">
  <img src="https://github.com/alfred-kctang/rt-rf-crime/blob/master/rtree.png?raw=true" alt="Regression Tree Visualized"/>
</p>

Compared with linear regression, Regression Tree seems to be limited in terms of descriptive power. Since the model essentially divides the data set into groups and use the mean response values of data points in a given group to make predictions, there could still exist huge differences between the data points within groups, in such a way that the mean is not an effective descriptive value for a given data point in the same group. And such differences within groups would be likely to exist with a small data set, because there is initially a limited number of data points to be divided into groups after all.

Nonetheless, the regression tree model can play a role of singling out important variables as the predictors, not only because the model output ranks variables by importance, but also because the data can be divided roughly by half by a given variable, which means that the variable could be an effective predictor for differentiating data points. It might help us to select a subset of explanatory variables in cases of many explanatory variables we have for a given data set.

On a deeper thought, the regression tree model or Classification and Regression Tree (CART) in general is similar to the clustering model: the regression tree model essentially looks for a given variable and its cut-off value that can divide data points roughtly into half with the minimum RSS, whereas the clustering model looks for some dimensions and thus its corresponding variables that can divide data points into certain clusters. Yet, one of their differences lies on how the mean values are used: the regression tree model uses the mean response values of the groups to make predictions - the mean is used for prediction after the grouping, whereas the clustering model uses the mean values as cluster centroids to do the grouping - the mean is used for grouping itself.

If the above line of thought related to clustering model is related to my first point, I believe the second point is connected with variable selection and Principal Component Analysis (PCA). If the regression tree model can help us single out some important explanatory variables that can differentiate data points well, then these variables are to be selected in a linear model and also play a larger role in constructing the first few Principal Components.

Similar to regression tree, random forest model appears to be less descriptive than linear models. As the former president of Kaggle Jeremy Howard answered a question in Quora in the following [link](https://www.quora.com/When-is-a-random-forest-a-poor-choice-relative-to-other-algorithms), “it is important to recognize that it is a predictive modeling tool, not a descriptive tool - if you are looking for a description of the relationships in your data, you should look at other options.” After all, it is hard to understand relationships between explanatory variables and the response variable, since a given tree is constructed by a randomly drawn set of some explanatory variables and a randomly drawn set of observations from the original data set, not to mention a multitude of trees in the model.

## Keywords

Classification and Regression Tree (CART); Holdout method; leave-one-out cross validation (LOOCV); Random Forest.

## Coding Style

This projects follows [Google's R Style Guide](https://google.github.io/styleguide/Rguide.html), which is a fork of the [Tidyverse Style Guide](https://style.tidyverse.org/) by Hadley Wickham.

## License

This repository is covered under the [MIT License](https://github.com/alfred-kctang/rt-rf-crime/blob/master/LICENSE).

