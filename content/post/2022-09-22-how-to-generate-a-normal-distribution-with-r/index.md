---
title: How to generate a normal distribution with R
author: 'Bruno Braga Montezano'
date: '2022-09-22'
slug: how-to-generate-a-normal-distribution-with-r
categories: ["r"]
tags: ["statistics"]
---

# What is a normal distribution?

When studying statistics, we often hear a term called *normal distribution*.
But what is it?

It is a type of continuous probability distribution for a random variable.

It is important to represent real-valued random variables in many contexts,
including biostatistics and health sciences, because we often can't know the
true distribution from what these characteristics come from. And we should be
able to further understand the importance of this concept when we study the
central limit theorem, stating that under some conditions, the average of
many observations of a random variable with finite mean and variance is *also*
a random variable, converging into a normal distribution. It's kind of an inception
(like that [Christopher Nolan movie](https://www.warnerbros.com/movies/inception)).

The normal distribution (or *bell curve*) has some specific properties like:
presenting similar values for the arithmetic mean, median and mode, among others.
But the point is that the focus of this post is not defining normal distribution,
but instead talking about one of the functions in the [R programming language](https://cran.r-project.org/index.html)
that allows us to create random normal distributions.

# The `rnorm` function

This function called `rnorm` is from the R Stats Package, that contains multiple
R statistical functions, and we can use it to randomly generate deviates.

This function can receive up to three arguments: `n`, `mean` and `sd`. The only
argument that cannot be a `NULL` is the first: `n`.
Right below, we have an example of using the function with just the `n` argument
plotted with `ggplot2` as a histogram and density plot.

    x <- rnorm(1000)
    
    x |> 
        tibble::as_tibble_col("x") |> 
        ggplot2::ggplot(ggplot2::aes(x = x))

![Normal distribution generated with `rnorm(1000)` in R 4.2.1](/images/posts/2022-09-22-how-to-generate-a-normal-distribution-with-r/normal.png)

Let's understand what each of these arguments can change in our output:

- `n` specifies the number of observations;

- `mean` specifies a vector of means (pay attention to the *vector* part);

- `sd` specifies a vector of standard deviations (same as above).

# Let's simulate

Let's suppose we want to simulate the distribution of the height of males and
females of a fictional country named *Metapholand*. In Metapholand, we assume
that we have a population of two thousand people (around 50/50 of males and females)
and men have a mean of 1.80
meters of height, and women have 1.50 meters in average. We know that the standard
deviation in this population is 5 centimeters. How could we simulate that with
`rnorm`?

Pretty simple:

    rnorm(n = 2000, mean = c(1.8, 1.5), sd = 0.05) |>
        tibble::as_tibble_col("x") |>
        ggplot2::ggplot(ggplot2::aes(x = x)) +
            ggplot2::geom_histogram(alpha = 0.85, fill = "darkgreen") +
            ggplot2::theme_minimal(base_size = 20) +
            ggplot2::labs(y = "Count")

![Bimodal distribution generated with the code above in R 4.2.1](/images/posts/2022-09-22-how-to-generate-a-normal-distribution-with-r/bimodal.png)

As we saw, the code generated a bimodal distribution (or double-peaked distribution),
in other words, a distribution
that has two peaks.
The values in this distribution are clustered at each peak, with a pattern of
increase and decrease. The two peaks represent the two local maximums (for men
and for women).
In this case, it happened because we are *hypothetically*
analyzing two different groups together. There are differences between gender in
their heights that we did not took into account when plotting.
