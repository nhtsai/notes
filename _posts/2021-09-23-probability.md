---
layout: post
title: "Introduction to Probability"
description: "Course notes from Harvard STAT 110, Fall 2011."
author: "Nathan Tsai"
toc: true
comments: false
hide: false
search_exclude: false
show_tags: true
categories: [course-notes, mathematics, probability, statistics]
permalink: /probability
---

# Harvard STAT 110: Introduction to Probability, Fall 2011

## Course Resources
* Professor Joe Blitzstein
* [Course Lectures](https://www.youtube.com/playlist?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo)
* [Course Website](https://projects.iq.harvard.edu/stat110)
* [Course Syllabus](https://projects.iq.harvard.edu/files/stat110/files/stat110harvarditunesusyllabus.pdf)
* [Course EdX](https://learning.edx.org/course/course-v1:HarvardX+STAT110x+2T2021/home)
* [Course Textbook](http://probabilitybook.net/): Introduction to Probability, 2 ed. by Blitzstein & Hwang
* Course focuses on using probability as a language and set of tools for understanding statistics, science, risk, and randomness.
* Topics: sample spaces/events, conditional probability, Bayes' Theorem, univariate distributions, multivariate distributions, limit laws, Markov chains


## 1. Probability, Counting

* Probability has its roots in gambling and is an important part of many disciplines today

* **Sample Space**: set of all possible outcomes of an experiment
    * **Experiment**: *anything* with certain possible outcomes that are unknown beforehand
    * **Event**: a *subset* of the sample space
    * *Example*: Rolling 2 dice has 36 total outcomes in the *sample space*, and an *event* could be the subset of all outcomes that sum up to a number

* **Naive Definition of Probability**
    $$ P(A)=\frac{\text{\# favorable outcomes}}{\text{\# possible outcomes}} $$
* *Example*: Flipping a fair coin twice, what is probability that both flips are heads?
    * 1 favorable outcome: $HH$
    * 4 total outcomes: $HH, HT, TH, TT$
    * Therefore, $P(\text{both heads})=\frac{1}{4}$
* Assumes that *all outcomes are equally likely* and a *finite sample space*
    * If the coin was not fair, the outcomes are not equally likely
    * If the outcomes could be infinite, the probability definition doesn't make sense

<br />

* **Multiplication Rule**
    * If we have an experiment with $n_1$ possible outcomes, and for each outcome of $1\text{st}$ experiment, there are $n_2$ possible outcomes for the $2\text{nd}$ experiment, $\ldots$, and for each outcome of $(r-1)\text{th}$ experiment, there are $n_r$ possible outcomes for the $r\text{th}$ experiment, then there are $n_1 * n_2 * \ldots * n_r$ overall possible outcomes.
* *Example*: 3 flavors of ice cream and 2 types of cones
    * Ice cream flavor experiment has 3 outcomes (chocolate, vanilla, strawberry)
    * Waffle cone experiment has 2 outcomes (cake, waffle)
    * There are $6 \text{ possible outcomes}= 2 \times 3 = 3 \times 2 $ , order of cone/ice cream choice does not matter

<br />

* **Binomial Coefficient**
    $$ \binom{n}{k}=\frac{n!}{(n-k)!k!}, \text{0 if k>n} $$
    * Pronounced "n choose k", choosing k things of n, where order does not matter
    * Number of subsets of size k given n elements, where order does not matter
    * Intuition from multiplication rule
        * Choose K elements: $n(n-1)(n-2) \ldots (n-k+1)$
        * In any order: divide by $k!$ because we over-counted by that factor
        * Therefore, $\frac{n(n-1)(n-2) \ldots (n-k+1)}{k!}=\frac{n!}{(n-k)!k!}$
* *Example*: Find probability of full house in poker, 5 card hand
    * A *full house* is 3 cards of 1 rank and 2 cards of another rank, e.g. 3 sevens and 2 tens
    * Assume deck is randomly shuffled so that all sets of 5 cards are equally likely
        * Because we assume cards are evenly shuffled, we can justify using naive definition
    * Number of possible hands: $\binom{52}{5}$ , "52 choose 5"
    * Favorable hands (full house): $\binom{13}{1} \binom{4}{3} \times \binom{12}{1} \binom{4}{2}$
        * Of 13 ranks choose 1 and of 4 cards need 3, then of 12 remaining ranks choose 1 and of 4 cards we need 2
    * Therefore, $\frac{\binom{13}{1} \binom{4}{3} \times \binom{12}{1} \binom{4}{2}}{\binom{52}{5}}$

<br />

* **Sampling Table**: choose k objects out of n
    * We want to know how many ways there are of doing this, which should be immediate from the multiplication rule
    * We can choose to sample **with replacement** (we can choose the same element again) or **without replacement** (we cannot choose the same element again)

    |                     | Order Matters | Order Doesn't Matter |
    |:--------------------|:-------------:|:--------------------:|
    | With Replacement    | $n^k$ | $\binom{n+k-1}{k}$ |
    | Without Replacement | $(n)(n-1)\ldots(n-k+1)$ | $\binom{n}{k}$ |


## 2. Story Proofs, Axioms of Probability

* 


## 3. Birthday Problem, Properties of Probability

## 4. Conditional Probability I

## 5. Conditional Probability II, Law of Total Probability

## 6. Monty Hall, Simpson's Paradox

## 7. Gambler's Ruin, Random Variables

## 8. Random Variables and Their Distributions

## 9. Expectation I, Indicator Random Variables, Linearity

## 10. Expectation II

## 11. Poisson Distribution

## 12. Discrete vs. Continuous, Uniform Distribution

## 13. Normal Distribution

## 14. Location, Scale, LOTUS

## 15. Midterm Review

## 16. Exponential Distribution

## 17. Moment Generating Functions I

## 18. Moment Generating Functions II

## 19. Joint, Conditional, Marginal Distributions

## 20. Multinomial, Cauchy Distributions

## 21. Covariance, Correlation

## 22. Transformations, Convolutions

## 23. Beta Distribution

## 24. Gamma Distribution, Poisson Process

## 25. Order Statistics, Conditional Expectation I

## 26. Conditional Expectation II

## 27. Conditional Expectation Given R.V.

## 28. Inequalities

## 29. Law of Large Numbers, Central Limit Theorem

## 30. Chi-Square, Student-T, Multivariate Normal

## 31. Markov Chains I

## 32. Markov Chains II

## 33. Markov Chains III

## 34. A Look Ahead

## References
[^1]: Footnote