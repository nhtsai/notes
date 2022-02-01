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
permalink: /harvard-stat110
---

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

<br />

* **Naive Definition of Probability**
    * $ P(A)=\frac{\text{favorable outcomes}}{\text{total possible outcomes}} $
        * The number of outcomes where Event A occurs over all possible outcomes
    * *Example*: Flipping a fair coin twice, what is probability that both flips are heads?
        * 1 favorable outcome: $(HH)$
        * 4 total outcomes: $(HH, HT, TH, TT)$
        * Therefore, $P(\text{both heads}) = \frac{1}{4}$
    * Assumes that *all outcomes are equally likely* and a *finite sample space*
        * If the coin was not fair, the outcomes would not be equally likely
        * If the sample space is infinite, the probability definition doesn't make sense

<br />

* **Multiplication Rule**
    * If we have an experiment with $n\_{1}$ possible outcomes, and for each outcome of $1\text{st}$ experiment, there are $n\_{2}$ possible outcomes for the $2\text{nd}$ experiment, $\ldots$, and for each outcome of $(r-1)\text{th}$ experiment, there are $n\_{r}$ possible outcomes for the $r\text{th}$ experiment, then there are $n\_{1} * n\_{2} * \ldots * n\_{r}$ overall possible outcomes
    * The total number of possible outcomes of a series of experiments is the *product of each experiment's outcomes*

    * *Example*: 3 flavors of ice cream and 2 types of cones
        * Ice cream flavor experiment has 3 outcomes (chocolate, vanilla, strawberry)
        * Waffle cone experiment has 2 outcomes (cake, waffle)
        * There are $6 \text{ possible outcomes}= 2 \times 3 = 3 \times 2$
            * Order of cone/ice cream choice does not matter

<br />

* **Binomial Coefficient**
    * $\binom{n}{k}=\frac{n!}{(n-k)!k!}, 0 \text{ if } k>n$
    * The number of possible subsets of size k given n elements, where *order does not matter*
        * Sets must contain distinct elements, so *without replacement*
        * Pronounced "n choose k"

    * *Intuition*: From the multiplication rule
        * Choose k elements from n elements: $n(n-1)(n-2) \ldots (n-k+1)$
        * Order does not matter: divide by $k!$ because we over-counted by that factor
            * Consider $\binom{5}{2}$ , we want to count $(1, 2)$ and $(2, 1)$ as equivalent, not separately
            * There are $k!$ possible orderings of a size k subset, so we divide by $k!$ to reduce it down to 1
        * $\therefore \binom{n}{k} = \frac{n(n-1)(n-2) \ldots (n-k+1)}{k!}=\frac{n!}{(n-k)!k!}$

    * *Example*: Find probability of full house in poker, 5 card hand
        * A *full house* is 3 cards of 1 rank and 2 cards of another rank, e.g. 3 sevens and 2 tens
        * Assume deck is randomly shuffled so that all sets of 5 cards are equally likely
            * Because we assume cards are evenly shuffled, we can justify using naive definition
        * Number of possible hands: $\binom{52}{5}$ , "52 choose 5"
        * Favorable hands (full house): $\binom{13}{1} \binom{4}{3} \times \binom{12}{1} \binom{4}{2}$
            * Of 13 ranks choose 1 and of 4 cards need 3, then of 12 remaining ranks choose 1 and of 4 cards we need 2
        * $\therefore \frac{\binom{13}{1} \binom{4}{3} \times \binom{12}{1} \binom{4}{2}}{\binom{52}{5}} = \frac{13 \binom{4}{3} \times 12 \binom{4}{2}}{\binom{52}{5}}$

<br />

* **Sampling Table**
    * From the multiplication rule, we can determine how many ways there are to choose $k$ elements out of $n$ total elements
    * We can choose to sample **with replacement** (we can choose the same element again) or **without replacement** (we cannot choose the same element again)

    |                         | Order Matters | Order Doesn't Matter |
    |------------------------:|:-------------:|:--------------------:|
    | **With Replacement**    | $n^k$         | $\binom{n+k-1}{k}$   |
    | **Without Replacement** | $nPk = \frac{n!}{(n-k)!}$ | $\binom{n}{k} = \frac{n!}{(n-k)!k!}$ |

    * *Sample Method*: With replacement, order matters
        * $n\_{1} \times n\_{2} \times \ldots \times n\_{k} = n^k$
        * Have $k$ experiments, each with $n$ outcomes and order matters, so $\{1, 2\} \neq \{2, 1\}$

    * *Sample Method*: Without replacement, order matters
        * $n \times (n-1) \times \ldots \times (n-k+1)$
        * Have $k$ experiments, with $n$ outcomes without replacement and order matters, so $\{1, 2\} \neq \{2, 1\}$

    * *Sample Method*: Without replacement, order does not matter (**Binomial Coefficient**)
        * $\frac{n \times (n-1) \times \ldots \times (n-k+1)}{k \times (k-1) \times \ldots \times 1} = \frac{n!}{(n-k)! \times k!} = \binom{n}{k}$
        * Have $k$ experiments, with $n$ outcomes without replacement and order does not matter, so $\{1, 2\} = \{2, 1\}$
        * Want 1 combination of $k$ elements to represent all $k!$ permutations with the same $k$ elements, so need to divide by $k!$
        * Think of grouping all possible permutations with the same $k$ elements into $k!$ groups
            * For the $k=3$ choice $(1,2,3)$, there are $3!$ orderings that we want to group into 1 representation $\{1,2,3\}$
            * To do this grouping for all different choices, we divide by $k!$ so that we're left with the number of unique combinations of $k=3$ elements

    * *Sample Method*: With replacement, order does not matter (**Bose-Einstein**)
        * $\frac{(n+k-1)!}{(n-1)! \times k!} = \binom{n+k-1}{n-1} = \binom{n+k-1}{k}$
        * Hardest to understand, not obvious from multiplication rule
        * Have $k$ experiments, with $n$ outcomes and order does not matter, so $(1, 2) = (2, 1)$


## 2. Story Proofs, Axioms of Probability

* General Advice for Homework
    * Don't lose common sense
    * Do check answers, especially by doing simple and extreme cases
    * Label people, objects, etc. (If we have n people, label them 1, 2, ..., n)

<br />

* **Bose-Einstein**
    * Bose said flipping a fair coin twice had 3 equally likely outcomes $(HH, TT, 1H+1T)$
    * Bose thought of the two coin flips as indistinguishable $(HT = TH)$, rather than $(HT, TH)$
    * Thinking of coins as indistinguishable led to the discovery Bose-Einstein condensate
    * *Sample Method*: Pick k times from a set of n objects, where order does not matter and with replacement
        * Prove the answer is $\binom{n+k-1}{k}$ ways
    * *Case*: if $k=0, \binom{n-1}{0} = 1$ 
        * Only 1 way to choose 0 elements from $n-1$ elements, choose none!
    * *Case*: if $k=1, \binom{n}{1} = n$
        * $n$ ways to choose 1 of $n$ elements
        * When only picking 1 element, no difference if with/without replacement or if order matters or does not matter
    * *Case*: if $n=2, \binom{k+1}{k} = \binom{k+1}{1} = k+1$
        * They are equal because: If we choose $k$ of $k+1$ elements, then the remaining is the 1. If we choose 1, then the remaining is the $k+1$
        * If we get the first element $x$ times, we know we got the second element $k-x$ times
        * Number of possible first elements we get is from $\{0, \ldots, k\}$ , so there are $k+1$ possibilities of choosing $k$ from $n=2$ elements
        * Can think of this as flipping a fair coin twice

    * *Equivalent*: How many ways are there to put k indistinguishable elements into n distinguishable groups?
        * *indistinguishable* here means order does not matter and with replacement
        * Say $n=4, k=6$, a possible way is $(***),(),(**),(*)$
        * We can also represent groups using separators: $( *** \mid \mid ** \mid * )$
        * There must be $k$ stars and $n-1$ separators, we just need to choose $k$ positions for stars in $n+k-1$ total positions, or $\binom{n+k-1}{k} = \binom{n+k-1}{n-1}$

<br />

* **Story Proof**
    * Proof by interpretation, rather than by algebra or calculus
    * *Example*: $\binom{n}{k} = \binom{n}{n-k}$
        * Think about what this means in terms of choosing k elements rather than calculating factorials
    * *Example*: $n \binom{n-1}{k-1} = k \binom{n}{k}$
        * Imagine we pick k of n people, with one of them designated as the president
        * How many ways can we do this?
        * Approach 1: choose the president, then choose k-1 of n-1 people to form the rest of the group (left side)
        * Approach 2: choose $k$ of $n$ people, then select $1$ of the $k$ people to be the president (right side)
        * Both approaches count the same thing in different ways
    * *Example*: $\binom{m+n}{k}=\sum\_{j=0}^{k}\binom{m}{j}\binom{n}{k-j}$ (Vandermonde's Identity)
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
                    * If we get an outcome $S\_{0}$ , if $S\_{0} \in A$, then we say A occurred, else $A$ did not occur
                    * If the empty set occurred, then $S\_{0} \in \emptyset$ , but nothing is in the empty set, so it cannot occur.
                    * We want impossible events to be impossible $P(\emptyset) = 0$
                * Why is an outcome in S a certainty?
                    * $S\_{0}$ must be in $S$ because $S$ represents the universe of all outcomes
            * $P(\cup\_{n=1}^{\infty} A\_{n}) = \sum\_{n=1}^{\infty}P(A\_{n}), \text{ if } A\_{1}, A\_{2}, \ldots \text{ are disjoint/non-overlapping}$
                * Probability of the countably infinitely many union equals the sum of the probabilities if the events $A\_{1}, A\_{2}, \ldots$ are disjoint/non-overlapping
    * Everything in probability basically derives from this definition with the two **Axioms of Probability**


## 3. Birthday Problem, Properties of Probability

* **Birthday Problem**
    * In a group of people, how likely is it that at least one pair of people share the same birthday?
    * How many people do you need for a 50% chance two people have the same birthday?
        * $K$ people, find probability that 2 have the same birthday
        * Exclude Feb 29, assume other 365 days are equally likely (use naive definition of probability)
        * Assume independence of births, one person's birthday has no effect on another person's birthday
    * **Pigeonhole Principle**: If there are more people than days in a year, then the probability two people share the same birthday is certain
        * If $K > 365$, then probability is $1$
        * Imagine 365 boxes, if there are more than 365 dots, at least one box must have more than one dot
    * Intuition says if probability of 1 for > 365 people, then to get probability of 0.5, we need around 180 people.
        * The answer is 23 people for 50.7% chance two people share the same birthday
    * Let $K <= 365$
    * Probability of no match (0 pairs share a birthday)
        * Each of K persons has $\frac{1}{365}$ chance of a particular birthday
        * If no matches, every person must have a different birthday (no replacement)
        * $P(\text{no match}) = \frac{365 \times 364 \times \ldots \times (365 - K + 1)}{365^{K}}$
    * Probability of match (at least 1 pair shares a birthday)
        * Complement: $P(\geq 1 \text{ match}) = 1 - P(\text{no match})$
        * If $K=23$, $P(\text{match}) = 1 - \frac{365 \times 364 \times \ldots \times 343}{365^{23}} \approx 1 - 0.492703 \approx 0.507297 \approx 50.7\%$
        * If $K=50$, $P(\text{match}) = 1 - \frac{365 \times 364 \times \ldots \times 316}{365^{50}} \approx 97\%$
        * If $K=100$, $P(\text{match}) = 1 - \frac{365 \times 364 \times \ldots \times 343}{365^{100}} \geq 99.999\%$
    * How could we get a birthday match with such low number of people?
        * The more relevant quantity is the number of pairs in K people
            * $\binom{K}{2} = \frac{K(K-1)}{2}$
            * For $K=23$, $\binom{23}{2} = \frac{23 * 22}{2} = 23 * 11 = 253 \text{ pairs}$
            * 253 is a lot of pairs, and any one of those pairs may share a birthday
    * If we then consider what K to have 50% chance of finding a pair with same birthday or off by 1 day, the answer is $K=14$
        * With 14 people, there is a better than 50% chance to find a pair of people with the same birthday or off by 1 day
        * More complicated problem to calculate

<br />

* **Axioms of Probability**
    1. $P(\emptyset) = 0, P(S) = 1$
        * Probability of empty set is 0 (impossible)
        * Probability of full sample space is 1 (certain)
    1. $P(\cup\_{n=1}^{\infty} A\_{n}) = \sum\_{n=1}^{\infty} P(A\_{n})$
        * If $A\_{1}, A\_{2}, \ldots \text{ are disjoint events}$ 
        * Probability of the union (infinite or finite) equals the sum of probabilities of all events if events are disjoint.
        * Think of probabilities as disjoint sets, union of sets is same as adding the sets up
    * Assume $0 \leq P(A) \leq 1$ , probability is between 0 and 1
    * As long as a thing satisfies both axioms, we can apply any theorems of probability to the thing.
    * From these 2 axioms, we can derive every theorem of probability.

<br />

* **Properties of Probability**
    1. $P(A^C) = 1 - P(A)$
        * Probability of complement of A is 1 minus probability of A
        * Proof:
            * $1 = P(S) = P(A \cup A^C)$
            * $1 = P(A) + P(A^C)$ , since $A \cap A^C = \emptyset$
                * A and its complement are disjoint, nothing can be in both by definition
            * Therefore, $P(A^C) = 1 - P(A)$
    1. If $A \in B$ , then $P(A) \leq P(B)$
        * A and B are events, A is *in* B, "if A occurs then B occurs"
        * Proof:
            * $B = A \cup (B \cap A^C)$ , where $A, (B \cap A^C)$ are disjoint
                * B is made of of A and everything in B that's not in A
            * $P(B) = P(A) + P(B \cap A^C) \geq P(A)$
                * From axiom #2, adding some non-negative probability so must be $\geq$
    1. $P(A \cup B) = P(A) + P(B) - P(A \cap B)$
        * To get a probability of A union B, we can sum probabilities if A and B are disjoint.
        * Otherwise, if we add non-disjoint events A and B, we double count the overlapping intersection
        * Simple case of **inclusion-exclusion** with 2 events
        * Proof:
            * *Technique*: "disjointification" of A and B so that we can apply axiom #2
            * $P(A \cup B) = P(A \cup (B \cap A^C))$
                * Union of A and B equals union of A and everything in B that's not in A
            * $P(A \cup B) = P(A) + P(B \cap A^C)$
                * Since $A, (B \cap A^C)$ are disjoint and we can apply axiom #2
            * *Technique*: "wishful thinking" to see if two sides are equal
            * $P(A) + P(B) - P(A \cap B) \eqsim P(A) + P(B \cap A^C)$
            * $P(B) \eqsim P(A \cap B) + P(B \cap A^C)$
                * $A \cap B, A^C \cap B$ are disjoint because no elements can be in both $A, A^C$
                * Using axiom #2, $P(B) = P((A \cap B) \cup (B \cap A^C))$ 
                * B is equal to the part of B in A and part of B not in A

<br />

* **General Inclusion-Exclusion**
    * Finds the probability of a union
    * Alternates inclusion and exclusion of portions
    * *Case*: Inclusion-Exclusion for 3 events
        * $P(A \cup B \cup C) = P(A) + P(B) + P(C) - P(A \cap B) - P(A \cap C) - P(B \cap C) + P(A \cap B \cap C)$
        * Add all 3 events, subtract the intersections of pairs, then add the intersection of all
    * *Case*: Inclusion-Exclusion for N events
        * $P(A\_{1} \cup A\_{2} \cup \ldots \cup A\_{N}) = \sum^{N}\_{j=1} P(A\_{j}) - \sum^{N}\_{i \lt j} P(A\_{i} \cap A\_{j}) + \sum^{N}\_{i \lt j \lt k} P(A\_{i} \cap A\_{j} \cap A\_{k}) - \ldots + (-1)^{N+1} \times P(A\_{1} \cap A\_{2} \cap \ldots \cap A\_{N})$

<br />

* *Example*: **Matching Problem** / **de Montmort's Problem (1713)**
    * Problem originated from gambling problems:
        * Imagine $N$ cards, labeled from 1 to N
        * Shuffle the cards
        * Flip over the cards while counting from 1 to N
        * If the count *matches* the flipped card's number, then win
    * What's the probability that one card has the same number as its position in the deck?
    * Let $A\_{j}$ be the event "jth card matches"
    * $P(\geq 1 \text{ match}) = P(A\_{1} \cup A\_{2} \cup \ldots \cup A\_{N})$
    * $P(A\_{j}) = \frac{1}{N}$
        * Probability that card's number matches position is 1 over N since all positions are equally likely for the card labeled j
        * Naive definition uses $\frac{(N-1)!}{N!} = \frac{1}{N}$
            * $N!$ possible permutations of deck of cards
            * $(N-1)!$ possible permutations of the deck of cards, fixing card labeled j is at jth position
    * $P(A\_{1} \cap A\_{2}) = \frac{(N-2)!}{N!} = \frac{1}{N(N-1)}$
        * $N!$ possible permutations of deck of cards
        * $(N-2)!$ possible permutations of the deck of cards, fixing card labeled i at ith position and card labeled j at jth position
    * $P(A\_{1} \cap \ldots \cap A\_{K}) = \frac{(N-K)!}{N!}$
        * $N!$ possible permutations of deck of cards
        * $(N-K)!$ possible permutations of the deck of cards, fixing $K$ cards at their corresponding positions
    * *Technique*: Apply Inclusion-Exclusion
        * $P(A\_{1} \cup A\_{2} \cup \ldots \cup A\_{N}) = N (\frac{1}{N}) - (\frac{N(N-1)}{2!})(\frac{1}{N(N-1)}) + (\frac{N(N-1)(N-2)}{3!})(\frac{1}{N(N-1)(N-2)}) - \ldots$
        * Include $N$ 1-matches, subtract $\binom{N}{2}$ 2-matches, add ..., subtract ...
        * Everything cancels!
        * $P(A\_{1} \cup A\_{2} \cup \ldots \cup A\_{N}) = \frac{1}{1} - \frac{1}{2!} + \frac{1}{3!} - \frac{1}{4!} + \frac{1}{5!} - \ldots + (-1)^{N+1} \times (\frac{1}{N!})$
        * This is the Taylor series for $e^x$ .
        * $P(A\_{1} \cup A\_{2} \cup \ldots \cup A\_{N}) \approx 1 - \frac{1}{e}$


## 4. Conditional Probability I

* *Example*: **Matching Problem** / **de Montmort's Problem (1713)**
    * Most famous example of *inclusion-exclusion*
        * Deck of N cards, labeled from 1 to N
        * Random shuffle and flip through deck
        * Win if a card's label matches its position
    * *Case*: $A\_{j}$ , the jth card in the deck is labeled j
        * $P(A\_{j}) = \frac{1}{N}$ : for the jth card, there are N possible positions in the deck
    * *Case* $P(\cup\_{j=1}^{N} A\_{j})$ , the probability of a match on any card j
        * To apply inclusion-exclusion we need:
            * For any $K$ cards, $P(A\_{1} \cap A\_{2} \cap \ldots \cap A\_{K}) = \frac{(N-K)!}{N!}$
                * If K cards match their positions, then we fix those cards in place
                * The rest of the $N-K$ cards can be in any order, but the $K$ cards are constrained
            * There are $\binom{N}{K} = \frac{N!}{(N-K)!K!}$ such terms like this because the inclusion-exclusion does $P(\cap\_{j=1}^{K} A\_{j})$ for any $K$
            * These terms are all the same by **symmetry**.
            * Therefore, $P(\text{match}) = P(\cup\_{j=1}^{N} A\_{j}) = 1 - \frac{1}{2!} + \frac{1}{3!} - \ldots (-1)^{N+1} \times \frac{1}{N!}$
                * $\frac{1}{N!}$ is the case where the cards are perfectly ordered from 1 to N, all cards match, which is 1 of $N!$ possible orderings
        * $P(\text{no match}) = P(\cap\_{j=1}^{N} A^{C}\_{J}) = 1 - (1 - \frac{1}{2!} + \frac{1}{3!} - \ldots (-1)^{N+1} \times \frac{1}{N!}) \approx \frac{1}{e}$
            * Complement of union is the intersection of those complements
            * The factorials in denominators should remind you of Taylor series of $e^x$
    * If there were an infinite amount of cards, what is $P(\text{no match})$ ?
        * The chance a card is in the exact position is very unlikely, but there are so many chances in an infinite deck.
        * The competing forces somehow reduce the answer to $\frac{1}{e}$ .

<br />

* **Independent Events**
    * *Case*: Two Events A, B are *independent* if $P(A \cap B) = P(A) \times P(B)$
        * The probability that both A and B occurs is the product of probability of A and probability of B
        * They're independent, so we just multiply
    * *Important*: Independence is completely different from **disjointness**
        * **Disjointnesss** says if A occurs, B *cannot* occur
    * *Case*: Three Events A, B, C are *independent* if $P(A \cap B) = P(A)P(B), P(A \cap C) = P(A)P(C), P(B \cap C) = P(B)P(C), P(A \cap B \cap C) = P(A)P(B)P(C)$
        * The pairwise independence does not imply independence of all 3 events, so we need the last equation too
    * *Case*: N Events
        * Follows the same pattern
        * Any 1, 2, 3, ..., N events need to be *independent*
    * *Basic Rule*: "independent means multiply" from multiplication rule

<br />

* *Example*: **Newton-Pepys Problem (1693)**
    * Samuel Pepys wanted to know a gambling problem solution, and Newton solved it for him.
    * Problem
        * Have some fair dice, each with 6 equally-likely sides, independent from each other
        * Which of these events is more likely?
            * Event A: at least 6 with 6 dice
            * Event B: at least two 6's with 12 dice
            * Event C: at least three 6's with 18 dice
    * Pepys strongly believed Event C was more likely, but Newton solved the answer was Event A.
    * Can use naive definition but we will use independence to solve.
        * Seeing "at least 1" should signal a union, but a complement of union is an intersection
        * Intersection of independent events signals multiply!
    * Probability of Event A
        * Take complement: 1 minus probability that all dices are not 6
        * $P(A) = 1 - (\frac{5}{6})^6 \approx 0.665$
    * Probability of Event B
        * Take complement: 1 minus probability of no 6's minus probability of exactly one 6
        * $P(B) = 1 - (\frac{5}{6})^6 - 12(\frac{1}{6})(\frac{5}{6})^11 \approx 0.619 $
            * Probability of exactly one 6: one dice is 6, the other 11 are not 6, and that one 6 dice can be any of the 12 total dice
    * Probability of Event C
        * Take complement: 1 minus sum of probabilities of exactly zero to two 6's
        * $P(C) = 1 - \sum\_{2}^{K=0} \binom{18}{K} \times (\frac{1}{6})^{K}(\frac{5}{6})^{18-K} \approx 0.597$
            * Choose position where the K dice will be 6
            * Then set each K dice as 6
            * Then the remaining dice can be anything not 6
    * Therefore, Event A is the most likely, and Event C is the least likely.
        * Though Newton got the calculation right, his intuition was confusing and *wrong* because it was not dependent on *fair* dice.

<br />

* **Conditional Probability**
    * How should you update probability/beliefs/uncertainty based on new evidence?
        * "Conditioning is the soul of statistics." - Professor Blitzstein
    * $P(A \mid B) = \frac{P(A \cap B)}{P(B)}, \text{ if } P(B) > 0$
        * Probability that A occurs given that B occurs
        * If A, B are independent, this doesn't matter
            * $P(A \mid B) = \frac{P(A \cap B)}{P(B)} = \frac{P(A)P(B)}{P(B)} = P(A)$
    * *Intuition 1*: **Pebble World**
        * Imagine a sample space S and not all outcomes are equally likely
        * Imagine a finite number of outcomes, each represented by a pebble
        * Consider in our sample space, there are 9 possible outcomes, or 9 pebbles, some larger than others, with a total mass of 1
            * Event is a subset of sample space
            * Say Event B is a set of 4 pebbles
            * Say Event A is a set of 1 pebble in B and 3 pebbles outside of B
        * $P(A \mid B)$
            * Since we know B occurred, the other 5 pebbles outside B are irrelevant
            * Get rid of pebbles in $B^C$, our universe got restricted to B since we know Event B occurred
            * In our new universe, find the pebbles that are also in Event A (numerator)
            * But now total mass is not 1, so we **renormalize**, or multiply by a constant so new total mass is 1 again (denominator)
    * *Intuition 2*: **Frequentist World**
        * If we flipped a coin 1000 times, and 612 of them are heads, then we can say $P(H) \approx 0.612$
        * Interpret probability as the fraction of event occurring from *repeating the experiment many times*
        * $P(A \mid B)$
            * Repeat an experiment many times
            * Circle the experiments where B occurred
            * Of the circled experiments, what fraction of them did A also occur?
    * Theorem 1
        * Suppose we wanted to find probability of A and B
        * $P(A \cap B) = P(B) \times P(A \mid B) = P(B \cap A) = P(A) \times P(B \mid A)$
        * If A and B are *independent*, then $P(A \mid B) = P(A), P(B \mid A) = P(B)$
    * Theorem 2
        * $P(A\_{1}, \ldots, A\_{N}) = P(A\_{1}) \times P(A\_{2} \mid A\_{1}) \times P(A\_{3} \mid A\_{2}, A\_{1}) \times \ldots \times P(A\_{N} \mid A\_{1}, \ldots, A\_{N-1})$
        * This is basically $N!$ theorems, repeatedly applying Theorem 1 multiple times
    * Theorem 3: **Bayes' Rule**
        * We want $P(A \mid B)$ to relate to $P(B \mid A)$
        * So we divide Theorem 1 by $P(B)$ on both sides
        * $P(B) \times P(A \mid B) \div P(B) = P(A) \times P(B \mid A) \div P(B)$
        * $P(A \mid B) = \frac{P(A)P(B \mid A)}{P(B)}$


## 5. Conditional Probability II, Law of Total Probability

* Problem Solving
    * "Thinking conditionally is a condition for thinking." - Professor Blitzstein
    * How to Solve Problems
        1. Try simple and extreme cases
        1. Break up the problem into simpler pieces/partitions

<br />

* **Law of Total Probability**
    * Let $A\_{1}, \ldots, A\_{N}$ be disjoint partitions of universe S
    * Consider a sample space B within universe S
    * $P(B) = P(B \cap A\_{1})*P(A\_{1}) + \ldots + P(B \cap A\_{N})*P(A\_{N})$

<br />

* *Example*: Get a random 2-card hand from standard deck of cards
    * Find $P(\text{both cards are aces} \mid \text{have an ace})$
        * $P(\text{both cards are aces} \mid \text{have an ace}) = \frac{P(\text{both cards are aces} \cap \text{have an ace})}{P(\text{have an ace})}$
        * $P(\text{both cards are aces} \cap \text{have an ace}) = P(\text{both cards are aces})$
            * Having both cards aces is same as having one of the cards is an ace
        * $P(\text{both cards are aces} \mid \text{have an ace}) = \frac{P(\text{both cards are aces})}{P(\text{have an ace})}$
        * $P(\text{both cards are aces} \mid \text{have an ace}) = \frac{\binom{4}{2} / \binom{52}{2}}{1 - \binom{48}{2}/\binom{52}{2}}$
            * Numerator: choose 2 of 4 Aces, out of all possible 2 card hands
            * Denominator: 1 minus choose 2 of 48 non-Aces, out of all possible 2 card hands
        * $P(\text{both cards are aces} \mid \text{have an ace}) = \frac{1}{33}$

    * Find $P(\text{both cards are aces} \mid \text{have Ace of Spades})$
        * We have the Ace of Spades, second card can be any card in the deck other than the Ace of Spades, by symmetry
        * $P(\text{both cards are aces} \mid \text{have Ace of Spades}) = \frac{3}{51} = \frac{1}{17}$
    * It's twice as likely to have double Aces just by knowing the suit of the Ace we have!
        * What is going on? Think about least one Ace vs. a specific Ace...

<br />

* *Example*: A patient gets tested for a disease that affects 1% of the population and tests positive, using a test that is "95% accurate".
    * Let $D = \text{ patient has disease}, T = \text{ patient tests positive}$
    * $P(T \mid D) = 0.95 = P(T^C \mid D^C)$
        * If the patient has the disease, 95% of the time the test will be positive.
        * If the patient does not have the disease, 95% of the time the test will be negative.
    * $P(D \mid T) = \frac{P(T \mid D) * P(D)}{P(T)}$
        * Patient wants to know: if the test is positive, how likely do they have the disease?
        * Bayes' Rule tells us how these are connected!
    * $P(D \mid T) = \frac{P(T \mid D) * P(D)}{P(T \mid D)P(D) + P(T|D^C)P(D^C)}$
        * We use the Law of Total Probability to find $P(T)$
        * Probability patient tests positive is the sum of positive tests if patient tests positive and positive tests if patient tests negative
    * $P(D \mid T) = \frac{(0.95)(0.01)}{(0.95)*(0.01) + (1 - 0.95)(1 - 0.01)} \approx 0.16$
    * Although the test is 95% accurate, there is only a 16% chance the patient has the disease if they have a positive test!
        * Why is this number so small and our intuition is so off?
        * We need to consider that it's both *rare that the test is wrong (5%)* and *rare that someone has the disease (1%)*!
    * *Intuition*: If we had 1,000 patients, roughly 10 would have the disease (1%). If the 10 patients all test positive, but 5% of the 990 patients also test positive.
        * Roughly speaking, 50 patients test positive who don't have the disease and 10 patients test positive who do have the disease
        * This ratio of 50/10 is what leads to the $0.16$ .
    * *Important*: Bayes' Rule is coherent
        * Updating probabilities using the intersection of two new evidence is equal to updating probabilities using the first new evidence then updating again using the second new evidence, in any order

<br />

* Common Mistakes with Conditional Probability
    1. Confusing $P(A \mid B)$ with $P(B \mid A)$ , "Prosecutor's Fallacy"
        * *Example*: Sally Clark Case
            * Clark's two babies died suddenly and was charged with murder
            * The only evidence that probability of baby dies not by murder is $\frac{1}{8500}$ 
            * But since Clark lost two babies, the probability of not murder was $\frac{1}{8500} * \frac{1}{8500}$
            * However, this assumes independence between baby deaths
            * Want $P(\text{innocence} \mid \text{evidence})$ , rather than $P(\text{evidence} \mid \text{innocence})$
    1. Confusing $P(A)$ , the **prior**, with $P(A \mid B)$ , the **posterior**
        * If Event A occurs, $P(A \mid A) = 1$ , but $P(A) \neq 1$
    1. Confusing independence with conditional independence
        * **Conditional Independence**: Events A and B are conditionally independent given Event C if $P(A \cap B \mid C) = P(A \mid C) * P(B \mid C)$
        * Conditional independence given Event C *does not imply* independence.
            * *Example*: Chess opponent of unknown strength
                * If you play a series of chess games with the opponent, conditioning on the opponent's strength, all games are independent (conditional independence)
                    * However, this does not imply the games are unconditionally independent!
                * If you win the first 5 games, then you think the opponent is weaker than you.
                    * The earlier games give you evidence of the opponent's strength, even though the games are seemingly independent
                * Independence says earlier games give no indication of the opponent's strength
                * It may happen that the game outcomes are *conditionally independent* given strength of opponent but *not unconditionally independent*
        * Unconditional independence *does not imply* conditional independence given Event C
            * *Example*: If the alarm goes off (Event A), which can be caused by two causes: fire (Event F) or popcorn (Event C)
                * Suppose F and C are independent.
                * But what's the probability that there's a fire given the alarm goes off and nobody is making popcorn?
                    * $P(F \mid A, C^C) = 1$
                    * Must be 1 if we eliminate the only other explanation of making popcorn
                * F and C are initially *independent*, but *not conditionally independent* given A


## 6. Monty Hall, Simpson's Paradox

* *Example*: **Monty Hall Problem**
    * Problem
        * Monty Hall was a game show host on "Let's Make a Deal". There are 3 doors (Door 1, Door 2, Door 3): one door has a car, the other two have goats.
        * Monty Hall asks you to choose a door. When you choose a door (Door 1), Monty Hall will open up either of the other doors (Door 2, Door 3), revealing a goat behind a door (Door 2)
        * You know the car is either the door you initially chose (Door 1) or the remaining unopened door (Door 3). Should you switch you initial door choice?
    * Assumptions
        * The doors are equally likely to have the car
        * Monty Hall knows which door has the car behind it
        * You want the car, not the goats
        * Monty always opens a goat door after you choose an initial door
        * If Monty has a choice of which door to open (you initially picked the car), he will open one of the goat doors with equal probabilities
    * The answer is that you should switch, which gives a winning chance of $2/3$.
        * Once the door is opened, the probabilities of each door is no longer $1/3$.
        * Monty opens Door 2 and reveals a goat, so it would seem the probabilities of winning is now $1/2$ each for Doors 1 and 3.
        * There is a subtle flaw in this thinking. We need to condition on all the evidence, which includes the fact Monty Hall opened Door 2.
    * *Intuition*: Use a tree diagram based on initial door choice, the car door, and the door Monty opens.

        | Initial Door | Car Door  | Monty Door |
        |:------------:|:---------:|:----------:|
        | 1            | 1 $(1/3)$ | 2 $(1/2)$  |
        | 1            | 1 $(1/3)$ | 3 $(1/2)$  |
        | 1            | 2 $(1/3)$ | 3 $(1)$    |
        | 1            | 3 $(1/3)$ | 2 $(1)$    |

        * If we condition on the fact Monty opened Door 2, we either took the top path (car is behind Door 1) or the bottom path (car is behind Door 3). The probability of the top path is $1/3 * 1/2 = 1/6$, and the probability of the bottom path is $1/3 * 1 = 1/3$. We renormalize the two options (make total sum = 1), to find the car has a $1/3$ chance of being behind Door 1 and a $2/3$ chance of being behind Door 3.
        * This means $1/3$ of the time your initial guess is correct, and the other $2/3$ of the time you should switch
    * *Intuition*: Law of Total Probability
        * What do we wish we knew beforehand? In this case, we wish we know which door has the car. Now we condition on that.
        * Let $S$ = succeed (assuming we always switch) and $D\_{j}$ = Door j has the car where $j \in {1, 2, 3}$
        * From the law of Total Probabilities, $P(S) = P(S \vert D\_{1}) \frac{1}{3} + P(S \vert D\_{2}) \frac{1}{3} + P(S \vert D\_{3}) \frac{1}{3}$
            * In the first case: we pick Door 1, car is behind Door 1, Monty opens either Door 2 or Door 3, and we switch our choice, so $P(S \vert D\_{1}) = 0$
            * In the second case: we pick Door 1, car is behind Door 2, Monty must open Door 3, and we switch our choice, so $P(S \vert D\_{2}) = 1$
            * In the third case: we pick Door 1, car is behind Door 3, Monty must open Door 2, and we switch our choice, so $P(S \vert D\_{3}) = 1$
        * Therefore, $P(S) = 0 + 1 (\frac{1}{3}) + 1 (\frac{1}{3}) = \frac{2}{3}$
        * By symmetry, we also know that $P(S \vert \text{ Monty opens Door 2}) = \frac{2}{3}$ 
    * *Intuition*: Consider if there were 1,000,000 doors and Monty opens 999,9988 doors. In this case it is clear you should switch since your initial guess is most likely incorrect.

* **Simpson's Paradox**
    * Is it possible to have 2 doctors, where the first doctor has a higher success rate at every single type of surgery than the second doctor, yet the second doctor overall has a higher success rate?
        * Simpson's Paradox says this is possible, but this is very counterintuitive. How can one thing better in all individual cases be worse after aggregating the cases?
    * Consider first doctor is Dr. Hibbert and second doctor is Dr. Nick, and they both do 100 cases total of two types of surgeries: heart surgery and band-aid removal

        | Hibbert | Heart | Band-Aid |
        |:-------:|:-----:|:--------:|
        | Success | 70    | 10       |
        | Failure | 20    | 0        |

        |  Nick   | Heart | Band-Aid |
        |:-------:|:-----:|:--------:|
        | Success | 2     | 81       |
        | Failure | 8     | 9        |

    * If you need surgery, who do you choose? For each individual operation (conditioning on a type of surgery), Dr. Hibbert has a higher success rate. But overall (unconditionally), Dr. Hibbert has a 80% success rate, and Dr. Nick has a 83% success rate.
    * Simpson's Paradox in terms of Conditional Probability
        * Let A = surgery is successful
        * Let B = treated by Dr. Nick
        * Let C = heart surgery
            * Called the *confounder* or *control*, the type of surgery you want to condition on
            * If we fail to condition on C, then Simpson's Paradox shows us the inequality can flip.
            * Knowing we got treated by Dr. Nick gives us information about what type of surgery we had, which then affects the probability of success.
        * $ P(A \vert B, C) < P(A \vert B^{C}, C)$
            * The probability of success of heart surgery from Dr. Nick is less than probability of success of heart surgery from Dr. Hibbert.
        * $ P(A \vert B, C^{C}) < P(A \vert B^{C}, C^{C})$
            * The probability of success of band-aid removal from Dr. Nick is less than probability of success of band-aid removal from Dr. Hibbert.
        * But overall, $P(A \vert B)$ > P(A \vert B^{C})$
            * The probability of successful surgery is higher with Dr. Nick than with Dr. Hibbert.
            * Notice how the inequality flips
    * How is Simpson's Paradox possible? How can the sign flip from the conditioning on individual events compared to overall?
        * From the Law of Total Probability using conditional probability, $P(A \vert B) = P(A \vert B, C) P(C \vert B) + P(A \vert B, C^{C}) P(C^{C} \vert B)$
        * We know that $P(A \vert B, C) < P(A \vert B^{C}, C)$ and that $P(A \vert B, C^{C}) < P(A \vert B^{C}, C^{C})$
        * However, we are unable to reduce it further to $P(A \vert B)$ due to the terms $P(C \vert B), P(C^{C} \vert B)$, which are weights conditional on B.
        * *Intuition*: $P(C \vert B)$ is the probability of performing heart surgery for Dr. Nick, which is very different than $P(C \vert B^{C})$ probability of performing heart surgery for Dr. Hibbert. The weights $P(C \vert B), P(C^{C} \vert B)$ change, and that is what enables Simpson's Paradox to happen.

* *Example*: There was a lawsuit against UC Berkeley claiming sex discriminations in graduate admission rates. Looking at the overall admission rates, it looked like it was easier for men to get into UC Berkeley for graduate school. However, looking at the data of each individual apartment, the admission rates were generally more fair. The reason was that certain departments are more popular for women applicants and certain departments are harder to get into than others.

* *Example*: There are four jars that each contain two flavors of jellybeans. Suppose you like one flavor over the other. Suppose jars are "better" if they have a higher percentage of the jellybean you like. If we combine the two "better" jars into one large jar and likewise with the two "worse" jars, Simpson's Paradox says the large jar created from the two "worse" jars may now have a higher percentage of the favorable jellybean and be "better".


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