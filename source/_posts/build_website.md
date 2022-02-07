---
title: Build Website with Github and Hexo
date: 2018-10-19 20:18:00
categories: Website
tags: 
    - Website
    - Hexo
    - Github
---

- Tutorial: https://zhuanlan.zhihu.com/p/26625249

- Theme configuration Reference: https://theme-next.iissnan.com

- Theme configuration
    - Style Setting
      - Run the command in the Git Bash to download the **Next** theme:
      ```
      git clone https://github.com/iissnan/hexo-theme-next themes/next
      ```
      - Change the default theme to **Next** by modifying the **Extensions** part in ./_config.yml
      ```yml
      # Extensions
      ## Plugins: https://hexo.io/plugins/
      ## Themes: https://hexo.io/themes/
      theme: next
      ```
      - Set the style of **Next** theme by changing the **Schemes** part in ./themes/next/_config.yml. There are three options, including: *Mist*, *Pisces*, and *Gemini*. I prefer the *Gemini* style so the **Schemes** part are setting like this:
      ```yml
      # Schemes
      # scheme: Muse
      # scheme: Mist
      # scheme: Pisces
      scheme: Gemini
      ```
    - Add a **Categories** pages
      - Register a new page named **Categories**
        ```yml
        hexo new page categories
        ```
      - Modify the .md in ./source/categories directory
        ```yml
        title: Categories
        date: 2019-02-10 12:39:04
        type: "Categories"
        comments: false
        ---
        ```
      - Modify the themes configuration file ./themes/next/_config.yml
        ```yml
        menu:
        home: /
        categories: /categories
        archives: /archives
        ```
    - Add Comment Platform
      - Register a disqus: https://disqus.com/
      - Modify the themes configuration files
        ```yml
        disqus:
            enable: true
            shortname: mamba413
            count: true
        ```
    - Google Analytics
      - Register a google analytics: https://analytics.google.com/analytics/web/
      - Set the following ID: https://theme-next.iissnan.com/third-party-services.html#analytics-google

- Issues:
    - could not read Username for 'https://github.com': Invalid argument
        - solution: https://github.com/angular-schule/angular-cli-ghpages/issues/14
        ```yml
        deploy:
            type: git
            repo: https://Mamba413:mypassword@github.com/Mamba413/Mamba413.github.io.git
        ```
    - After you push the newest style to github, the website style might not change immediately, just be patience~