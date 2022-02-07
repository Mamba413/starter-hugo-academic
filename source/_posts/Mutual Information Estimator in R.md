---
title: Mutual Information Estimator in R
date: 2019-02-10 11:12:00
categories: 
    - Machine Learnning
tags: 
    - R
    - Mutual information
    - KSG estimator
---

Recently, I am interested in the mutual information and its estimator. So far as I know, a large amount of work has been devoted to estimate mutual information, such as Maximum Likelihood Estimator, Miller-Madow corrected Estimator, Bayesian Estimators, Chao-Shen Estimator, k-Nearest Neighbor Estimator, etc. Among them, it seems that k-Nearest Neighbor Estimator developed by Kraskov A. etc al is accepted widely and employed extensively. Actually, Kraskov provided two k-Nearest Neighbor Estimators, KSG1 and KSG2, which have similar performance and computational time though KSG2 is more accepted compared with KSG1. Henceforth, we pay more our attention on the KSG1 and KSG2.

There are several R-package implements KSG1 and KSG2, including **FNN**, **rmi** and **parmigene**.  We summary the R packages and their characteristics in the following Table.

| R-package     | R-Function | Estimator     | Time Complexity | Algorithm      | Speed      | Approximate |
| ------------- | ---------- | ------------- | --------------- | -------------- | ---------- | ----------- |
| **parmigene** | knnmi      | KSG2          | $O(N\log{N})$   | Grid Searching | Top method | Yes         |
| **FNN**       | mutinfo    | KSG1          | $O(N^2)$        | Crude          | Second     | No          |
| **rmi**       | knn_mi     | KSG1 and KSG2 | $O(N^2)$        | Crude          | Third      | No          |

You can run the code below to validate the *Approximate* Column.
```R
set.seed(1)
num <- 300
x <- rnorm(num)
y <- rnorm(num)
rmi::knn_mi(cbind(x, y), splits = c(1, 1), options = list(method = "KSG2", k = 3))
parmigene::knnmi(x, y, k = 3, noise = 0)
rmi::knn_mi(cbind(x, y), splits = c(1, 1), options = list(method = "KSG1", k = 3))
FNN::mutinfo(x, Y = y, k = 3)
```

You can run the code below to validate the *Speed* Column.
```R
set.seed(1)
num <- 3000
x <- rnorm(num)
y <- rnorm(num)
system.time(rmi::knn_mi(cbind(x, y), splits = c(1, 1), options = list(method = "KSG2", k = 3)))
system.time(parmigene::knnmi(x, y, k = 3, noise = 0))
system.time(FNN::mutinfo(x, Y = y, k = 3))
```

You can see the source code of three packages to validate the *Estimator*, *Algorithm* Columns, 

| R-package     | Estimator     | Algorithm                                                                                               |
| ------------- | ------------- | ------------------------------------------------------------------------------------------------------- |
| **parmigene** | KSG2          | [Grid Searching](https://github.com/cran/parmigene/blob/master/src/mi.c), *mutual_information* function |
| **FNN**       | KSG1          | [Crude](https://github.com/cran/FNN/blob/master/src/KNN_mutual_information.cpp), *mdmutinfo* function   |
| **rmi**       | KSG1 and KSG2 | [Crude](https://github.com/cran/rmi/blob/master/src/get_nearest_neighbors.cpp), *knn_mi* function       |
