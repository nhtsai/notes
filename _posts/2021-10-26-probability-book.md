---
layout: post
title: "Introduction to Probability"
description: "Book notes on probability."
author: "Nathan Tsai"
toc: true
comments: false
hide: false
search_exclude: false
show_tags: true
categories: [book-notes, mathematics, probability, statistics]
permalink: /probability-book
---

# Introduction to Probability by Blitzstein & Hwang
* [Online Book](www.probabilitybook.net)

## 1. Probability and Counting

**Why study probability?**
* Probability is the logic of uncertainty.
* Applications of probability: statistics, physics, biology, computer science, meteorology, gambling, finance, political science, medicine, life.

**Sample spaces and Pebble World**
* An experiment results in a set of outcomes, and the outcome is unknown before the experiment is run.
* **Sample Space**: set of all possible outcomes of the experiment
    * The sample space $S$ can be finite, countably infinite, or un-countably infinite
* **Event**: subset of the sample space
    * Event $A$ *occurred* if the actual outcome of the experiment is in $A$
* If $S$ is finite, we can visualize it as *pebble world*, in which each pebble is an outcome and each event is a set of pebbles
    * Performing an experiment is randomly selecting one pebble
    * If all the pebbles are of the same mass, they are equally likely to be chosen, a special case
* *Example*: let $S$ be the sample space of an experiment and let $A,B \subseteq S$ be events
    * Union $A \cup B$ : the event that occurs if and only if *at least one* of $A, B$ occurs, "OR"
    * Intersection $A \cap B$ : the event that occurs if and only if *both* $A$ and $B$ occur, "AND"
    * Complement $A^C$ : the event that occurs if and only if $A$ does *not* occur, "NOT"
* **De Morgan's Laws**
    * $(A \cup B)^C = A^C \cap B^C$ : it is not the case that at least one of $A, B$ occur is the same as $A$ does not occur and $B$ does not occur
    * $(A \cap B)^C = A^C \cup B^C$ : it is not the case that both $A,B$ occur is the same as at least one $A$ or $B$ does not occur
* *Example*: A coin is flipped 10 times.
    * A possible outcome is $HHHTHHTTHT$. The sample space is all possible strings of length 10 of $H$ and $T$. Let $H=1, T=0$, so the sample space is the set of all sequences in $(s_{1}, \dots, s_{10}), s_{j} \in {0, 1}$
    1. Let $A_{1}$ be the event that the first flip is Heads.  
    $A_{1} = {(1, s_{2}, \dots, s_{10}) : s_{j} \in {0, 1} \text{ for } 2 \leq j \leq 10}$
    This event occurs when the first flip is Heads.

    1. Let $B$ be the event that at least one flip was Heads.
    $B = \bigcup_{j=1}^{10} A_{j}$
    Where $A_{j}$ is the event that the *j*-th flip is Heads for $j = 2, 3, \dots, 10$
    This event is the combination of events where any of the 10 coins being Heads.

    1. Let $C$ be the event that all flips were Heads.
    $C = \bigcap_{j=1}^{10} A_{j}$

    1. Let $D$ be the event there were at least two consecutive Heads.
    $D  \bigcup_{j=1}^{9} (A_{j} \cap A_{j+1})$

* *Example*: Picking a card from a standard deck of 52 cards.
    1. Let $A$ be the event the card is an ace.
    1. Let $B$ be the event the card has a black suit.
    1. Let $D$ be the event the card is a diamond.
    1. Let $H$ be the event the card is a heart.
    * $A \cap H$ is the event the card is ace of hearts.
    * $A \cap B$ is the event the card is a black (spades, clubs) ace.
    * $A \cup D \cup H$ is the event the card is red (hearts, diamonds) or an ace.
    * $(A \cup B)^C = A^C \cap B^C$ is the event the card is not either an ace or black, or not ace and not black, or a red non-ace.
    * $(D \cup H)^C = D^C \cap H^C = B$ is the event the card is not either a diamond or heart, or not diamond and not heart, or black
    * If the card was a joker, then we had the wrong sample space since we assume the outcome of the experiment is guaranteed to an element of the sample space of the experiment.

**Notation**

| English                                       | Sets |
|:--------------------------------------------- |:----:|
| *Events and occurrences*                      | |
| sample space                                  | $S$ |
| $s$ is a possible outcome                     | $s \in S$ |
| $A$ is an event                               | $A \subseteq S $ |
| $A$ occurred                                  | $s_{\text{actual}} \in A$ |
| something must happen                         | $s_{\text{actual}} \in S$ |
| *New events from old events*                  | |
| $A$ or $B$ (inclusive)                        | $A \cup B$ |
| $A$ and $B$                                   | $A \cap B$ |
| not $A$                                       | $A^{C}$ |
| $A$ or $B$ (exclusive)                        | $(A \cap B^{C}) \cup (A^{C} \cap B)$ |
| at least one of $A_{1}, \dots , A_{n}$        | $A_{1} \cup \dots \cup A_{n}$ |
| all of $A_{1}, \dots , A_{n}$                 | $A_{1} \cap \dots \cap A_{n}$ |
| *Relationships between events*                | |
| $A$ implies $B$ | $A \subseteq B$             |
| $A$ and $B$ are mutually exclusive            | $A \cap B = \emptyset$ |
| $A_{1}, \dots , A_{n}$ are a partition of $S$ | $A_{1} \cup \dots \cup A_{n} = S, A_{i} \cap A_{j} = \emptyset \text{ for } i \neq j$ |

**Naive definition of probability**
* The naive probability of an event is the count the number of ways the event could happen divided by total number of possible outcomes for the experiment
    * Restrictive and relies on strong assumptions
* Let $A$ be an event for an experiment with finite sample space $S$. The **naive probability** of event $A$ is:

$P_{\text{naive}}(A) = \frac{\lvert A \rvert}{\lvert S \rvert} = \frac{\text{number of outcomes favorable to } A}{\text{total number of outcomes in } S}$

* In terms of Pebble World, probability of $A$ is the fraction of all pebbles that are in $A$.
* Similarly, the probability of the complement of event $A$ is made of the remaining events:

$P_{\text{naive}}(A^{C}) = \frac{\lvert A^{C} \rvert}{\lvert S \rvert} = \frac{\lvert S \rvert - \lvert A \rvert}{\lvert S \rvert} = 1 - \frac{\lvert A \rvert}{\lvert S \rvert} = 1 - P_{\text{naive}(A)}$

* The naive definition requires $S$ to be finite with equally likely outcomes.
* Can be applied in certain situations:
    * There is *symmetry* in the problem that makes outcomes equally likely. E.g. fair coin or deck of cards.
    * The outcomes are equally likely *by design*. E.g. conducting a survey of $n$ people in a population of $N$ people using a simple random sample to select people.
    * The naive definition serves as a useful *null model*. We *assume* the naive definition just to see what predictions it would yield, and then compare observed data with predicted values to assess whether outcomes are equally likely.

**How to count**
* Calculating probability often involves counting outcomes in event $A$ and outcomes in sample space $S$, but sets are often extremely large to count, so counting methods are used.

* **Multiplication Rule**: If an Experiment A has $a$ possible outcomes, and for each of those outcomes Experiment B has $b$ possible outcomes, then the compound experiment as $ab$ possible outcomes.
    * Each of the $a$ outcomes can lead to $b$ outcomes, so there are $b_{1} + \dots + b_{a} = ab$ possibilities.
    * There's no requirement Experiment A has to be performed before Experiment B, so no chronological order.
* *Example*: Suppose 10 runners in a race, no ties and all 10 will complete the race. How many possibilities are there for first, second, and third place winners?
    * Any of the 10 runners can be in the first place, 9 remaining runners can be in second place, and 8 remaining runners can be in third place.
    * There are $10 \times 9 \times 8 = 720$ possibilities.
    * Could have also considered 10 runners in third place first, order of consideration not important.
* *Example*: How many squares are there in an $8 \times 8$ chessboard?
    * To specify a square, can consider its position within 8 rows and 8 columns.
    * There are $8 \times 8 = 64$ squares on the board.
* *Example*: How many possible ice cream cone combinations if there are 2 types of cones and 3 types of flavors?
    * There are $2 \times 3 = 3 \times 2 = 6$ possibilities.
    * Order of choice doesn't matter, can either choose cone then flavor, or flavor then cone.
    * Which flavors each cone could have doesn't matter, only the *number of flavor choices* for each cone matters. If the waffle cones could only have 2 flavors, multiplication rule does not apply and there are $3 + 2 = 5$ possibilities.
    * If you bought 2 ice creams in a single day $(x, y)$, there would be $6 \times 6 = 36$ possible combinations.
    * If you didn't want to distinguish between combinations $(x,y) = (y,x)$ , then there are 15 $(x, y), x \neq y$ possibilities and 6 $(x,x)$ possibilities, for a total of 21 possibilities.
        * Cannot just divide $36 / 2 = 18$ ! Since $(x,x)$ pairs are already only listed once each, must do $((6 \times 5) / 2) + 6 = 21$ possibilities.
    * If the original 36 cone-flavor pairs are equally likely, then the 21 possibilities are not equally likely.
        * Counting $(x,y) = (y, x)$ doubles likeliness, but $(x,x)$ pairs stay the same.
* *Example*: A set of $n$ elements has $2^{n}$ subsets, including the empty set $\emptyset$ and the set itself.
    * From the multiplication rule, we can choose to include or exclude each element of the set.
    * Multiplication rule: think of each number consideration as an experiment with 2 outcomes: include or exclude.
    * $\{1, 2, 3\}$ has 8 subsets: $\emptyset, \{1\}, \{2\}, \{3\}, \{1,2\}, \{1,3\}, \{2,3\}, \{1,2,3\}$
        * Consider first element 1, then consider second element 2, and then consider last element 3.
        * $\emptyset$ is (exclude, exclude, exclude).
        * $\{3\}$ is (exclude, exclude, include).
        * Order of consideration does not matter; could have considered element 3 first.

* Multiplication rule counting principle is used for problems of sampling with and without replacement.
* **Sampling with Replacement**: Consider $n$ objects and making $k$ choices from them, one at a time *with replacement*, then there are $n^{k}$ possible outcomes, where order matters $(x,y) \neq (y,x)$.
* *Example*: Jar with $n$ balls, labeled from 1 to $n$.
    * Choosing a ball is an experiment with $n$ outcomes, and there are $k$ experiments.
    * Therefore, multiplication rule says there are $n_{1} \times n_{k} = n^{k}$ possibilities.

* **Sampling without Replacement**: Consider $n$ objects and making $k$ choices from them, one at a time *without replacement*, then there are $n(n-1) \dots (n-k+1)$ possible outcomes, where order matters, for $1 \leq k \leq n$.
    * There are 0 possibilities, where order matters, for $k > n$. You cannot choose more than the amount available.
    * There are $n(n-1) \dots (n-k+1) = n$ possible outcomes for $k=1$.
    * Multiplication rule: each sampled object is an experiment and the the number of outcomes decreases by 1 each time.
* *Example*: A **permutation** of $1, 2, \dots, n$ is an arrangement of the $n$ elements in some order.
    * Sampling without replacement: if $k=n$, there are $n!$ permutations of $n$ elements, or ways to choose $n$ items without replacement
* We can use sampling with and without replacement as probabilities when the naive definition of probability applies.
* *Example*: (**Birthday Problem**) There are $k$ people in a room. Assume each person's birthday is equally likely to be any of the 365 days of the year, and people's birthdays are independent, e.g. no twins. What is the probability that at least one pair of people in the group have the same birthday?
    * If we sample with replacement $k$ days from $n = 365$ total days, what is the probability at least two samples are the same?
        * There are $365^{k}$ possible outcomes.
        * But how can we count all the ways that two more more people have the same birthday?
    * Instead, think about the complement: the number of ways to sample without replacement $k$ different days of $n = 365$ total days. What is the probability that no two people share the same birthday?
        * $P(\text{no match}) = \frac{365 \times 364 \times \dots \times (365 - k + 1)}{365^{k}}$
    * Therefore, the probability of at least one birthday match is the complement.
        * $P(\geq 1\text{ match}) = 1 - \frac{365 \times 364 \times \dots \times (365 - k + 1)}{365^{k}}$
    * At $k = 23$, there is over a 50% chance that two people share the same birthday, and over a 99% chance at $k = 57$.
    * Intuition: For $k = 23$ people, there are $\binom{23}{2} = 253$ *pairs* of people, any of which could be a birthday match.
* Think of the objects or people in the population as *named* or *labeled*, rather than indistinguishable.
* *Example*: If we roll two fair dice, is a sum of 11 or sum of 12 more likely?
    * Label dice A and B and consider each die to be a sub-experiment. Multiplication rule says there are $6 \times 6 = 36$ possibilities, in the form $(a,b)$.
    * 11 can be made from $(5,6), (6,5)$, while 12 can only be made from $(6,6)$. Therefore, a sum of 11, which has a probability of $\frac{2}{38} = \frac{1}{18}$, is twice as likely as a sum of 12, which has a probability $\frac{1}{36}$.

* **Overcounting**: It is difficult to directly count each possible outcome once, and we may need to *adjust for overcounting* by dividing the count by some factor $c$.
* *Example*: For a group of four people, how many ways are there to
    * Choose a two-person group?
        * Labeling people as $1, 2, 3, 4$, we can see possible groups are $\{12, 13, 14, 23, 24, 34\}$, so there are 6 possible ways.
        * Multiplication rule and adjust for overcounting: There are 4 ways to choose the first person, and 3 ways to choose the second person, or $4 \times 3 = 12$. But since $(a,b) = (b,a)$, we have overcounted by 2, so there are $(4 \cdot 3) / 2 = 6$ possible ways.

    * Break people into two teams of two?
        * Labeling people as $1, 2, 3, 4$, we can see possible groups are $(12, 34), (13, 24), (14, 23)$, so there are 3 possible ways.
        * We can also note that choosing a team of 2  (choosing person 1's partner) determines other team to be formed from remaining people. There are 3 other people to pair to person 1, so there are 3 possible ways.
        * We can also see that the 6 possible ways to choose a two-person group is overcounting by a factor of 2, since the other team is determined from choosing one team $(a,b) = (c,d)$, so there are $6 / 2 = 3$ possible ways.

* **Binomial Coefficient**: For any nonnegative integers $k,n$, the binomial coefficient $\binom{n}{k}$ ("n choose k") is the number of subsets of size $k$ for a set of size $n$. E.g. The number of ways to choose a group of size $k = 2$ from $n = 4$ people is $\binom{4}{2} = 6$.
    * Sets are *unordered* by definition $(a,b) = (b,a)$, so a binomial coefficient counts the number of ways to choose $k$ objects out of $n$, *without replacement* and *without distinguishing between different orders*.
    * For $k \leq n$, $\binom{n}{k} = \frac{n (n-1) \dots (n-k+1)}{k!} = \frac{n!}{(n-k)!k!}$.
    * For $k > n, \binom{n}{k} = 0$.
* *Proof*: Let $A$ be set with $\lvert A \rvert = n$. Any subset of $A$ has size at most $n$, so $\binom{n}{k} = 0$ for $k > n$, or no ways to create a larger subset than what's available. If $k \leq n$, then sampling without replacement tells us there are $n(n-1) \dots (n-k+1)$ ways to make *ordered* choice of $k$ elements without replacement. But this overcounts each subset by $k!$ since order does not matter for binomial coefficient $(a,b) = (b,a)$, so we divide by $k!$ to get $\binom{n}{k} = \frac{n (n-1) \dots (n-k+1)}{k!} = \frac{n!}{(n-k)!k!}$.
* *Example*: In a club of $n$ people, how many ways are there to choose 3 officers?
    * There are $\binom{n}{3} = \frac{n(n-1)(n-2)}{3!}$ possible ways.
* *Example*: How many ways are there to permute the letters in the word "LALALAAA"?
    * We can just decide the positions of either the 3 L's or the 5 A's. So there are $\binom{8}{3} = \binom{8}{5} = \frac{8 \cdot 7 \cdot 6}{3!} = 56$ possible permutations.
* *Example*: How many ways are there to permute the letters in the word "STATISTICS"?
    * We can choose positions for 3 S's, 3 T's, 2 I's, 1 A, and 1 C is determined. So there are $\binom{10}{3}\binom{7}{3}\binom{4}{2}\binom{2}{1} = 50400$ permutations.
    * We can also start with $10!$ permutations and account for overcounting of a factor of $3!3!2!$ for the 3 S's, 3 T's, and 2 I's because the repeated letters can be permuted among themselves in any way. So there are $\frac{10!}{3!3!2!} = 50400$ permutations.
* **Binomial Theorem**: For any nonnegative integer $n$, $(x + y)^{n} = \sum_{k=0}^{n} \binom{n}{k} x^{k} y^{n-k}$.
* *Proof*: $(x + y)_{1} (x + y)_{2} \dots (x + y)_{n}$
    * The terms of $(x + y)^{n}$ are obtained by picking either the $x$ or the $y$ from each factor $(x + y)$. This is similar to $(a + b)(c + d) = ab + ac + bc + bd$, where each product term is created from only one term of each factor.
    * There are $\binom{n}{k}$ ways to choose exactly $k$ of the $x \text{'s}$ to make the term $x^{k} y^{n-k}$.

**Story Proofs**


**Non-naive definition of probability**


**Recap**


## 2. Conditional Probability

##

## References
[^1]: Footnote