---
title: Thoughts on OOP
date: 2022-04-27 20:13:43
tags: 
    - OOP
    - interview
    - technical
    - leetcode
categories:
    - algorithms
excerpt: 'Object Oriented Programming Guide'
photos: 
    - images/oop.jpeg

---

# How to approach OOP interview questions

If you have problems with OOP technical interviews, here are some pointers on how to approach this kind of problems. 

1. outline (bottom up): write all the classes/components from small scope to big scope
2. implementation (top down): write all method signatures from big scope to small scope.  
  - for the biggest class, start with the constructor, properties and then methods from the requirements(use cases, properties). 
  - for the smaller classes, requirements are from their upper classes' implementation details + initial requirements(use cases, properties). 
3. changes (bottom up): if there's any structural changes(deletion, addition, modification) on a particular part of the code, trace all places that used these changed part bottom up.   

For example, designing an Excel, each cell has it's own styles(font, size, color, etc), each cell can have different types of data(integer, string, boolean, etc), users can append or insert rows or columns
## First step: outline(bottom up)：
``` java 
class Cell<T> {}
class Row {}
class Sheet {}
```
## Second Step: implementation (top down):
### start filling the classes from top down


Start with the Sheet's constructor and some properties, on first thought, we just need an empty constructor 
```java 
public Sheet () {
}
```
However, because we might need the dimension of the sheet upon constructing, either passed by user or by default. so we need a list of rows first, and the global column.
```java 
List<Row> sheet = new ArrayList<Row>();
int column;

public Sheet () {
 this(20, 20);
}

public Sheet (int row, int column) {
this.column = column; 
 for (int i = 0; i < row; i++) {
     sheet.add(new Row(column));
 }
}
```

Notice, after we finished up the code above, we already added more requirements for the subclass `Row` like `new Row(column)`.  

Then based on the requirements, we have `appendRow`, `insertRow`, `appendColumn`, `insertColumn` 4 functions for sheet, let's implement them. 
```java 
 public void appendRow() {
     sheet.add(new Row(column));
 }
 public void insertRow(int idx) {
     sheet.add(idx, new Row(column));
 }
 
 // insert and append Col
 public void appendColumn() { 
     for (int j = 0; j < sheet.size(); j++) {
         sheet.get(j).add(new Cell(""));
     }
     this.column++; 
 }
 public void insertColumn(int idx) {
    for (Row row : sheet) {
        row.add(idx, new Cell(""));
    } 
     this.column++;
 }
```
So based on code above, we can conclude the Cell's constructor signature `new Cell("")`, and also `Row`'s method to add a cell `row.add(new Cell("))`

Now, let's move to the next level of class, which is `Row`. 
First, let's start with the constructor, based on the codes we added to `Sheet`'s class, we already have our signature. 
```java
List<Cell> row = new ArrayList<Cell>();
public Row (int column) {
    for (int i = 0; i < column; i++)
        row.add(new Cell(""));
}
``` 
and we also know we need `add` functions for `Row` class too, one for `appendRow`, another for `insertRow`. 
```java 
public void add(Cell cell) {
   row.add(cell); 
}
public void add(int idx, Cell cell) {
   row.add(idx, cell); 
}
```
That's it for now for `Row`

Lastly(for now), we need to complete our `Cell`.
Last also start with the constructor, we already know the signature from the code above. 
```java 
T content;
public Cell (T content) {
        this.content = content;
}
```
Because we want to add font, size, color, properties to each `Cell`, let add these here
```java 
String font = "Hei" ;
double size = 12;
boolean bold = false;
```

That's pretty much it if that's all we need to do. 

However, if we think a little deeper or more realistic, we need a way to update each cell's data, so let's do that too. 

Let's go top down again from Sheet. 
```java add to Sheet
 public void update(int row, int column, String content) {
     sheet.get(row).set(column, new Cell<String>(content));
 }
 public void update(int row, int column, Integer content) {
     sheet.get(row).set(column, new Cell<Integer>(content));
 }
 public void update(int row, int column, Boolean content) {
     sheet.get(row).set(column, new Cell<Boolean>(content));
 }
``` 

This, in turns, determines one function `set` for `Row`, so let's do that. 

```java add to Row 
public void set(int col,Cell cell) {
    row.set(col, cell); 
}
```

That's it, if we want to update cells. 

## Third Step: Changes (bottom up)
### In our current situation, we have `font`, `size` and `bold` in each `Cell`, however, what if we want to select a range of cells and apply the same style to these Cells. so that requires changes in our `Cell` class, so let's create a `Style` class that's shared across more than one Cell. 

```java 
class Cell<T> {
    Style style;
    T content;
   
    public Cell (T content) {
        this(content, new Style());
    }
    public Cell (T content, Style style) {
        this.content = content;
        this.style = style;
    }
}
class Style {
    String font = "Hei" ;
    double size = 12;
    boolean bold = false;
}
``` 
Noticed that we added a style as a constructor param to `Cell`, you don't have to add it to the constructor if you want to create a setter(in java) or a property(in C#) or a variable (in PHP). 

Now, because we changed the cell, we need to go bottom-up styles to fix all the places it sifts up to. Let's keep a mental queue to add what we need to modify next (just like BFS*). 

So in `Row`'s constructors, we have two `new` statements, let's change that. 
```java 
public Row (int column) {
    for (int i = 0; i < column; i++)
        row.add(new Cell(""));
}
public Row (int column, Style style) {
    for (int i = 0; i < column; i++)
        row.add(new Cell("", style));
}
```
We kept the previous constructor and overloaded a new constructor with a `Style` object for the whole row
Let's keep looking up the usage of `Cell` while adding `Row` to the queue to look up next. 
We found `appendColumn`, `insertColumn` and all the `update`s in `Sheet` that use `Cell`s constructor, let's change that. 
```java 
 // insert and append Col
 public void appendColumn() { 
     for (int j = 0; j < sheet.size(); j++) {
         sheet.get(j).add(new Cell("", style));
     }
     this.column++;
 }
 public void insertColumn(int idx) {
    for (Row row : sheet) {
        row.add(idx, new Cell("", style));
    } 
     this.column++;
 }
// update 
 public void update(int row, int column, String content) {
     Style s = sheet.get(row).get(column).style;
     sheet.get(row).set(column, new Cell<String>(content, s));
 }
 public void update(int row, int column, Integer content) {
     Style s = sheet.get(row).get(column).style;
     sheet.get(row).set(column, new Cell<Integer>(content, s));
 }
 public void update(int row, int column, Boolean content) {
     Style s = sheet.get(row).get(column).style;
     sheet.get(row).set(column, new Cell<Boolean>(content, s));
 }
```
Notice that when we update, we keep the old style and assign it to the new cell created. 

Also, this means we need to have a global `style` in the sheet, and initialize the style in the constructor. 
```java 
 Style style;
 public Sheet () {
     this(20, 20);
 }
 public Sheet (int row, int column) {
    this.column = column; 
     style = new Style();
     for (int i = 0; i < row; i++) {
         sheet.add(new Row(column));
     }
 }
```
After that, we are done with `Cell`'s changes(bottom up), but we are done yet, remember, we added the `Row`'s constructor to the queue, now let's look at the places where `Row`'s constructor is used. 

We found the usages in `Sheet`'s constructor, `appendRow` and `insertRow`. 
For the `Sheet`'s constructor, either we update it to `sheet.add(new Row(column, style))` or just keep what it is if you don't care about the extra same styles created for each `Cell`. A better solution is sharing the same style, so let's do that. 
```java 
 public Sheet (int row, int column) {
    this.column = column; 
     style = new Style();
     for (int i = 0; i < row; i++) {
         sheet.add(new Row(column, style));
     }
 }
```

for `appendRow`, and `insertRow`, let's change them. 
```java 
 // insert and append Row
 public void appendRow() {
     sheet.add(new Row(column, style));
 }
 public void insertRow(int idx) {
     sheet.add(idx, new Row(column, style));
 }
```
Since this is the biggest class, they are not used anywhere, so we don't need to add the changes to the queue(that's not always the case though). 
So the queue is empty now, so that means we are done with the effect by extracting styles to a `Style` object. 

Viola, that's it. 

If you want to read the complete code, here's the [link](https://leetcode.com/playground/JrKZZcjh), I also added codes to apply styles to a range of cells over there. 

**Notes** *Overall programming habit, BFS: don't program too deep when there's a thought, just go down 1 step down and just write the signature and keep writing. 