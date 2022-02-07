---
title: Analysis Source Code of Mutual Information KSG2 estimator
date: 2019-02-10 11:12:00
categories: 
    - Machine Learnning
tags: 
    - R
    - Rcpp
    - Mutual information
    - KSG estimator
---

Recently, I am very interested in Mutual Information Estimator, especically, KSG estimator. However, I always concern that I haven't fully understand the KSG estimator, i.e.
$$I^{2}(X, Y) = \psi(k) - 1/k - <\psi(n_x) + \psi(n_y)> + \psi(N),$$
where $n_x$ and $n_y$ by the number of points with $\|x_i - x_j\| \leq \epsilon_{x}(i)$ and $\|y_i - y_j\| \leq \epsilon_{y}(i)$. (For more detail, see [Estimating mutual information](https://link.aps.org/doi/10.1103/PhysRevE.69.066138))

The key loop for computing $I^{2}(X, Y)$ are demonstrated in the following. (The all code can be view at [github](https://github.com/cran/rmi/blob/master/src/knn_mi.cpp))
```cpp
// update KSG estimator component : $<\psi(n_x) + \psi(n_y)>$:
for (int i = 0; i < N; i++) {
    for (int j = 1; j <= vars; j++) { // count marginal sum for jth variables
        N_x = 0;
        epsilon = 0;

        /**
         * @desrciption The following loop compute the marginal maximum $L_{\infty}$ distance to i-th point, 
            in other word, it compute the distances between the same points projected into the subspaces.
         * @param nn_inds : is a N \times K array. nn_inds(i, j) is a integer, 
            which records the index of the j-th nearest point.
        */
        for (int m = 1; m < K; m++) {
            // compute the marginal $L_{\infty}$-norm between two points:
            dist = 0;
            for (int n = d_start(j); n <= d_end(j); n++) {
                if (dist < abs(data(i,n)-data(nn_inds(i,m),n))) {
                    dist = abs(data(i,n)-data(nn_inds(i,m),n));
                }
            }
            if (dist > epsilon) {
                epsilon = dist;
            }
        }

        /** 
         * @description: the following loop iterates over all points and 
            count the points insides the subspace.
        */
        for (int m = 0; m < N; m++) {
            // compute the marginal $L_{\infty}$-norm between two points:
            dist = 0;
            for (int n = d_start(j); n <= d_end(j); n++) {
                if (dist < abs(data(i,n)-data(m,n))) {
                    dist = abs(data(i,n)-data(m,n));
                }
            }

            // evaluate whether inside:
            if (dist <= epsilon) N_x++;
        }

        // update:
        digamma_x = digamma_x + boost::math::digamma(N_x-1);
    }
}
// compute mi value:
mi = mi - digamma_x/(double)N - (vars - 1)/(double)k ;
```

