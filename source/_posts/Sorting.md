---
title: Sorting
date: 2022-03-05 18:53:41
tags: 
    - algorithms 
    - technical
    - leetcode
    - sorting
categories:
    - algorithms

# Insertion Sort

## Keys
- playing card
- sorted part + unsorted part
- insert in sorted array

## Code

### array
```java  
import java.util.*;
class GFG
{
// Function to sort an array using insertion sort
static void insertionSort(int arr[], int n)
{
	int i, key, j;
	for (i = 1; i < n; i++)
	{
		key = arr[i];
		j = i - 1;

		// Move elements of arr[0..i-1],
		// that are greater than key to
		// one position ahead of their
		// current position
		while (j >= 0 && arr[j] > key)
		{
			arr[j + 1] = arr[j];
			j = j - 1;
		}
		arr[j + 1] = key;
	}
}

// Function to print an array of size N
static void printArray(int arr[], int n)
{
	int i;

	// Print the array
	for (i = 0; i < n; i++) {
		System.out.print(arr[i] + " ");
	}
	System.out.println();
}

// Driver code
public static void main(String[] args)
{
	int arr[] = { 12, 11, 13, 5, 6 };
	int N = arr.length;

	// Function Call
	insertionSort(arr, N);
	printArray(arr, N);
}
}

```

### Singly linkedlist 

- My solution (not optimal, hard to remember)
```java 
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode insertionSortList(ListNode head) {
        //base case:
        if (head == null) return head;
       // loop invariants: 
        // for each i in [0, n], list before head[i] is sorted, after is not sorted. 
        // initialization: i = 1, becasue head[0] is already sorted by itself
        // maintenance: as i++, we need to insert head[i] into it's right place.
        // temination condition: when it passes all the elements
        ListNode local = head.next;
        int localTraverseCount = 1;
        ListNode localPrev = head;
        ListNode headCopy = head;
        
        while(local != null) {
             // break current node
              localPrev.next = local.next;
            ListNode next = local.next;
            
           // if it's the smallest 
          if (local.val < headCopy.val) {
              local.next = headCopy; 
              headCopy = local;
          } else {
               // loop through the first part to insert the node 
            ListNode insertTraverse = headCopy;
            int insertTraverseCount = 1;
                //local invariants: insertTraverse.val is smaller than local.val but insertTraverse.next.val is bigger
                while (insertTraverseCount < localTraverseCount && insertTraverse.val < local.val && insertTraverse.next.val < local.val) {
                  insertTraverse = insertTraverse.next;  
                    insertTraverseCount++;
                }
                // termination: it's own place, or found a node whose .next.val is bigger
                // insert local after insertTraverse
                   ListNode temp = insertTraverse.next; 
                insertTraverse.next = local;
                local.next = temp;
                if (insertTraverseCount == localTraverseCount) {
                   localPrev = local; 
                 }
              } 
              localTraverseCount++;
            local = next;
        }
        
        return headCopy;
    }
}
```
- given solutions: 
```java 
    public ListNode insertionSortList(ListNode head) {
        ListNode dummy = new ListNode();
        ListNode curr = head;

        while (curr != null) {
            // At each iteration, we insert an element into the resulting list.
            ListNode prev = dummy;

            // find the position to insert the current node
            while (prev.next != null && prev.next.val < curr.val) {
                prev = prev.next;
            }

            ListNode next = curr.next;
            // insert the current node to the new list
            curr.next = prev.next;
            prev.next = curr;

            // moving on to the next iteration
            curr = next;
        }

        return dummy.next;
    }
```
*https://leetcode.com/problems/insertion-sort-list/*
# Selection Sort

## Keys
- for for loop
- `swap(i, minSoFar)`
 
## Examples
- sort an array with two stacks 
- sort an array with one stack 

## Code

### selection sort
```java
// Selection sort in Java

import java.util.Arrays;

class SelectionSort {
  void selectionSort(int array[]) {
    int size = array.length;

    for (int step = 0; step < size - 1; step++) {
      int min_idx = step;

      for (int i = step + 1; i < size; i++) {

        // To sort in descending order, change > to < in this line.
        // Select the minimum element in each loop.
        if (array[i] < array[min_idx]) {
          min_idx = i;
        }
      }

      // put min at the correct position
      swap(array, min_idx, step);
    }
  }

  void swap(array, x, y) {
      int temp = array[x];
      array[x] = array[y];
      array[y] = temp;
  }

  // driver code
  public static void main(String args[]) {
    int[] data = { 20, 12, 10, 15, 2 };
    SelectionSort ss = new SelectionSort();
    ss.selectionSort(data);
    System.out.println("Sorted Array in Ascending Order: ");
    System.out.println(Arrays.toString(data));
  }
}
```
### stacks
```java 
// Java program to sort an
// array using stack
import java.io.*;
import java.util.*;

class GFG
{
	// This function return the sorted stack
	static Stack<Integer> sortStack(Stack<Integer> input)
	{
		Stack<Integer> tmpStack = new Stack<Integer>();
	
		while (!input.empty())
		{
			// pop out the first element
			int tmp = input.peek();
			input.pop();
	
			// while temporary stack is not empty and top of stack is smaller than temp
			while (!tmpStack.empty() && tmpStack.peek() < tmp)
			{
				// pop from temporary stack and push it to the input stack
				input.push(tmpStack.peek());
				tmpStack.pop();
			}
	
			// push tmp in temporary of stack
			tmpStack.push(tmp);
		}
	
		return tmpStack;
	}
	
	static void sortArrayUsingStacks(int []arr, int n)
	{
		// push array elements
		// to stack
		Stack<Integer> input = new Stack<Integer>();
		for (int i = 0; i < n; i++)
			input.push(arr[i]);
	
		// Sort the temporary stack
		Stack<Integer> tmpStack = sortStack(input);
	
		// Put stack elements in arr[]
		for (int i = 0; i < n; i++)
		{
			arr[i] = tmpStack.peek();
			tmpStack.pop();
		}
	}
	
	// Driver Code
	public static void main(String args[])
	{
		int []arr = {10, 5, 15, 45};
		int n = arr.length;
	
		sortArrayUsingStacks(arr, n);
	
		for (int i = 0; i < n; i++)
			System.out.print(arr[i] + " ");
	}
}
```
*https://www.geeksforgeeks.org/sorting-array-using-stacks/*


# Merge Sort

![Merge Sort](Sorting/Merge-Sort-Tutorial.png)
## Keys
- Divide and conquer 
- Divide, Conquer, Combine

## Psudo code
```
MergeSort(A, p, r)
if p < r
    q = [p + r] / 2
    MergeSort(A, p, q)
    MergeSort(A, q + 1, r)
    Merge(A, p, q, r)

Merge(A, p, q, r)
    
```

## Code

```java 
/* Java program for Merge Sort */
class MergeSort
{
	// Merges two subarrays of arr[].
	// First subarray is arr[l..m]
	// Second subarray is arr[m+1..r]
	void merge(int arr[], int l, int m, int r)
	{
		// Find sizes of two subarrays to be merged
		int n1 = m - l + 1;
		int n2 = r - m;

		/* Create temp arrays */
		int L[] = new int[n1];
		int R[] = new int[n2];

		/*Copy data to temp arrays*/
		for (int i = 0; i < n1; ++i)
			L[i] = arr[l + i];
		for (int j = 0; j < n2; ++j)
			R[j] = arr[m + 1 + j];

		/* Merge the temp arrays */

		// Initial indexes of first and second subarrays
		int i = 0, j = 0;

		// Initial index of merged subarray array
		int k = l;
		while (i < n1 && j < n2) {
			if (L[i] <= R[j]) {
				arr[k] = L[i];
				i++;
			}
			else {
				arr[k] = R[j];
				j++;
			}
			k++;
		}

		/* Copy remaining elements of L[] if any */
		while (i < n1) {
			arr[k] = L[i];
			i++;
			k++;
		}

		/* Copy remaining elements of R[] if any */
		while (j < n2) {
			arr[k] = R[j];
			j++;
			k++;
		}
	}

	// Main function that sorts arr[l..r] using
	// merge()
	void sort(int arr[], int l, int r)
	{
		if (l < r) {
			// Find the middle point
			int m =l+ (r-l)/2;

			// Sort first and second halves
			sort(arr, l, m);
			sort(arr, m + 1, r);

			// Merge the sorted halves
			merge(arr, l, m, r);
		}
	}

	/* A utility function to print array of size n */
	static void printArray(int arr[])
	{
		int n = arr.length;
		for (int i = 0; i < n; ++i)
			System.out.print(arr[i] + " ");
		System.out.println();
	}

	// Driver code
	public static void main(String args[])
	{
		int arr[] = { 12, 11, 13, 5, 6, 7 };

		System.out.println("Given Array");
		printArray(arr);

		MergeSort ob = new MergeSort();
		ob.sort(arr, 0, arr.length - 1);

		System.out.println("\nSorted array");
		printArray(arr);
	}
}

```
*https://www.geeksforgeeks.org/merge-sort/*

Merge Two sorted arrays, in place. 

Keys: 
- principle: three pointers
- in order not to override, it's best to move from right to left. so the condition need to check which bigger instead of who's smaller 
- my first thought was moving all the elements of nums1 to the end and go from left. it works too, but there's more to write. 
- how to write the condition is worth memorizing

```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
    int p = m - 1;
    int q = n - 1;
    
    for (int i = m + n - 1; i >= 0; i--) {
        // positive condition: when will we need to 
        // swap with nums1
        // 1. when nums1 still has elements and nums2 is out
        // 2. when nums1 still has elements and nums1[p] > nums2[q]
        // notes: it's ">" because we go from right to left. 
        if (p >= 0 && (q < 0 || (nums1[p] > nums2[q]))) {
            nums1[i] = nums1[p--];
            continue;
        }
        nums1[i] = nums2[q--];
    }
}
```
*https://leetcode.com/problems/merge-sorted-array/*

# Heapsort

Keys: 
- heapify the whole array
- keep swapping with the largest element

Can be used to implement a priority queue
Time Complexity: O(nlog(n))
# Quick Sort

## Psudo
<details> 
    <summary>Psudo</summary>
    
```
PARTITION(A, p, r) 
 x = A[r]
 i = p - 1
 for j = p to r - 1
    if (A[j] <= x)
        i = i + 1
        swap(A[i], A[j])
 swap(A[r], A[i+1])
 return i+1;

//--------Normal quicksort--------
QUICKSORT(A, p, r)
 if p < r 
    q = PARTATION(A, p, r)
    QUICKSORT(A, p, q -1) 
    QUICKSORT(A, q + 1, r) 

//--------Tail quicksort--------
TAIL-RECURSIVE-QUICKSORT(A, p, r)
 while(p < r) 
    q = PARTITION(A, p, r)
    TAIL-RECURSIVE-QUICKSORT(A, p, q - 1)
    p = q + 1

//--------Tail quicksort with logn most depth:sub-call on the smaller half--------
TAIL-RECURSIVE-QUICKSORT(A, p, r)
 while(p < r) 
    q = PARTITION(A, p, r)
    if (p - q > r - p) 
      TAIL-RECURSIVE-QUICKSORT(A, p, q - 1)
      p = q + 1
    else
      TAIL-RECURSIVE-QUICKSORT(A, q + 1, r)
      r = q - 1
```
</details>
 
## Sorting related problems

Merge Intervals
- https://leetcode.com/problems/merge-intervals/
- Keys: 
    - sort the array based on the first value, then use a stack to linear scan back.
    - <details> 
    <summary> My attempt</summary>
```java 
class Solution {
    public int[][] merge(int[][] intervals) {
        // sort intervals based on the start time
        // quick sort it
        quicksort(intervals, 0, intervals.length - 1);
        for(int[] interval : intervals) {
          System.out.println(interval[0] + " " + interval[1]);
        }
        Stack<int[]> stack = new Stack();
        for (int[] interval: intervals) {
            if (stack.isEmpty() || interval[0] > stack.peek()[1]) {
                stack.push(interval);
            } else {
               stack.peek()[1] = Math.max(interval[1],stack.peek()[1]); 
            }
        }
        int[][] ret = new int[stack.size()][2];
        int i = stack.size() - 1;
        while(!stack.isEmpty()) {
            ret[i--] = stack.pop();
        }
       return ret; 
    }
    private void quicksort(int[][] intervals, int q, int r) {
       while (q < r)  {
           int p = partition(intervals, q, r);
           if (q - p < r - p) {
             quicksort(intervals, q, p - 1);
             q = p + 1;
           } else {
             quicksort(intervals, p + 1, r);
             r = p - 1;
           }
       }
    }
    
    private int partition(int[][] intervals, int q, int r) {
       int[] pi = intervals[r];
       int i = q - 1; 
        for (int j = q; j < r; j++) {
            if (intervals[j][0] <= pi[0]) {
                i++;
                swap(intervals, i, j);
            }
        }
        swap(intervals, i + 1, r);
        return i + 1;
    }
    
    private void swap(int[][] intervals, int i, int j) {
        int[] temp = intervals[i];
        intervals[i] = intervals[j];
        intervals[j] = temp;
    }
}
```
   </details>  

