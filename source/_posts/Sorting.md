---
title: Sorting
date: 2022-03-05 18:53:41
tags:
---

# Insertion Sort

## Keys
- playing card
- sorted part + unsorted part
- insert in sorted array

## Code
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

Merge Two sorted arrays, in place. 

Keys: 
- in order not to override, it's best to move from right to left. so the condition need to check which bigger instead of who's smaller 
- my first thought was moving all the elements of nums1 to the end and go from left. it works too, but there's more to write. 

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

# Quick Sort