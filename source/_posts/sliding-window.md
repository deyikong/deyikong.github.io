---
title: sliding window
date: 2022-05-05 22:10:43
tags:
---

# Thoughts
Periodical logic: 
- before k is filled, skip or special case
- after k is filled, + end, - start.
  - solve sub-problems in the window, it could be maximum, minimum, average in int, average in double, sum(overflow warning) etc. 
  
Notes:
- if it's a fixed size array, most of the time, we don't need additional data structures, just the index
- if it's a data stream, we might need to save the elements in the window in an array, deque, linkedlist, etc. 

Method:
- outline sliding window 
    - pick data structure or index
- focus on inside each window, 
  - only three things/steps to do: 
    - delete (if fixed size, in order not to be overriden, delete before insert)
    - insert
    - process and save
  - list all the cases for each step
  - might need extra structure, changing states based on inserting and deleting elements, deque


## Moving Average from Data Stream
https://leetcode.com/problems/moving-average-from-data-stream/
### Solutions
- two period: 
    - before k values are filled 
    - after k values are filled
```java 
class MovingAverage {
    int size = 0;
    int count = 0; 
    int[] window = null;
    int sum = 0;
    int tail = -1;

    public MovingAverage(int size) {
       this.size = size; 
        window = new int[size];
    }
    
    public double next(int val) {
        // move tail
        tail = tail == size - 1 ? 0 : tail+1;
        // delete
        if (++count > size) {
           sum -= window[tail];
        }
        // insert
        window[tail] = val;
        sum += val;
        // save
        return (double)sum / Math.min(count, size);
    }
}
```
```java 
class MovingAverage {

    int size = 0;
    int curSize = 0;
    double sum = 0;
    int i = 0; 
    int[] window = null;
    
    public MovingAverage(int size) {
       this.size = size; 
        window = new int[size];
    }
    
    public double next(int val) {
        curSize++;
       if (curSize <= this.size) {
           window[curSize - 1] = val;
           sum+=val;
            return sum / curSize;        
       }
       sum -= window[i]; 
        window[i] = val;
        sum+=val;
       double res = sum / size; 
        i++;
        if (i == size) i = 0;
        return res;
    }
}
```

## K Radius Subarray Average
https://leetcode.com/problems/k-radius-subarray-averages/

### Solutions
- use end as i, update the middle. could do one pass, or fill with -1 first
- trick, sum might be overflowing, so use double. 
```java 
class Solution {
    public int[] getAverages(int[] nums, int k) {
        double sum = 0;
        int m = nums.length;
        int[] res = new int[m];
        int windowLength = 2 * k + 1;
        if (k >= m) {
            Arrays.fill(res, -1);
            return res;
        };
        
        for (int i = 0; i < m; i++) {
            // before and after
            if (i < windowLength - 1) {
                res[i] = -1;
                sum = nums[i] + sum;
                continue;
            }
            // in between
            int end = nums[i];
            int start = i - windowLength < 0 ? 0 : nums[i - windowLength];
            sum = sum + end - start;
            if (i >= m - k) {
               res[i] = -1; 
            }
            res[i - k] = (int)(sum / (double)windowLength);
        }
        return res;
    }
}
```
## Sliding window maximum 
https://leetcode.com/problems/sliding-window-maximum/solution/
### Solutions
- pick the right data structure
- outline the window
- follow three steps: insert, delete and save
- idea: monotonous stack/queue
```java 
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] ans = new int[n - k + 1];
        //.    index
        Deque<Integer> pq = new LinkedList();
        
        for (int i = 0; i < n; i++) {
            // before k - 1
            // insert
            while (!pq.isEmpty() && nums[i] > nums[pq.peekLast()]) {
                pq.pollLast();
            }
            pq.offerLast(i);
            
            // delete
            if (pq.peekFirst() == i - k) pq.pollFirst(); 
            
            // after k is filled, add local solution to global solution             
           if (i >= k - 1) {
               ans[i - k + 1] = nums[pq.peek()];
           } 
        }
        return ans;
    }
}
```
