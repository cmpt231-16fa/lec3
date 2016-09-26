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
  + Edges may be *directed* or *undirected*
+ **Tree**: directed acyclic graph *(DAG)*
  + One node designated **root**
  + **Parent**: immediate neighbour toward *root*
  + **Leaf**: node with no *children*
  + **Degree**: maximum number of *children* per node
  + **Height** of node: max num edges to *leaf* descendant
  + **Depth** of node: num edges to *root*
  + **Level**: all nodes of same *depth*
+ **Binary tree**: degree *2*

>>>
TODO: diagram, split in 2col?

---
## Binary heaps
+ **Array** storage for certain types of **binary trees**:
  + Put node *i*'s two **children** at *2i* and *2i+1*
  + **Fill** tree left-to-right, one level at a time
  + e.g.: `[ 2, 8, 4, 7, 5, 3, 1, 6 ]`
+ **Max-heap**: node's value always &le; parent's value
  + (*min-heap*: &ge;)
+ `max_heapify()` (*O(lg n)*):
  move a node *i* to satisfy **max-heap property**
+ `build_max_heap()` (*O(n)*):
  turn an unordered array into a max-heap
+ `heapsort()` (*O(n lg n)*): in-place **sort**

---
## max_heapify() on single node
+ **Input**: binary heap *A* and node index *i*
  + **Precondition**: left and right sub-trees of *i*
    satisfy *max-heap property*
+ **Postcondition**: entire subtree at *i* satisfies
  *max-heap property*
+ **Algorithm**:
  1. Find **largest** of the 3 nodes: *i*, left-child of *i*,
    or right-child of *i*
  2. If *i* is **not** the largest, then:
    1. **Swap** *i* with the largest
    2. **Recurse** (or iterate) on that subtree

---
## max_heapify()
```
def max_heapify( A, i ):
  largest = i
  if 2i <= length(A) and A[ 2i ] > A[ largest ]:            # left child
    largest = 2i
  else if 2i+1 <= length(A) and A[ 2i+1 ] > A[ largest ]:   # right child
    largest = 2i+1
  if largest != i:
    swap( A[ i ], A[ largest ] )
    max_heapify( A, largest )
```

+ **Try** it on previous heap at *i=1*
+ **Running time**?

---
## Building a max-heap
+ **Input**: binary heap *A*, in any order
  + **Postcondition**: *A* has *max-heap property*
+ **Algorithm**:
  1. **Last half** of array is all **leaves**
  2. Apply `max_heapify()` in turn to each item in **first half**
    + In **descending** order, so the **subtrees** are already max-heaps

```
for i = floor( length(A)/2 ) to 1:
  max_heapify( A, i )
```

+ **Try** it: `[ 5, 2, 7, 4, 8, 1 ]`

---
## Max-heap: complexity
+ **Group** iterations of `for` loop by **height** *h* of node:
  + Each call to `max_heapify(i)` takes *O(h)*
  + **Num of nodes** with height *h* is \`<= |~ n/2^(h+1) ~| \`
    + Reaches that bound when tree is **full**
+ Total **running time** *T(n)*:
  \` T(n) = sum_(h=0)^(text(lg)n)
  \` <= n sum_(h=0)^oo (1/2^(h+1))O(h) \`
  \` = n sum_(h=1)^oo (1/2^h)O(h) \`
  \` = O(n) \`
+ &rArr; can *build* a max-heap in **linear** time!
  + But it's not quite a **sorting** algorithm....

---
## Heap sort
+ **Algorithm**:
  1. Make array a **max-heap**
  2. **Repeat**, working *backwards* from end of array:
    1. **Swap** *root* with *last leaf* of heap
    2. **Shrink** heap by 1 and re-apply `max_heapify()`
+ **Loop invariant**:
  + First portion of array is still a **max-heap**
  + Last portion of array is **sorted** (largest items)
+ **Complexity**: T(n) = *&Theta;(n lg n)*
  + *&Theta;(n)* calls to `max_heapify()` (*&Theta;(lg n)*)
+ **Try** it: `[ 5, 2, 7, 4, 8, 1 ]`

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
