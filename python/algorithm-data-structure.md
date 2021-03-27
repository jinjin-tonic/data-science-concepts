### 1. Know what "complexity" means in the context of algorithms and data structures, including the two kinds of complexity we talked about.
1. Time complexity
- complexity here means complexity of the algorithm
- we can think of this as "if we double n, what happens to the number of steps?"

2. Space complexity
- A measure of the amount of working storage an algorithm needs. (memory)

### 2. Be able to express complexity using Big-O notation
- Big-O tells us the approximate number of steps an algorithm performs as a function of the input size.
- So is code that runs in  𝑂(log𝑛)  time faster than code that runs in  𝑂(𝑛)  time?
    - When the size of n is small, it depends.

- What affects the Big O?
    - Just the algorithm

- What affects the constant?
    - The algorithm, the implementation. 

| Big O  |  name  | change in runtime if I double n? |
|-------|--------|-------|
| $O(1)$ | constant | same |
| $O(\log n)$ | logarithmic | increased by a constant |
| $O(n)$ | linear | 2x | 
| $O(n \log n)$ | linearithmic | a bit more than 2x |
| $O(n^2)$ | quadratic | 4x |
| $O(n^3)$ | cubic | 8x |
| $O(n^k)$ | polynomial | increase by a factor of $2^k$ |
| $O(2^n)$ = O(3^n), O(k^n), O(n!) ... | exponential | squared |

### 3. Be able to identify the time complexity of simple (iterative) functions with input n
- Cheatsheet : common data structures and sort
![](https://miro.medium.com/max/700/1*Uzrw9faXdYgg20I6NjUTBw.png)
![](https://miro.medium.com/max/700/1*X1hZCxNdfgZ0sT_2tynPKA.png)

### 4. Be able to identify the space complexity of simple (iterative) function with input n
- When the function doesn't depend on the size of n, then the space complexity is usually O(1) ... 
    ex. np.zeros(5) --> O(1)

-  `getsizeof` only gives you the size of the container, not the contents

### 5. Be able to translate a function of the steps of an algorithm (with input n) into correct Big-O notation (i.e. excluding constants, lower order terms)
### 6. Explain when and why the relative timings of different algorithms may not directly reflect their relative Big-O complexity
- When the input size is too small, it is hard to directly measure the algorithms.. 

For all the following algorithms:
     - linear search (just iterating through it)
     - binary search
     - search in a hashmap
     - search in a binary search tree
     - insertion sort
     - merge sort

### 7. Be able to provide the major steps of the algorithm, the time complexity, and discuss any special setup required before the algorithm can be applied.
1. Linear search O(n)
```python
    for element in data:
        if element == key:
            return True
    return False
```

2. Binary search O(logn)
```python
def search_sorted(data, key):
    """
    Searches the key in data using binary search 
    and returns True if found and False otherwise. 
    
    Arguments
    data -- (list) the elements to search within
    key -- (int) the key to search for
    """
    low = 0 
    high = len(data) - 1
    
    while low <= high:
        mid = (high + low) // 2 ## floor division
        
        if data[mid] == key:
            return True
        elif key < data[mid]:
            high = mid - 1 # since we've already checked mid, we don't need mid
        else:
            low = mid + 1
    
    return False
```
- The function loops over log2n elements, as the search space reduces by half in each iteration of the loop.
- The function is based on the assumption that the data is sorted in ascending order
    - if you pass unsorted data, you might miss the element you are looking for.

3. sort (general)
- the best sorting algorithm has O(nlogn)
    - it's can't be O(logn) because even iterating through the whole elements takes O(n).
    - timsort(`.sort()`), merge sort : O(nlogn)
    - insertion sort : O(n^2)

- Insertion sort
```python
def insertion_sort(x):
    """Sorts x inplace using insertion sort."""
    
    n = len(x)
    print(x)
    
    for i in range(n):
        # Get the index of the smallest value from location i onward
        min_index = i
        
        for j in range(i+1, n):
            if x[j] < x[min_index]:
                min_index = j
        x[min_index], x[i] = x[i], x[min_index]
        
        print(x)
```

- Merge sort
```python
def mergesort(L):
    '''returns a version of L sorted from smallest to largest, using the recursive mergesort algorithm'''
    if len(L) == 1:
        print("base case", L)
        return L
    
    print("mergesorting ",L)
    mid = len(L) // 2
    L1 = L[:mid]
    L2 = L[mid:]
      
    print("divided into ", L1, "and", L2, "for sorting")
    
    L1 = mergesort(L1)
    L2 = mergesort(L2)
    
    print("merging ", L1, "and", L2)
    
    L = merge(L1, L2)
     
    print("returning sorted", L)
    return L
```

4. set: a hash table
- Python `set` type needs to support the following operations:
    - inserting a new element
    - deleting an element
    - checking if an element is present

- Python `set` cannot store elements that are not hashable
```python
    s.add([1,2,3]) ## Error!
```
- Python `dict` also uses hash functions

5. Search in a hashmap
- search set : O(1)
```python
S in my_set
```
6. Search in a binary search tree
- search binary search tree : O(logn)
- Binary search tree
    - For all nodes, all keys in its left subtree are smaller than its key, and all keys in its right subtree are larger than its key
    - We **must** able to compare keys (string, numerical dtypes)
    - Each node can have at most two children

- What is the time complexity of the recursive algorithm just written ( 𝑛  = number of nodes)? (size of the data structure?)
Answer: It's  𝑂(𝑛) , when a tree is valid we visit every node once

- What would be the time complexity if we did it "brute force", for each node checking that all nodes to the right/left are smaller/larger?
Answer: This is tricky! It is definitely worse than  𝑂(𝑛)  (checking each node once), and no worse than  𝑂(𝑛2)  (checking each other node for each node). In a worst case tree (a entirely right or left branching tree) the average number of children is indeed a direct function of  𝑛  (and hence complexity O( 𝑛2 ), however for a well balanced tree, the number of nodes that need to be compared for the average node will be much less than  𝑛 ; the leaves, which make up roughly half the nodes in a perfect binary tree, do not need to be checked at all!

- What is the time complexity of the inserting or searching a binary search tree? (what you are doing in lab)

Answer: We just need to traverse the tree down to the appropriate node. For a well balanced binary tree, this will be  𝑂(log𝑛) , though in worst case it could be  𝑂(𝑛) . In short, if you are using BSTs, you need to keep them balanced!

- What about deletion from a BST?
Answer: Deletion is really tricky. Imagine if we delete the top node in the tree? Let's not go there right now!


### 8. Be able to identify the most likely complexity of an algorithm (from the options given in Lecture 1) based on a table of timings for several possible values of n (multipled by 2 or 10 each time)
1. `.sort()` : O(nlogn)
2. `in` : O(n) if it's looking in lists, O(1) if it's looking in sets
    - note: binary search is O(logn) so it's faster than list in operation, but when n < 10000, list in operation is faster.

### 9. Know what a hash function does, and what kind of Python objects can't be hashed
- A hash function is any function that can be used to map data of arbitrary size to fixed-size values.
- lists, dicts are not hashable : they are mutable.
- tuple, string, integer, None... are hashable. : they are immutable.
- The hash table is basically a big array (list), and the hash function (mod the array size) maps an object to its location in the array.
    - The array typically expands and contracts automatically as needed.
    
- These operations may be slow, but averaged or "amortized" over many operations, the runtime is  𝑂(1)
    - The hash function depends on this array size.
- There's also an issue of collisions: when two different objects hash to the same place.
    - To avoid collisions, we would need some mechanism to have more than one object in each bucket. 
    - We might also consider inserting (key,value) tuples to create a dictionary, like Python dict.
- Roughly speaking, we can insert, retrieve, and delete things in  𝑂(1)  time so long as we have a "good" hash function.


### 10. Explain the effect of a dynamic versus fixed-size hash table on the big-O complexity of hashmap lookup, and know which is used in Python
- Python uses a dynamic hash table
- Dynamic hash table can save memory space.
- For fixed hash table:
    - you need to search through n elements, so O(n)
- For dynamic hash table search:
    - O(1)
    - Python sets, dicts can accomplish O(1) operations by being dynamic.

### 11. Be able to define recursion.
### 12. Know what the base case of a recursive function is, and why all recursive functions must have one
- Base case
    - A solution to a small/simple enough version of the problem that it does NOT require the recursive step to calculate
    - This allows our recursion to stop.

### 13. Given a recursive function with print statements and an initial call, be able to construct the exact output of the function.
- factorial : O(n)
```python
def factorial(n):
    print(f"Entering factorial {n}")
    
    if n == 0:
        print("Finished factorial 0, returning 1")
        return 1
    else:
        result = n*factorial(n-1)
        print(f"Finished factorial {n}, returning {result}")
        return result
```
```
Entering factorial 10
Entering factorial 9
Entering factorial 8
Entering factorial 7
Entering factorial 6
Entering factorial 5
Entering factorial 4
Entering factorial 3
Entering factorial 2
Entering factorial 1
Entering factorial 0    ## entering base case
Finished factorial 0, returning 1   ## clearing base case
Finished factorial 1, returning 1
Finished factorial 2, returning 2
Finished factorial 3, returning 6
Finished factorial 4, returning 24
Finished factorial 5, returning 120
Finished factorial 6, returning 720
Finished factorial 7, returning 5040
Finished factorial 8, returning 40320
Finished factorial 9, returning 362880
Finished factorial 10, returning 3628800
3628800
```


### 14. Be able to write Python code corresponding to the base case and/or a recursive step in a recursive function, given a description of what the function should do
### 15. Give the Big-O complexity of a simple recursive function
- fibonacci() : O(2^n)
    - As the input size increases, the steps the function takes increase by a multiple of 2


### 16. Be able to define what a recursive data structure is (with examples), and how it is different from a "nested" data structure
- Tree! (linked list)
    - Nested data struction doesn't involve "recursion"

```python
class TreasureBox:
    def __init__(self, treasure):
        self.innerTreasureBox = None
        self.treasure = treasure
        
    def append_outer(self, treasure):
        """Add a new treasure box to the outside, put everything else inside it."""
        new_box = TreasureBox(treasure)     ## create the instance again
        new_box.innerTreasureBox = self     ## Make a connection with inside box and outside box
        return new_box 
```
- Two base cases in TreasureBox
    - when depth = 0, and when the tresaure box is None

- Take-home ideas
    - The signature of a recursive data type is that an instance of the class contains **one or more instances** of that type.
    - In our case, each TreasureBox contains another TreasureBox (or None).
    - get and append_inner are also a recursive, in that they call themselves
    - Recursion is an important idea in understanding algorithms and data structures.

### 17. Know the meaning of all the basic terminology for trees (root, children, parents, leaves, internal nodes, height, label/value)
1. A tree is either empty or a node with zero or more children that are themselves trees (or "subtrees").
2. If  𝑋  is the child of  𝑌 , then  𝑌  is the parent of  𝑋 .
3. The root is the only node without a parent (e.g. General).
4. A leaf is a node that does not have children (e.g. Private A). // bottom of the trees (recursion will stop)
5. An internal node is a node that is not a leaf (e.g. Captain A). // Have children = leaf vs. doesn't have children = internal node
6. The height of the tree is the number of nodes on the longest path from the root to a leaf (here, 5). // related to a syntax's complexity.
7. Some trees might have a label associated with each node (or other some other data which it contains, e.g. a value)
8. Relative to a given node, we can also talk about decendents (all nodes under a node) and ancestors (all nodes leading up from the node to the root)

### 18. Be able to apply the above terminology in the context of a specific provided tree.
### 19. Know what must be true of values in a binary search tree, and be able to identify (manually) if one is valid.

## True/False Questions
Assume Algorithm A runs in 𝑂(𝑛) time and Algorithm B runs in 𝑂(sqrt(n))  time.

1. Running Algorithm A will take longer than running Algorithm B. (F, but not 100% False)
2. Running Algorithm A with $n=20000$ will probably take 2x as long as running it with $n=10000$. (T)
- O(n) is the linear complexity

3. Running Algorithm A with $n=2$ will definitely take 2x as long as running it with $n=1$. (F)
- False, because when the input size is small, the algorithm maynot directly reflect the complexity.

4. Running Algorithm B with $n=20000$ will probably take 4x as long as running it with $n=10000$. (F)
- No, probably less than doubling. 

5. If Algorithm B is the best algorithm for a given task, then there is no way to further speed up one's code. (F)
- No, you can change the programming language, upgrade your laptop, etc.

6. To get the total memory used by an instance of a class, it is sufficient to call getsizeof on all the Python objects contained in self (F)
- No, `getsizeof` only calculate the size of the container. 

7. Searching for a key in a `dict` takes $O(1)$ time, but searching for a value takes $O(n)$. (T)
- Searching for a value without the key corresponding to the value will take O(n) since we have to iterate through the whole data. 

8. The elements of a `set` are maintained in sorted order for fast searching. (F)
- `set` is unsorted. It uses a hash table. 

9. The best sorting algorithms run in $O(n)$ time. (F)
- No, it's O(nlogn).

10. If you hash two different Python objects, you will always get two different hashes. (F)
- Not always. It might happen to be the same hash value.

11. If I have to search inside a list $1000$ times, it is better to first sort the list and then use binary search, rather than just using linear search. (Most of the time T)
- sort list - O(nlogn) + binary search - O(logn)
- linear search - O(n)

12. A hash table with a fixed number of locations/buckets is $O(1)$ lookup regardless of the number of items it holds (F, but in some cases T)

13. Every recursive algorithm will have exactly one base case.  (F)
- No, ex. fibonacci

14. A recursive algorithm will never involve an instance of a function calling itself with the same arguments. (F)
- It cannot call ifself with "exact the same arguments" --> the recursion never ends

15. In recursion, the first instance of the function called is the last to complete/return. (T)
- In the end, the function will complete the first instance of the function called.

16. The mergesort sorting algorithm can only be accomplished using recursion. (F)
- No, but the recursion solution is much clearer

17. The complexity of a recursive function can be determined directly based on the number of recursive calls within it. (F)

18. A `TreasureBox` contains a copy of itself. (F)
- No, it contains a copy of a different `TreasureBox` instance.

19. If you have a `TreasureBox` with $n$ items, adding a new `TreasureBox` inside of all the others is $O(n)$. (T)
- Yes, you need to insert n times.

20. A tree is similar to the `TreasureBox`, except that you can have multiple treasure boxes inside each treasure box. (T)

21. A binary tree of height 4 can have at most 8 leaves. (T)

22. Balanced binary search trees are faster to use than unbalanced ones (T)
- For a well balanced tree, O(logn), but in worst unbalanced tree, O(n).
