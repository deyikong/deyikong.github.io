---
title: Kth Biggest
date: 2022-04-20 21:13:31
tags:
---

## Kth Largest Element in an Array
https://leetcode.com/problems/kth-largest-element-in-an-array/

### Solutions
- sort
- max heap
- min heap
- quick select
```java min heap 
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<Integer>((a, b) -> a - b);
        for (int i = 0; i < k; i++) {
            pq.add(nums[i]);
        }
        for (int i = k; i < nums.length; i++) {
            int top = pq.peek(); 
            if (nums[i] > top) {
                pq.poll();
                pq.add(nums[i]);
            }
        }
        return pq.peek();
    }
}
```
``` java quickselect
class Solution {
    public int findKthLargest(int[] nums, int k) {
        int start = 0, end = nums.length - 1;
        while (start <= end) {
           int p = partition(nums, start, end); 
            if (p == k - 1) {
                return nums[p];
            }
            if (p > k - 1) {
                end = p - 1;
            }
            if (p < k - 1) {
                start = p + 1;
            }
        }
        return -1;
    }
    private int partition(int[] nums, int s, int e) {
        if (s == e) return s;
        int p = nums[e];
        int i = s - 1, j = s;
        //loop invariant, left of i is all smaller than p, including i. between i and j are all bigger than p
        while (j < e) {
           if (nums[j] > p) {
               i++;
               swap(nums, i, j);
           } 
           j++;
        }
        swap(nums, i+1, e);
        return i + 1;
    }
    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```