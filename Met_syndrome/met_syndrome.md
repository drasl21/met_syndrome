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





