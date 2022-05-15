---
title: Bucket Sort
date: 2022-04-12 00:05:41
tags:
---


# 扫描线问题 Line Sweep

## Range Addition
https://leetcode.com/problems/range-addition/
### Solutions
- S1. normal O(n * k)
- S2. only update start and end+1 to get an array of difference,  O(n + k), 
    - how to come up with this thought: 
        - loop order from left to right, order plays a key part
        - record the difference 
```java 
class Solution {
    public int[] getModifiedArray(int length, int[][] updates) {
       int[] res = new int[length]; 
        int l = updates.length;
        for (int i = 0; i < l; i++) {
            res[updates[i][0]] += updates[i][2];
            if (updates[i][1] + 1 < length) {
                res[updates[i][1] + 1] -= updates[i][2];
            }
        }
        
        for (int i = 1; i < length; i++) {
            res[i] = res[i - 1] + res[i];
        }
        return res;
    }
}
```

## Similar problem as above, Car Pooling
https://leetcode.com/problems/car-pooling/
### Solutions
- bucket sort
- timestamp, TreeMap
```java 
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {
        int[] res = new int[1000];
        
        for (int i = 0; i < trips.length; i++) {
            if (trips[i][0] > capacity) return false;
            res[trips[i][1] - 1] += trips[i][0];
            res[trips[i][2] - 1] -= trips[i][0];
        }
        for (int i = 1; i < res.length; i++) {
            res[i] = res[i - 1] + res[i];
            if (res[i] > capacity) {
                return false;
            }
        }
        return true;
    }
}
```
```java 
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {
        
        Map<Integer, Integer> timestamps = new TreeMap();
        
        for (int i = 0; i < trips.length; i++) {
            if (trips[i][0] > capacity) return false;
            timestamps.put(trips[i][1], timestamps.getOrDefault(trips[i][1], 0) + trips[i][0]);
            timestamps.put(trips[i][2], timestamps.getOrDefault(trips[i][2], 0) - trips[i][0]);
        }
        int usedCapacity = 0;
        for (int change : timestamps.values()) {
            usedCapacity += change;
            if (usedCapacity > capacity) return false;
        }
        return true;
    }
}
```

## Corporate Flight Booking
https://leetcode.com/problems/corporate-flight-bookings/
## Solutions
- same as above


## Meeting Room II
https://leetcode.com/problems/meeting-rooms-ii/
### Solutions
- TreeMap
- PriorityQueue

```java 
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        Map<Integer, Integer> timestamps = new TreeMap();
        
        for (int[] interval: intervals) {
            timestamps.put(interval[0], timestamps.getOrDefault(interval[0], 0) + 1);
            timestamps.put(interval[1], timestamps.getOrDefault(interval[1], 0) - 1);
        }
        int last = 0;
        int max = 0;
        for (int timestamp : timestamps.values()) {
           last += timestamp;
            max = Math.max(max, last);
        }
        return max;
    }
}
```

```java 
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        
        // base case, if there's no intervals, return 0
        if (intervals.length == 0) return 0;
        // min heap
        PriorityQueue<Integer> allocator = new PriorityQueue<Integer>(
            intervals.length,
            new Comparator<Integer>() {
                public int compare(Integer a, Integer b) {
                    return a - b;
                }
            }
        );
        
        //Sort the intervals by start time
        Arrays.sort(
            intervals,
            new Comparator<int[]>() {
                public int compare(int[] a, int[] b) {
                    return a[0] - b[0];
                }
            }
        );
        
        // add the first meeting
        allocator.add(intervals[0][1]);
        
        //Interate over remaining intervals
        for (int i = 1; i < intervals.length; i++) {
            
            // If the earliest room is free, assign this room to this meeting
            if (intervals[i][0] >= allocator.peek()) {
                allocator.poll();
            }
            //if a new room is to be assigned, add it to the heap. 
            allocator.add(intervals[i][1]);
        }
        
        return allocator.size();
    }
}
```
## 252 meeting room
https://leetcode.com/problems/meeting-rooms/
## 253 meeting room II
https://leetcode.com/problems/meeting-rooms-ii/
## 56 merge interval
https://leetcode.com/problems/merge-intervals/
## 1272 Remove interval
https://leetcode.com/problems/remove-interval/
## 435 non overlapping intervals
https://leetcode.com/problems/non-overlapping-intervals/
## 1288 Remove Covered intervals
https://leetcode.com/problems/remove-covered-intervals/
## 1229 Meeting Scheduler
https://leetcode.com/problems/meeting-scheduler/
## 986 interval list intersections
https://leetcode.com/problems/interval-list-intersections/
## 759 Employee Free Time
https://leetcode.com/problems/employee-free-time/
