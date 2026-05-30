---
title: 周总结-tibble中添加行
author: 桑峰
date: 2023-09-20T00:00:00.000Z
categories:
  - 编程
tags:
  - R
  - tibble
format: hugo-md
---


## Add a new row into a empty tibble in R

There is a simple example below.

``` r
library(tidyverse)

a <- tibble()
a <- a %>%
  bind_rows(list(A = 1, B = 2))
```

This is the results:

<figure>
<img src="./20230920_fig1.png" alt="A simple example" />
<figcaption aria-hidden="true">A simple example</figcaption>
</figure>
