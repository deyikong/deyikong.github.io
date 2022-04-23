---
title: Roblox
date: 2022-04-22 18:40:16
tags:
---

## Design Browser History
https://leetcode.com/problems/design-browser-history/
### Solutions
- two stacks
    - trick, while loop two conditions (steps > 0 && back.size() > 1)
- doubly linked list (trickier to write)

```java 
class BrowserHistory {
    Stack<String> back;
    Stack<String> forward;

    public BrowserHistory(String homepage) {
       back = new Stack(); 
       forward = new Stack(); 
        back.add(homepage);
    }
    
    public void visit(String url) {
       forward.clear(); 
        back.add(url);
    }
    
    public String back(int steps) {
       String page = ""; 
        while (steps > 0 && back.size() > 1) {
            page = back.pop();
            forward.add(page); 
            steps--;
        }
        return back.peek();
    }
    
    public String forward(int steps) {
        String page = "";
        while(steps > 0 && !forward.isEmpty()) {
            page = forward.pop();
            back.add(page);
            steps--;
        } 
        return back.peek();
    }
}
```

## Text Justification 
https://leetcode.com/problems/text-justification/
### Solutions
- Single Responsibility Principle (split into smaller functions)
- It's a special case when there's only one word in the row
- last row is different too, because it needs to split into two parts, one appending 1 space, one append all the rest of the spaces
```java my own solution
class Solution {
    public List<String> fullJustify(String[] words, int maxWidth) {
        if (words.length == 0) return new ArrayList<String>();
        List<String> res = new ArrayList<String>();
        int left = 0, right = 0, wordCharLength = 0;
        
        while (right < words.length) {
            int wordLength = words[right].length();
            int rowLengthWithSpaces = wordCharLength + (right - left) - 1;
            // if previous range plus current word exceeds max width, justify the previous range, and reset the range start
            if (rowLengthWithSpaces + 1 + wordLength > maxWidth) {
                res.add(justify(words, left, right - 1, wordCharLength, maxWidth - rowLengthWithSpaces + (right - left) - 1, maxWidth));
                left = right;
                wordCharLength = 0;
            } 
            right = right + 1;
            wordCharLength += wordLength;
        }
        
        // add last range to the solution
        res.add(lastRowConverting(words, left, words.length - 1, maxWidth));
        return res;
    }
    // converting last row includes two parts, 
    // 1. everything before the last word should be concatinate with 1 space
    // 2. last word should be treated using a normal justify function with a shorter maxWidth
    private String lastRowConverting(String[] words, int left, int right, int maxWidth) {
        StringBuilder lastRow = new StringBuilder();
        // if there's more than one word, 
        // 1. append these words with a space to the answer, 
        // 2. maxWidth decrease by the size of the word + space 
        while (left < right) {
            lastRow.append(words[left] + " ");    
            maxWidth -= (words[left].length() + 1);
            left++;
        }
        int lastWordLength = words[right].length();
        // justify last word
        lastRow.append(justify(words, left, right, lastWordLength, maxWidth - lastWordLength, maxWidth));
        return lastRow.toString();
    }
    private String justify(String[] words, int left, int right, int charLength,  int totalSpaces, int maxWidth) {
        // space slots
        int slots = right - left; 
        // if there's no slots, that means it's one word, so each spacePerSlot should be totalSpace
        int spacePerSlot = slots == 0 ? totalSpaces : totalSpaces / slots;
        // only valid if there's more than one slots
        int spaceDiff = slots == 0 ? 0 : totalSpaces % slots;
        StringBuilder sb = new StringBuilder();
        while (left <= right) {
            sb.append(words[left]); 
            // if there's more than one slot, don't append space to the last word
            int spacesAfterThisWord = (slots != 0 && left == right) ? 0 : spacePerSlot + (spaceDiff > 0 ? 1 : 0); 
            for (int j = 0; j < spacesAfterThisWord;j++) {
                sb.append(" ");
            }
            left++;
            spaceDiff--;
        } 
        return sb.toString();
    }
}
```

## Word Search 
https://leetcode.com/problems/word-search/
### Solutions
- **use existing board instead of extra space, mark visited cell "#"**
- check if there's char that's not in the board
- check if the word's length is longer than total num of chars
- check 4 directions at each cell. 

```java 
class Solution {
    public boolean exist(char[][] board, String word) {
        int m = board.length;
        int n = board[0].length;
        if (word.length() > m * n) return false;
        HashSet<Character> set = new HashSet();
        for (char[] cs : board) 
            for (char c : cs)
                set.add(c); 
        
        for (char c : word.toCharArray()) 
            if (!set.contains(c)) return false;
        
        //actual traverse
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == word.charAt(0)) {
                    boolean res = dfs(board, word, 0, i, j);
                    if (res) return true;
                }
            }
        }
       return false; 
    }
    private boolean dfs(char[][] board, String word, int idx, int i, int j) {
        int m = board.length;
        int n = board[0].length;
       if (i < 0 || i >= m || j < 0 || j >= n || board[i][j] != word.charAt(idx)) return false; 
       if (idx == word.length() - 1) return true;
        
        board[i][j] = '#';
        boolean up = dfs(board, word, idx + 1, i - 1, j);
        boolean down = dfs(board, word, idx + 1, i + 1, j);
        boolean left = dfs(board, word, idx + 1, i, j - 1);
        boolean right = dfs(board, word, idx + 1, i, j + 1);
        board[i][j] = word.charAt(idx);
        return up || down || left || right;
    }
}
```