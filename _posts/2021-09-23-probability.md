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
    * *Example*: The *experiment* of rolling 2 dice has 36 total outcomes in the *sample space*, and an *event* could be the subset of all outcomes that sum up to a number

* **Naive Definition of Probability**
    $$ P(A)=\frac{\text{\# favorable outcomes}}{\text{\# possible outcomes}} $$
* *Example*: Flipping a fair coin twice, what is probability that both flips are heads?
    * 1 favorable outcome: $HH$
    * 4 total outcomes: $HH, HT, TH, TT$
    * Therefore, $P(\text{both heads})=\frac{1}{4}$
* Assumes that *all outcomes are equally likely* and a *finite sample space*
    * If the coin was not fair, the outcomes would not be equally likely
    * If the sample space is infinite, the probability definition doesn't make sense

<br />

* **Multiplication Rule**
    * If we have an experiment with $n_1$ possible outcomes, and for each outcome of $1\text{st}$ experiment, there are $n_2$ possible outcomes for the $2\text{nd}$ experiment, $\ldots$, and for each outcome of $(r-1)\text{th}$ experiment, there are $n_r$ possible outcomes for the $r\text{th}$ experiment, then there are $n_1 * n_2 * \ldots * n_r$ overall possible outcomes.
    * The total number of possible outcomes of a series of experiments is the *product of each experiment's outcomes*.
* *Example*: 3 flavors of ice cream and 2 types of cones
    * Ice cream flavor experiment has 3 outcomes (chocolate, vanilla, strawberry)
    * Waffle cone experiment has 2 outcomes (cake, waffle)
    * There are $6 \text{ possible outcomes}= 2 \times 3 = 3 \times 2 $ , order of cone/ice cream choice does not matter

<br />

* **Binomial Coefficient**
    $$ \binom{n}{k}=\frac{n!}{(n-k)!k!}, \text{0 if k>n} $$
    * Number of subsets of size k given n elements, where order does not matter
    * Pronounced "n choose k", choosing k elements of n possible elements, where order does not matter
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

* **Sampling Table**: choose $k$ elements out of $n$ total elements
    * We want to know how many ways there are of doing this, which should be immediate from the multiplication rule
    * We can choose to sample **with replacement** (we can choose the same element again) or **without replacement** (we cannot choose the same element again)

    * With replacement, order matters
        * Have $k$ experiments, each with $n$ outcomes and order matters, so $\{1, 2\} \neq \{2, 1\}$

    $$n_1 \times n_2 \times \ldots \times n_k = n^k$$

    * Without replacement, order matters
        * Have $k$ experiments, with $n$ outcomes without replacement and order matters, so $\{1, 2\} \neq \{2, 1\}$

    $$n \times (n-1) \times \ldots \times (n-k+1)$$
    
    * Without replacement, order does not matter
        * Have $k$ experiments, with $n$ outcomes without replacement and order does not matter, so $\{1, 2\} = \{2, 1\}$
        * Want 1 combination of $k$ elements to represent all $k!$ permutations with the same $k$ elements, so need to divide by $k!$
        * Think of grouping all possible permutations with the same $k$ elements into $k!$ groups
            * For the $k=3$ choice $(1,2,3)$, there are $3!$ orderings that we want to group into 1 representation $\{1,2,3\}$
            * To do this grouping for all different choices, we divide by $k!$ so that we're left with the number of unique combinations of $k=3$ elements
    
    $$\frac{n \times (n-1) \times \ldots \times (n-k+1)}{k \times (k-1) \times \ldots \times 1} = \frac{n!}{(n-k)! \times k!} = \binom{n}{k}$$

    * With replacement, order does not matter
        * Hardest to understand, not obvious from multiplication rule
        * Have $k$ experiments, with $n$ outcomes and order does not matter, so $\{1, 2\} = \{2, 1\}$

    $$\frac{(n+k-1)!}{(n-1)! \times k!} = \binom{n+k-1}{n-1} = \binom{n+k-1}{k}$$

    |                     | Order Matters | Order Doesn't Matter |
    |:--------------------|:-------------:|:--------------------:|
    | With Replacement    | $n^k$         | $\binom{n+k-1}{k}$   |
    | Without Replacement | $nPk = \frac{n!}{(n-k)!}$ | $\binom{n}{k} = \frac{n!}{(n-k)!k!}$ |


## 2. Story Proofs, Axioms of Probability

* General Advice for Homework
    * Don't lose common sense
    * Do check answers, especially by doing simple and extreme cases
    * Label people, objects, etc. (If we have n people, label them 1, 2, ..., n)

<br />

* Pick k times from a set of n objects, where order does not matter and with replacement
    * Prove the answer is $\binom{n+k-1}{k}$ ways
    * *Example*: $k=0, \binom{n-1}{0} = 1$ 
        * Only 1 way to choose 0 elements from n-1 elements, choose none!
    * *Example*: $k=1, \binom{n}{1} = n$
        * When only picking 1 element, no difference if with/without replacement or if order matters or does not matter
    * *Example*: $n=2, \binom{k+1}{k} = \binom{k+1}{1} = k+1$
        * They are equal because: If we choose $k$ of $k+1$ elements, then the remaining is the 1. If we choose 1, then the remaining is the $k+1$
        * Choosing k elements from 2 total elements with replacement and order doesn't matter
            * If we get the first element $x$ times, we know we got the second element $k-x$ times
            * Number of possible first elements we get is from $\{0, \ldots, k\}$ , so there are $k+1$ possibilities of choosing $k$ from $n=2$ elements
        * Can think of this as flipping a fair coin twice

    * Equivalent: how many ways are there to put k indistinguishable elements into n distinguishable groups?
        * Say $n=4,k=6$, a possible way is  $(***),(),(**),(*) \rightarrow ***| |**|*$
        * There must be $k$ stars and $n-1$ separators, we just need to choose $k$ positions for stars in $n+k-1$ total positions, or $\binom{n+k-1}{k} = \binom{n+k-1}{n-1}$

    * Bose-Einstein
        * Bose said flipping a fair coin twice had 3 equally likely outcomes $\{HH, TT, 1H+1T\}$
        * Bose thought of the two coin flips as indistinguishable, rather than $\{HT, TH\}$
        * Thinking of coins as indistinguishable led to the discovery Bose-Einstein condensate

<br />

* **Story Proof**: proof by interpretation, rather than by algebra or calculus
    * *Example*: $\binom{n}{k} = \binom{n}{n-k}$
        * Think about what this means in terms of choosing k elements rather than calculating factorials
    * *Example*: $n \binom{n-1}{k-1} = k \binom{n}{k}$
        * Imagine we pick k of n people, with one of them designated as the president
        * How many ways can we do this?
        * Approach 1: choose the president, then choose k-1 of n-1 people to form the rest of the group (left side)
        * Approach 2: choose $k$ of $n$ people, then select $1$ of the $k$ people to be the president (right side)
        * Both approaches count the same thing in different ways
    * *Example*: $\binom{m+n}{k}=\Sigma_{j=0}^{k}\binom{m}{j}\binom{n}{k-j}$ (Vandermonde's Identity)
        * Imagine a group of $m$ people and a group of $n$ people, and we want to select $k$ people total from both groups
        * One way is we choose $j$ from group $m$ and $k-j$ from group $n$
        * How many ways are there to choose $j$ from group $m$ and $k-j$ from group $n$?
            * $\binom{m}{j}\binom{n}{k-j}$ ways, using multiplication rule to get total number of ways for this $j$
        * Then, we add those disjoint cases up to get the total number of ways!

<br />

* **Non-Naive Definition of Probability**
    * **Probability Space**: $S$ and $P$, where
        * S is sample space, or the set of all possible outcomes of an experiment
        * P is a function that maps
            * Input (Domain): any event A, which is a subset $A \subseteq S$
            * Output (Range): $P(A) \in [0,1]$ a number between 0 and 1
        * such that
            * $P(\emptyset)=0, P(S)=1$
                * Probability of empty set is 0 and probability of full set is 1
                * What does it mean for the empty set to occur?
                    * If we get an outcome $S_0$ , if $S_0 \in A$, then we say A occurred, else $A$ did not occur
                    * If the empty set occurred, then $S_0 \in \emptyset$ , but nothing is in the empty set, so it cannot occur.
                    * We want impossible events to be impossible $P(\emptyset) = 0$
                * Why is an outcome in S a certainty?
                    * $S_0$ must be in $S$ because $S$ represents the universe of all outcomes
            * $P(\cup_{n=1}^{\infty} A_n) = \Sigma_{n=1}^{\infty}P(A_n), \text{ if } A_1,A_2,\ldots \text{ are disjoint/non-overlapping}$
                * Probability of the countably infinitely many union equals the sum of the probabilities if the events $A_1,A_2,\ldots$ are disjoint/non-overlapping
    * Everything in probability basically derives from this definition with the two constraints


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