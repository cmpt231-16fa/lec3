<!-- .slide: data-background-image="http://sermons.seanho.com/img/bg/unsplash-e6XsI7qqvAA-forest_sunbeam.jpg" -->
# CMPT231
## Lecture 3: ch5-6
### Heaps, Queues, and Quicksort

---
## Outline for today

---
## Summary of sorting algorithms
+ **Comparison** sorts *(ch2, 6, 7)*:
  + **Insertion** sort:  *&Theta;(n^2)*, easy to program, slow
  + **Merge** sort: *&Theta;(n lg n)*, out-of-place copy (slow)
  + **Heap** sort: *&Theta;(n lg n)*, in-place, max-heap
  + **Quicksort**: *&Theta;(n^2)* worst-case, *&Theta;(n lg n)* average,
    fast constant factors
+ **Linear**-time non-comparison sorts *(ch8)*:
  + **Counting** sort: *k* distinct values: *&Theta;(k)*
  + **Radix** sort: *d* digits, *k* values: *&Theta;(d(n+k))*
  + **Bucket** sort: uniform distribution: *&Theta;(n)*

>>>
TODO: which sorts are **stable**

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
+ We can use *binary heaps* to make a **priority queue**:
  + Set of *items* with attached *priorities*
+ **Interface** (set of operations) for priority queue:
  + `insert(A, item, pri)`: **add** an item
  + `find_max(A)`: **get** item with *highest* priority
  + `pop_max(A)`: same but also **delete** the item
  + `set_pri(A, item, pri)`: **change** (increase) priority of item
+ **Initialise** queue by building a *max-heap*:
  + `find_max()` is easy: just return *A[1]*
  + `pop_max()` also easy: remove *A[1]* and **heapify**

---
## Insert into queue
+ `set_pri(A, i, pri)`: start from *i* and **bubble** item up
  to proper place:

    A[ i ] = pri
    while i > 1 and A[ i/2 ] < A[ i ]:
      swap( A[ i/2 ], A[ i ] )
      i = i/2

+ **Complexity**: num iterations = *&Theta;(lg n)*
+ `insert(A, pri)`: make **new** node, then set its **priority**:

    A.size++
    set_pri( A, A.size, pri )

+ For speed, often **pre-allocate** *A* as fixed-length array,
  and track *size* of queue in separate var
  + **Complexity**: same as `set_pri()`: *&Theta;(lg n)*

---
## Priority queue operations
+ **Build** queue (with max-heap): *&Theta;(n)*
+ **Fetch** highest priority item: *&Theta;(1)*
+ Fetch and **remove** highest priority item: *&Theta;(lg n)*
+ **Change** priority of an item: *&Theta;(lg n)*
+ **Insert** new item: *&Theta;(lg n)*

---
## Outline

---
## Randomised algorithms
+ **Vegas**-style: always **correct**, fast on **average**
  + But still slow in **worst-case**
+ **Monte Carlo**: always **fast**
  + But not always **correct**!
  + **Approximate**: margin of *error* &epsilon;
  + **Stochastic**: *probability* P of being correct
  + Estimate **improves** with more computation time (iterations)

---
## Quicksort
+ **Divide**: partition *A[ p .. r ]* such that:
  \` max( A[p .. q-1] ) <= A[q] <= min( A[q+1 .. r] ) \`
  + "*magic sauce*" is in this step
+ **Conquer**: recurse on each part:
  + `quicksort(A, p, q-1)` and `quicksort(A, q+1, r)`
+ No **combine** / merge step needed
+ **In-place** sort (only uses swaps) (unlike *merge sort*)
+ **Worst** case still \`Theta(n^2)\`, but
  + **Average** case is *&Theta;(n lg n)*, with small *constants*
+ In practise, Quicksort is one of the **best** sorts
  + when inputs are **arbitrary** and can only use **comparisons**

---
## Partitioning (Lomuto)
+ One option: pick *last* item as the **pivot**
+ **Walk** through array:
  + Throw items **smaller** than pivot to *first* part of array
  + Items **larger** than pivot stay in *last* part of array
  + Partition is not necessarily **balanced**!
+ Lastly, **swap** pivot in-between two parts
+ **Complexity**?

```
def partition( A, lo, hi ):
  piv = A[ hi ]                     # Lomuto partition
  split = lo                        # Where to send items <= piv
  for cur = lo to hi-1:
    if A[ cur ] <= piv:             # Send small items away
      swap( A[ split ], A[ cur ] )
      split++
  swap( A[ split ], A[ piv ] )      # Put pivot in its place
  return split
```

---
## Quicksort: complexity
+ **Worst** case: every partition is **uneven**:
  + **Pivot** is either *largest* or *smallest* in subarray
  + T(n) = *T(n-1) + T(0) + &Theta;(n)* = \`Theta(n^2)\`
  + Example **inputs** that do this?
+ **Best** case: every partition is exactly **half**:
  + T(n) = *2T(n/2) + &Theta;(n)* = *&Theta;(n lg n)*
  + Example **inputs** that give this?
+ What about **average** case, assuming *random* input?

---
## Average case complexity
+ **Intuition**: on average, get splits *in-between* best and worst
  + If say, average split is *90%* vs *10%*, then:
  + T(n) = T( *(9/10)*n ) + T( *(1/10)*n ) + *&Theta;(n)*
  + Still results in *O(n lg n)*
+ If we assume splits **alternate** between best and worst, then:
  + Only adds *O(n)* work to each of *O(lg n)* levels
  + Still *O(n lg n)*! (But maybe larger constants)

>>>
TODO: figure

---
## Quicksort with constant splits
+ *(p.178 #7.2-5)*: All splits are *&alpha;* vs *1-&alpha;*, with
  0 &lt; *&alpha;* &lt; 1/2
  + &rArr; what is min/max **depth** of leaf in recursion tree?
+ **Min depth**: follow smaller (*&alpha;*) side of each split
  + How many splits *m* until **leaf** (1 item array)?
    \` alpha^m n = 1 => m = -log(n)/log(alpha) \`
+ **Max depth**: same with *1-&alpha;* side:
  + \` -log(n)/log(1-alpha) \`
+ Both are *&Theta;(lg n)*
+ &rArr; with **constant-ratio** splits, complexity still *&Theta;(n lg n)*

---
## Quicksort with median split
+ **Best** case splits happen when *pivot* is **median**:
  + Half of items *smaller*, half of items *larger*
  + Not same as *average* when distribution is **skewed**
+ **Median** (rank finding) algorithm in *O(n)*: see *ch9*
  + **Partitioning** also takes only *O(n)*, so
  + Quicksort T(n) = *2T(n/2) + O(n)* = *O(n lg n)*!
+ But, in practise, it's **extra** work for **marginal**
  improvement in splits
  + Benchmarks show **slower** than *merge sort*

---
## Outline

---
## Randomised Quicksort
+ With **random** input, get nice *&Theta;(n lg n)* behaviour
+ But **presorted** input gives **worst** case behaviour
  + Much **real-world** data is at least partially presorted
  + Choice of **last** element (*hi*) as pivot
+ Great candidate for **randomised** algorithm:
  + *Before* partitioning, **swap** *hi* with a random item
+ Still **possible** to get worst-case behaviour, but **unlikely**
  + **Vegas**-style: always **correct**, and usually **fast**

```
def rand_partition( A, lo, hi ):
  swap( A[ hi ], A[ random( lo, hi ) ] )
  partition( A, lo, hi )
```

---
## R-Quicksort: comparisons
+ **Name** items in order: \`{z\_i}\_(i=1)^n\` (assume *distinct*)
+ Analyse complexity by counting **comparisons**:
  + **Worst** case: all pairs \`(z_\i, z\_j): Theta(n^2)\`
+ No comparison happens **multiple** times, because
  + Comparisons only done against **pivots**, and
  + Each pivot is used only **once** and not **revisited**
+ A pair \`(z\_i, z\_j)\` is compared only if:
  + Either item is chosen as *pivot* **before** any other item
    in between them: \`{z_i, z_(i+1), ..., z_(j-1), z_j}\`
  + Otherwise \`z_i\` and \`z_j\` would be on **opposite** sides of
    a split, and would **never** be compared
  + **Probability** of this happening is \` 2( 1 / (j-i+1) ) \`

---
## R-Quicksort: complexity
**Sum** over all possible pairs \`(z\_i, z\_j)\`: <br/>
\` sum_(i=1)^(n-1) sum_(j=i+1)^n P(text(compare) z\_i text(with) z\_j) \`
\` = sum_(i=1)^(n-1) sum_(j=i+1)^n 2/(j-i+1) \`
\` = sum_(i=1)^(n-1) sum_(k=1)^(n-i) 2/(k+1) text((let k=j-i)) \`
\` < sum_(i=1)^(n-1) sum_(k=1)^n 2/k \`
\` = sum_(i=1)^(n-1) O(text(lg) n) text((e.g., Riemann sums)) \`
\` = O(n text(lg) n) \`

---
## Visualisations of sorting

---
## Outline

---
## Randomised mat-mul check
+ **Check** if n x n matrices A \* B = C
  + in \`Theta(n^2)\` time
+
