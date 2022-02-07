---
title: A Fast Way to Build an R Package Website 
date: 2019-04-15 19:48:40
categories: 
    - Algorithm
tags: 
    - C++
    - LeetCode
    - Count of the smaller number after self 
---

Build an R package facilites the usage of an this package and helps spread this package. 
By integrating github action, pkgdown and usethis packages, R users can efficiently create a website for their package. 
An interesting example might be the website for the ***Ball*** package: Mamba.github.io/Ball. 
Here, I will summary the steps for building this website. 

1. Run `usethis::use_pkgdown()` to declare that a website to be build for your package. Some necessary files for extablishing static website will be automatically generated.
2. Run `pkgdown::build_site()` to start building a static website. You would see that many .html files are created and put into the *docs* folder. Open any .html file can browser you website in the local computer. However, we are more interesting in deploy this website to a public available website, and thus, we require an important component: github action. Before that, please commit your change at present.
3. To publish you website, we need to create an empty gh-pages branch. In bash, run:
   ```bash
    git checkout --orphan gh-pages
    git rm -rf .
    git commit --allow-empty -m 'Initial gh-pages commit'
    git push origin gh-pages
    git checkout master
   ```
4. Simply run the command:
   ```R
   usethis::use_github_action(url = "https://raw.githubusercontent.com/r-lib/actions/master/examples/pkgdown.yaml")
   ```
   to get a .yaml file to add in the github action for pkgdown to automatically add documentation.
5. Setting the ACCESS.TOKEN such that the github action would be start for your push action. Specifically, we should conduct the following steps:
   - Get a personal access tokens. Go to https://github.com/settings/tokens and click “Generate new token”. Then you must copy the value of the token.
   - Add a gitHub secrets. GitHub secrets are a way to use values in your yaml that need to remain secret, such as credentials or information you want to keep private. Create a github secret in the *setting* on the repostory, and copy the value of the personal access token into a secret for your repository so that a GitHub action can be authenticated as you.
6. Push your newest into github, then you would see the website for your package is building. 