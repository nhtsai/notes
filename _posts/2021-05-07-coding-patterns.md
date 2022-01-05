---
layout: post
title: "Coding Patterns"
description: "Notes on Coding Interview Patterns."
author: "Nathan Tsai"
toc: true
comments: false
hide: false
search_exclude: false
show_tags: true
categories: [coding]
permalink: /coding-patterns
---

# Coding Patterns

## Sliding Window

The Sliding Window is a pattern that shifts a window one element at a time, adjusting the window length according to the problem.

The Sliding Window pattern is used to manipulate a specific window size of a given linear data structure, like an `array`, `string`, or `linked list`, to find the longest or shortest **substring**, **subarray**, or a **desired value**.

Example problems:
* Maximum sum subarray of size `K`
* Longest substring with `K` distinct characters
* String anagrams

```python
start = 0
window_sum = 0
max_sum = 0
for end in range(len(arr)):

    # window calculation
    window_sum += arr[end]

    # shrink condition
    window_length = end - start + 1
    if window_length == k:
        max_sum = max(max_sum, window_sum)
        window_sum -= arr[start]
        start += 1

return max_sum
```

## Two Pointers

The Two Pointers is a pattern of two pointers that iterate through the data structure in tandem until one or both of the pointers hits a certain condition.

The Two Pointers pattern is used to search for pairs in a `sorted array` or `linked list`, to find a set of elements (**pair**, **triplet**, or **subarray**) that fulfills certain constraints. A single pointer instead often loops through the array multiple times to find the answer inefficiently (`O(n^2)`).

Example Problems:
* Squaring a sorted array
* Triplets that sum to zero
* Comparing strings that contain backspaces

```python
l, r = 0, len(arr) - 1

```


## Fast and Slow Pointers

Also known as the Hare & Tortoise algorithm, Fast and Slow Pointers is a pattern of two pointers that iterate through the data structure at different speeds.

The Fast and Slow Pointers pattern is used to deal with `cyclic linked lists` or `looped arrays`, to find the position of a certain element or the overall length of the `linked list`.

The Fast and Slow Pointers should be used over the Two Pointers in problems without backward traversal, like in `singly linked lists`.

Examples:
* Linked List Cycle
* Palindrome Linked List
* Cycle in Circular Array


```python
# Floyd's Algorithm
slow = fast = head
while fast and fast.next:
    fast = fast.next.next
    slow = slow.next

    # cycle detection
    if slow == fast:
        # cycle exists
        break
else:
    # no cycle exists


# cycle start
slow = head
while slow != fast:
    slow = slow.next
    fast = fast.next

# cycle length
cycle_length = 1
fast = fast.next
while fast != slow:
    slow = slow.next
    cycle_length += 1
```

## Merge Intervals

* **Method**: Iterate through interval pairs to find overlapping intervals, merge overlapping intervals, or find mutually exclusive intervals.

* **Input**: list of intervals with start and end values

* **Output**: 

* **Examples**:
    * Interval Intersection
    * Maximum CPU Load

* Basic Example:
```python
def merge_intervals(intervals: list[list[int]]) -> list[list[int]]:
    """Returns list of intervals with overlapping intervals merged.
    Time: O(NlogN)
    Space: O(N)
    """
    intervals.sort(key=lambda x: x[0])
    merged = []
    start, end = intervals[0]
    for i in range(1, len(intervals)):
        next_start, next_end = intervals[i]

        # no overlap between intervals A and B
        if end < next_start:
            merged.append([start, end])
            start, end = next_start, next_end
        # overlap between intervals A and B
        else:
            # A either overlaps start of B or all of B
            end = max(end, next_end)
    # add last interval
    merged.append([start, end])
    return merged
```


## Cyclic Sort

The Cyclic Sort pattern iterates over the array one number at a time and swaps numbers if they are not at the correct indices. It's unoptimal (`O(n^2)`) to place numbers in their correct indices.

The Cyclic Sort pattern is used to deal with a `sorted array` with numbers in a given range and find the missing, duplicate, or smallest number in a `sorted array` or `rotated array`.

Input: sorted array of numbers, helps if numbers are in index range
Output: 

Examples:
* Find the Missing Number
* Find the Smallest Missing Positive Number


```python
def cyclic_sort(arr: list[int]) -> list[int]:
    """Swap until elements are placed in correct spots.
    Time: O(N)
    Space: O(1)
    """
    i = 0
    while i < len(arr):
        # correct index of current element
        j = arr[i] - 1
        # keep swapping if not at correct index
        if arr[i] != arr[j]:
            arr[i], arr[j] = arr[j], arr[i]
        # otherwise, move to next element
        else:
            i += 1
    return arr
```


## In-Place Reversal of Linked List

This pattern reverses the links between a set of nodes of a `linked list` in-place. It reverses one node at a time, starting with the current pointer pointing to the head of the `linked list`, and the previous pointer pointing to the previously processed node. Reverse the current node by pointing it to the previous before moving on to the next node

Example:
* Reverse a sub-list
* Reverse every K-element sub-list


## Tree BFS

* **Method**: Breadth First Search is an iterative method of traversing tree structures that uses a FIFO queue to visit neighboring nodes.
* **Input**:
* **Output**:
* **Basic Structure**:

    ```python
    ```

* **Example Problems**:
    * 




## Tree DFS


## Two Heaps

* **Method**: Breadth First Search is an iterative method of traversing tree structures that uses a FIFO queue to visit neighboring nodes.
* **Input**:
* **Output**:
* **Basic Structure**:

    ```python
    from heapq import *
    ```

* **Example Problems**:
    * Median of Number Stream
    * Sliding Window Median
    * Maximize Capital
    * Next Interval

## Subsets

## Modified Binary Search

## Bitwise XOR

## Top K Elements

## K-Way Merge

* **Input**: List of sorted arrays/lists, a sorted matrix
* **Output**: sorted merged arrays
* **Method**
    1. Push first element, list index, and element index of each list into a minheap.
    1. The minheap will find the smallest element of all elements.
    1. Once
* **Basic Structure**:

    ```python
    from heapq import *
    ```

* **Example Problems**:

## 0/1 Knapsack

## Topological Sort

## Other

## References
1. ul Haq, Fahim. 2019. [*14 Patterns to Ace Any Coding Interview Question.*](https://hackernoon.com/14-patterns-to-ace-any-coding-interview-question-c5bb3357f6ed)
1. Bolat, Emre. 2020. [Coding Patterns Blog Posts](https://emre.me/categories/#coding-patterns)