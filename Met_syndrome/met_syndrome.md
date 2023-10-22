---
title: "met_syndrome"
output:
  html_document: 
    keep_md: yes
  pdf_document: default
date: "2023-10-22"
---



## R Markdown

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:


```r
summary(cars)
```

```
##      speed           dist       
##  Min.   : 4.0   Min.   :  2.00  
##  1st Qu.:12.0   1st Qu.: 26.00  
##  Median :15.0   Median : 36.00  
##  Mean   :15.4   Mean   : 42.98  
##  3rd Qu.:19.0   3rd Qu.: 56.00  
##  Max.   :25.0   Max.   :120.00
```

## Including Plots

You can also embed plots, for example:

![](met_syndrome_files/figure-html/pressure-1.png)<!-- -->

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.


```r
library(tidyverse)
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.2     ✔ readr     2.1.4
## ✔ forcats   1.0.0     ✔ stringr   1.5.0
## ✔ ggplot2   3.4.2     ✔ tibble    3.2.1
## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
## ✔ purrr     1.0.1     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

```r
library(here)
```

```
## here() starts at C:/Users/HP/OneDrive - Universiti Sains Malaysia/Desktop/Statistical computaion/Met_syndrome
```

```r
library(haven)
library(readxl)
library(kableExtra)
```

```
## Warning in !is.null(rmarkdown::metadata$output) && rmarkdown::metadata$output
## %in% : 'length(x) = 2 > 1' in coercion to 'logical(1)'
```

```
## 
## Attaching package: 'kableExtra'
## 
## The following object is masked from 'package:dplyr':
## 
##     group_rows
```

```r
library(broom)
library(cdata)
```

```
## Loading required package: wrapr
## 
## Attaching package: 'wrapr'
## 
## The following object is masked from 'package:dplyr':
## 
##     coalesce
## 
## The following objects are masked from 'package:tidyr':
## 
##     pack, unpack
## 
## The following object is masked from 'package:tibble':
## 
##     view
```

```r
library(corrplot)
```

```
## corrplot 0.92 loaded
```

```r
library(survival)
```

```r
met <- read_xlsx(here('metab_syndrome.xlsx')) %>% 
  janitor::clean_names()
```

```
## Warning: Expecting numeric in C2373 / R2373C3: got '#NULL!'
```

```
## Warning: Expecting numeric in F2406 / R2406C6: got '#NULL!'
```

```
## Warning: Expecting numeric in G2406 / R2406C7: got '#NULL!'
```

```
## Warning: Expecting numeric in H2406 / R2406C8: got '#NULL!'
```

```
## Warning: Expecting numeric in F2421 / R2421C6: got '#NULL!'
```

```
## Warning: Expecting numeric in G2421 / R2421C7: got '#NULL!'
```

```
## Warning: Expecting numeric in H2421 / R2421C8: got '#NULL!'
```

```
## Warning: Expecting numeric in G2493 / R2493C7: got '#NULL!'
```

```
## Warning: Expecting numeric in E4002 / R4002C5: got '#NULL!'
```

```
## Warning: Expecting numeric in G4041 / R4041C7: got '#NULL!'
```

```
## Warning: Expecting numeric in G4227 / R4227C7: got '#NULL!'
```

```
## Warning: Expecting numeric in E4250 / R4250C5: got '#NULL!'
```

```r
glimpse(met)
```

```
## Rows: 4,341
## Columns: 17
## $ id      <chr> "A11", "B11", "C12", "D11", "E11", "F11", "G12", "H12", "I12",…
## $ age     <chr> "46", "47", "48", "63", "39", "74", "43", "55", "22", "42", "4…
## $ dmdx    <dbl> 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 1,…
## $ height  <chr> "1.6", "1.68", "1.47", "1.5", "1.51", "1.43", "1.77", "1.63", …
## $ weight  <dbl> 70.0, 52.0, 88.6, 81.0, 63.5, 50.0, 90.3, 69.0, 74.0, 53.2, 49…
## $ waist   <dbl> 89.0, 89.0, 84.0, 125.0, 87.0, 85.0, 101.0, 94.0, 87.0, 72.0, …
## $ neck    <dbl> 38.0, 38.0, 32.0, 34.0, 40.0, 34.5, 39.0, 43.3, 34.0, 33.0, 32…
## $ hip     <dbl> 97.0, 98.0, 94.0, 95.0, 105.0, 95.0, 112.0, 103.0, 106.0, 89.0…
## $ pulse   <chr> "80", "83", "78", "94", "99", "96", "82", "89", "75", "78", "6…
## $ msbpr   <dbl> 133.0, 163.0, 146.5, 206.5, 129.0, 190.5, 160.0, 138.0, 114.0,…
## $ mdbpr   <dbl> 83.5, 84.0, 93.5, 94.0, 70.0, 92.0, 101.0, 86.0, 63.5, 69.0, 8…
## $ hba1c   <chr> "5.3", "5.6", "5.7", "7.2", "5.4", "5.7", "5.0999999999999996"…
## $ fbs     <chr> "5.82", "6.29", "8.2899999999999991", "8.39", "5.23", "6.45", …
## $ mogtt2h <chr> "7.89", "6.05", "17.39", "#NULL!", "7.84", "14.88", "7.61", "#…
## $ totchol <chr> "5.27", "6.77", "5.87", "8.09", "5.55", "6.33", "7.48", "5.5",…
## $ hdl     <chr> "0.84", "1.45", "0.82", "1.79", "1.04", "1.62", "1.57", "1.159…
## $ ldl     <chr> "3.45", "3.7", "3.96", "4.68", "4.33", "3.03", "4.59", "3.8", …
```

```r
library(janitor)
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```


```r
summary(met)
```

```
##       id                age                 dmdx           height         
##  Length:4341        Length:4341        Min.   :0.0000   Length:4341       
##  Class :character   Class :character   1st Qu.:0.0000   Class :character  
##  Mode  :character   Mode  :character   Median :0.0000   Mode  :character  
##                                        Mean   :0.1083                     
##                                        3rd Qu.:0.0000                     
##                                        Max.   :1.0000                     
##                                        NA's   :1                          
##      weight           waist             neck            hip        
##  Min.   : 30.00   Min.   : 50.80   Min.   :22.00   Min.   : 61.00  
##  1st Qu.: 53.80   1st Qu.: 77.00   1st Qu.:33.00   1st Qu.: 91.00  
##  Median : 62.00   Median : 86.00   Median :35.00   Median : 97.00  
##  Mean   : 63.75   Mean   : 86.32   Mean   :35.38   Mean   : 97.88  
##  3rd Qu.: 71.95   3rd Qu.: 95.00   3rd Qu.:38.00   3rd Qu.:104.00  
##  Max.   :187.80   Max.   :154.50   Max.   :50.00   Max.   :160.00  
##  NA's   :2        NA's   :2        NA's   :5       NA's   :2       
##     pulse               msbpr           mdbpr           hba1c          
##  Length:4341        Min.   : 68.5   Min.   : 41.50   Length:4341       
##  Class :character   1st Qu.:117.0   1st Qu.: 70.00   Class :character  
##  Mode  :character   Median :130.0   Median : 77.50   Mode  :character  
##                     Mean   :133.5   Mean   : 78.46                     
##                     3rd Qu.:146.5   3rd Qu.: 86.00                     
##                     Max.   :237.0   Max.   :128.50                     
##                                                                        
##      fbs              mogtt2h            totchol              hdl           
##  Length:4341        Length:4341        Length:4341        Length:4341       
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##      ldl           
##  Length:4341       
##  Class :character  
##  Mode  :character  
##                    
##                    
##                    
## 
```

```r
met <- met %>% 
  mutate_at(vars(-id), ~as.numeric(.))
```

```
## Warning: There were 9 warnings in `mutate()`.
## The first warning was:
## ℹ In argument: `age = (structure(function (..., .x = ..1, .y = ..2, . = ..1)
##   ...`.
## Caused by warning in `as.numeric()`:
## ! NAs introduced by coercion
## ℹ Run `dplyr::last_dplyr_warnings()` to see the 8 remaining warnings.
```

```r
summary(met)
```

```
##       id                 age             dmdx            height     
##  Length:4341        Min.   :18.00   Min.   :0.0000   Min.   :1.270  
##  Class :character   1st Qu.:38.00   1st Qu.:0.0000   1st Qu.:1.510  
##  Mode  :character   Median :48.00   Median :0.0000   Median :1.560  
##                     Mean   :47.84   Mean   :0.1083   Mean   :1.568  
##                     3rd Qu.:58.00   3rd Qu.:0.0000   3rd Qu.:1.630  
##                     Max.   :89.00   Max.   :1.0000   Max.   :1.960  
##                     NA's   :1       NA's   :1        NA's   :1      
##      weight           waist             neck            hip        
##  Min.   : 30.00   Min.   : 50.80   Min.   :22.00   Min.   : 61.00  
##  1st Qu.: 53.80   1st Qu.: 77.00   1st Qu.:33.00   1st Qu.: 91.00  
##  Median : 62.00   Median : 86.00   Median :35.00   Median : 97.00  
##  Mean   : 63.75   Mean   : 86.32   Mean   :35.38   Mean   : 97.88  
##  3rd Qu.: 71.95   3rd Qu.: 95.00   3rd Qu.:38.00   3rd Qu.:104.00  
##  Max.   :187.80   Max.   :154.50   Max.   :50.00   Max.   :160.00  
##  NA's   :2        NA's   :2        NA's   :5       NA's   :2       
##      pulse            msbpr           mdbpr            hba1c       
##  Min.   : 24.00   Min.   : 68.5   Min.   : 41.50   Min.   : 0.200  
##  1st Qu.: 70.00   1st Qu.:117.0   1st Qu.: 70.00   1st Qu.: 5.100  
##  Median : 78.00   Median :130.0   Median : 77.50   Median : 5.400  
##  Mean   : 79.27   Mean   :133.5   Mean   : 78.46   Mean   : 5.805  
##  3rd Qu.: 86.00   3rd Qu.:146.5   3rd Qu.: 86.00   3rd Qu.: 5.800  
##  Max.   :975.00   Max.   :237.0   Max.   :128.50   Max.   :15.000  
##  NA's   :8                                         NA's   :70      
##       fbs            mogtt2h          totchol            hdl       
##  Min.   : 0.160   Min.   : 0.160   Min.   : 0.180   Min.   :0.080  
##  1st Qu.: 4.400   1st Qu.: 5.150   1st Qu.: 4.970   1st Qu.:1.110  
##  Median : 5.150   Median : 6.600   Median : 5.700   Median :1.320  
##  Mean   : 5.622   Mean   : 7.343   Mean   : 5.792   Mean   :1.346  
##  3rd Qu.: 5.982   3rd Qu.: 8.410   3rd Qu.: 6.530   3rd Qu.:1.540  
##  Max.   :34.270   Max.   :37.370   Max.   :23.140   Max.   :4.430  
##  NA's   :117      NA's   :608      NA's   :54       NA's   :52     
##       ldl       
##  Min.   : 0.14  
##  1st Qu.: 2.79  
##  Median : 3.46  
##  Mean   : 3.55  
##  3rd Qu.: 4.25  
##  Max.   :10.56  
##  NA's   :53
```





```r
met %>% 
  ggplot(aes(x = hba1c)) + 
  geom_histogram() 
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

```
## Warning: Removed 70 rows containing non-finite values (`stat_bin()`).
```

![](met_syndrome_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

```r
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```


```r
met <- met %>% 
  filter(hba1c > 2.5, hba1c < 25.0, 
         ldl > 0.5, hdl > 0.2,  
         totchol > 2.0, totchol < 15.0,
         fbs > 2, fbs < 20,
         pulse < 140) %>% 
  mutate(bmi = weight/(height^2), 
         overweight = if_else(bmi >=25.0,'overwt','not_overwt'),
         overweight = factor(overweight),
         dmdx = factor(dmdx, labels = c('NoDM', 'HaveDM')))
```




```r
summary(met)
```

```
##       id                 age           dmdx          height     
##  Length:4078        Min.   :18.0   NoDM  :3631   Min.   :1.270  
##  Class :character   1st Qu.:38.0   HaveDM: 447   1st Qu.:1.510  
##  Mode  :character   Median :48.0                 Median :1.560  
##                     Mean   :47.9                 Mean   :1.569  
##                     3rd Qu.:58.0                 3rd Qu.:1.630  
##                     Max.   :89.0                 Max.   :1.960  
##                     NA's   :1                                   
##      weight           waist             neck            hip        
##  Min.   : 30.00   Min.   : 50.80   Min.   :22.00   Min.   : 61.00  
##  1st Qu.: 53.80   1st Qu.: 77.00   1st Qu.:33.00   1st Qu.: 91.00  
##  Median : 62.23   Median : 86.00   Median :35.00   Median : 97.00  
##  Mean   : 63.89   Mean   : 86.41   Mean   :35.36   Mean   : 97.89  
##  3rd Qu.: 72.00   3rd Qu.: 95.00   3rd Qu.:38.00   3rd Qu.:104.00  
##  Max.   :187.80   Max.   :154.50   Max.   :50.00   Max.   :160.00  
##  NA's   :2        NA's   :1        NA's   :3       NA's   :1       
##      pulse            msbpr           mdbpr            hba1c       
##  Min.   : 24.00   Min.   : 68.5   Min.   : 41.50   Min.   : 3.800  
##  1st Qu.: 69.00   1st Qu.:117.1   1st Qu.: 70.00   1st Qu.: 5.100  
##  Median : 78.00   Median :130.0   Median : 78.00   Median : 5.400  
##  Mean   : 78.29   Mean   :133.8   Mean   : 78.57   Mean   : 5.803  
##  3rd Qu.: 86.00   3rd Qu.:147.0   3rd Qu.: 86.00   3rd Qu.: 5.800  
##  Max.   :135.00   Max.   :237.0   Max.   :128.50   Max.   :15.000  
##                                                                    
##       fbs            mogtt2h          totchol           hdl       
##  Min.   : 2.030   Min.   : 0.160   Min.   : 2.13   Min.   :0.280  
##  1st Qu.: 4.442   1st Qu.: 5.200   1st Qu.: 4.97   1st Qu.:1.110  
##  Median : 5.160   Median : 6.630   Median : 5.70   Median :1.320  
##  Mean   : 5.628   Mean   : 7.368   Mean   : 5.79   Mean   :1.354  
##  3rd Qu.: 6.000   3rd Qu.: 8.430   3rd Qu.: 6.52   3rd Qu.:1.550  
##  Max.   :19.340   Max.   :37.370   Max.   :14.91   Max.   :4.430  
##                   NA's   :477                                     
##       ldl             bmi              overweight  
##  Min.   : 0.51   Min.   : 9.241   not_overwt:1922  
##  1st Qu.: 2.81   1st Qu.:22.232   overwt    :2154  
##  Median : 3.46   Median :25.402   NA's      :   2  
##  Mean   : 3.56   Mean   :25.938                    
##  3rd Qu.: 4.23   3rd Qu.:28.870                    
##  Max.   :10.56   Max.   :57.040                    
##                  NA's   :2
```

```r
met %>% 
  ggplot(aes(x = hba1c)) + 
  geom_histogram() +
  facet_wrap(~ dmdx)
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](met_syndrome_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.


```r
met_num <- met %>% 
  select_if(is.numeric)
```


```r
cor.met <- cor(met_num, use="complete.obs", method="pearson")
head(round(cor.met,2))
```

```
##          age height weight waist neck  hip pulse msbpr mdbpr hba1c   fbs
## age     1.00  -0.21  -0.08  0.12 0.04 0.00 -0.15  0.45  0.23  0.15  0.17
## height -0.21   1.00   0.41  0.15 0.40 0.11 -0.19 -0.05  0.01 -0.04 -0.05
## weight -0.08   0.41   1.00  0.80 0.67 0.80 -0.03  0.18  0.27  0.17  0.13
## waist   0.12   0.15   0.80  1.00 0.60 0.61  0.00  0.27  0.29  0.23  0.16
## neck    0.04   0.40   0.67  0.60 1.00 0.51 -0.04  0.21  0.25  0.14  0.15
## hip     0.00   0.11   0.80  0.61 0.51 1.00  0.04  0.16  0.23  0.15  0.13
##        mogtt2h totchol   hdl   ldl   bmi
## age       0.20    0.31  0.17  0.26  0.02
## height   -0.15   -0.08 -0.19 -0.07 -0.09
## weight    0.15    0.05 -0.23  0.10  0.87
## waist     0.22    0.12 -0.21  0.13  0.79
## neck      0.13    0.09 -0.29  0.10  0.51
## hip       0.18    0.08 -0.13  0.14  0.82
```

```r
corrplot(cor.met, method="circle")
```

![](met_syndrome_files/figure-html/unnamed-chunk-14-1.png)<!-- -->





```r
met_hba1c <- lm(hba1c ~ fbs, data = met)
summary(met_hba1c)
```

```
## 
## Call:
## lm(formula = hba1c ~ fbs, data = met)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -6.0666 -0.4776 -0.1052  0.3091  8.5293 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 3.502193   0.040972   85.48   <2e-16 ***
## fbs         0.408802   0.006702   60.99   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 1.021 on 4076 degrees of freedom
## Multiple R-squared:  0.4772,	Adjusted R-squared:  0.4771 
## F-statistic:  3720 on 1 and 4076 DF,  p-value: < 2.2e-16
```



```r
met_hba1c_mv <- lm(hba1c ~ fbs + age + msbpr + mdbpr, data = met)
summary(met_hba1c_mv)
```

```
## 
## Call:
## lm(formula = hba1c ~ fbs + age + msbpr + mdbpr, data = met)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -6.0630 -0.4718 -0.1051  0.3098  8.6575 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  3.009202   0.111352  27.024  < 2e-16 ***
## fbs          0.400576   0.006822  58.721  < 2e-16 ***
## age          0.008054   0.001259   6.396 1.77e-10 ***
## msbpr       -0.004954   0.001080  -4.588 4.61e-06 ***
## mdbpr        0.010390   0.001928   5.390 7.43e-08 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 1.014 on 4072 degrees of freedom
##   (1 observation deleted due to missingness)
## Multiple R-squared:  0.485,	Adjusted R-squared:  0.4845 
## F-statistic: 958.8 on 4 and 4072 DF,  p-value: < 2.2e-16
```



```r
met_hba1c_mv2 <- lm(hba1c ~ fbs + age + msbpr + mdbpr + bmi + hdl + ldl + dmdx, 
                    data = met)
summary(met_hba1c_mv2)
```

```
## 
## Call:
## lm(formula = hba1c ~ fbs + age + msbpr + mdbpr + bmi + hdl + 
##     ldl + dmdx, data = met)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -5.0202 -0.4007 -0.0588  0.2985  8.9164 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  3.170719   0.130759  24.249  < 2e-16 ***
## fbs          0.318074   0.007102  44.785  < 2e-16 ***
## age          0.003454   0.001210   2.854  0.00434 ** 
## msbpr       -0.004441   0.001003  -4.425 9.88e-06 ***
## mdbpr        0.007240   0.001827   3.964 7.51e-05 ***
## bmi          0.013409   0.003019   4.441 9.18e-06 ***
## hdl         -0.039540   0.044232  -0.894  0.37141    
## ldl          0.073575   0.014573   5.049 4.64e-07 ***
## dmdxHaveDM   1.330114   0.053865  24.693  < 2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.9412 on 4066 degrees of freedom
##   (3 observations deleted due to missingness)
## Multiple R-squared:  0.5569,	Adjusted R-squared:  0.556 
## F-statistic: 638.8 on 8 and 4066 DF,  p-value: < 2.2e-16
```



```r
met_hba1c_ia <- lm(hba1c ~ fbs + age + msbpr + mdbpr + bmi + hdl + ldl + dmdx +
                     dmdx*age, data = met)
summary(met_hba1c_ia)
```

```
## 
## Call:
## lm(formula = hba1c ~ fbs + age + msbpr + mdbpr + bmi + hdl + 
##     ldl + dmdx + dmdx * age, data = met)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -5.0362 -0.4004 -0.0557  0.3003  8.9217 
## 
## Coefficients:
##                 Estimate Std. Error t value Pr(>|t|)    
## (Intercept)     3.168289   0.130759  24.230  < 2e-16 ***
## fbs             0.317246   0.007129  44.501  < 2e-16 ***
## age             0.003838   0.001244   3.085 0.002049 ** 
## msbpr          -0.004355   0.001005  -4.332 1.52e-05 ***
## mdbpr           0.007038   0.001833   3.840 0.000125 ***
## bmi             0.013302   0.003020   4.405 1.09e-05 ***
## hdl            -0.041075   0.044243  -0.928 0.353253    
## ldl             0.073038   0.014577   5.010 5.66e-07 ***
## dmdxHaveDM      1.639626   0.239192   6.855 8.21e-12 ***
## age:dmdxHaveDM -0.005475   0.004123  -1.328 0.184220    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.9411 on 4065 degrees of freedom
##   (3 observations deleted due to missingness)
## Multiple R-squared:  0.5571,	Adjusted R-squared:  0.5561 
## F-statistic: 568.1 on 9 and 4065 DF,  p-value: < 2.2e-16
```





```r
tidy(met_hba1c_mv2, conf.int = TRUE)
```

```
## # A tibble: 9 × 7
##   term        estimate std.error statistic   p.value conf.low conf.high
##   <chr>          <dbl>     <dbl>     <dbl>     <dbl>    <dbl>     <dbl>
## 1 (Intercept)  3.17      0.131      24.2   1.96e-121  2.91      3.43   
## 2 fbs          0.318     0.00710    44.8   0          0.304     0.332  
## 3 age          0.00345   0.00121     2.85  4.34e-  3  0.00108   0.00583
## 4 msbpr       -0.00444   0.00100    -4.43  9.88e-  6 -0.00641  -0.00247
## 5 mdbpr        0.00724   0.00183     3.96  7.51e-  5  0.00366   0.0108 
## 6 bmi          0.0134    0.00302     4.44  9.18e-  6  0.00749   0.0193 
## 7 hdl         -0.0395    0.0442     -0.894 3.71e-  1 -0.126     0.0472 
## 8 ldl          0.0736    0.0146      5.05  4.64e-  7  0.0450    0.102  
## 9 dmdxHaveDM   1.33      0.0539     24.7   1.47e-125  1.22      1.44
```




```r
pred_met <- augment(met_hba1c_mv2)
head(pred_met)
```

```
## # A tibble: 6 × 16
##   .rownames hba1c   fbs   age msbpr mdbpr   bmi   hdl   ldl dmdx   .fitted
##   <chr>     <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <fct>    <dbl>
## 1 1           5.3  5.82    46  133   83.5  27.3  0.84  3.45 NoDM      5.78
## 2 2           5.6  6.29    47  163   84    18.4  1.45  3.7  NoDM      5.68
## 3 3           5.7  8.29    48  146.  93.5  41.0  0.82  3.96 NoDM      6.81
## 4 4           7.2  8.39    63  206.  94    36    1.79  4.68 HaveDM    7.91
## 5 5           5.4  5.23    39  129   70    27.8  1.04  4.33 NoDM      5.55
## 6 6           5.7  6.45    74  190.  92    24.5  1.62  3.03 NoDM      5.78
## # ℹ 5 more variables: .resid <dbl>, .hat <dbl>, .sigma <dbl>, .cooksd <dbl>,
## #   .std.resid <dbl>
```




```r
pred_met %>% 
  ggplot(aes(x = .resid)) +
  geom_histogram()
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](met_syndrome_files/figure-html/unnamed-chunk-22-1.png)<!-- -->




```r
pred_met %>% 
  ggplot(aes(x = .fitted, y = .std.resid)) +
  geom_point()
```

![](met_syndrome_files/figure-html/unnamed-chunk-23-1.png)<!-- -->




```r
pred_met %>% 
  filter(between(.std.resid, -3, 3)) %>% 
  ggplot(aes(x = .fitted, y = .std.resid)) +
  geom_point() 
```

![](met_syndrome_files/figure-html/unnamed-chunk-24-1.png)<!-- -->





#logistic regression

#Read data

```r
PUP2 <- read_excel(here('PUP2.xlsx'))
```



```r
glimpse(PUP2)
```

```
## Rows: 121
## Columns: 23
## $ no                  <dbl> 105, 107, 94, 103, 109, 111, 79, 85, 88, 89, 100, …
## $ age                 <dbl> 42, 66, 67, 19, 77, 39, 62, 71, 69, 97, 52, 21, 57…
## $ gender              <chr> "male", "female", "male", "male", "male", "male", …
## $ epigastric_pain     <chr> "yes", "yes", "yes", "yes", "yes", "yes", "yes", "…
## $ onset_more_24_hrs   <chr> "no", "no", "no", "yes", "yes", "yes", "yes", "no"…
## $ diabetes            <chr> "no", "no", "no", "no", "no", "no", "yes", "no", "…
## $ NSAIDS              <chr> "no", "no", "yes", "no", "no", "no", "no", "no", "…
## $ previous_OGDS       <chr> "no", "no", "no", "yes", "no", "no", "no", "no", "…
## $ ASA                 <dbl> 1, 1, 1, 1, 2, 1, 2, 2, 1, 1, 2, 1, 2, 1, 1, 2, 2,…
## $ systolic            <dbl> 141, 197, 126, 90, 147, 115, 103, 159, 145, 105, 1…
## $ diastolic           <dbl> 98, 88, 73, 40, 82, 86, 55, 68, 75, 65, 74, 50, 86…
## $ tenderness          <chr> "generalized", "generalized", "generalized", "loca…
## $ guarding            <chr> "yes", "yes", "yes", "yes", "no", "yes", "yes", "n…
## $ hemoglobin          <dbl> 18.0, 12.0, 12.0, 12.0, 11.0, 18.0, 8.1, 13.3, 11.…
## $ twc                 <dbl> 6.0, 6.0, 13.0, 20.0, 21.0, 4.0, 5.0, 12.0, 6.0, 2…
## $ platelet            <dbl> 415, 292, 201, 432, 324, 260, 461, 210, 293, 592, …
## $ PULP                <dbl> 2, 3, 3, 2, 7, 1, 2, 5, 3, 4, 2, 3, 4, 3, 5, 5, 1,…
## $ admission_to_op_hrs <dbl> 2, 2, 3, 3, 3, 3, 4, 4, 4, 4, 4, 5, 5, 6, 6, 6, 6,…
## $ perforation         <dbl> 0.5, 1.0, 0.5, 0.5, 1.0, 1.0, 3.0, 1.5, 0.5, 1.5, …
## $ degree_perforation  <chr> "small", "small", "small", "small", "small", "smal…
## $ SSSI                <chr> "no", "no", "no", "no", "no", "no", "no", "no", "n…
## $ sepsis              <chr> "no", "no", "no", "no", "no", "no", "yes", "no", "…
## $ outcome             <chr> "alive", "alive", "alive", "alive", "alive", "aliv…
```



```r
summary(PUP2)
```

```
##        no           age           gender          epigastric_pain   
##  Min.   :  1   Min.   :19.00   Length:121         Length:121        
##  1st Qu.: 31   1st Qu.:49.00   Class :character   Class :character  
##  Median : 61   Median :64.00   Mode  :character   Mode  :character  
##  Mean   : 61   Mean   :60.43                                        
##  3rd Qu.: 91   3rd Qu.:75.00                                        
##  Max.   :121   Max.   :97.00                                        
##  onset_more_24_hrs    diabetes            NSAIDS          previous_OGDS     
##  Length:121         Length:121         Length:121         Length:121        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##       ASA           systolic       diastolic       tenderness       
##  Min.   :1.000   Min.   : 67.0   Min.   : 38.00   Length:121        
##  1st Qu.:1.000   1st Qu.:112.0   1st Qu.: 63.00   Class :character  
##  Median :1.000   Median :128.0   Median : 71.00   Mode  :character  
##  Mean   :1.545   Mean   :128.6   Mean   : 72.07                     
##  3rd Qu.:2.000   3rd Qu.:143.0   3rd Qu.: 81.00                     
##  Max.   :3.000   Max.   :197.0   Max.   :116.00                     
##    guarding           hemoglobin         twc           platelet    
##  Length:121         Min.   : 3.30   Min.   : 2.00   Min.   : 11.0  
##  Class :character   1st Qu.:10.00   1st Qu.: 9.00   1st Qu.:224.0  
##  Mode  :character   Median :12.00   Median :12.00   Median :308.0  
##                     Mean   :12.32   Mean   :13.03   Mean   :314.4  
##                     3rd Qu.:15.00   3rd Qu.:16.00   3rd Qu.:392.0  
##                     Max.   :19.40   Max.   :37.00   Max.   :798.0  
##       PULP       admission_to_op_hrs  perforation    degree_perforation
##  Min.   :0.000   Min.   : 1.00       Min.   :0.300   Length:121        
##  1st Qu.:2.000   1st Qu.: 5.00       1st Qu.:0.500   Class :character  
##  Median :3.000   Median : 8.00       Median :1.000   Mode  :character  
##  Mean   :3.529   Mean   :10.07       Mean   :1.225                     
##  3rd Qu.:5.000   3rd Qu.:12.00       3rd Qu.:1.500                     
##  Max.   :9.000   Max.   :72.00       Max.   :5.000                     
##      SSSI              sepsis            outcome         
##  Length:121         Length:121         Length:121        
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
## 
```


#data wrangly


```r
library(summarytools)
```

```
## 
## Attaching package: 'summarytools'
```

```
## The following object is masked from 'package:wrapr':
## 
##     view
```

```
## The following object is masked from 'package:tibble':
## 
##     view
```



```r
descr(PUP2)
```

```
## Non-numerical variable(s) ignored: gender, epigastric_pain, onset_more_24_hrs, diabetes, NSAIDS, previous_OGDS, tenderness, guarding, degree_perforation, SSSI, sepsis, outcome
```

```
## Descriptive Statistics  
## PUP2  
## N: 121  
## 
##                     admission_to_op_hrs      age      ASA   diastolic   hemoglobin       no
## ----------------- --------------------- -------- -------- ----------- ------------ --------
##              Mean                 10.07    60.43     1.55       72.07        12.32    61.00
##           Std.Dev                  9.44    18.05     0.62       13.99         3.33    35.07
##               Min                  1.00    19.00     1.00       38.00         3.30     1.00
##                Q1                  5.00    49.00     1.00       63.00        10.00    31.00
##            Median                  8.00    64.00     1.00       71.00        12.00    61.00
##                Q3                 12.00    75.00     2.00       81.00        15.00    91.00
##               Max                 72.00    97.00     3.00      116.00        19.40   121.00
##               MAD                  5.93    17.79     0.00       13.34         2.97    44.48
##               IQR                  7.00    26.00     1.00       18.00         5.00    60.00
##                CV                  0.94     0.30     0.40        0.19         0.27     0.57
##          Skewness                  3.36    -0.55     0.66        0.12        -0.22     0.00
##       SE.Skewness                  0.22     0.22     0.22        0.22         0.22     0.22
##          Kurtosis                 15.95    -0.53    -0.55        0.22         0.02    -1.23
##           N.Valid                121.00   121.00   121.00      121.00       121.00   121.00
##         Pct.Valid                100.00   100.00   100.00      100.00       100.00   100.00
## 
## Table: Table continues below
## 
##  
## 
##                     perforation   platelet     PULP   systolic      twc
## ----------------- ------------- ---------- -------- ---------- --------
##              Mean          1.22     314.43     3.53     128.56    13.03
##           Std.Dev          0.91     140.32     2.28      24.51     6.66
##               Min          0.30      11.00     0.00      67.00     2.00
##                Q1          0.50     224.00     2.00     112.00     9.00
##            Median          1.00     308.00     3.00     128.00    12.00
##                Q3          1.50     392.00     5.00     143.00    16.00
##               Max          5.00     798.00     9.00     197.00    37.00
##               MAD          0.74     124.54     2.97      22.24     5.93
##               IQR          1.00     168.00     3.00      31.00     7.00
##                CV          0.74       0.45     0.65       0.19     0.51
##          Skewness          1.66       0.75     0.25       0.36     0.78
##       SE.Skewness          0.22       0.22     0.22       0.22     0.22
##          Kurtosis          3.34       1.02    -0.91       0.58     0.60
##           N.Valid        121.00     121.00   121.00     121.00   121.00
##         Pct.Valid        100.00     100.00   100.00     100.00   100.00
```

#outcome and predictor variable


```r
PUP2 %>% group_by(outcome) %>% 
  count()
```

```
## # A tibble: 2 × 2
## # Groups:   outcome [2]
##   outcome     n
##   <chr>   <int>
## 1 alive      83
## 2 dead       38
```



```r
PUP2 %>% split(.$outcome) %>% 
  map(~descr(.x, stats = c('mean', 'sd', 'min', 'med', 'max')),
      transpose = TRUE)
```

```
## x must either be a summarytools object created with freq(), descr(), or a list of summarytools objects created using by()
```



```r
PUP2 <- PUP2 %>% 
  mutate(oc2 = factor(outcome))
```



```r
PUP2 %>% 
  ggplot(aes(x = oc2)) + 
  geom_bar() + 
  xlab('outcome') + ylab('freq')
```

![](met_syndrome_files/figure-html/unnamed-chunk-34-1.png)<!-- -->



```r
PUP2 %>% 
  ggplot(aes(ASA)) + 
  geom_bar() +
  facet_wrap(~oc2)
```

![](met_syndrome_files/figure-html/unnamed-chunk-35-1.png)<!-- -->


estimation

univariate logistic regression



```r
options(scipen = 999)
pup_uni <- PUP2 %>% 
  select(oc2, age, gender, epigastric_pain, onset_more_24_hrs, diabetes, 
         NSAIDS, previous_OGDS, systolic, diastolic, ASA, PULP, perforation,
         admission_to_op_hrs, hemoglobin, twc) %>% 
  map_dfr(~tidy(glm(oc2 ~ .x, family = binomial, data = PUP2), 
                conf.int = T), .id = 'variable') %>%
  filter(variable != 'oc2')
```

```
## Warning: glm.fit: algorithm did not converge
```

```
## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred
```

```
## Warning in regularize.values(x, y, ties, missing(ties), na.rm = na.rm):
## collapsing to unique 'x' values
```



```r
kable(pup_uni) %>% 
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> variable </th>
   <th style="text-align:left;"> term </th>
   <th style="text-align:right;"> estimate </th>
   <th style="text-align:right;"> std.error </th>
   <th style="text-align:right;"> statistic </th>
   <th style="text-align:right;"> p.value </th>
   <th style="text-align:right;"> conf.low </th>
   <th style="text-align:right;"> conf.high </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> age </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> -2.2466889 </td>
   <td style="text-align:right;"> 0.7843206 </td>
   <td style="text-align:right;"> -2.8645030 </td>
   <td style="text-align:right;"> 0.0041766 </td>
   <td style="text-align:right;"> -3.8954373 </td>
   <td style="text-align:right;"> -0.7936808 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 0.0236740 </td>
   <td style="text-align:right;"> 0.0119691 </td>
   <td style="text-align:right;"> 1.9779287 </td>
   <td style="text-align:right;"> 0.0479368 </td>
   <td style="text-align:right;"> 0.0010929 </td>
   <td style="text-align:right;"> 0.0483811 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> gender </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> -0.0800427 </td>
   <td style="text-align:right;"> 0.4003204 </td>
   <td style="text-align:right;"> -0.1999466 </td>
   <td style="text-align:right;"> 0.8415223 </td>
   <td style="text-align:right;"> -0.8789758 </td>
   <td style="text-align:right;"> 0.7105500 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> gender </td>
   <td style="text-align:left;"> .xmale </td>
   <td style="text-align:right;"> -0.9103560 </td>
   <td style="text-align:right;"> 0.4615231 </td>
   <td style="text-align:right;"> -1.9725036 </td>
   <td style="text-align:right;"> 0.0485521 </td>
   <td style="text-align:right;"> -1.8225388 </td>
   <td style="text-align:right;"> 0.0007672 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> epigastric_pain </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> -1.3862944 </td>
   <td style="text-align:right;"> 1.1180337 </td>
   <td style="text-align:right;"> -1.2399397 </td>
   <td style="text-align:right;"> 0.2149977 </td>
   <td style="text-align:right;"> -4.3598898 </td>
   <td style="text-align:right;"> 0.5246956 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> epigastric_pain </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 0.6277644 </td>
   <td style="text-align:right;"> 1.1356428 </td>
   <td style="text-align:right;"> 0.5527833 </td>
   <td style="text-align:right;"> 0.5804118 </td>
   <td style="text-align:right;"> -1.3256526 </td>
   <td style="text-align:right;"> 3.6198522 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> onset_more_24_hrs </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> -1.0986123 </td>
   <td style="text-align:right;"> 0.3333330 </td>
   <td style="text-align:right;"> -3.2958397 </td>
   <td style="text-align:right;"> 0.0009813 </td>
   <td style="text-align:right;"> -1.7942261 </td>
   <td style="text-align:right;"> -0.4749176 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> onset_more_24_hrs </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 0.5065612 </td>
   <td style="text-align:right;"> 0.4133389 </td>
   <td style="text-align:right;"> 1.2255348 </td>
   <td style="text-align:right;"> 0.2203738 </td>
   <td style="text-align:right;"> -0.2889264 </td>
   <td style="text-align:right;"> 1.3414593 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> diabetes </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> -0.9019020 </td>
   <td style="text-align:right;"> 0.2240691 </td>
   <td style="text-align:right;"> -4.0251058 </td>
   <td style="text-align:right;"> 0.0000569 </td>
   <td style="text-align:right;"> -1.3566078 </td>
   <td style="text-align:right;"> -0.4745240 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> diabetes </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 0.5654298 </td>
   <td style="text-align:right;"> 0.4707818 </td>
   <td style="text-align:right;"> 1.2010441 </td>
   <td style="text-align:right;"> 0.2297341 </td>
   <td style="text-align:right;"> -0.3772175 </td>
   <td style="text-align:right;"> 1.4849256 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NSAIDS </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> -0.9490806 </td>
   <td style="text-align:right;"> 0.2404072 </td>
   <td style="text-align:right;"> -3.9478044 </td>
   <td style="text-align:right;"> 0.0000789 </td>
   <td style="text-align:right;"> -1.4390465 </td>
   <td style="text-align:right;"> -0.4920164 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NSAIDS </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 0.5436154 </td>
   <td style="text-align:right;"> 0.4205273 </td>
   <td style="text-align:right;"> 1.2926994 </td>
   <td style="text-align:right;"> 0.1961150 </td>
   <td style="text-align:right;"> -0.2904184 </td>
   <td style="text-align:right;"> 1.3672726 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> previous_OGDS </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> -0.7753853 </td>
   <td style="text-align:right;"> 0.2042775 </td>
   <td style="text-align:right;"> -3.7957449 </td>
   <td style="text-align:right;"> 0.0001472 </td>
   <td style="text-align:right;"> -1.1870841 </td>
   <td style="text-align:right;"> -0.3835253 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> previous_OGDS </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> -0.0719126 </td>
   <td style="text-align:right;"> 0.7196661 </td>
   <td style="text-align:right;"> -0.0999249 </td>
   <td style="text-align:right;"> 0.9204039 </td>
   <td style="text-align:right;"> -1.6508310 </td>
   <td style="text-align:right;"> 1.2715818 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> systolic </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 0.9192236 </td>
   <td style="text-align:right;"> 1.0873957 </td>
   <td style="text-align:right;"> 0.8453441 </td>
   <td style="text-align:right;"> 0.3979187 </td>
   <td style="text-align:right;"> -1.1798631 </td>
   <td style="text-align:right;"> 3.1196146 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> systolic </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> -0.0133813 </td>
   <td style="text-align:right;"> 0.0085103 </td>
   <td style="text-align:right;"> -1.5723584 </td>
   <td style="text-align:right;"> 0.1158675 </td>
   <td style="text-align:right;"> -0.0308281 </td>
   <td style="text-align:right;"> 0.0028134 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> diastolic </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 0.6301161 </td>
   <td style="text-align:right;"> 1.0434752 </td>
   <td style="text-align:right;"> 0.6038630 </td>
   <td style="text-align:right;"> 0.5459347 </td>
   <td style="text-align:right;"> -1.4068184 </td>
   <td style="text-align:right;"> 2.7166343 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> diastolic </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> -0.0197762 </td>
   <td style="text-align:right;"> 0.0144968 </td>
   <td style="text-align:right;"> -1.3641724 </td>
   <td style="text-align:right;"> 0.1725133 </td>
   <td style="text-align:right;"> -0.0491267 </td>
   <td style="text-align:right;"> 0.0081354 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ASA </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> -1.6065753 </td>
   <td style="text-align:right;"> 0.5455487 </td>
   <td style="text-align:right;"> -2.9448796 </td>
   <td style="text-align:right;"> 0.0032308 </td>
   <td style="text-align:right;"> -2.7150116 </td>
   <td style="text-align:right;"> -0.5629706 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ASA </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 0.5224183 </td>
   <td style="text-align:right;"> 0.3154616 </td>
   <td style="text-align:right;"> 1.6560438 </td>
   <td style="text-align:right;"> 0.0977129 </td>
   <td style="text-align:right;"> -0.0941774 </td>
   <td style="text-align:right;"> 1.1523982 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PULP </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> -1.6645299 </td>
   <td style="text-align:right;"> 0.4098748 </td>
   <td style="text-align:right;"> -4.0610691 </td>
   <td style="text-align:right;"> 0.0000488 </td>
   <td style="text-align:right;"> -2.5155950 </td>
   <td style="text-align:right;"> -0.8987627 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PULP </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 0.2361062 </td>
   <td style="text-align:right;"> 0.0906398 </td>
   <td style="text-align:right;"> 2.6048836 </td>
   <td style="text-align:right;"> 0.0091905 </td>
   <td style="text-align:right;"> 0.0624931 </td>
   <td style="text-align:right;"> 0.4201174 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> perforation </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> -2.3438540 </td>
   <td style="text-align:right;"> 0.4324202 </td>
   <td style="text-align:right;"> -5.4203151 </td>
   <td style="text-align:right;"> 0.0000001 </td>
   <td style="text-align:right;"> -3.2517343 </td>
   <td style="text-align:right;"> -1.5466796 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> perforation </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 1.2032426 </td>
   <td style="text-align:right;"> 0.2843708 </td>
   <td style="text-align:right;"> 4.2312458 </td>
   <td style="text-align:right;"> 0.0000232 </td>
   <td style="text-align:right;"> 0.6827088 </td>
   <td style="text-align:right;"> 1.8029077 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> admission_to_op_hrs </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> -0.8431923 </td>
   <td style="text-align:right;"> 0.2853226 </td>
   <td style="text-align:right;"> -2.9552246 </td>
   <td style="text-align:right;"> 0.0031244 </td>
   <td style="text-align:right;"> -1.4139903 </td>
   <td style="text-align:right;"> -0.2840466 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> admission_to_op_hrs </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 0.0060960 </td>
   <td style="text-align:right;"> 0.0202384 </td>
   <td style="text-align:right;"> 0.3012113 </td>
   <td style="text-align:right;"> 0.7632534 </td>
   <td style="text-align:right;"> -0.0373817 </td>
   <td style="text-align:right;"> 0.0459273 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> hemoglobin </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 0.7138662 </td>
   <td style="text-align:right;"> 0.7573475 </td>
   <td style="text-align:right;"> 0.9425874 </td>
   <td style="text-align:right;"> 0.3458920 </td>
   <td style="text-align:right;"> -0.7617317 </td>
   <td style="text-align:right;"> 2.2369643 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> hemoglobin </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> -0.1237478 </td>
   <td style="text-align:right;"> 0.0615749 </td>
   <td style="text-align:right;"> -2.0097125 </td>
   <td style="text-align:right;"> 0.0444616 </td>
   <td style="text-align:right;"> -0.2493337 </td>
   <td style="text-align:right;"> -0.0057555 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> twc </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> -1.0461722 </td>
   <td style="text-align:right;"> 0.4361081 </td>
   <td style="text-align:right;"> -2.3988830 </td>
   <td style="text-align:right;"> 0.0164452 </td>
   <td style="text-align:right;"> -1.9266672 </td>
   <td style="text-align:right;"> -0.2058904 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> twc </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 0.0200912 </td>
   <td style="text-align:right;"> 0.0291979 </td>
   <td style="text-align:right;"> 0.6881022 </td>
   <td style="text-align:right;"> 0.4913885 </td>
   <td style="text-align:right;"> -0.0379630 </td>
   <td style="text-align:right;"> 0.0776960 </td>
  </tr>
</tbody>
</table>

odd ratio


```r
crude_or_co <- PUP2 %>% 
  select(oc2, age, gender, epigastric_pain, onset_more_24_hrs, diabetes, 
         NSAIDS, previous_OGDS, systolic, diastolic, ASA, PULP, perforation,
         admission_to_op_hrs, hemoglobin, twc) %>%
  map(~glm(oc2 ~ .x, family = binomial, data = PUP2)) %>%
  map_dfr(~tidy(., exponentiate = TRUE, conf.int = TRUE), .id = 'variable') %>%
  filter(variable != 'oc2')
```

```
## Warning: glm.fit: algorithm did not converge
```

```
## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred
```

```
## Warning in regularize.values(x, y, ties, missing(ties), na.rm = na.rm):
## collapsing to unique 'x' values
```




```r
kable(crude_or_co)  %>% 
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> variable </th>
   <th style="text-align:left;"> term </th>
   <th style="text-align:right;"> estimate </th>
   <th style="text-align:right;"> std.error </th>
   <th style="text-align:right;"> statistic </th>
   <th style="text-align:right;"> p.value </th>
   <th style="text-align:right;"> conf.low </th>
   <th style="text-align:right;"> conf.high </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> age </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 0.1057488 </td>
   <td style="text-align:right;"> 0.7843206 </td>
   <td style="text-align:right;"> -2.8645030 </td>
   <td style="text-align:right;"> 0.0041766 </td>
   <td style="text-align:right;"> 0.0203345 </td>
   <td style="text-align:right;"> 0.4521773 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> age </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 1.0239565 </td>
   <td style="text-align:right;"> 0.0119691 </td>
   <td style="text-align:right;"> 1.9779287 </td>
   <td style="text-align:right;"> 0.0479368 </td>
   <td style="text-align:right;"> 1.0010935 </td>
   <td style="text-align:right;"> 1.0495706 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> gender </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 0.9230769 </td>
   <td style="text-align:right;"> 0.4003204 </td>
   <td style="text-align:right;"> -0.1999466 </td>
   <td style="text-align:right;"> 0.8415223 </td>
   <td style="text-align:right;"> 0.4152080 </td>
   <td style="text-align:right;"> 2.0351103 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> gender </td>
   <td style="text-align:left;"> .xmale </td>
   <td style="text-align:right;"> 0.4023810 </td>
   <td style="text-align:right;"> 0.4615231 </td>
   <td style="text-align:right;"> -1.9725036 </td>
   <td style="text-align:right;"> 0.0485521 </td>
   <td style="text-align:right;"> 0.1616149 </td>
   <td style="text-align:right;"> 1.0007675 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> epigastric_pain </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 0.2500000 </td>
   <td style="text-align:right;"> 1.1180337 </td>
   <td style="text-align:right;"> -1.2399397 </td>
   <td style="text-align:right;"> 0.2149977 </td>
   <td style="text-align:right;"> 0.0127798 </td>
   <td style="text-align:right;"> 1.6899444 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> epigastric_pain </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 1.8734177 </td>
   <td style="text-align:right;"> 1.1356428 </td>
   <td style="text-align:right;"> 0.5527833 </td>
   <td style="text-align:right;"> 0.5804118 </td>
   <td style="text-align:right;"> 0.2656295 </td>
   <td style="text-align:right;"> 37.3320493 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> onset_more_24_hrs </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 0.3333333 </td>
   <td style="text-align:right;"> 0.3333330 </td>
   <td style="text-align:right;"> -3.2958397 </td>
   <td style="text-align:right;"> 0.0009813 </td>
   <td style="text-align:right;"> 0.1662561 </td>
   <td style="text-align:right;"> 0.6219363 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> onset_more_24_hrs </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 1.6595745 </td>
   <td style="text-align:right;"> 0.4133389 </td>
   <td style="text-align:right;"> 1.2255348 </td>
   <td style="text-align:right;"> 0.2203738 </td>
   <td style="text-align:right;"> 0.7490674 </td>
   <td style="text-align:right;"> 3.8246207 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> diabetes </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 0.4057971 </td>
   <td style="text-align:right;"> 0.2240691 </td>
   <td style="text-align:right;"> -4.0251058 </td>
   <td style="text-align:right;"> 0.0000569 </td>
   <td style="text-align:right;"> 0.2575329 </td>
   <td style="text-align:right;"> 0.6221811 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> diabetes </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 1.7602041 </td>
   <td style="text-align:right;"> 0.4707818 </td>
   <td style="text-align:right;"> 1.2010441 </td>
   <td style="text-align:right;"> 0.2297341 </td>
   <td style="text-align:right;"> 0.6857669 </td>
   <td style="text-align:right;"> 4.4146368 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NSAIDS </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 0.3870968 </td>
   <td style="text-align:right;"> 0.2404072 </td>
   <td style="text-align:right;"> -3.9478044 </td>
   <td style="text-align:right;"> 0.0000789 </td>
   <td style="text-align:right;"> 0.2371538 </td>
   <td style="text-align:right;"> 0.6113924 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NSAIDS </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 1.7222222 </td>
   <td style="text-align:right;"> 0.4205273 </td>
   <td style="text-align:right;"> 1.2926994 </td>
   <td style="text-align:right;"> 0.1961150 </td>
   <td style="text-align:right;"> 0.7479506 </td>
   <td style="text-align:right;"> 3.9246320 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> previous_OGDS </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 0.4605263 </td>
   <td style="text-align:right;"> 0.2042775 </td>
   <td style="text-align:right;"> -3.7957449 </td>
   <td style="text-align:right;"> 0.0001472 </td>
   <td style="text-align:right;"> 0.3051097 </td>
   <td style="text-align:right;"> 0.6814549 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> previous_OGDS </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 0.9306122 </td>
   <td style="text-align:right;"> 0.7196661 </td>
   <td style="text-align:right;"> -0.0999249 </td>
   <td style="text-align:right;"> 0.9204039 </td>
   <td style="text-align:right;"> 0.1918904 </td>
   <td style="text-align:right;"> 3.5664897 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> systolic </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 2.5073428 </td>
   <td style="text-align:right;"> 1.0873957 </td>
   <td style="text-align:right;"> 0.8453441 </td>
   <td style="text-align:right;"> 0.3979187 </td>
   <td style="text-align:right;"> 0.3073208 </td>
   <td style="text-align:right;"> 22.6376538 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> systolic </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 0.9867078 </td>
   <td style="text-align:right;"> 0.0085103 </td>
   <td style="text-align:right;"> -1.5723584 </td>
   <td style="text-align:right;"> 0.1158675 </td>
   <td style="text-align:right;"> 0.9696422 </td>
   <td style="text-align:right;"> 1.0028173 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> diastolic </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 1.8778285 </td>
   <td style="text-align:right;"> 1.0434752 </td>
   <td style="text-align:right;"> 0.6038630 </td>
   <td style="text-align:right;"> 0.5459347 </td>
   <td style="text-align:right;"> 0.2449213 </td>
   <td style="text-align:right;"> 15.1293161 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> diastolic </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 0.9804181 </td>
   <td style="text-align:right;"> 0.0144968 </td>
   <td style="text-align:right;"> -1.3641724 </td>
   <td style="text-align:right;"> 0.1725133 </td>
   <td style="text-align:right;"> 0.9520605 </td>
   <td style="text-align:right;"> 1.0081686 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ASA </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 0.2005733 </td>
   <td style="text-align:right;"> 0.5455487 </td>
   <td style="text-align:right;"> -2.9448796 </td>
   <td style="text-align:right;"> 0.0032308 </td>
   <td style="text-align:right;"> 0.0662042 </td>
   <td style="text-align:right;"> 0.5695148 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ASA </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 1.6861001 </td>
   <td style="text-align:right;"> 0.3154616 </td>
   <td style="text-align:right;"> 1.6560438 </td>
   <td style="text-align:right;"> 0.0977129 </td>
   <td style="text-align:right;"> 0.9101213 </td>
   <td style="text-align:right;"> 3.1657760 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PULP </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 0.1892796 </td>
   <td style="text-align:right;"> 0.4098748 </td>
   <td style="text-align:right;"> -4.0610691 </td>
   <td style="text-align:right;"> 0.0000488 </td>
   <td style="text-align:right;"> 0.0808148 </td>
   <td style="text-align:right;"> 0.4070730 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PULP </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 1.2663088 </td>
   <td style="text-align:right;"> 0.0906398 </td>
   <td style="text-align:right;"> 2.6048836 </td>
   <td style="text-align:right;"> 0.0091905 </td>
   <td style="text-align:right;"> 1.0644871 </td>
   <td style="text-align:right;"> 1.5221402 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> perforation </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 0.0959571 </td>
   <td style="text-align:right;"> 0.4324202 </td>
   <td style="text-align:right;"> -5.4203151 </td>
   <td style="text-align:right;"> 0.0000001 </td>
   <td style="text-align:right;"> 0.0387070 </td>
   <td style="text-align:right;"> 0.2129539 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> perforation </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 3.3309004 </td>
   <td style="text-align:right;"> 0.2843708 </td>
   <td style="text-align:right;"> 4.2312458 </td>
   <td style="text-align:right;"> 0.0000232 </td>
   <td style="text-align:right;"> 1.9792317 </td>
   <td style="text-align:right;"> 6.0672637 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> admission_to_op_hrs </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 0.4303346 </td>
   <td style="text-align:right;"> 0.2853226 </td>
   <td style="text-align:right;"> -2.9552246 </td>
   <td style="text-align:right;"> 0.0031244 </td>
   <td style="text-align:right;"> 0.2431710 </td>
   <td style="text-align:right;"> 0.7527315 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> admission_to_op_hrs </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 1.0061147 </td>
   <td style="text-align:right;"> 0.0202384 </td>
   <td style="text-align:right;"> 0.3012113 </td>
   <td style="text-align:right;"> 0.7632534 </td>
   <td style="text-align:right;"> 0.9633084 </td>
   <td style="text-align:right;"> 1.0469983 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> hemoglobin </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 2.0418703 </td>
   <td style="text-align:right;"> 0.7573475 </td>
   <td style="text-align:right;"> 0.9425874 </td>
   <td style="text-align:right;"> 0.3458920 </td>
   <td style="text-align:right;"> 0.4668573 </td>
   <td style="text-align:right;"> 9.3648596 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> hemoglobin </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 0.8836027 </td>
   <td style="text-align:right;"> 0.0615749 </td>
   <td style="text-align:right;"> -2.0097125 </td>
   <td style="text-align:right;"> 0.0444616 </td>
   <td style="text-align:right;"> 0.7793198 </td>
   <td style="text-align:right;"> 0.9942611 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> twc </td>
   <td style="text-align:left;"> (Intercept) </td>
   <td style="text-align:right;"> 0.3512798 </td>
   <td style="text-align:right;"> 0.4361081 </td>
   <td style="text-align:right;"> -2.3988830 </td>
   <td style="text-align:right;"> 0.0164452 </td>
   <td style="text-align:right;"> 0.1456328 </td>
   <td style="text-align:right;"> 0.8139223 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> twc </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 1.0202943 </td>
   <td style="text-align:right;"> 0.0291979 </td>
   <td style="text-align:right;"> 0.6881022 </td>
   <td style="text-align:right;"> 0.4913885 </td>
   <td style="text-align:right;"> 0.9627485 </td>
   <td style="text-align:right;"> 1.0807940 </td>
  </tr>
</tbody>
</table>


combined result


```r
model_uni <- bind_cols(pup_uni, crude_or_co) %>% 
  janitor::clean_names() %>%
  filter(term_2 != '(Intercept)') %>%
  select(-c(statistic_5, p_value_6)) %>%
  rename(odds_ratio = estimate_3)
```

```
## New names:
## • `variable` -> `variable...1`
## • `term` -> `term...2`
## • `estimate` -> `estimate...3`
## • `std.error` -> `std.error...4`
## • `statistic` -> `statistic...5`
## • `p.value` -> `p.value...6`
## • `conf.low` -> `conf.low...7`
## • `conf.high` -> `conf.high...8`
## • `variable` -> `variable...9`
## • `term` -> `term...10`
## • `estimate` -> `estimate...11`
## • `std.error` -> `std.error...12`
## • `statistic` -> `statistic...13`
## • `p.value` -> `p.value...14`
## • `conf.low` -> `conf.low...15`
## • `conf.high` -> `conf.high...16`
```




```r
kable(model_uni)  %>% 
  kable_styling(bootstrap_options = c("striped", "hover", 
                                      "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> variable_1 </th>
   <th style="text-align:left;"> term_2 </th>
   <th style="text-align:right;"> odds_ratio </th>
   <th style="text-align:right;"> std_error_4 </th>
   <th style="text-align:right;"> conf_low_7 </th>
   <th style="text-align:right;"> conf_high_8 </th>
   <th style="text-align:left;"> variable_9 </th>
   <th style="text-align:left;"> term_10 </th>
   <th style="text-align:right;"> estimate_11 </th>
   <th style="text-align:right;"> std_error_12 </th>
   <th style="text-align:right;"> statistic_13 </th>
   <th style="text-align:right;"> p_value_14 </th>
   <th style="text-align:right;"> conf_low_15 </th>
   <th style="text-align:right;"> conf_high_16 </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> age </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 0.0236740 </td>
   <td style="text-align:right;"> 0.0119691 </td>
   <td style="text-align:right;"> 0.0010929 </td>
   <td style="text-align:right;"> 0.0483811 </td>
   <td style="text-align:left;"> age </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 1.0239565 </td>
   <td style="text-align:right;"> 0.0119691 </td>
   <td style="text-align:right;"> 1.9779287 </td>
   <td style="text-align:right;"> 0.0479368 </td>
   <td style="text-align:right;"> 1.0010935 </td>
   <td style="text-align:right;"> 1.0495706 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> gender </td>
   <td style="text-align:left;"> .xmale </td>
   <td style="text-align:right;"> -0.9103560 </td>
   <td style="text-align:right;"> 0.4615231 </td>
   <td style="text-align:right;"> -1.8225388 </td>
   <td style="text-align:right;"> 0.0007672 </td>
   <td style="text-align:left;"> gender </td>
   <td style="text-align:left;"> .xmale </td>
   <td style="text-align:right;"> 0.4023810 </td>
   <td style="text-align:right;"> 0.4615231 </td>
   <td style="text-align:right;"> -1.9725036 </td>
   <td style="text-align:right;"> 0.0485521 </td>
   <td style="text-align:right;"> 0.1616149 </td>
   <td style="text-align:right;"> 1.0007675 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> epigastric_pain </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 0.6277644 </td>
   <td style="text-align:right;"> 1.1356428 </td>
   <td style="text-align:right;"> -1.3256526 </td>
   <td style="text-align:right;"> 3.6198522 </td>
   <td style="text-align:left;"> epigastric_pain </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 1.8734177 </td>
   <td style="text-align:right;"> 1.1356428 </td>
   <td style="text-align:right;"> 0.5527833 </td>
   <td style="text-align:right;"> 0.5804118 </td>
   <td style="text-align:right;"> 0.2656295 </td>
   <td style="text-align:right;"> 37.3320493 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> onset_more_24_hrs </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 0.5065612 </td>
   <td style="text-align:right;"> 0.4133389 </td>
   <td style="text-align:right;"> -0.2889264 </td>
   <td style="text-align:right;"> 1.3414593 </td>
   <td style="text-align:left;"> onset_more_24_hrs </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 1.6595745 </td>
   <td style="text-align:right;"> 0.4133389 </td>
   <td style="text-align:right;"> 1.2255348 </td>
   <td style="text-align:right;"> 0.2203738 </td>
   <td style="text-align:right;"> 0.7490674 </td>
   <td style="text-align:right;"> 3.8246207 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> diabetes </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 0.5654298 </td>
   <td style="text-align:right;"> 0.4707818 </td>
   <td style="text-align:right;"> -0.3772175 </td>
   <td style="text-align:right;"> 1.4849256 </td>
   <td style="text-align:left;"> diabetes </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 1.7602041 </td>
   <td style="text-align:right;"> 0.4707818 </td>
   <td style="text-align:right;"> 1.2010441 </td>
   <td style="text-align:right;"> 0.2297341 </td>
   <td style="text-align:right;"> 0.6857669 </td>
   <td style="text-align:right;"> 4.4146368 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NSAIDS </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 0.5436154 </td>
   <td style="text-align:right;"> 0.4205273 </td>
   <td style="text-align:right;"> -0.2904184 </td>
   <td style="text-align:right;"> 1.3672726 </td>
   <td style="text-align:left;"> NSAIDS </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 1.7222222 </td>
   <td style="text-align:right;"> 0.4205273 </td>
   <td style="text-align:right;"> 1.2926994 </td>
   <td style="text-align:right;"> 0.1961150 </td>
   <td style="text-align:right;"> 0.7479506 </td>
   <td style="text-align:right;"> 3.9246320 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> previous_OGDS </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> -0.0719126 </td>
   <td style="text-align:right;"> 0.7196661 </td>
   <td style="text-align:right;"> -1.6508310 </td>
   <td style="text-align:right;"> 1.2715818 </td>
   <td style="text-align:left;"> previous_OGDS </td>
   <td style="text-align:left;"> .xyes </td>
   <td style="text-align:right;"> 0.9306122 </td>
   <td style="text-align:right;"> 0.7196661 </td>
   <td style="text-align:right;"> -0.0999249 </td>
   <td style="text-align:right;"> 0.9204039 </td>
   <td style="text-align:right;"> 0.1918904 </td>
   <td style="text-align:right;"> 3.5664897 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> systolic </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> -0.0133813 </td>
   <td style="text-align:right;"> 0.0085103 </td>
   <td style="text-align:right;"> -0.0308281 </td>
   <td style="text-align:right;"> 0.0028134 </td>
   <td style="text-align:left;"> systolic </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 0.9867078 </td>
   <td style="text-align:right;"> 0.0085103 </td>
   <td style="text-align:right;"> -1.5723584 </td>
   <td style="text-align:right;"> 0.1158675 </td>
   <td style="text-align:right;"> 0.9696422 </td>
   <td style="text-align:right;"> 1.0028173 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> diastolic </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> -0.0197762 </td>
   <td style="text-align:right;"> 0.0144968 </td>
   <td style="text-align:right;"> -0.0491267 </td>
   <td style="text-align:right;"> 0.0081354 </td>
   <td style="text-align:left;"> diastolic </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 0.9804181 </td>
   <td style="text-align:right;"> 0.0144968 </td>
   <td style="text-align:right;"> -1.3641724 </td>
   <td style="text-align:right;"> 0.1725133 </td>
   <td style="text-align:right;"> 0.9520605 </td>
   <td style="text-align:right;"> 1.0081686 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ASA </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 0.5224183 </td>
   <td style="text-align:right;"> 0.3154616 </td>
   <td style="text-align:right;"> -0.0941774 </td>
   <td style="text-align:right;"> 1.1523982 </td>
   <td style="text-align:left;"> ASA </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 1.6861001 </td>
   <td style="text-align:right;"> 0.3154616 </td>
   <td style="text-align:right;"> 1.6560438 </td>
   <td style="text-align:right;"> 0.0977129 </td>
   <td style="text-align:right;"> 0.9101213 </td>
   <td style="text-align:right;"> 3.1657760 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PULP </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 0.2361062 </td>
   <td style="text-align:right;"> 0.0906398 </td>
   <td style="text-align:right;"> 0.0624931 </td>
   <td style="text-align:right;"> 0.4201174 </td>
   <td style="text-align:left;"> PULP </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 1.2663088 </td>
   <td style="text-align:right;"> 0.0906398 </td>
   <td style="text-align:right;"> 2.6048836 </td>
   <td style="text-align:right;"> 0.0091905 </td>
   <td style="text-align:right;"> 1.0644871 </td>
   <td style="text-align:right;"> 1.5221402 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> perforation </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 1.2032426 </td>
   <td style="text-align:right;"> 0.2843708 </td>
   <td style="text-align:right;"> 0.6827088 </td>
   <td style="text-align:right;"> 1.8029077 </td>
   <td style="text-align:left;"> perforation </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 3.3309004 </td>
   <td style="text-align:right;"> 0.2843708 </td>
   <td style="text-align:right;"> 4.2312458 </td>
   <td style="text-align:right;"> 0.0000232 </td>
   <td style="text-align:right;"> 1.9792317 </td>
   <td style="text-align:right;"> 6.0672637 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> admission_to_op_hrs </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 0.0060960 </td>
   <td style="text-align:right;"> 0.0202384 </td>
   <td style="text-align:right;"> -0.0373817 </td>
   <td style="text-align:right;"> 0.0459273 </td>
   <td style="text-align:left;"> admission_to_op_hrs </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 1.0061147 </td>
   <td style="text-align:right;"> 0.0202384 </td>
   <td style="text-align:right;"> 0.3012113 </td>
   <td style="text-align:right;"> 0.7632534 </td>
   <td style="text-align:right;"> 0.9633084 </td>
   <td style="text-align:right;"> 1.0469983 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> hemoglobin </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> -0.1237478 </td>
   <td style="text-align:right;"> 0.0615749 </td>
   <td style="text-align:right;"> -0.2493337 </td>
   <td style="text-align:right;"> -0.0057555 </td>
   <td style="text-align:left;"> hemoglobin </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 0.8836027 </td>
   <td style="text-align:right;"> 0.0615749 </td>
   <td style="text-align:right;"> -2.0097125 </td>
   <td style="text-align:right;"> 0.0444616 </td>
   <td style="text-align:right;"> 0.7793198 </td>
   <td style="text-align:right;"> 0.9942611 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> twc </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 0.0200912 </td>
   <td style="text-align:right;"> 0.0291979 </td>
   <td style="text-align:right;"> -0.0379630 </td>
   <td style="text-align:right;"> 0.0776960 </td>
   <td style="text-align:left;"> twc </td>
   <td style="text-align:left;"> .x </td>
   <td style="text-align:right;"> 1.0202943 </td>
   <td style="text-align:right;"> 0.0291979 </td>
   <td style="text-align:right;"> 0.6881022 </td>
   <td style="text-align:right;"> 0.4913885 </td>
   <td style="text-align:right;"> 0.9627485 </td>
   <td style="text-align:right;"> 1.0807940 </td>
  </tr>
</tbody>
</table>

multivariable logistic  regression


```r
write_csv(model_uni, 'model_uni.csv')
```



```r
model_multivar <- glm(oc2 ~ age + gender + epigastric_pain + onset_more_24_hrs + 
                        diabetes + NSAIDS + factor(ASA) + PULP + perforation + 
                        hemoglobin, 
                      family = binomial(link = 'logit'), data = PUP2)
model_multivar2 <- tidy(model_multivar, conf.int = T)
```




