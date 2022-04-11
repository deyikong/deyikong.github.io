---
title: Problems
date: 2022-04-05 11:32:04
tags:
---

# 04/05/2022
## Longest Common Prefix 
https://leetcode.com/problems/longest-common-prefix/
###Solutions:
- horizontal scanning(from left to right)
- vertical scanning(compare the ith char on all the elements each time)
- divide and conquer 
- Binary search
###Follow up
LCP called multiple times frequently 
- use a trie
    - node path must only have one child element
    - stop at the "isWord" node
    - must match the character
    
    
## Letter Combinations of a phone number
https://leetcode.com/problems/letter-combinations-of-a-phone-number/

### Solutions:
- backtracking
- DFS

## Remove Nth Node From End of List
https://leetcode.com/problems/remove-nth-node-from-end-of-list/

### Solutions
- two pass, one to find the length, the other to find the first Node.


##  Generate Parentheses
https://leetcode.com/problems/generate-parentheses/
- DFS(permutation: only allow certain paths when conditions met)
- Closure Number:  (To be reviewed)


## swap nodes in pairs
https://leetcode.com/problems/swap-nodes-in-pairs/
- set up loop invariant and keep them. prev, first, second. 

## remove element 
https://leetcode.com/problems/remove-element/

###Solutions
- two pointer, slow fast pointer. copy over all the not equal elements
- two pointer, opposite direction, reduce right pointer by one when equal. 


## implement strstr
https://leetcode.com/problems/implement-strstr/
```java 
public int strStr(String haystack, String needle) {
       int index = 0; 
        char[] hays = haystack.toCharArray();
        char[] needles = needle.toCharArray();
        int i = 0;
        for (; i < hays.length; i++) {
            int j = 0;
            while (j < needles.length && i+j < hays.length && hays[i+j] == needles[j]) {
               j++; 
            }
            if (j == needles.length) {
                return i;
            }
        }
        return -1;
    }
```

##  Find First and Last Position of Element in Sorted Array
https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/

###  solutions
- binary search two ends. stop at two elements, because we might get into a dead loop. 

## Count and Say
https://leetcode.com/problems/count-and-say/

```java 
  public String countAndSay(int n) {
       if (n == 1) {
           return "1";
       } 
       
       String prev = "11";
        for (int i = 1; i < n - 1; i++) {
            prev = convert(prev);
        }
        return prev;
    }
    private String convert(String prev) {
        StringBuilder sb = new StringBuilder();
        char[] chars = prev.toCharArray();
        for (int i = 0; i < chars.length; i++) {
            char c = chars[i];
            int count = 1;
            
            while (i+1 < chars.length && chars[i+1] == c) {
               count++; 
                i++;
            } 
            sb.append(count);
            sb.append(c);
        }
        return sb.toString();
    }
```

## Combination Sum
https://leetcode.com/problems/combination-sum/

```java 
class Solution {
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        combine(candidates, 0, target, new ArrayList<Integer>());
        return res;    
    }
    
    private void combine(int[] candidates, int level, int target, List<Integer> list) {
        if (target == 0) {
            res.add(list);
            return;
        }
        
        if (level == candidates.length) {
            return;
        }
        
        int count = 0;
        while(count * candidates[level] <= target) {
            List<Integer> copy = new ArrayList<Integer>(list);
            for (int i = 0; i < count; i++) {
                copy.add(candidates[level]);
            }
            combine(candidates, level + 1, target - count * candidates[level], copy);
            count++;
        }
    }
}
```

##Jump game II
https://leetcode.com/problems/jump-game-ii/
###Solutions
- dp N^2
- BFS, N 
- greedy

# 04/06/2022

## Permutations
https://leetcode.com/problems/permutations/
###Solutions
- key point: fill each spot with available elements
- dps
- swap swap

## Combination Sum II
https://leetcode.com/problems/combination-sum-ii/
### Solutions
- think of each element can only show up on a level repeated times
```java 
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        int sum = Arrays.stream(candidates).sum();
        if (target > sum) {
            return res;
        }
        Arrays.sort(candidates);
        dps(candidates, target, 0, new ArrayList<Integer>()); 
        return res;
    }
    
    private void dps(int[] candidates, int target, int idx, List<Integer> list) {
        if (target == 0) {
            res.add(list);
            return;
        }
        if (idx >= candidates.length) {
            return;
        }
        int nextUnique = idx + 1;
        int count = 1;
        while (nextUnique < candidates.length && candidates[nextUnique] == candidates[idx]) {
           nextUnique++; 
            count++;
        }
        for (int i = 0; i <= count; i++) {
            int rem = target - i * candidates[idx];
            if (rem < 0) return;
            List<Integer> copy = new ArrayList<Integer>(list); 
            for (int j = 0; j < i; j++) {
                copy.add(candidates[idx]);
            }
            dps(candidates, rem, nextUnique, copy);
        }
    } 
```

## Permutations II (with duplicated elements)
https://leetcode.com/problems/permutations-ii/

### Solutions
- sort array
- use a set to check on each level to see if the element was added already 
```java 
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);
        permute(nums, 0);
        return res;
    }
    
    private void permute(int[] nums, int idx) {
        int n = nums.length;
       if (idx == n) {
           List<Integer> list = new ArrayList<Integer>();
           for (int i = 0; i < n; i++) {
               list.add(nums[i]);
           }
           res.add(list);
           return;
       } 
        HashSet<Integer> set = new HashSet();
        // not adding current 
        for (int k = idx; k < n; k++) {
            if (set.contains(nums[k])) {
               continue; 
            }
               swap(nums, idx, k); 
               permute(nums, idx + 1);
               swap(nums, idx, k); 
            set.add(nums[k]);
        }
    }
    private void swap(int[] nums, int i, int j) {
        int temp = nums[j];
        nums[j] = nums[i];
        nums[i] = temp;
    }
```

## Power(x, n)
https://leetcode.com/problems/powx-n/

###Solutions
- cut in half each time, dp to remember half. 
- check negative and odd situation

```java 
 public double myPow(double x, int n) {
        if (n < 0) {
            return 1 / helper(x, -n);
        }
        return helper(x, n);
    }
     public double helper(double x, int n) {
         if (n == 0) {
             return 1;
         }
        if (n == 1) {
            return x;
        }
        int half = n / 2;
        double halfValue = helper(x, half);
        int mod = n % 2;
        if (mod != 0) {
           return halfValue * halfValue * x; 
        }
        return halfValue * halfValue;
    }
```

## Spiral Matrix
https://leetcode.com/problems/spiral-matrix/
###Solutions
- separate directions
- check boundary 

```java 
 public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList<Integer>();
        int row = matrix.length;
        int col = matrix[0].length;
        int layers = (Math.min(row, col) + 1) / 2;
        for (int i = 0; i < layers; i++) {
            int innerRow = row - 2 * i;
            int innerCol = col - 2 * i;
            if (innerCol == 1) {
                for (int j = 0; j < innerRow; j++) {
                    res.add(matrix[i + j][i]);
                }
                return res;
            }
            if (innerRow == 1) {
                for (int j = 0; j < innerCol; j++) {
                    res.add(matrix[i][i + j]);
                }
                return res;
            }
            // print top line
            for (int j = 0; j < innerCol - 1; j++) {
                res.add(matrix[i][i + j]);
            }
            // print right line
            if (col - i - 1 > 0) {
                for (int j = 0; j < innerRow - 1; j++) {
                    res.add(matrix[i + j][col - i -1]);
                }
            }
            // print bottom line
            if (row - i - 1 > 0) {
                for (int j = 0; j < innerCol - 1; j++) {
                    res.add(matrix[row - i - 1][col - i - 1 - j]);
                }
            }
            // print left line
            if (i < col) {
                for (int j = 0; j < innerRow - 1; j++) {
                    res.add(matrix[row - i - 1 - j][i]);
                }
            }
        }
        return res;
    }
```

## Jump Game
https://leetcode.com/problems/jump-game/
### Solutions
- go from left to right
- remember last reachable element
```java 
  public boolean canJump(int[] nums) {
        int n = nums.length;
        if (n <= 1) return true;
       int last = 0; 
        for (int i = 0; i < n; i++) {
            if (i > last) return false;
            int next = nums[i] + i;
            if (next >= n - 1) {
                return true;
            }
            last = Math.max(next, last);
        }
        return false;
    }
```

## Spiral Matrix II (fill out a matrix spirally )
https://leetcode.com/problems/spiral-matrix-ii/
### Solutions
- different directions
```java 
 public int[][] generateMatrix(int n) {
        int total = n * n;
        int count = 1;
        int layers = (n + 1) / 2;
        int[][] res = new int[n][n];
        
        for (int i = 0; i < layers; i++) {
            int l = n - 2 * i;
            if (l == 1) {
                res[i][i] = count;
            }
            // top
            for (int j = 0; j < l - 1; j++) {
                res[i][i + j] = count++;
            }
            // right
            for (int j = 0; j < l - 1; j++) {
                res[i + j][n - i - 1] = count++;
            }
            // bottom
            for (int j = 0; j < l - 1; j++) {
                res[n - i - 1][n - i - 1 - j] = count++;
            }
            // left
            for (int j = 0; j < l - 1; j++) {
                res[n - i - 1 - j][i] = count++;
            }
        }
        return res;
    }
```


##unique paths
https://leetcode.com/problems/unique-paths/
###Solutions
- dp
- basic math: choose m - 1 or n - 1 from m - n - 2

##unique paths II (with obstacles)
https://leetcode.com/problems/unique-paths-ii/
###solutions
- dp 
```java
public int uniquePathsWithObstacles(int[][] obstacleGrid) {
    int m = obstacleGrid.length;
    int n = obstacleGrid[0].length;
    if (obstacleGrid[0][0] == 1 || obstacleGrid[m-1][n-1] == 1) return 0;
    int[][] dp = new int[m][n];
    dp[0][0] = 1; 
    
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (i == 0 && j == 0) continue;
            if (obstacleGrid[i][j] == 1) {
              dp[i][j] = 0;
            } else {
                int left = j == 0 ? 0 : dp[i][j-1]; 
                int up = i == 0 ? 0 : dp[i - 1][j]; 
                dp[i][j] = left + up;
            }
        }
    }
    return dp[m-1][n-1];
}
```

##Minimum path sum
https://leetcode.com/problems/minimum-path-sum/
###Solutions
- dp 2D
- dp 1D
```java 
public int minPathSum(int[][] grid) {
    int m = grid.length;
    int n = grid[0].length;
    int[][] dp = new int[m][n];
    dp[0][0] = grid[0][0];
    
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
           if (i == 0 && j == 0) continue; 
           int left = j == 0 ? Integer.MAX_VALUE : dp[i][j-1];
           int up = i == 0 ? Integer.MAX_VALUE : dp[i - 1][j];
           dp[i][j] = Math.min(left, up) + grid[i][j];
        }
    }
    return dp[m - 1][n - 1];
}
```
```java
public int minPathSum(int[][] grid) {
    int m = grid.length;
    int n = grid[0].length;
    int[] dp = new int[n];
    dp[0] = grid[0][0];
    
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
           if (i == 0 && j == 0) continue; 
           if (j == 0) {
               dp[j] = dp[j] + grid[i][j];
               continue;
           }
           int left = j == 0 ? Integer.MAX_VALUE : dp[j - 1];
           int up = i == 0 ? Integer.MAX_VALUE : dp[j];
           dp[j] = Math.min(left, up) + grid[i][j];
        }
    }
    return dp[n - 1];
}
```

##Set Matrix Zeros
https://leetcode.com/problems/set-matrix-zeroes/
### solutions
- use two sets
- use first column and first row as marker

## search a 2d matrix
https://leetcode.com/problems/search-a-2d-matrix/
###Solutions
- get row and col like: mid / n; mid % n;
```java 
public boolean searchMatrix(int[][] matrix, int target) {
    int m = matrix.length;
    int n = matrix[0].length;
    
    int l = 0, r = m * n - 1;
    
    while (l <= r) {
        int mid = l + (r - l)/2;
        int row = mid / n;
        int col = mid % n;
        if (matrix[row][col] == target) {
            return true;
        }
        if (matrix[row][col] > target) {
            r = mid - 1;
        } else {
            l = mid + 1; 
        }
    }
    return false;
}
```

# 04/07/2022

## Sort color
https://leetcode.com/problems/sort-colors/
###Solutions
- quicksort O(NlogN) (more general)
- O(N), one pass solution 
loop invariants:
three pointers, left i and right j, and cur
- left elements of i are all zeros
- right elements of j are all twos
- elements between i and j  are all ones including i and j (this is not needed)

initialization: start at cur = 0, i = 0, j = nums.length -1;
maintenance: 
- if nums[cur] = 0, move it to the left of i, so swap with i and then move i to the right, then move cur to the right by one too. 
- if nums[cur] = 2, move it to the right of j, so swap with j and then move j to the left.  
- if nums[cur] = 1, ignore i or j, move cur to the right by one. 
termination: cur > j

```java 
  public void sortColors(int[] nums) {
        int i = 0, j = nums.length -1, cur = 0; 
        while (cur <= j) {
            if (nums[cur] == 2) {
                swap(nums, cur, j);
                j--;
                continue;
            }
            if (nums[cur] == 0) {
                swap(nums, cur, i);
                i++;
                cur++;
                continue;
            }
            cur++;
        }
    }
    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
```
##Subsets
https://leetcode.com/problems/subsets/
###solutions
- backtracking (needs review)
- dps, each level with or without the current element
```java 
class Solution {
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    public List<List<Integer>> subsets(int[] nums) {
       dps(nums, 0, new ArrayList<Integer>()); 
        return res;
    }
    private void dps(int[] nums, int idx, List<Integer> list) {
        if (idx == nums.length) { 
            res.add(list);
            return;
        }
        
        List<Integer> copy = new ArrayList<Integer>(list); 
        dps(nums, idx + 1, copy);
        
        List<Integer> copy2 = new ArrayList<Integer>(list); 
        copy2.add(nums[idx]);
        dps(nums, idx + 1, copy2);
    }
}
```
##Word search
https://leetcode.com/problems/word-search/
### solutions
- mark visited grid '#' and then change it back after dps 
- pruning before dps would save time: 
   - check if word's length is longer than total number of elements in grid
   - check if there's any element that's not in the grid
```java 
class Solution {
    public boolean exist(char[][] board, String word) {
        int m = board.length;
        int n = board[0].length;
        if (word.length() > m * n) return false; 
        HashSet<Character> uniqueChars = new HashSet();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                uniqueChars.add(board[i][j]);
            }
        }
        for (int i = 0; i < word.length(); i++) {
            if (!uniqueChars.contains(word.charAt(i))) {
                return false;
            }
        } 
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == word.charAt(0)) {
                    boolean exist = checkWord(board, i, j, word, 0);
                    if (exist) return true;
                }
            }
        }
        return false;
    }
    private boolean checkWord(char[][] board, int i, int j, String word, int idx) {
        if (i < 0 || j < 0 || i >= board.length || j >= board[0].length) return false;
           if (board[i][j] == word.charAt(idx)){
               if (idx == word.length() - 1) {
                   return true; 
               } 
               board[i][j] = '#';
               boolean up = checkWord(board, i - 1, j, word, idx + 1);
               boolean left =  checkWord(board, i, j - 1, word, idx + 1);
               boolean down =  checkWord(board, i + 1, j, word, idx + 1);
               boolean right = checkWord(board, i, j + 1, word, idx + 1);
               board[i][j] = word.charAt(idx);
               return up || left || down || right; 
               
           } 
        return false;
    }
}
```

## Remove duplicates from sorted array II
https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/
###Solutions
- copy the last two elements of its kind instead of the first two because it might be overriden. 
```java 
class Solution {
    public int removeDuplicates(int[] nums) {
        int i = 0, j = 0;
        for (; j < nums.length; j++) {
           if (j + 2 >=  nums.length || nums[j] != nums[j + 2]) {
               nums[i] = nums[j];
               i++;
           } 
        }
        return i;
    }
}
```

#04/08/2022

##search in rotated sorted array
https://leetcode.com/problems/search-in-rotated-sorted-array/
###solutions
- S1. find the pivot first, then binary search one of them
- S2. one binary search, add more conditions to move left or right

```java 
class Solution {
    public int search(int[] nums, int target) {
        int m = nums.length;
       if (m == 0) return -1;
       int first = nums[0]; 
        if (target == first) return 0;
        
        int start = 0, end = m - 1;
        int last = 0;
        while (start <= end) {
           int mid = start + (end - start) / 2; 
            if (nums[mid] == target) return mid;
            if (mid + 1 < m && nums[mid] > nums[mid + 1]) {
                last = mid;
                break;
            } 
            if (nums[mid] >= first){
               start = mid + 1; 
            } else {
               end = mid - 1; 
            }
        }
        // first half
        if (target > first) {
           return binarySearch(nums, 0, last == 0 ? m - 1 : last, target);
        } else { // last half
           return binarySearch(nums, last + 1, m - 1, target);
        }
    }
    private int binarySearch(int[] nums, int l, int r, int target) {
        while (l <= r) {
           int mid = l + (r - l) / 2; 
            if (nums[mid] == target) return mid; 
            if (nums[mid] > target) {
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return -1;
    }
}
```
```java 
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length-1;
        while(left<=right)
        {
            int mid = left + (right-left)/2;

            if(target == nums[mid])
            {
                return mid;
            }
            if(nums[mid] <= nums[right])
            {
                if(target>nums[mid]&&target<=nums[right])
                {
                    left = mid +1;
                }else{
                    right = mid -1;
                }
            }else{
                if(target<nums[mid]&&target>=nums[left]){
                    right = mid - 1;
                }else{
                    left = mid + 1;
                }
            }
        }
        return -1;
    }
}
```

##Search in Rotated Sorted Array II
https://leetcode.com/problems/search-in-rotated-sorted-array-ii/
###Solutions
- same as the first one except when trying to locate pivot, check which side the pivot it located in by checking to the right or left  
```java 
class Solution {
   public boolean search(int[] nums, int target) {
        int m = nums.length;
       if (m == 0) return false;
       int first = nums[0]; 
        if (target == first) return true;
        
        int start = 0, end = m - 1;
        int last = 0;
        while (start <= end) {
           int mid = start + (end - start) / 2; 
            if (nums[mid] == target) return true;
            if (mid + 1 < m && nums[mid] > nums[mid + 1]) {
                last = mid;
                break;
            } 
            if (nums[mid] == first) {
                int count = mid + 1;
                while(count < m && nums[count] == nums[mid]) {
                    count++;
                }
                if (count == m) {
                   end = mid - 1; 
                } else {
                   start = mid + 1;
                }
            } else if (nums[mid] > first){
               start = mid + 1; 
            } else {
               end = mid - 1; 
            }
        }
        // first half
        if (target > first) {
           return binarySearch(nums, 0, last == 0 ? m - 1 : last, target);
        } else { // last half
           return binarySearch(nums, last + 1, m - 1, target);
        }
    }
    private boolean binarySearch(int[] nums, int l, int r, int target) {
        while (l <= r) {
           int mid = l + (r - l) / 2; 
            if (nums[mid] == target) return true; 
            if (nums[mid] > target) {
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return false;
    }
}
```

## Remove Duplicates from Sorted List
https://leetcode.com/problems/remove-duplicates-from-sorted-list/
###Solutions
- two pointer, prev and cur
- loop invariants: 
   - everything left to prev including prev are all unique
   - move cur to the right without connecting if it's the same as prev
    
## Remove Duplicates from Sorted List II
https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/solution/
###Solutions
- Sentinel head solution: dummy head, because we are not sure what's the head, so we check which is head first, then append it to the dummy head. 
- loop invariants: 
    - everything left to last are correct answer
    - set last.next to the first element of its kind, and then override it if it's not the only element of its kind  
    - move "last" if current element is different than the last element

```java 
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        //loop invariants: cur 
        if (head == null || head.next == null) return head;
        ListNode dummy = new ListNode(0, head);
        ListNode last = dummy;
        ListNode cur = head;
        while (cur != null) {
            // if two are the same
            if (cur.next != null && cur.val == cur.next.val) {
                // skip all duplicates
                while (cur.next != null && cur.val == cur.next.val) {
                    cur = cur.next;
                }
                last.next = cur.next;
            } else { // two are not the same
                last = last.next; 
            } 
            // must have
            cur = cur.next;
        }
        return dummy.next;
    }
}
``` 

## subsets ii
https://leetcode.com/problems/subsets-ii/submissions/
### Solutions
- skip the dps level for the same elements. so for duplicated levels, add different number of elements there
- trick: set count to 1, and loop at least 0 and 1. if ther's more, loop extra. 
```java 
class Solution {
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);    
        dps(nums, 0, new ArrayList<Integer>());
        return res;
    }
    
    private void dps(int[] nums, int idx, List<Integer> list) {
        int m = nums.length;
        if (idx == m) {
            res.add(list);
            return;
        }    
        int count = 1;
        while (idx+count < m && nums[idx] == nums[idx + count]) {
            count++;
        }
        List<Integer> copy = new ArrayList<Integer>(list);
        for (int i = 0; i <= count; i++) {
            dps(nums, idx + count, copy);
            copy = new ArrayList<Integer>(copy);
            copy.add(nums[idx]);
        }
    }
}
```
## Reverse LinkedList II
https://leetcode.com/problems/reverse-linked-list-ii/

trick part, remember the element before the reversal, and also the element where the reversal stops. 

###Solutions
- use dummy head, because we don't know the new head ahead of time
- record "newHead" and "newTail" and "before" 
```java 
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
      ListNode dummy = new ListNode();
        dummy.next = head;
      ListNode before = head;
       ListNode cur = head; 
        int count = 1;
        while (cur != null) {
           if (count == left - 1) {
               before = cur; 
           } else if (count == left) {
                ListNode newHead = head; 
                ListNode newTail = head; 
               ListNode reverseHead = cur; 
               newTail = cur;
               ListNode prev = null; 
               while (count <= right) {
                   if (count == right) {
                     newHead = reverseHead;       
                   }
                  ListNode next = reverseHead.next; 
                   reverseHead.next = prev;
                   prev = reverseHead;
                  count++; 
                  reverseHead = next;
               }
               newTail.next = reverseHead;
               if (left == 1) {
                  dummy.next = newHead; 
               } else {
                  before.next = newHead; 
               }
               return dummy.next;
           } 
            
           cur = cur.next;
           count++; 
        }
        return dummy.next;
    }
}
```

## Restore IP Address
https://leetcode.com/problems/restore-ip-addresses/
### Solutions
- backtrack (my prefered dps way)

```java 
class Solution {
    List<String> res = new ArrayList<String>();
    public List<String> restoreIpAddresses(String s) {
       char[] chars = s.toCharArray(); 
        dps(chars, 0, 1, "");
        return res;
    }
    private void dps(char[] chars, int idx, int count, String s) {
        int m = chars.length;
       if (count == 4) {
           if (idx >= m) return;
           int lastNumber = 0;
           if (Character.getNumericValue(chars[idx]) == 0 && idx != m-1) return; 
            while (idx < m) {
                int tempNum = Character.getNumericValue(chars[idx]);
                lastNumber = lastNumber * 10 + tempNum; 
                idx++;
                if(lastNumber > 255) return;
            }
           res.add(s + lastNumber); 
           return;
       } 
        if (idx == m) return;
        //if (!Character.isDigit(chars[idx])) return;
        int num = Character.getNumericValue(chars[idx]);
        if (num == 0) {
           dps(chars, ++idx, count+1, s  + num + ".");
            return;
        };
        while (idx < m && num <= 255) {
           dps(chars, ++idx, count+1, s  + num + ".");
            if (idx >=m) return;
            int tempNum = Character.getNumericValue(chars[idx]);
            num = num * 10 + tempNum; 
        }
        
    }
}
```

## Flip String to Monotone increasing
https://leetcode.com/problems/flip-string-to-monotone-increasing/
##Solutions
- s1. check how many 0s before element and how many zeros after element i using prefix sum
- s2. very tricky thought process, dp thought process too
    - if current element is 1
        - not flip it, stays 1, so flips[i] = flips[i - 1] 
        - flip it to 0, flips = flips[i] = previous ones + 1
        - get min
    - if current element is 0
        - not flipping it, stay 0, flips[i] = flips[i - 1]
        - flip it to 1, then flips[i] = flips[i - 1] + 1, because previous doesnt' need to change. 
        - get min

```java 
class Solution {
    public int minFlipsMonoIncr(String s) {
        int one = 0;
        int flip =0;
        for(int i=0;i<s.length();i++)
        {
            if(s.charAt(i)=='1') {
                one++;
            } else {
                flip++;
            }
            flip = Math.min(one,flip);
        }
        return flip;
    }
}
```

##Count Unique Characters of All Substrings of a Given String
https://leetcode.com/problems/count-unique-characters-of-all-substrings-of-a-given-string/

### Solutions
- S1. count contributions between last two same character: https://leetcode.com/problems/count-unique-characters-of-all-substrings-of-a-given-string/discuss/128952/C%2B%2BJavaPython-One-pass-O(N)
- S2. remember lastIndex and contribution of each character: https://leetcode.com/problems/count-unique-characters-of-all-substrings-of-a-given-string/discuss/129021/O(N)-Java-Solution-DP-Clear-and-easy-to-Understand

## Sum of Subarray Ranges
https://leetcode.com/problems/sum-of-subarray-ranges/
### Solutions
- S1. O(n^2), for for loop 
- S2. O(n) (to be reviewed) https://leetcode.com/problems/sum-of-subarray-ranges/discuss/1624222/JavaC%2B%2BPython-O(n)-solution-detailed-explanation

#04/10/2022

## LRU Cache
https://leetcode.com/problems/lru-cache/
### Solutions
- Node has "prev" and "next" pointer so the removal is easier
- Have two dummy node "head" and "tail", change everything in between so that we don't have check if it's the head or tail
- separate methods to smaller ones
    - popHead
    - appendToEnd
    - removeNode
```java 
class LRUCache {
    class Node {
       int key = 0, val = 0; 
        Node next, prev;
        public Node (int key, int val) {
            this.key = key;
            this.val = val;
        }
        public Node () {
            this(0, 0);
        }
    }
    HashMap<Integer, Node> map;
    int capacity = 0;
    Node head, tail;
    
    public LRUCache(int capacity) {
        this.capacity = capacity;
       this.map =  new HashMap();
        this.head = new Node();
        this.tail = new Node();
        head.next = tail;
        tail.prev = head;
    }
    
    public int get(int key) {
       if(!map.containsKey(key)) return -1;
       Node node = map.get(key);
        moveNodeToEnd(node);
        return node.val;
    }
   
    
    public void put(int key, int value) {
        if (map.containsKey(key)) {
            Node node = map.get(key);
            node.val = value;
            moveNodeToEnd(node);
            return;
        }
       if (map.size() == capacity) removeHead(); 
        Node node = new Node(key, value);
        map.put(key, node);
       appendNode(node); 
    }
    private void moveNodeToEnd(Node node) {
        removeNode(node);
        appendNode(node);
    }
    private void removeHead() {
        head = head.next; 
        head.prev = null;
        map.remove(head.key);
        head.key = 0;
        head.val = 0;
    }
    private void appendNode(Node node) {
        Node prev = tail.prev;
        
        prev.next = node;
        node.prev = prev;
        
        tail.prev = node;
        node.next = tail;
    }
    private void removeNode(Node node) {
        Node prev = node.prev;
        Node next = node.next;
        prev.next = next;
        next.prev = prev;
        
        node.next = null;
        node.prev = null;
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

## Count Binary Substrings
https://leetcode.com/problems/count-binary-substrings/

###Solutions
- count ones and zeros, take min of the two consecutive groups

```java 
class Solution {
    public int countBinarySubstrings(String s) {
        if (s.length() == 0) return 0;
        
        int prev = 0;
        int cur = 1;
        int res = 0;
        for (int i = 1; i < s.length(); i++) {
            if (s.charAt(i) == s.charAt(i - 1)) {
                cur++;
            } else {
                res += Math.min(prev, cur);
                prev = cur;
                cur = 1;
            }
        }
        return res + Math.min(prev, cur);
    }
}
```

## The kth Factor of n
https://leetcode.com/problems/the-kth-factor-of-n/
###Solutions
- O(sqrt(N))
- S1. remember both halves. 
- S2. check if the k is in the first half, only save divisors
```java 
class Solution {
    public int kthFactor(int n, int k) {
        int count = 1;
        List<Integer> secondHalf = new ArrayList();
        int i = 1;
        int j = n;
        do {
           int mod = n % i; 
            if (mod == 0) {
                if (k == 1) {
                  return i;    
                }
                j = n / i;
                if (i != j) {
                  secondHalf.add(j);  
                } 
                k--;
            }
            i++;
        } while (i < j);
        int size = secondHalf.size();
        if (k > size) return -1;
        return secondHalf.get(size - k);
    }
}
```
```java 
class Solution {
    public int kthFactor(int n, int k) {
        int count = 1;
        List<Integer> firstHalf = new ArrayList();
        List<Integer> secondHalf = new ArrayList();
        int i = 1;
        int j = n;
        do {
           int mod = n % i; 
            if (mod == 0) {
               firstHalf.add(i); 
                j = n / i;
                if (i != j) {
                    secondHalf.add(j);
                }
            }
            i++;
        } while (i < j);
        int size1 = firstHalf.size();
        if (k <= size1) return firstHalf.get(k - 1);
        int size2 = secondHalf.size();
        if (k > size1 + size2) return -1;
        return secondHalf.get(size2 - (k - size1));
    }
}
```