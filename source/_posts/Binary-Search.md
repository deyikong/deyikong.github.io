---
title: Binary Search
date: 2022-03-05 22:19:39
tags:
---

## Concepts

## Key
- get rid of a side that's not what we are looking for
- whether you can git rid of the border depending on if the border might be the final answer
- use the smallest sample size(size = 1) to test if the condition allows entering the while loop, and test if the condition allows exiting the loop

## Questions
- closet to a target number(stop at two elements)
- kth closet to a target number
    - find the closest, then traverse left or right
- find the first occurrence of a number 
    - 1,2,2,2 find 2; 
    - stop at two element
    - don't stop at the target, keep moving the left: `l = m`, not `m - 1` because can't get rid of m
- sorted array of unknown size
    - double the range to find the end index first
    - then try to find it in the found range 

