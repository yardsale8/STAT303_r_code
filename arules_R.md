arules_R
========================================================
author: Todd Iverson
date: 11/21/2017
autosize: true



Recall - Association Rules
========================================================

* Example: $\{butter\} \rightarrow \{whole.milk\}$
  * $Support(\textrm{butter and milk}) = \frac{\textrm{Num butter and milk}}{\textrm{Num Total}}$
  * $Support(\textrm{butter}) = \frac{\textrm{Num butter}}{\textrm{Nu
  m Total}}$
  * $Confidence= \frac{Support(\textrm{butter and milk})}{Support(\textrm{butter})}$
  * $Confidence= \frac{Confidence}{Support(\textrm{butter})}$
  
Exercise - Compute the confidence and lift (small example)
========================================================

![](./small_example.png)

Arules with R (The long way)
========================================================

* Use `dyplr` to 
  * mutate to compute logical values
  * summarize to compute counts and percents
  
  
Read the data set
========================================================


```r
groceries <- read.csv("Groceries.csv",
                      header = TRUE,
                      sep=",")
head(groceries[, 1:6])
```

```
  frankfurter sausage liver.loaf ham meat finished.products
1           0       0          0   0    0                 0
2           0       0          0   0    0                 0
3           0       0          0   0    0                 0
4           0       0          0   0    0                 0
5           0       0          0   0    0                 0
6           0       0          0   0    0                 0
```

Set Up
========================================================


```r
library(dplyr)
butter_milk <- groceries %>%
                select("butter", "whole.milk")
head(butter_milk)
```

```
  butter whole.milk
1      0          0
2      0          0
3      0          1
4      0          0
5      0          1
6      1          1
```

```r
N <- nrow(groceries)
N
```

```
[1] 9835
```

Compute the Total for Butter
========================================================


```r
total_butter <- butter_milk %>%
  filter(butter == 1) %>%
  summarize(total_butter = sum(butter))
total_butter
```

```
  total_butter
1          545
```

```r
support_butter = total_butter/N
support_butter
```

```
  total_butter
1   0.05541434
```

Compute the Proportion for Butter
========================================================


```r
support_butter <- butter_milk %>%
  filter(butter == 1) %>%
  summarize(support_butter = sum(butter)/N)
support_butter
```

```
  support_butter
1     0.05541434
```

Exercise - Compute the Support of whole.milk
========================================================

* Verify that the Whole Milk column is `whole.milk`
* Compute the support


Solution
========================================================


```r
support_milk <- 
  butter_milk %>%
  mutate(bought_milk = whole.milk == 1) %>%
  summarize(total_milk = sum(bought_milk)/N)
support_milk
```

```
  total_milk
1   0.255516
```

Support for Butter and Milk
========================================================


```r
support_butter_milk <- 
  butter_milk %>%
  mutate(bought_butter_and_milk = (butter == 1 & whole.milk == 1)) %>%
  summarize(total_butter = sum(bought_butter_and_milk)/N)
support_butter_milk
```

```
  total_butter
1   0.02755465
```

Confidence and Lift
========================================================


```r
conf_butter_milk <- support_butter_milk/support_butter
conf_butter_milk
```

```
  total_butter
1    0.4972477
```

```r
lift_butter_milk <- conf_butter_milk/support_milk
lift_butter_milk
```

```
  total_butter
1     1.946053
```

All at once
========================================================


```r
groceries %>%
  mutate(bought_milk = whole.milk == 1,
          bought_butter = butter == 1,
          bought_butter_milk = (butter == 1 & whole.milk == 1)) %>%
          summarize(support_milk = sum(bought_milk)/N,
                    support_butter = sum(bought_butter)/N,
                    support_butter_milk = sum(bought_butter_milk)/N) %>%
          mutate(confidence = support_butter_milk/support_butter) %>%
          mutate(lift = confidence/support_milk)
```

```
  support_milk support_butter support_butter_milk confidence     lift
1     0.255516     0.05541434          0.02755465  0.4972477 1.946053
```

Notes
========================================================

* Must compute values before you use them
  * Compute supports before confidence
  * Compute confidence before lift

Exercise
========================================================

* Compute the confidence and lift for the following rules
  * $\{Cereal\} \rightarrow \{Whole Milk\}$
  * $\{Liquor\} \rightarrow \{Whole Milk\}$
* Try doing this with one `mutate`
* Interpret the values of the lift for each.







