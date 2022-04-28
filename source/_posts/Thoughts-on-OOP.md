---
title: Thoughts on OOD
date: 2022-04-27 20:13:43
tags: 
    - OOP
    - interview
---

## How to approach OOP interview questions

I was at a friend's place three days ago, and we were talking uncertain OOP technical interviews are. 

Then we sat down and went through an OOP problem and conclude the following three steps to offer ideas to approach OOP interviews. 
1. outline (bottom up): write all the classes/components from small scope to big scope
2. implementation (top down): write all method signatures from big scope to small scope.  
  - for the biggest class, start with the constructor, properties and then methods from the requirements(use cases, properties). 
  - for the smaller classes, requirements are from their upper classes' implementation details + initial requirements(use cases, properties). 
3. changes (bottom up): if there's any structural changes(deletion, addition, modification) on a particular part of the code, trace all places that used these changed part bottom up.   


Overall programming habit, BFS: don't program too deep when there's a thought, just go down 1 step down and just write the signature and keep writing. 