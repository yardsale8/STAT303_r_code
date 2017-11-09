Simulating the Notebook Context
========================================================
author: Todd Iverson
date: 
autosize: true

The Notebook Context
========================================================
![notebook context](notebook_context.png)
***
- Useful way to imagine an experiment
- Each row represents 1 trial
- First column is an outcome
- Subsequent columns are TRUE/FALSE questions

Two types of experiments
========================================================

- Most experiments can be thoughts of as sampling problems
- There are two types of sampling
  - with replacement
  - without replacement 
- Types of experiments
  - **simple experiments** have sample size 1
  - **compound experiments** have sample size $n > 1$

Example: Coin Tosses
========================================================
![coin urns](coin_sampling.png)
***
- Flipping a fair coin
- Sample from a container with `"H"` and `"T"`
- Questions
  - with or without replacement?
  - How would you simulate a coin that comes up heads 60% of the time?


Today's Goals
========================================================

- Simulate simple and compound events with `sample`
- Combine compound outcomes into one string with `str_c`
- Mimic the notebook context with a data frame

Using `sample` for simple experiments
========================================================


```r
# Make the "container" (char vector)
sides = c("H", "T")
```

```r
# Use sample to simulate the experiment
sample(sides, 1, replace = TRUE)
```

```
[1] "T"
```

```r
sample(sides, 1, replace = TRUE)
```

```
[1] "T"
```

Using `sample` for compound experiments
========================================================


```r
# Use sample to simulate the experiment
sample(sides, 3, replace = TRUE)
```

```
[1] "T" "T" "T"
```

```r
sample(sides, 3, replace = TRUE)
```

```
[1] "T" "T" "T"
```

Use `replicate` to repeat the experiment
========================================================

- `replicate` repeats an expression $n$ times
- Syntax: `replicate(n, expr)`
- Useful for simulations
  - Make a function for 1 trials
  - Use replicate + function call for many trials
  
Replicate in action
========================================================


```r
trials <- replicate(7, sample(sides, 3, replace = TRUE))
trials
```

```
     [,1] [,2] [,3] [,4] [,5] [,6] [,7]
[1,] "H"  "T"  "T"  "T"  "H"  "T"  "H" 
[2,] "T"  "T"  "T"  "H"  "H"  "T"  "H" 
[3,] "T"  "H"  "H"  "T"  "H"  "H"  "H" 
```

Transpose with `t` to make the data tall
========================================================

- `t` is transpose
- switches rows and columns


```r
t(trials)
```

```
     [,1] [,2] [,3]
[1,] "H"  "T"  "T" 
[2,] "T"  "T"  "H" 
[3,] "T"  "T"  "H" 
[4,] "T"  "H"  "T" 
[5,] "H"  "H"  "H" 
[6,] "T"  "T"  "H" 
[7,] "H"  "H"  "H" 
```

Package the outcomes in a data frame
========================================================


```r
library(dplyr)
notebook <- trials %>% 
              t() %>% 
              as.data.frame() %>% 
              setNames(c("Roll_1", "Roll_2", "Roll_3"))
notebook
```

```
  Roll_1 Roll_2 Roll_3
1      H      T      T
2      T      T      H
3      T      T      H
4      T      H      T
5      H      H      H
6      T      T      H
7      H      H      H
```

Using `mutate` to asks questions about the trials
========================================================
- **Question 1:** Was the first roll a head?


```r
library(dplyr)
notebook <- notebook %>% 
              mutate(first_head = (Roll_1 == "H"))
notebook
```

```
  Roll_1 Roll_2 Roll_3 first_head
1      H      T      T       TRUE
2      T      T      H      FALSE
3      T      T      H      FALSE
4      T      H      T      FALSE
5      H      H      H       TRUE
6      T      T      H      FALSE
7      H      H      H       TRUE
```

Using `mutate` to asks questions about the trials
========================================================
- **Question 2:** Was there at least one head in the last two rolls
- **Note:** The symbol `|` and `&` are *or* and *and*, respectively


```r
library(dplyr)
notebook <- notebook %>% 
  mutate(at_least_one_head = (Roll_2 == "H") | (Roll_3 == "H"))
notebook
```

```
  Roll_1 Roll_2 Roll_3 first_head at_least_one_head
1      H      T      T       TRUE             FALSE
2      T      T      H      FALSE              TRUE
3      T      T      H      FALSE              TRUE
4      T      H      T      FALSE              TRUE
5      H      H      H       TRUE              TRUE
6      T      T      H      FALSE              TRUE
7      H      H      H       TRUE              TRUE
```

Package the whole process in a pipe
========================================================


```r
notebook <- replicate(100, sample(sides, 3, replace = TRUE)) %>%
              t() %>%
              as.data.frame() %>%
              setNames(c("Roll_1", "Roll_2", "Roll_3")) %>%
              mutate(first_head = (Roll_1 == "H")) %>%
              mutate(at_least_one_head = (Roll_2 == "H") | (Roll_3 == "H"))
notebook
```

```
    Roll_1 Roll_2 Roll_3 first_head at_least_one_head
1        H      T      H       TRUE              TRUE
2        H      T      H       TRUE              TRUE
3        T      T      T      FALSE             FALSE
4        T      T      T      FALSE             FALSE
5        H      H      T       TRUE              TRUE
6        T      H      T      FALSE              TRUE
7        H      T      H       TRUE              TRUE
8        T      H      H      FALSE              TRUE
9        T      T      H      FALSE              TRUE
10       H      H      H       TRUE              TRUE
11       H      H      H       TRUE              TRUE
12       H      T      T       TRUE             FALSE
13       T      T      T      FALSE             FALSE
14       T      T      T      FALSE             FALSE
15       H      T      H       TRUE              TRUE
16       T      T      H      FALSE              TRUE
17       H      T      H       TRUE              TRUE
18       H      T      H       TRUE              TRUE
19       T      T      H      FALSE              TRUE
20       T      H      H      FALSE              TRUE
21       T      H      T      FALSE              TRUE
22       H      H      T       TRUE              TRUE
23       T      T      H      FALSE              TRUE
24       T      H      T      FALSE              TRUE
25       T      T      T      FALSE             FALSE
26       H      H      T       TRUE              TRUE
27       T      H      H      FALSE              TRUE
28       H      T      T       TRUE             FALSE
29       T      T      H      FALSE              TRUE
30       T      T      T      FALSE             FALSE
31       H      H      T       TRUE              TRUE
32       H      T      H       TRUE              TRUE
33       T      H      H      FALSE              TRUE
34       H      H      T       TRUE              TRUE
35       T      T      T      FALSE             FALSE
36       H      T      T       TRUE             FALSE
37       H      T      H       TRUE              TRUE
38       H      T      H       TRUE              TRUE
39       T      T      T      FALSE             FALSE
40       T      T      T      FALSE             FALSE
41       H      T      H       TRUE              TRUE
42       H      H      H       TRUE              TRUE
43       T      H      T      FALSE              TRUE
44       T      T      T      FALSE             FALSE
45       H      T      T       TRUE             FALSE
46       H      T      H       TRUE              TRUE
47       T      H      H      FALSE              TRUE
48       H      T      H       TRUE              TRUE
49       T      H      T      FALSE              TRUE
50       H      H      H       TRUE              TRUE
51       H      T      T       TRUE             FALSE
52       H      T      H       TRUE              TRUE
53       T      H      H      FALSE              TRUE
54       T      H      H      FALSE              TRUE
55       T      H      H      FALSE              TRUE
56       H      H      H       TRUE              TRUE
57       T      T      H      FALSE              TRUE
58       H      H      T       TRUE              TRUE
59       T      T      T      FALSE             FALSE
60       H      T      T       TRUE             FALSE
61       H      H      H       TRUE              TRUE
62       T      H      T      FALSE              TRUE
63       H      H      H       TRUE              TRUE
64       H      H      T       TRUE              TRUE
65       T      T      T      FALSE             FALSE
66       T      H      T      FALSE              TRUE
67       T      T      T      FALSE             FALSE
68       H      H      H       TRUE              TRUE
69       T      H      H      FALSE              TRUE
70       T      H      T      FALSE              TRUE
71       T      H      H      FALSE              TRUE
72       H      H      T       TRUE              TRUE
73       T      H      H      FALSE              TRUE
74       H      H      H       TRUE              TRUE
75       T      H      H      FALSE              TRUE
76       T      H      H      FALSE              TRUE
77       T      H      H      FALSE              TRUE
78       H      H      H       TRUE              TRUE
79       H      H      H       TRUE              TRUE
80       H      H      H       TRUE              TRUE
81       T      H      T      FALSE              TRUE
82       T      H      H      FALSE              TRUE
83       T      H      T      FALSE              TRUE
84       T      T      T      FALSE             FALSE
85       T      T      H      FALSE              TRUE
86       T      H      H      FALSE              TRUE
87       T      H      T      FALSE              TRUE
88       T      T      T      FALSE             FALSE
89       H      H      T       TRUE              TRUE
90       H      H      H       TRUE              TRUE
91       H      T      H       TRUE              TRUE
92       T      T      H      FALSE              TRUE
93       H      H      H       TRUE              TRUE
94       H      T      H       TRUE              TRUE
95       H      H      H       TRUE              TRUE
96       H      H      H       TRUE              TRUE
97       T      T      T      FALSE             FALSE
98       T      T      H      FALSE              TRUE
99       T      H      H      FALSE              TRUE
100      H      H      T       TRUE              TRUE
```

The `mean` of a logical vector is the number of `TRUE` entries
========================================================

- recall that `TRUE` and `FALSE` promotes to 0 and 1


```r
mean(c(TRUE, FALSE, TRUE, TRUE))
```

```
[1] 0.75
```

Estimate the probability with `summarize` and `mean`
========================================================


```r
notebook %>%
  summarize("P(first is head)" = mean(first_head), 
            "P(at least one head)" = mean(at_least_one_head))
```

```
  P(first is head) P(at least one head)
1             0.47                 0.78
```

Exercises
========================================================
  
1. Create a data frame for the experiment "Roll 2 fair 6-sided dice"
2. Create logical columns for each of the following
  1. The first roll is larger than 4
  2. The sum is larger than 7
3. Estimate the probability of each of the events given above
  
  
