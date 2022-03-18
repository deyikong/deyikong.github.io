---
title: Binary Search Tree
date: 2022-03-18 01:48:05
tags:
---

## Binary Search Tree

### Keys
- pre order traverse 
- in order traverse -> sorted array
- post order traverse 

### Searching
Recursive
```
TREE-SEARCH(x, k)
    if x == NIL or k == x.key
        return x
    if x < x.key
        return TREE-SEARCH(x.left, k)
    else return TREE-SEARCH(x.right, k)
``` 
Iterative
```
ITERATIVE-TREE-SEARCH(x, k)
    while x != NIL or k != x.key
        if k < x.key
            x = x.left
        else x = x.right
    return x 
``` 
### minimum and maximum  
```
TREE-MINIMUM(x)
    while x != NIL AND x.left != NIL
        x = x.left
    return x 
```
```
TREE-MAXIMUM(x)
    while x != NIL AND x.right != NIL
        x = x.right
    return x 
```
 ### successor and predecessor
 
 Successor
```
TREE-SUCCESSOR(x)
    // if this node has a right subtree, just return the minimum in the right subtree
    if x!= NIL AND x.right != NIL
        return TREE_MINIMUM(x)
    // if this node doesn't have a right subtree, go up and find the first parent that's the left child of its parent 
    y = x.parent
    while y != NIL AND y.right = x
        x = y
        y = y.plarent
    return y
```
 predecessor
```
TREE-PREDECESSOR(x)
    // if this node has a left subtree, just return the maximum in the left subtree
    if x!= NIL AND x.left != NIL
        return TREE_MINIMUM(x)
    // if this node doesn't have a left subtree, go up and find the first parent that's the right child of its parent 
    y = x.parent
    while y != NIL AND y.left = x
        x = y
        y = y.plarent
    return y
```

####insertion
The key point is to find the NIL location, keep the parent, and then append it to the parent
loop invariant: p is always the parent of x
- maintain p is the parent of x 
- termination: x is NIL
```
TREE-INSERT(T, z)
    p = NIL
    x = T.root
    while x != NIL
        p = x.parent
        if z.key > x.key
            x = x.right
        else x = x.left
    if p = NIL 
        T.root = z // Tree is empty
    elseif z.key > p.key
        p.right = z
    else p.left = z
```
