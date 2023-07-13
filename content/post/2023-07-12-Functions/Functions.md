---
title: "Presentation Ninja"
subtitle: "⚔<br/>with xaringan"
author: "Yihui Xie"
institute: "RStudio, PBC"
date: "2016/12/12 (updated: 2023-07-12)"
output:
  xaringan::moon_reader:
    seal: false
    lib_dir: libs
    self_contained: true
    nature:
      highlightStyle: github
      highlightLines: true
      countIncrementalSlides: false
---

---

# Functions in R 

--

- Packages allow you to download sets of functions put together by others. 

--

- You can also write your own custom functions in R 

--

- Any process that you find you want to carry out repeatedly can be made into a function

--

- In fact, you *should* make repetitive processes into functions

---

# DRY vs WET Code

--

- Both acronyms 

--

- **DRY - "Don't Repeat Yourself"** 

--

> DRY (“Don't Repeat Yourself”) principle follows the idea of every logic duplication being eliminated by abstraction. This means that during the development process we should avoid writing repetitive duplicated code as much as possible. This principle can easily be implemented in any programming language

--

- Instead of reating chunks of code over and over, turn that chunk of code into a *function* to keep the code dry. 

--

**WET - "Write Evrything Twice", "We Enjoy Typing", "Waste Everyone's Time"** 

--

- WET code does have some specific utility - especially when producing carefully currated final panels for publication that include chart-specific annotations and signficiance indicators.

---

# Functions in R

--


```r
my_awesome_function <- function(Input){
  a <- Input
  b <- a / 10
  print(b)
}
```

--

- Assign a name to the new function

--

- Assign some number of "inputs" (you may name them anything)

--

- List a set of processes inside curly brackets 

--

- Include a print() command at the end, if you want your new function what to return something (e.g. a chart or a statistic table)

---

# Functions in R


```r
my_awesome_function <- function(Input){
  a <- Input
  b <- a / 10
  print(b)
}
```

--


```r
my_value <- 100
```

--


```r
my_awesome_function(my_value)
```

---

# Functions in R


```r
my_awesome_function <- function(Input){
  a <- Input
  b <- a / 10
  print(b)
}
```


```r
my_value <- 100
```


```r
my_awesome_function(my_value)
```

```
## [1] 10
```

--

- Custom functions can be scaled to execute extremely comples processes (e.g. graphs & stats)

--

- functions can be compounded (functions inside of functions)

--

Whether you want your code to be **WET** or **DRY** depends on what you are trying to accomplish.

























