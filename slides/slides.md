<!-- .slide: data-background-image="http://sermons.seanho.com/img/bg/unsplash-e6XsI7qqvAA-forest_sunbeam.jpg" -->
# CMPT231
## Lecture 3: ch5-6
### Heaps, Queues, and QuickSort

---
## Outline for today

---
## Summary of sorting algorithms
+ **Comparison** sorts *(ch2, 6, 7)*:
  + **Insertion** sort:  *&Theta;(n^2)*, easy to program, slow
  + **Merge** sort: *&Theta;(n lg n)*, out-of-place copy (slow)
  + **Heap** sort: *&Theta;(n lg n)*, in-place, max-heap
  + **QuickSort**: *&Theta;(n^2)* worst-case, *&Theta;(n lg n)* average,
    fast constant factors
+ **Linear**-time non-comparison sorts *(ch8)*:
  + **Counting** sort: *k* distinct values: *&Theta;(k)*
  + **Radix** sort: *d* digits, *k* values: *&Theta;(d(n+k))*
  + **Bucket** sort: uniform distribution: *&Theta;(n)*

---
## Binary trees
+ **Graph**: collection of *nodes* and *edges*

---
## Binary heaps

---
## max_heapify() on single node

---
## max_heapify()

---
## Building a max-heap

---
## Max-heap: complexity

---
## Heap sort

---
## Outline

---
## Priority queue

---
## Insert into queue

---
## Priority queue operations

---
## Outline

---
## Randomised algorithms
+ **Vegas**-style: always **correct**, fast on **average**
  + But still slow in **worst-case**
+ **Monte Carlo**: always **fast**
  + But not always **correct**!
  + **Approximate**: margin of error &epsilon;, shrinks with more iterations
  + **Stochastic**: bound on the **probability** of being correct

---
## QuickSort

---
## Partitioning

---
## QuickSort: complexity

---
## Average case complexity

---
## QuickSort with constant splits

---
## QuickSort with median split

---
## Outline

---
## Randomised QuickSort

---
## R-QuickSort: complexity

---
## Complexity, cont.

---
## Visualisations of sorting

---
## Outline

---
## Randomised mat-mul check
+ **Check** if n x n matrices A \* B = C
  + in \`Theta(n^2)\` time
+ 
