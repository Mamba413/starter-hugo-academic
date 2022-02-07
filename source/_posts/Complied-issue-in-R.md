---
title: Issue——compile C code in R
date: 2018-11-06 19:48:40
categories: 
  - Statistical Software
tags: 
  - R
  - expm package
  - Complie error
---

- I met the following problem when I try to install R package *expm*:

```shell
* installing *source* package ‘expm’ ...
** package ‘expm’ successfully unpacked and MD5 sums checked
** libs
gcc -std=gnu99 -I/app/vendor/R/lib/R/include -DNDEBUG  -I/usr/local/include    -fpic  -g -O2  -c R_dgebal.c -o R_dgebal.o
In file included from locale.h:4:0,
                 from expm.h:10,
                 from R_dgebal.c:4:
R_dgebal.c: In function ‘ebal_type’:
locale.h:5:19: error: ‘LC_MESSAGES’ undeclared (first use in this function)
 #define _(String) dgettext ("expm", String)
                   ^
R_dgebal.c:11:8: note: in expansion of macro ‘_’
  error(_("argument type='%s' must be a character string of string length 1"),
        ^
locale.h:5:19: note: each undeclared identifier is reported only once for each function it appears in
 #define _(String) dgettext ("expm", String)
                   ^
R_dgebal.c:11:8: note: in expansion of macro ‘_’
  error(_("argument type='%s' must be a character string of string length 1"),
        ^
R_dgebal.c: In function ‘R_dgebal’:
locale.h:5:19: error: ‘LC_MESSAGES’ undeclared (first use in this function)
 #define _(String) dgettext ("expm", String)
                   ^
R_dgebal.c:28:8: note: in expansion of macro ‘_’
  error(_("invalid 'x': not a numeric (classical R) matrix"));
        ^
make: *** [R_dgebal.o] Error 1
ERROR: compilation failed for package ‘expm’
* removing ‘/app/vendor/R/lib/R/library/expm’
ERROR: dependency ‘expm’ is not available for package ‘msm’
* removing ‘/app/vendor/R/lib/R/library/msm’
ERROR: dependency ‘msm’ is not available for package ‘ltm’
* removing ‘/app/vendor/R/lib/R/library/ltm’
```

- To fix this issue, I conduct these steps:
    - Download *expm_0.999-3.tar.gz* from https://cran.r-project.org/web/packages=expm
    - Unzip *expm_0.999-3.tar.gz* file and go into the src/ folder
    - Add a command in the first line of locale.h
    ```h
    #define LC_MESSAGES "en_US.UTF-8" /* Add this line */
    #include <R.h>
    #ifdef ENABLE_NLS
    #include <libintl.h>
    #define _(String) dgettext ("expm", String)
    #else
    #define _(String) (String)
    #endif  
    ```
    - Save the modification and **R CMD INSTALL expm**

- Reference
  - https://www.jianshu.com/p/7588472285f4
  - https://www.jianshu.com/p/b8c1e96edaab