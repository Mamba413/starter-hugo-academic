---
title: Weighted Count of the Smaller Number After Self
date: 2019-04-15 19:48:40
categories: 
    - Algorithm
tags: 
    - C++
    - LeetCode
    - Count of the smaller number after self 
---

"[Count of the smaller number after self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/) (CSNAS)" is a Algorithms Problem in [LeetCode](https://leetcode.com/problemset/algorithms/). It is considered as a Hard issue but can be solved by Merge Sort algorithm in an efficient way. In my project, I encounter a problem which is quite similar to problem CSNAS, and I call it as **Weighted Count of the Smaller Number After Self** (WCSNAS) problem. Before introducing the WCSNAS problem and related efficient solution, we first describe the CSNAS in the following.

### Problem Description for CSNAS
- You are given an integer array _nums_ and you have to return a new _counts_ array. The _counts_ array has the property where `counts[i]` is the number of smaller elements to the right of `nums[i]`.

**Example:**
```
Input: [5,2,6,1]
Output: [2,1,1,0] 
Explanation:
To the right of 5 there are 2 smaller elements (2 and 1).
To the right of 2 there is only 1 smaller element (1).
To the right of 6 there is 1 smaller element (1).
To the right of 1 there is 0 smaller element.
```

### Problem Description for WCSNAS
- You are given two arrays, the first array is an integer array _nums_ and the second array is a double array _weight_. You have to return a new _weight_counts_ array. The _counts_ array has the property where `wcounts[i]` is the summation of weights of smaller elements to the right of `nums[i]`.

**Example:**
```
Input:
nums = [1, 3, 5, 4, 2, 2]
weight = [3.0, 2.0, 4.0, 1.0, 2.0, 5.0]
Output:
wcounts = [0.0, 7.0, 8.0, 7.0, 5.0, 0.0]
Explanation:
To the right of 1 there are 0 smaller element.
To the right of 3 there are 2 smaller elements (2 and 2) and their related weights are 2.0 and 5.0. Therefore, the summation of weights is 7.0.
To the right of 5 there are 3 smaller elements (4, 2, and 2) and their related weights are 1.0, 2.0 and 5.0. Therefore, the summation of weights is 8.0.
```

### Solve WCSNAS with Merge Sort
[StefanPochmann](https://leetcode.com/stefanpochmann/) suggest a merge sort 
solution for CSNAS with merge sort: "The smaller numbers on the right of a number are exactly those that jump from its right to its left during a stable sort. So I do mergesort with added tracking of those right-to-left jumps."

My solution mimic the solution of CSNAS by adding tracking of those right-to-left jumps for weight array. The source code implemented in C++ is shown below.

```C++
template<typename T>
std::vector<T> weight_sum_count_smaller_number_after_self(std::vector<T> &vector, std::vector<double> &weight) {
    std::vector<double> right_smaller_weight_sum(vector.size(), 0);
    std::vector<std::pair<int, T>> vec(vector.size());
    std::vector<std::pair<int, double>> weight_vec(weight.size());
    for (uint i = 0; i < vector.size(); i++) {
        vec[i] = std::make_pair(i, vector[i]);
        weight_vec[i] = std::make_pair(i, weight[i]);
    }
    weight_sum_merge_sort(vec, weight_vec, 0, (int) vec.size(), right_smaller_weight_sum);
    return right_smaller_weight_sum;
}

template<typename T>
void
weight_sum_merge_sort(std::vector<std::pair<int, T>> &vec, std::vector<std::pair<int, double>> &weight_vec,
                      int start, int end,
                      std::vector<double> &right_smaller_weight_sum) {
    if (end - start <= 1) return;
    int mid = (start + end) >> 1;
    weight_sum_merge_sort(vec, weight_vec, start, mid, right_smaller_weight_sum);
    weight_sum_merge_sort(vec, weight_vec, mid, end, right_smaller_weight_sum);
    weight_sum_merge(vec, weight_vec, start, mid, end, right_smaller_weight_sum);
}

template<typename T>
void weight_sum_merge(std::vector<std::pair<int, T>> &vec, std::vector<std::pair<int, double>> &weight_vec,
                      int start, int mid, int end,
                      std::vector<double> &right_smaller_weight_sum) {
    std::vector<std::pair<int, T>> left(vec.begin() + start, vec.begin() + mid);
    std::vector<std::pair<int, T>> right(vec.begin() + mid, vec.begin() + end);
    std::vector<std::pair<int, double>> weight_left(weight_vec.begin() + start, weight_vec.begin() + mid);
    std::vector<std::pair<int, double>> weight_right(weight_vec.begin() + mid, weight_vec.begin() + end);
    uint left_merged = 0, right_merged = 0, right_merged_tmp = 0, total_merged = 0;
    while (left_merged < left.size() && right_merged < right.size()) {
        if (left[left_merged].second < right[right_merged].second) {
            vec[start + total_merged] = left[left_merged];
            weight_vec[start + total_merged] = weight_left[left_merged];
            right_merged_tmp = right_merged;
            while (right_merged_tmp > 0) {
                right_smaller_weight_sum[left[left_merged].first] += weight_right[right_merged_tmp - 1].second;
                right_merged_tmp--;
            }
            ++left_merged;
            ++total_merged;
        } else {
            vec[start + total_merged] = right[right_merged];
            weight_vec[start + total_merged] = weight_right[right_merged];
            ++right_merged;
            ++total_merged;
        }
    }
    while (left_merged < left.size()) {
        vec[start + total_merged] = left[left_merged];
        weight_vec[start + total_merged] = weight_left[left_merged];
        right_merged_tmp = right_merged;
        while (right_merged_tmp > 0) {
            right_smaller_weight_sum[left[left_merged].first] += weight_right[right_merged_tmp - 1].second;
            right_merged_tmp--;
        }
        ++left_merged;
        ++total_merged;
    }
    while (right_merged < right.size()) {
        vec[start + total_merged] = right[right_merged];
        weight_vec[start + total_merged] = weight_right[right_merged];
        ++right_merged;
        ++total_merged;
    }
}

```
