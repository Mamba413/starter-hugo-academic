---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "robustlm: Robust variable selection with exponential squared loss"
subtitle: ""
summary: ""
authors: []
tags: [robust variable selection]
categories: []
featured: false
draft: false
show_date: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

The goal of robustlm is to carry out robust variable selection through exponential squared loss. Specifically, it solves a non-convex optimization problem specified in (Wang et al., 2013). Regularization parameters are chosen adaptively by default, while they can be supplied by the user. Block coordinate gradient descent algorithm is used to efficiently solve the optimization problem.