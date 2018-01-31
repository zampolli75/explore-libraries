01\_explore-libraries\_jenny.R
================
Joaquin
Wed Jan 31 14:05:29 2018

``` r
## how jenny might do this in a first exploration
## purposely leaving a few things to change later!
```

Which libraries does R search for packages?

``` r
.libPaths()
```

    ## [1] "/Library/Frameworks/R.framework/Versions/3.4/Resources/library"

``` r
## let's confirm the second element is, in fact, the default library
.Library
```

    ## [1] "/Library/Frameworks/R.framework/Resources/library"

``` r
library(fs)
path_real(.Library)
```

    ## /Library/Frameworks/R.framework/Versions/3.4/Resources/library

Installed packages

``` r
library(tidyverse)
```

    ## ── Attaching packages ───────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 2.2.1     ✔ purrr   0.2.4
    ## ✔ tibble  1.3.4     ✔ dplyr   0.7.4
    ## ✔ tidyr   0.7.2     ✔ stringr 1.2.0
    ## ✔ readr   1.1.1     ✔ forcats 0.2.0

    ## ── Conflicts ──────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
ipt <- installed.packages() %>%
  as_tibble()

## how many packages?
nrow(ipt)
```

    ## [1] 243

Exploring the packages

``` r
## count some things! inspiration
##   * tabulate by LibPath, Priority, or both
ipt %>%
  count(LibPath, Priority)
```

    ## # A tibble: 3 x 3
    ##                                                          LibPath
    ##                                                            <chr>
    ## 1 /Library/Frameworks/R.framework/Versions/3.4/Resources/library
    ## 2 /Library/Frameworks/R.framework/Versions/3.4/Resources/library
    ## 3 /Library/Frameworks/R.framework/Versions/3.4/Resources/library
    ## # ... with 2 more variables: Priority <chr>, n <int>

``` r
##   * what proportion need compilation?
ipt %>%
  count(NeedsCompilation) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 3 x 3
    ##   NeedsCompilation     n       prop
    ##              <chr> <int>      <dbl>
    ## 1               no   110 0.45267490
    ## 2              yes   124 0.51028807
    ## 3             <NA>     9 0.03703704

``` r
##   * how break down re: version of R they were built on
ipt %>%
  count(Built) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 4 x 3
    ##   Built     n       prop
    ##   <chr> <int>      <dbl>
    ## 1 3.4.0   121 0.49794239
    ## 2 3.4.1    34 0.13991770
    ## 3 3.4.2    78 0.32098765
    ## 4 3.4.3    10 0.04115226

Reflections

``` r
## reflect on ^^ and make a few notes to yourself; inspiration
##   * does the number of base + recommended packages make sense to you?
##   * how does the result of .libPaths() relate to the result of .Library?
```

Going further

``` r
## if you have time to do more ...

## is every package in .Library either base or recommended?
all_default_pkgs <- list.files(.Library)
all_br_pkgs <- ipt %>%
  filter(Priority %in% c("base", "recommended")) %>%
  pull(Package)
setdiff(all_default_pkgs, all_br_pkgs)
```

    ##   [1] "assertthat"       "backports"        "base64enc"       
    ##   [4] "BH"               "bindr"            "bindrcpp"        
    ##   [7] "binman"           "bitops"           "blogdown"        
    ##  [10] "bookdown"         "bootstrap"        "broom"           
    ##  [13] "callr"            "car"              "caret"           
    ##  [16] "caTools"          "cellranger"       "cli"             
    ##  [19] "clipr"            "clisymbols"       "colorspace"      
    ##  [22] "combinat"         "crayon"           "curl"            
    ##  [25] "CVST"             "data.table"       "DBI"             
    ##  [28] "dbplyr"           "ddalpha"          "DEoptimR"        
    ##  [31] "desc"             "devtools"         "dichromat"       
    ##  [34] "digest"           "dimRed"           "dplyr"           
    ##  [37] "DRR"              "enc"              "evaluate"        
    ##  [40] "forcats"          "foreach"          "formatR"         
    ##  [43] "fs"               "gam"              "gbm"             
    ##  [46] "GGally"           "ggforce"          "ggfortify"       
    ##  [49] "ggplot2"          "ggraph"           "ggrepel"         
    ##  [52] "gh"               "git2r"            "glue"            
    ##  [55] "gower"            "gridExtra"        "gtable"          
    ##  [58] "haven"            "here"             "highr"           
    ##  [61] "hms"              "htmltools"        "htmlwidgets"     
    ##  [64] "httpuv"           "httr"             "hunspell"        
    ##  [67] "igraph"           "influence.ME"     "ini"             
    ##  [70] "ipred"            "irlba"            "ISLR"            
    ##  [73] "iterators"        "janeaustenr"      "jsonlite"        
    ##  [76] "kableExtra"       "kernlab"          "klaR"            
    ##  [79] "knitr"            "labeling"         "lava"            
    ##  [82] "lazyeval"         "leaps"            "lme4"            
    ##  [85] "lmtest"           "lubridate"        "magick"          
    ##  [88] "magrittr"         "mailR"            "markdown"        
    ##  [91] "MatrixModels"     "memoise"          "mime"            
    ##  [94] "miniUI"           "minpack.lm"       "minqa"           
    ##  [97] "MKmisc"           "mlbench"          "mnormt"          
    ## [100] "ModelMetrics"     "modelr"           "mongolite"       
    ## [103] "MPV"              "munsell"          "nloptr"          
    ## [106] "NLP"              "nlstools"         "numDeriv"        
    ## [109] "openssl"          "orcutt"           "ordinal"         
    ## [112] "packrat"          "pbkrtest"         "pdftools"        
    ## [115] "pkgconfig"        "PKI"              "plogr"           
    ## [118] "plyr"             "prettyunits"      "prodlim"         
    ## [121] "progress"         "pscl"             "psych"           
    ## [124] "purrr"            "qpcR"             "quantreg"        
    ## [127] "R.methodsS3"      "R.oo"             "R.utils"         
    ## [130] "R6"               "randomForest"     "rappdirs"        
    ## [133] "raster"           "RColorBrewer"     "Rcpp"            
    ## [136] "RcppEigen"        "RcppRoll"         "RCurl"           
    ## [139] "readr"            "readxl"           "recipes"         
    ## [142] "REEMtree"         "rematch"          "rematch2"        
    ## [145] "reprex"           "repurrrsive"      "reshape"         
    ## [148] "reshape2"         "rgl"              "RGoogleAnalytics"
    ## [151] "ridge"            "rJava"            "rjson"           
    ## [154] "RJSONIO"          "rlang"            "rmarkdown"       
    ## [157] "RMongo"           "RMySQL"           "robustbase"      
    ## [160] "rpart.plot"       "rprojroot"        "rsconnect"       
    ## [163] "RSelenium"        "rstudioapi"       "rvest"           
    ## [166] "scales"           "selectr"          "semver"          
    ## [169] "sendmailR"        "servr"            "sfsmisc"         
    ## [172] "shiny"            "slam"             "SnowballC"       
    ## [175] "sourcetools"      "sp"               "SparseM"         
    ## [178] "stargazer"        "stringi"          "stringr"         
    ## [181] "styler"           "subprocess"       "survey"          
    ## [184] "tau"              "textcat"          "tibble"          
    ## [187] "tidyr"            "tidyselect"       "tidytext"        
    ## [190] "tidyverse"        "timeDate"         "tm"              
    ## [193] "tokenizers"       "translations"     "tree"            
    ## [196] "tweenr"           "ucminf"           "udunits2"        
    ## [199] "units"            "usdm"             "usethis"         
    ## [202] "V8"               "vcd"              "viridis"         
    ## [205] "viridisLite"      "wdman"            "whisker"         
    ## [208] "widyr"            "withr"            "XML"             
    ## [211] "xml2"             "xray"             "xtable"          
    ## [214] "yaml"             "zoo"

``` r
## study package naming style (all lower case, contains '.', etc

## use `fields` argument to installed.packages() to get more info and use it!
ipt2 <- installed.packages(fields = "URL") %>%
  as_tibble()
ipt2 %>%
  mutate(github = grepl("github", URL)) %>%
  count(github) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 2 x 3
    ##   github     n      prop
    ##    <lgl> <int>     <dbl>
    ## 1  FALSE   133 0.5473251
    ## 2   TRUE   110 0.4526749
