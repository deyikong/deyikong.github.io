---
title: Problems
date: 2022-04-05 11:32:04
tags:
---

# 04/05/2022
## Longest Common Prefix 
https://leetcode.com/problems/longest-common-prefix/
### Solutions:
- horizontal scanning(from left to right)
- vertical scanning(compare the ith char on all the elements each time)
- divide and conquer 
- Binary search
### Follow up
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

### Solutions
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

## Jump game II
https://leetcode.com/problems/jump-game-ii/
### Solutions
- dp N^2
- BFS, N 
- greedy

# 04/06/2022

## Permutations
https://leetcode.com/problems/permutations/
### Solutions
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

### Solutions
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
### Solutions
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


## unique paths
https://leetcode.com/problems/unique-paths/
### Solutions
- dp
- basic math: choose m - 1 or n - 1 from m - n - 2

## unique paths II (with obstacles)
https://leetcode.com/problems/unique-paths-ii/
### solutions
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

## Minimum path sum
https://leetcode.com/problems/minimum-path-sum/
### Solutions
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

## Set Matrix Zeros
https://leetcode.com/problems/set-matrix-zeroes/
### solutions
- use two sets
- use first column and first row as marker

## search a 2d matrix
https://leetcode.com/problems/search-a-2d-matrix/
### Solutions
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
### Solutions
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
## Subsets
https://leetcode.com/problems/subsets/
### solutions
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
## Word search
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
### Solutions
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

## search in rotated sorted array
https://leetcode.com/problems/search-in-rotated-sorted-array/
### solutions
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

## Search in Rotated Sorted Array II
https://leetcode.com/problems/search-in-rotated-sorted-array-ii/
### Solutions
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
### Solutions
- two pointer, prev and cur
- loop invariants: 
   - everything left to prev including prev are all unique
   - move cur to the right without connecting if it's the same as prev
## Remove Duplicates from Sorted List II
https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/solution/
### Solutions
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

### Solutions
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
## Solutions
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

## Count Unique Characters of All Substrings of a Given String
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

### Solutions
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
### Solutions
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

## valid palindrome II
https://leetcode.com/problems/valid-palindrome-ii/
### Solutions
- try both substrings generated by deleting each of the mismatched pair
```java 
class Solution {
    public boolean validPalindrome(String s) {
        int i = 0; 
        int j = s.length() - 1;
        int count = 1;
        while (i < j) {
           if (s.charAt(i) != s.charAt(j)) {
              return checkPalindrom(s, i, j-1) || checkPalindrom(s, i+1, j);
           } 
           i++; 
           j--;
        }
        return true;
    }
    private boolean checkPalindrom(String s, int i, int j) {
        while (i < j) {
            if (s.charAt(i) != s.charAt(j)) {
                return false;
            }
            i++;
            j--;
        }
        return true;
    }
}
```
## Maximum Units on a Truck
https://leetcode.com/problems/maximum-units-on-a-truck/

### Solutions
- sort the array and pick from the bigger box first.  
```java 
class Solution {
    public int maximumUnits(int[][] boxTypes, int truckSize) {
        // sort boxTypes based on num of units per box
        Arrays.sort(boxTypes, (int[] a, int[] b) -> b[1] - a[1]);
        int remaining = truckSize;
        int total = 0;
        for (int i = 0; i < boxTypes.length; i++) {
            if (remaining <= 0) return total;
            total += Math.min(remaining, boxTypes[i][0]) * boxTypes[i][1]; 
            remaining -= boxTypes[i][0];
        }
        return total;
    }
}
```

##  Find Winner on a Tic Tac Toe Game
https://leetcode.com/problems/find-winner-on-a-tic-tac-toe-game/
```java 
class Solution {
    public String tictactoe(int[][] moves) {
        int[][]  board = new int[3][3];
        for (int i = 0; i < moves.length; i++) {
            int num = 1;
            // first player
           if (i % 2 == 0)  {
              num = -1; 
           } 
            board[moves[i][0]][moves[i][1]] = num;
        }
        // check rows
        for (int i = 0; i < 3; i++) {
           int num = board[i][0]; 
            if (num == 0) continue;
            boolean win = true;
            for (int j = 1; j < 3; j++) {
               if (board[i][j] != num) {
                   win = false;
                   break;
               } 
            }
            if (win) {
                return num == -1 ? "A" : "B";
            }
        }
        // check cols
        for (int i = 0; i < 3; i++) {
           int num = board[0][i]; 
            if (num == 0) continue;
            boolean win = true;
            for (int j = 1; j < 3; j++) {
               if (board[j][i] != num) {
                   win = false;
                   break;
               } 
            }
            if (win) {
                return num == -1 ? "A" : "B";
            }
        }
        // check diagonal
        if (board[1][1] != 0 &&((board[0][0] == board[1][1] && board[1][1] == board[2][2]) || (board[0][2] == board[1][1] && board[1][1] == board[2][0]))) {
            return board[1][1] == -1 ? "A" : "B";
        }
        if (moves.length < 9) {
            return "Pending";
        }
        return "Draw";
    }
}
```
```java 
class Solution {
    public String tictactoe(int[][] moves) {

        // n stands for the size of the board, n = 3 for the current game.
        int n = 3;

        // Use rows and cols to record the value on each row and each column.
        // diag1 and diag2 to record value on diagonal or anti-diagonal.
        int[] rows = new int[n], cols = new int[n];
        int diag = 0, anti_diag = 0;
        
        // Two players having value of 1 and -1, player_1 with value = 1 places first.
        int player = 1;
        
        for (int[] move : moves){

            // Get the row number and column number for this move.
            int row = move[0], col = move[1];
            
            // Update the row value and column value.
            rows[row] += player;
            cols[col] += player;
            
            // If this move is placed on diagonal or anti-diagonal, 
            // we shall update the relative value as well.
            if (row == col){
                diag += player;
            }
            if (row + col == n - 1){
                anti_diag += player;
            }
            
            // Check if this move meets any of the winning conditions.
            if (Math.abs(rows[row]) == n || Math.abs(cols[col]) == n || 
                Math.abs(diag) == n || Math.abs(anti_diag) == n){
                return player == 1 ? "A" : "B";
            }
            
            // If no one wins so far, change to the other player alternatively. 
            // That is from 1 to -1, from -1 to 1.
            player *= -1;
        }
        
        // If all moves are completed and there is still no result, we shall check if 
        // the grid is full or not. If so, the game ends with draw, otherwise pending.
        return moves.length == n * n ? "Draw" : "Pending";
    }
}
```

## Sum of Subarray Minimums
https://leetcode.com/problems/sum-of-subarray-minimums/
### Solutions
- count the contributions of each element being the smallest element
```java 
class Solution {
    public int sumSubarrayMins(int[] arr) {
        int l = arr.length;
        if (l == 0) return 0;
        Stack<Integer> s = new Stack();
        long res = 0;
        s.add(0); 
        int mod = (int)(Math.pow(10, 9) + 7);
        for (int i = 1; i <= l; i++) {
            while (!s.isEmpty() && arr[s.peek()] > (i == l? 0 : arr[i])) {
                int j = s.pop(); 
                int k = s.isEmpty() ? -1 :s.peek();
                res += (long)arr[j] * (j - k) * (i - j);
                res = res % mod;
            } 
            s.push(i);
        }
        return (int)res;
    }
}
```

## Minimum Swaps to Group All 1's Together
https://leetcode.com/problems/minimum-swaps-to-group-all-1s-together/

### Solutions
- sliding window O(n) time, O(1) space
- prefix sum: O(n) time, O(n) space

``` java
class Solution {
    public int minSwaps(int[] data) {
        int ones = Arrays.stream(data).sum();
        int cnt_one = 0, max_one = 0;
        int left = 0, right = 0;

        while (right < data.length) {
            // updating the number of 1's by adding the new element
            cnt_one += data[right++];
            // maintain the length of the window to ones
            if (right - left > ones) {
                // updating the number of 1's by removing the oldest element
                cnt_one -= data[left++];
            }
            // record the maximum number of 1's in the window
            max_one = Math.max(max_one, cnt_one);
        }
        return ones - max_one;
    }
}
```
``` java My attempt 
class Solution {
    public int minSwaps(int[] data) {
        int l = data.length;
        int[] sum = new int[l]; 
        sum[0] = data[0];
        for (int i = 1; i < l; i++) {
            sum[i] = data[i] + sum[i - 1];
        }
        if (sum[l - 1] < 2 || sum[l - 1] == l) {
            return 0;
        }
        
        int min = Integer.MAX_VALUE;
        for (int i = 0; i <= l - sum[l - 1]; i++) {
            int lastIndex = i + sum[l-1] - 1;
            int localMin = sum[l - 1] - sum[lastIndex] + sum[i];
            if (data[i] == 1) {
                localMin--;
            } 
            min = Math.min(localMin, min);
        }
        return min; 
    }
}
```

##  Maximum Length of Subarray With Positive Product
https://leetcode.com/problems/maximum-length-of-subarray-with-positive-product/
### Solutions
- record the first negative number's index(my solution)
- dry run

``` java My solution 
class Solution {
    public int getMaxLen(int[] nums) {
        int negatives = 0;
        int max = 0;
        int count = 0;
        int firstNegative = -1;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) {
                negatives = 0;
                count = 0;
                firstNegative = -1;
                continue;
            }
            if (nums[i] < 0) {
                if (firstNegative == -1) {
                    firstNegative = i;
                }
                negatives++;
           } 
           if (negatives % 2 == 1 && firstNegative != -1) { // but there's odd number of negatives, count between first negative and current
               max = Math.max(max, i - firstNegative); 
           } else if (negatives % 2 == 0) {
               max = Math.max(max, count + 1); 
           }
            count++; 
        }
        return max;
    }
}
```
``` java dry run https://leetcode.com/problems/maximum-length-of-subarray-with-positive-product/discuss/820072/EASY-soultion-with-DRY-RUN-JAVA
class Solution {
    public int getMaxLen(int[] nums) {
        int positive = 0, negative = 0;    // length of positive and negative results
        int ans = 0;
        for(int x : nums) {
            if(x == 0)  {
                positive = 0;
                negative = 0;
            }
            else if(x > 0) {
                positive++;
                negative = negative == 0 ? 0  : negative+1;
            }
            else {
                int temp = positive;
                positive = negative == 0 ? 0  : negative+1;
                negative = temp+1;
            }
            ans = Math.max(ans, positive);
        }
        return ans;
    }
}
```

## Nested List Weight Sum
https://leetcode.com/problems/nested-list-weight-sum/
### solutions
- DFS
- BFS
```java Coding tricks 
  Queue<NestedInteger> queue = new LinkedList<NestedInteger>(nestedList);
  queue.addAll(ni.getList());
```

## Lowest Common Ancestor with parent pointer 
https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-iii/

### Solutions
- S1. Set
- S2. traverse both paths
```java Set Solution
class Solution {
    public Node lowestCommonAncestor(Node p, Node q) {
        HashSet<Integer> set = new HashSet();
        while (p != null) {
            set.add(p.val);
            p = p.parent;
        }
        while (q != null) {
            if (set.contains(q.val)) return q;
            q = q.parent;
        }
        return q;
    }
}
```
```java travers both paths solution 
class Solution {
    public Node lowestCommonAncestor(Node p, Node q) {
        Node n1 = p;
        Node n2 = q;
        while (n1 != n2) {
            n1 = n1.parent == null ? q : n1.parent;
            n2 = n2.parent == null ? p : n2.parent;
        }
        return n1;
    }
}
```

## Smallest Common Region
https://leetcode.com/problems/smallest-common-region/
### Solutions
- use a tree structure to record the parent of each node(my method)
- use a hashMap to record the parent 
```java 
class Node {
    //List<Node> children = new ArrayList<Node>();
    String val = "";
    public Node(String val) {
        this.val = val;
    }
    Node parent = null;
}
class Solution {
    public String findSmallestRegion(List<List<String>> regions, String region1, String region2) {
        // thoughts: create a tree struction and a set of all tree nodes, then find the lowest common ancestor. 
        
        HashMap<String, Node> map = new HashMap();
        // create a tree auto of the regions. 
        for (List<String> region: regions) {
            Node parentNode = map.getOrDefault(region.get(0), new Node(region.get(0))); 
            map.put(region.get(0), parentNode);
            for (int i = 1; i < region.size(); i++) {
                Node child = map.getOrDefault(region.get(i), new Node(region.get(i)));
                map.put(region.get(i), child);
                //parentNode.children.add(child);
                child.parent = parentNode;
            }
        }
        
        // find the lowest common ancestor 
        Node n1 = map.get(region1);
        Node n2 = map.get(region2);
        HashSet<String> paths = new HashSet();
        while (n1 != null) {
            paths.add(n1.val);
            n1 = n1.parent;
        }
        while (n2 != null) {
            if (paths.contains(n2.val)) return n2.val; 
            n2 = n2.parent;
        }
        return "";
    }
}
```
```java 
class Solution {
    public String findSmallestRegion(List<List<String>> regions, String region1, String region2) {
        HashMap<String, String> map = new HashMap();
        for (List<String> region: regions) {
            for (int i = 1; i < region.size(); i++) {
                map.put(region.get(i), region.get(0));
            }
        }
        
        // find the lowest common ancestor 
        HashSet<String> paths = new HashSet();
        while (region1 != null) {
            paths.add(region1);
            region1 = map.get(region1);
        }
        while (region2 != null) {
            if (paths.contains(region2)) return region2; 
            region2 = map.get(region2);
        }
        return "";
    }
}
```
``` java without set
Map<String, String> parent = new HashMap<>();
for(List<String> rs: regions) {
    for(int j = 1; j < rs.size(); j++) {
        parent.put(rs.get(j), rs.get(0));
    }
}
String p1 = region1, p2 = region2;
while(!p1.equals(p2)) {
    p1 = parent.getOrDefault(p1, region2);
    p2 = parent.getOrDefault(p2, region1);
}
return p1;
```


## Cinema seat allocation
https://leetcode.com/problems/cinema-seat-allocation/submissions/

### Solutions
- only iterate reserved rows
- S1. use a boolean array for each row
- S2. use a set for each row
- S2. use an integer for the row, use bitwise operations

```javascript 1.5
class Solution {
    public int maxNumberOfFamilies(int n, int[][] reservedSeats) {
        HashMap<Integer, boolean[]> reservedRows = new HashMap();
        for (int[] seat: reservedSeats) {
            boolean[] cols = reservedRows.getOrDefault(seat[0] - 1, new boolean[10]);
            cols[seat[1] - 1] = true;
            reservedRows.put(seat[0] - 1, cols);
        }
        //System.out.println(reservedRows.size());
        int result = 2 * (n - reservedRows.size());
        
        for (boolean[] cols : reservedRows.values()) {
            if (!cols[1] && !cols[2] && !cols[3] && !cols[4]) {
                cols[3] = true;
                cols[4] = true;
                result++;
            }
            if (!cols[3] && !cols[4] && !cols[5] && !cols[6]) {
                cols[5] = true;
                cols[6] = true;
                result++;
            }
            if (!cols[5] && !cols[6] && !cols[7] && !cols[8]) {
                result++;
            }
        }
        return result; 
    }
}
```
```java S2
class Solution {
    public int maxNumberOfFamilies(int n, int[][] reservedSeats) {
        HashMap<Integer, HashSet<Integer>> reservedRows = new HashMap();
        for (int[] seat: reservedSeats) {
            HashSet<Integer> cols = reservedRows.getOrDefault(seat[0] - 1, new HashSet<Integer>());
            cols.add(seat[1] - 1);
            reservedRows.put(seat[0] - 1, cols);
        }
        //System.out.println(reservedRows.size());
        int result = 2 * (n - reservedRows.size());
        
        for (HashSet<Integer> cols : reservedRows.values()) {
            if (!cols.contains(1) && !cols.contains(2) && !cols.contains(3) && !cols.contains(4)) {
                cols.add(3);
                cols.add(4);
                result++;
            }
            if (!cols.contains(3) && !cols.contains(4) && !cols.contains(5) && !cols.contains(6)) {
                cols.add(5);
                cols.add(6);
                result++;
            }
            if (!cols.contains(5) && !cols.contains(6) && !cols.contains(7) && !cols.contains(8)) {
                result++;
            }
        }
        return result; 
    }
}
```
```java S3
class Solution {
    public int maxNumberOfFamilies(int n, int[][] reservedSeats) {
        HashMap<Integer, Integer> reservedRows = new HashMap();
        for (int[] seat: reservedSeats) {
            reservedRows.put(seat[0], reservedRows.getOrDefault(seat[0], 0) | (1 << seat[1]));
        }
        int result = 2 * (n - reservedRows.size());
        
        for (int cols : reservedRows.values()) {
            boolean reserved = false;
            if ((cols & 60) == 0) {
                reserved = true;
                result++;
            }
            if ((cols & 960) == 0) {
                reserved = true;
                result++;
            }
            if (!reserved && (cols & 240) == 0) {
                result++;
            }
        }
        return result; 
    }
}
```