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