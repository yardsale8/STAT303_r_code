Introduction to R
========================================================
author: 
date: 
autosize: true

Why use R?
========================================================

- Save and rerun code
- Lots of stats/data science packages
- Great graphics
- Built for data
- free, open source, cross-platform
- Large community

Marketshare
========================================================

![](Fig-1a-IndeedJobs-2017.png)

Why RStudio?
========================================================

- Adds *quality of life* improvements
  - Code completion
  - Variable explorer
  - Keyboard shortcuts
- Adds more advanced file-types
  - Rmarkdown
  - Rpresentation
  
More about RStudio
========================================================

![](RStudio.png)
  
Interacting with R
========================================================

- **Ctrl/Cmd + Enter** to run code
- **Ctrl + 1** and **Ctrl + 2** switches between script and console

Getting Help
========================================================


```r
?barplot
args(lm)
```

```
function (formula, data, subset, weights, na.action, method = "qr", 
    model = TRUE, x = FALSE, y = FALSE, qr = TRUE, singular.ok = TRUE, 
    contrasts = NULL, offset, ...) 
NULL
```

```r
??kruskal
```

Evaluating Expression in the Console
========================================================


```r
3 + 5
```

```
[1] 8
```

```r
12 / 7
```

```
[1] 1.714286
```

Saving and Retrieving a Value
========================================================


```r
weight_kg <- 55
weight_kg
```

```
[1] 55
```

Using Variables in Expressions
========================================================


```r
# A variable in an expression
2.2 * weight_kg
```

```
[1] 121
```

```r
# The value didn't change
weight_kg
```

```
[1] 55
```

Using Variables in Expressions
========================================================

```r
# Changing the value
weight_kg <- 57.5
2.2 * weight_kg
```

```
[1] 126.5
```

```r
# Creating a new variable
weight_lb <- 2.2 * weight_kg
weight_lb
```

```
[1] 126.5
```

Challenge
========================================================

What are the values after each statement in the following?

```r
mass <- 47.5            # mass?
age  <- 122             # age?
mass <- mass * 2.0      # mass?
age  <- age - 20        # age?
mass_index <- mass/age  # mass_index?
```

Calling functions
========================================================


```r
a <- 2
b <- sqrt(a)
b
```

```
[1] 1.414214
```

```r
round(3.14159)
```

```
[1] 3
```

More on calling functions
========================================================


```r
# Changing a default value
args(round)
```

```
function (x, digits = 0) 
NULL
```

```r
round(3.14159, digits = 2)
```

```
[1] 3.14
```

More ways to provide arguments
========================================================

```r
args(round)
```

```
function (x, digits = 0) 
NULL
```

```r
# Pass arguments by position
round(3.14159, 2)
```

```
[1] 3.14
```

```r
# Pass arguments using name = value (order doesn't matter)
round(digits = 2, x = 3.14159)
```

```
[1] 3.14
```

Vectors
========================================================

- Used to store an ordered sequence
- 6 basic types
  - character
  - numeric
  - logical
  - integer
  - complex
  - raw
  
Vector Examples
========================================================


```r
weight_g <- c(50, 60, 65, 82)
weight_g
```

```
[1] 50 60 65 82
```

```r
animals <- c("mouse", "rat", "dog")
animals
```

```
[1] "mouse" "rat"   "dog"  
```

Vector Examples
========================================================


```r
length(weight_g)
```

```
[1] 4
```

```r
class(weight_g)
```

```
[1] "numeric"
```

```r
str(weight_g)
```

```
 num [1:4] 50 60 65 82
```

Adding to a Vector with `c`
========================================================


```r
weight_g <- c(weight_g, 90) # add to the end of the vector
weight_g <- c(30, weight_g) # add to the beginning of the vector
weight_g
```

```
[1] 30 50 60 65 82 90
```

Challenge - Mixing Data Types
========================================================

Look at the classes of the following vectors.


```r
num_char <- c(1, 2, 3, 'a')
num_logical <- c(1, 2, 3, TRUE)
char_logical <- c('a', 'b', 'c', TRUE)
tricky <- c(1, 2, 3, '4')
```

Subsetting Vectors
========================================================


```r
animals <- c("mouse", "rat", "dog", "cat")
# R indexes starting at 1!
animals[2]
```

```
[1] "rat"
```

```r
animals[c(3, 2)]
```

```
[1] "dog" "rat"
```

```r
more_animals <- animals[c(1, 2, 3, 2, 1, 4)]
more_animals
```

```
[1] "mouse" "rat"   "dog"   "rat"   "mouse" "cat"  
```

Conditional Subsetting
========================================================


```r
weight_g <- c(21, 34, 39, 54, 55)
weight_g[c(TRUE, FALSE, TRUE, TRUE, FALSE)]
```

```
[1] 21 39 54
```

Conditional Subsetting
========================================================


```r
weight_g > 50
```

```
[1] FALSE FALSE FALSE  TRUE  TRUE
```

```r
weight_g[weight_g > 50]
```

```
[1] 54 55
```

Conditional Subsetting
========================================================


```r
weight_g[weight_g < 30 | weight_g > 50]
```

```
[1] 21 54 55
```

```r
weight_g[weight_g >= 30 & weight_g == 21]
```

```
numeric(0)
```

Checking if a value is **in** a collection
========================================================


```r
animals %in% c("rat", "cat", "dog", "duck", "goat")
```

```
[1] FALSE  TRUE  TRUE  TRUE
```

```r
animals[animals %in% c("rat", "cat", "dog", "duck", "goat")]
```

```
[1] "rat" "dog" "cat"
```

Be your own interpreter! (How to think like R)
========================================================

- Work *inside out*
- Evaluate the inner most expression
- Replace code with a value

Be your own interpreter! (How to think like R)
========================================================


```r
# Original expression
animals[animals %in% c("rat", "cat", "dog", "duck", "goat")]
```

```
[1] "rat" "dog" "cat"
```

```r
# Inner-most expression
animals
```

```
[1] "mouse" "rat"   "dog"   "cat"  
```

```r
# Replace animals with its value
c("mouse", "rat", "dog", "cat") %in% c("rat", "cat", "dog", "duck", "goat")
```

```
[1] FALSE  TRUE  TRUE  TRUE
```


Review of Expressions and Interpreter
========================================================

- An expression is code with value
- An interpreter converts expressions to values
- Learn R == Learning to think like R
  - Be your own interpreter
  

Be your own interpreter! (How to think like R)
========================================================


```r
# Move out, replacing the inner expression with its value
animals[animals %in% c("rat", "cat", "dog", "duck", "goat")]
```

```
[1] "rat" "dog" "cat"
```

```r
animals[c(FALSE, TRUE, TRUE, TRUE)]
```

```
[1] "rat" "dog" "cat"
```

```r
c("mouse", "rat", "dog", "cat")[c(FALSE, TRUE, TRUE, TRUE)]
```

```
[1] "rat" "dog" "cat"
```

Challenge (optional)
========================================================

Can you figure out why `"four" > "five"` returns TRUE?


```r
"four" > "five"
```

```
[1] TRUE
```

Missing Values
========================================================

- Real data as missing values
- R is designed for real data
- `NA` is a missing value

Computing with Missing Values
========================================================


```r
heights <- c(2, 4, 4, NA, 6)
mean(heights)
```

```
[1] NA
```

```r
mean(heights, na.rm = TRUE)
```

```
[1] 4
```

Removing `NA`s
========================================================


```r
## Extract those elements which are not missing values.
heights[!is.na(heights)]
```

```
[1] 2 4 4 6
```

Be your own interpreter
========================================================
Explore the following expression.


```r
heights <- c(2, 4, 4, NA, 6)
heights[!is.na(heights)]
```

```
[1] 2 4 4 6
```

Removing `NA`s
========================================================


```r
## Returns the object with incomplete cases removed. The returned object is atomic.
na.omit(heights)
```

```
[1] 2 4 4 6
attr(,"na.action")
[1] 4
attr(,"class")
[1] "omit"
```

```r
## Extract those elements which are complete cases.
heights[complete.cases(heights)]
```

```
[1] 2 4 4 6
```



