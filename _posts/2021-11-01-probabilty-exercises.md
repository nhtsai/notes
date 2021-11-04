---
layout: post
title: "Introduction to Probability Exercises"
description: "Solutions to Exercises in Introduction to Probability."
author: "Nathan Tsai"
toc: true
comments: false
# image: 
hide: true
search_exclude: false
show_tags: true
# sticky_rank: 1
categories: [mathematics, probability, statistics]
permalink: /probability-exercises
---

# Introduction to Probability 2ed. by Blitzstein & Hwang
View the book [here](https://www.probabilitybook.net) or [here](https://drive.google.com/file/d/1VmkAAGOYCTORq1wxSQqy255qLJjTNvBI/view).

## Chapter 1

### Counting
1. There are $11!$ total permutations. Accounting for overcounting of repeated letters that can be permuted in any order ('S', 'I', 'P'), we need to divide by $4! \times 4! \times 2!$. We find there are $\frac{11!}{4!4!2!}$ permutations. We can also think of this in terms of a binomial coefficient, choosing positions for letters, to find $\binom{11}{4} \binom{7}{4} \binom{3}{2}$ permutations.
1. 
    1. For the first digit, there are 8 possible digits (2-9). For the remaining six digits, there are 10 possible digits (0-9). Using the multiplication rule, there are $8 \times 10^{6}$ numbers.
    1. We need to find the number of 7-digit phone numbers that cannot start with 0 or 1 in the first digit and cannot start with 911. We can subtract the complement from the total, or excluding all numbers that start with 0 or 1 or 911. We find there are $10^{7} - (2 \times 10^{6}) - (1 \times 1 \times 1 \times 10^{4})$ numbers.
1. 
    1. We can use the multiplication rule to get $5 \times 4 \times 3 \times 2 \times 1$ possibilities, where Fred must eat a new restaurant each day.
    1. We can use the multiplication rule to get $5 \times 4 \times 4 \times 4 \times 4$ possibilities, where Fred can eat at any restaurant on Monday and restaurants excluding the one from the day before on subsequent days.
1. 
    1. For one game, there are $2$ possible outcomes. There are $2^{\binom{n}{2}}$ possible outcomes.
    1. We can think of this as the number of ways to pick 2 players with replacement, where order does not matter. Using binomial coefficients, we find $\binom{n}{2}$ outcomes, one for each match.
1. 
    1. There are $2^{n}$ players with half of the players eliminated per round, so there are $\log_{2}2^{n} = n$ rounds.
    1. We add up the number of games played each round to find $\sum_{i=0}^{n - 1} 2^{i}$ total games played.
    1. Thinking about how $16$ total players results in $8 + 4 + 2 + 1 = 15$ games played, we find that each game eliminates one player. So if we need to eliminate all but one player, we find the pattern that there are $2^{n} - 1$ games played.
1. 
    1. We can use binomial coefficients to consider which of the 7 games have which results (3 wins, 2 draws, 2 losses) to find $\binom{7}{3} \binom{4}{2}$ games. We can also think of all possible outcomes given 3 wins, 2 draws, and 2 losses, and account for the overcounting of permutation of same game results to find $\frac{7!}{3!2!2!}$ games.
1. 
    1. We can use binomial coefficient to choose the team of 2 and a team of 5, where the order within teams does not matter, to find $\binom{12}{2} \binom{10}{5}$ teams. The last team is determined by choosing the other two teams. The order of choice does not matter $(2, 5, 5) = (5, 5, 2)$. We can also think of all orderings and grouping the people into teams by their ordering, i.e. the first 3, next 5, and last 5. Accounting for overcounting because ordering within teams does not matter, we find $\frac{12!}{3!5!5!}$ teams.
    1. Similarly, we can use the binomial coefficient to find $\binom{12}{4} \binom{8}{4}$ teams, or consider all orderings and account for overcounting to find $\frac{12!}{4!4!4!}$ teams.
1. 
    1. We need to take 110 steps up and 111 steps right for a total of 221 moves. We can use the binomial coefficient to choose the up moves, which will determine the right moves, to find $\binom{221}{110} = \binom{221}{111}$ paths. We can also consider all possible paths and account for overcounting because same moves can be permuted in any order to find $\frac{221!}{110! 111!}$ moves.
    1. We need to make 210 up moves and 211 right moves, with fixing the path through (110, 111). We can consider the first 221 moves to reach (110, 111), then count the remaining 110 up moves and 100 right moves to reach (210, 211) to find $\binom{221}{110} \times \binom{200}{100}$ paths. We can also think of all possible separate paths and overcounting to find $\frac{221!}{110! 111!} \times \frac{200!}{100! 100!}$ paths.
1. 
    1. We can consider the complement of total possible choices minus the course choices that have no statistics courses to find $\binom{20}{7} - \binom{15}{7} = 71085$ possible valid course choices. We can also consider each case of a student choosing 1 to 5 statistics courses to find $\binom{5}{1} \binom{15}{6} + \binom{5}{2} \binom{15}{5} + \binom{5}{3} \binom{15}{4} + \binom{5}{4} \binom{15}{3} + \binom{5}{5} \binom{15}{2} = 71085$ possible valid course choices.
    1. The answer is not $\binom{5}{1} \binom{19}{6} = 135660$ course choices because this method overcounts the course choices with the same statistics courses. For example, the two course choices $(STAT1, STAT2, A, B, C, D) \text{ and } (STAT2, STAT1, A, B, C, D)$ are equivalent, which is an example of overcounted course choices.

1. 
    1. In this case, there can be many elements of A that map to an element of B. We can think of this as the number of ways to pair an element from A to every element in B to find $n \times m$ functions.
    1. A one-to-one function is a function in which for each element in B, there can be at most one element in A that maps to it. We can think of this as the number of ways to 

## Story Proofs
15. Imagine you have $n$ items and want to find all subsets. You start with the empty set, and for each new item, you create a new subset by appending it to an existing subset. The number of subsets doubles each time because adding a new item creates a new subset from an existing subset, leading to $2^{n}$ subsets.
<!-- 1. 
    1. For all positive integers $n,k$ with $n \geq k$:
        $$
        \begin{aligned}
        \binom{n}{k} + \binom{n}{k-1} &= \binom{n+1}{k} \\
        \frac{n!}{(n-k)! k!} + \frac{n!}{(n-k+1)! (k-1)!} &= \frac{(n+1)!}{(n + 1 - k)! k!} \\
        \frac{n (n-1) \dots (n-k+1)}{k!} + \frac{n (n-1) \dots (n-k+2)}{(k-1)!}
        \end{aligned}
        $$ -->






## References
[^1]: Footnote