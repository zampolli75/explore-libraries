
how jenny might do this in a first exploration purposely leaving a few things to change later!

Which libraries does R search for packages?

``` r
.libPaths()
```

    ## [1] "/Library/Frameworks/R.framework/Versions/3.4/Resources/library"

let's confirm the second element is, in fact, the default library
-----------------------------------------------------------------

``` r
.Library
```

    ## [1] "/Library/Frameworks/R.framework/Resources/library"

``` r
library(fs)
path_real(.Library)
```

    ## /Library/Frameworks/R.framework/Versions/3.4/Resources/library

' Installed packages
====================

library(tidyverse) ipt &lt;- installed.packages() %&gt;% as\_tibble()

how many packages?
------------------

nrow(ipt)

' Exploring the packages
========================

count some things! inspiration
------------------------------

\* tabulate by LibPath, Priority, or both
-----------------------------------------

ipt %&gt;% count(LibPath, Priority)

\* what proportion need compilation?
------------------------------------

ipt %&gt;% count(NeedsCompilation) %&gt;% mutate(prop = n / sum(n))

\* how break down re: version of R they were built on
-----------------------------------------------------

ipt %&gt;% count(Built) %&gt;% mutate(prop = n / sum(n))

' Reflections
=============

reflect on ^^ and make a few notes to yourself; inspiration
-----------------------------------------------------------

\* does the number of base + recommended packages make sense to you?
--------------------------------------------------------------------

\* how does the result of .libPaths() relate to the result of .Library?
-----------------------------------------------------------------------

' Going further
===============

if you have time to do more ...
-------------------------------

is every package in .Library either base or recommended?
--------------------------------------------------------

all\_default\_pkgs &lt;- list.files(.Library) all\_br\_pkgs &lt;- ipt %&gt;% filter(Priority %in% c("base", "recommended")) %&gt;% pull(Package) setdiff(all\_default\_pkgs, all\_br\_pkgs)

study package naming style (all lower case, contains '.', etc
-------------------------------------------------------------

use `fields` argument to installed.packages() to get more info and use it!
--------------------------------------------------------------------------

ipt2 &lt;- installed.packages(fields = "URL") %&gt;% as\_tibble() ipt2 %&gt;% mutate(github = grepl("github", URL)) %&gt;% count(github) %&gt;% mutate(prop = n / sum(n))

sessionInfo()
