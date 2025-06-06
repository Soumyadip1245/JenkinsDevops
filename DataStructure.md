# Complete Java Data Structures & Algorithms Guide

## 1. Arrays

### What is an Array?
An array is a collection of elements stored in contiguous memory locations. Each element can be accessed directly using its index. Arrays are the foundation of most other data structures and provide constant-time access to elements.

**Key Characteristics:**
- **Fixed size** (static arrays) or **dynamic size** (ArrayList)
- **Homogeneous elements** (same data type)
- **Zero-based indexing** in Java
- **Random access** - can access any element directly via index
- **Cache-friendly** due to contiguous memory layout

**How it works internally:**
- Elements are stored in consecutive memory addresses
- Index calculation: `address = base_address + (index × element_size)`
- ArrayList uses an internal array that grows when capacity is exceeded

### Implementation
```java
// Static Array
int[] arr = new int[10];
int[] arr2 = {1, 2, 3, 4, 5};

// Dynamic Array (ArrayList)
ArrayList<Integer> list = new ArrayList<>();
list.add(10);
list.get(0);
list.remove(0);
list.set(0, 20);
```

### Usage
- **Best for**: Random access, cache-friendly operations, mathematical computations
- **Avoid when**: Frequent insertions/deletions in middle, unknown size

---

## 2. Linked Lists

### What is a Linked List?
A linked list is a linear data structure where elements (nodes) are stored in sequence, but not in contiguous memory locations. Each node contains data and a reference (pointer) to the next node in the sequence.

**Key Characteristics:**
- **Dynamic size** - can grow/shrink at runtime
- **Sequential access** - must traverse from head to reach a specific element
- **Efficient insertion/deletion** at any position (O(1) if you have the node reference)
- **No memory waste** - allocates exactly what's needed
- **No random access** - cannot directly access elements by index

**Types:**
1. **Singly Linked List** - Each node points to the next node
2. **Doubly Linked List** - Each node has pointers to both next and previous nodes
3. **Circular Linked List** - Last node points back to the first node

**How it works internally:**
- Nodes are scattered in memory, connected via pointers
- Head pointer stores the address of the first node
- Traversal requires following the chain of pointers
- Java handles memory allocation/deallocation automatically

### Singly Linked List Implementation
```java
class ListNode {
    int val;
    ListNode next;
    
    ListNode(int val) {
        this.val = val;
    }
}

class LinkedList {
    ListNode head;
    
    void addFirst(int val) {
        ListNode newNode = new ListNode(val);
        newNode.next = head;
        head = newNode;
    }
    
    void addLast(int val) {
        if (head == null) {
            head = new ListNode(val);
            return;
        }
        ListNode curr = head;
        while (curr.next != null) {
            curr = curr.next;
        }
        curr.next = new ListNode(val);
    }
    
    boolean delete(int val) {
        if (head == null) return false;
        if (head.val == val) {
            head = head.next;
            return true;
        }
        ListNode curr = head;
        while (curr.next != null && curr.next.val != val) {
            curr = curr.next;
        }
        if (curr.next != null) {
            curr.next = curr.next.next;
            return true;
        }
        return false;
    }
}
```

### Doubly Linked List Implementation
```java
class DoublyListNode {
    int val;
    DoublyListNode prev, next;
    
    DoublyListNode(int val) {
        this.val = val;
    }
}
```

### Usage
- **Best for**: Frequent insertions/deletions, unknown size, implementing other data structures
- **Avoid when**: Need random access, memory is limited

---

## 3. Stacks

### What is a Stack?
A stack is a linear data structure that follows the Last In, First Out (LIFO) principle. Elements can only be added or removed from one end, called the "top" of the stack.

**Key Characteristics:**
- **LIFO ordering** - last element inserted is first to be removed
- **Restricted access** - can only access the top element
- **Two main operations**: push (insert) and pop (remove)
- **Used implicitly** in function calls, recursion, and expression evaluation

**Real-world analogies:**
- Stack of plates - you add/remove from the top
- Browser back button - last visited page is first to go back to
- Undo functionality - last action is first to be undone

**How it works internally:**
- Can be implemented using arrays or linked lists
- Array implementation: use an index to track the top
- Linked list implementation: insert/remove from the head
- Java's Stack class extends Vector (legacy, prefer Deque interface)

**Applications:**
- Function call management (call stack)
- Expression evaluation and syntax parsing
- Backtracking algorithms
- Undo mechanisms in applications
- Memory management

### Implementation
```java
// Using ArrayList
class Stack<T> {
    private ArrayList<T> stack = new ArrayList<>();
    
    void push(T item) {
        stack.add(item);
    }
    
    T pop() {
        if (isEmpty()) throw new EmptyStackException();
        return stack.remove(stack.size() - 1);
    }
    
    T peek() {
        if (isEmpty()) throw new EmptyStackException();
        return stack.get(stack.size() - 1);
    }
    
    boolean isEmpty() {
        return stack.isEmpty();
    }
}

// Using Java's Built-in Stack
Stack<Integer> stack = new Stack<>();
stack.push(10);
int top = stack.pop();
int peek = stack.peek();
```

### Usage
- **Best for**: LIFO operations, function calls, expression evaluation, backtracking
- **Examples**: Undo operations, browser history, parentheses matching

---

## 4. Queues

### What is a Queue?
A queue is a linear data structure that follows the First In, First Out (FIFO) principle. Elements are added at one end (rear/back) and removed from the other end (front).

**Key Characteristics:**
- **FIFO ordering** - first element inserted is first to be removed
- **Two-end access** - insertion at rear, deletion from front
- **Main operations**: enqueue (insert) and dequeue (remove)
- **Fair processing** - elements are processed in arrival order

**Types of Queues:**
1. **Simple Queue** - Basic FIFO queue
2. **Circular Queue** - Last position connects back to first (saves space)
3. **Priority Queue** - Elements have priorities, highest priority served first
4. **Deque (Double-ended Queue)** - Insert/delete from both ends

**Real-world analogies:**
- Line at a bank or store - first person in line is served first
- Print queue - documents print in the order they were submitted
- CPU task scheduling - processes are handled in order

**How it works internally:**
- Array implementation: use front and rear pointers
- Linked list implementation: maintain head and tail pointers
- Circular queue: use modulo arithmetic to wrap around
- Priority queue: typically implemented using heaps

**Applications:**
- CPU and disk scheduling
- Breadth-First Search (BFS) in graphs
- Handling requests in web servers
- Buffer for data streams
- Print spooling

### Queue Implementation
```java
// Using LinkedList
Queue<Integer> queue = new LinkedList<>();
queue.offer(10);  // add to rear
int front = queue.poll();  // remove from front
int peek = queue.peek();   // see front without removing

// Circular Queue Implementation
class CircularQueue {
    private int[] queue;
    private int front, rear, size, capacity;
    
    public CircularQueue(int capacity) {
        this.capacity = capacity;
        this.queue = new int[capacity];
        this.front = 0;
        this.rear = -1;
        this.size = 0;
    }
    
    boolean enqueue(int val) {
        if (isFull()) return false;
        rear = (rear + 1) % capacity;
        queue[rear] = val;
        size++;
        return true;
    }
    
    int dequeue() {
        if (isEmpty()) return -1;
        int val = queue[front];
        front = (front + 1) % capacity;
        size--;
        return val;
    }
    
    boolean isEmpty() { return size == 0; }
    boolean isFull() { return size == capacity; }
}
```

### Priority Queue Implementation
```java
// Min Heap (default)
PriorityQueue<Integer> minHeap = new PriorityQueue<>();

// Max Heap
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());

// Custom Comparator
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
```

### Usage
- **Regular Queue**: BFS, job scheduling, buffering
- **Priority Queue**: Dijkstra's algorithm, A* search, task scheduling with priorities

---

## 5. Trees

### What is a Tree?
A tree is a hierarchical data structure consisting of nodes connected by edges. It has a root node and subtrees of children with a parent-child relationship. Trees are non-linear and have no cycles.

**Key Characteristics:**
- **Hierarchical structure** - organized in levels
- **Root node** - topmost node with no parent
- **Leaf nodes** - nodes with no children
- **No cycles** - exactly one path between any two nodes
- **Recursive structure** - each subtree is also a tree

**Tree Terminology:**
- **Parent/Child** - direct connection between nodes
- **Siblings** - nodes with the same parent
- **Depth** - number of edges from root to a node
- **Height** - number of edges from a node to deepest leaf
- **Level** - all nodes at the same depth

**Types of Trees:**
1. **Binary Tree** - Each node has at most 2 children
2. **Binary Search Tree (BST)** - Binary tree with ordering property
3. **AVL Tree** - Self-balancing BST
4. **Red-Black Tree** - Another type of self-balancing BST
5. **B-Tree** - Multi-way tree used in databases
6. **Trie** - Tree for storing strings

**Binary Search Tree Properties:**
- **Left subtree** contains only nodes with values less than parent
- **Right subtree** contains only nodes with values greater than parent
- **In-order traversal** gives sorted sequence
- **Search, insert, delete** are O(log n) in balanced trees

**Tree Traversals:**
1. **In-order** (Left, Root, Right) - gives sorted order in BST
2. **Pre-order** (Root, Left, Right) - useful for copying tree
3. **Post-order** (Left, Right, Root) - useful for deleting tree
4. **Level-order** (BFS) - visit level by level

**Applications:**
- File systems (directory structure)
- HTML DOM (web page structure)
- Database indexing
- Expression parsing
- Decision trees in AI
- Heap implementation

### Binary Tree Implementation
```java
class TreeNode {
    int val;
    TreeNode left, right;
    
    TreeNode(int val) {
        this.val = val;
    }
}

class BinaryTree {
    TreeNode root;
    
    // Inorder Traversal (Left, Root, Right)
    void inorder(TreeNode node) {
        if (node != null) {
            inorder(node.left);
            System.out.print(node.val + " ");
            inorder(node.right);
        }
    }
    
    // Preorder Traversal (Root, Left, Right)
    void preorder(TreeNode node) {
        if (node != null) {
            System.out.print(node.val + " ");
            preorder(node.left);
            preorder(node.right);
        }
    }
    
    // Postorder Traversal (Left, Right, Root)
    void postorder(TreeNode node) {
        if (node != null) {
            postorder(node.left);
            postorder(node.right);
            System.out.print(node.val + " ");
        }
    }
    
    // Level Order Traversal
    void levelOrder() {
        if (root == null) return;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            System.out.print(node.val + " ");
            
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
    }
}
```

### Binary Search Tree Implementation
```java
class BST {
    TreeNode root;
    
    TreeNode insert(TreeNode root, int val) {
        if (root == null) return new TreeNode(val);
        
        if (val < root.val) {
            root.left = insert(root.left, val);
        } else if (val > root.val) {
            root.right = insert(root.right, val);
        }
        return root;
    }
    
    TreeNode search(TreeNode root, int val) {
        if (root == null || root.val == val) return root;
        
        if (val < root.val) return search(root.left, val);
        return search(root.right, val);
    }
    
    TreeNode delete(TreeNode root, int val) {
        if (root == null) return root;
        
        if (val < root.val) {
            root.left = delete(root.left, val);
        } else if (val > root.val) {
            root.right = delete(root.right, val);
        } else {
            // Node to be deleted found
            if (root.left == null) return root.right;
            if (root.right == null) return root.left;
            
            // Node with two children
            TreeNode minNode = findMin(root.right);
            root.val = minNode.val;
            root.right = delete(root.right, minNode.val);
        }
        return root;
    }
    
    TreeNode findMin(TreeNode root) {
        while (root.left != null) root = root.left;
        return root;
    }
}
```

### Usage
- **Binary Tree**: Hierarchical data, expression trees, decision trees
- **BST**: Sorted data with frequent search operations, range queries

---

## 6. Heaps

### What is a Heap?
A heap is a specialized complete binary tree that satisfies the heap property. It's commonly used to implement priority queues and is the foundation of the heap sort algorithm.

**Key Characteristics:**
- **Complete binary tree** - all levels are filled except possibly the last, which is filled left to right
- **Heap property** - parent-child relationship follows a specific order
- **Array representation** - can be efficiently stored in an array
- **Efficient priority operations** - O(log n) insert/delete, O(1) peek

**Types of Heaps:**
1. **Min Heap** - parent is smaller than or equal to children (smallest element at root)
2. **Max Heap** - parent is greater than or equal to children (largest element at root)

**Heap Array Representation:**
- For node at index `i`:
  - Left child at index `2*i + 1`
  - Right child at index `2*i + 2`
  - Parent at index `(i-1)/2`
- Root is always at index 0

**Heap Operations:**
1. **Insert (Heapify Up)** - Add element at end, bubble up to maintain heap property
2. **Extract (Heapify Down)** - Remove root, move last element to root, bubble down
3. **Peek** - View root element without removing

**How Heapify Works:**
- **Heapify Up**: Compare with parent, swap if needed, repeat
- **Heapify Down**: Compare with children, swap with appropriate child, repeat

**Applications:**
- Priority queues (task scheduling, event simulation)
- Heap sort algorithm
- Dijkstra's shortest path algorithm
- Huffman coding
- Finding kth largest/smallest elements
- Median finding (using two heaps)

### Min Heap Implementation
```java
class MinHeap {
    private ArrayList<Integer> heap;
    
    public MinHeap() {
        heap = new ArrayList<>();
    }
    
    void insert(int val) {
        heap.add(val);
        heapifyUp(heap.size() - 1);
    }
    
    int extractMin() {
        if (heap.isEmpty()) throw new IllegalStateException("Heap is empty");
        
        int min = heap.get(0);
        int lastElement = heap.remove(heap.size() - 1);
        
        if (!heap.isEmpty()) {
            heap.set(0, lastElement);
            heapifyDown(0);
        }
        return min;
    }
    
    private void heapifyUp(int index) {
        while (index > 0) {
            int parentIndex = (index - 1) / 2;
            if (heap.get(index) >= heap.get(parentIndex)) break;
            
            swap(index, parentIndex);
            index = parentIndex;
        }
    }
    
    private void heapifyDown(int index) {
        while (true) {
            int smallest = index;
            int leftChild = 2 * index + 1;
            int rightChild = 2 * index + 2;
            
            if (leftChild < heap.size() && heap.get(leftChild) < heap.get(smallest)) {
                smallest = leftChild;
            }
            if (rightChild < heap.size() && heap.get(rightChild) < heap.get(smallest)) {
                smallest = rightChild;
            }
            
            if (smallest == index) break;
            swap(index, smallest);
            index = smallest;
        }
    }
    
    private void swap(int i, int j) {
        Collections.swap(heap, i, j);
    }
}
```

### Usage
- **Best for**: Priority queues, heap sort, finding kth smallest/largest element
- **Examples**: Task scheduling, Dijkstra's algorithm, median finding

---

## 7. Hash Tables (HashMap/HashSet)

### What is a Hash Table?
A hash table (hash map) is a data structure that implements an associative array, mapping keys to values using a hash function. It provides average O(1) time complexity for basic operations.

**Key Characteristics:**
- **Key-value pairs** - stores data as (key, value) associations
- **Hash function** - converts keys into array indices
- **Constant time operations** - O(1) average case for insert, delete, search
- **Dynamic sizing** - can grow/shrink as needed
- **Collision handling** - deals with multiple keys hashing to same index

**How Hash Tables Work:**
1. **Hash function** takes a key and produces an index
2. **Collision resolution** handles when multiple keys hash to same index
3. **Load factor** determines when to resize the table
4. **Rehashing** occurs when load factor exceeds threshold

**Collision Resolution Techniques:**
1. **Chaining** - each bucket contains a linked list of elements
2. **Open Addressing** - find next available slot using probing
   - Linear probing: check next slot
   - Quadratic probing: check slots at quadratic intervals
   - Double hashing: use second hash function

**Hash Functions:**
- Should distribute keys uniformly across the array
- Common approach: `hash(key) = key.hashCode() % table_size`
- Java uses `hashCode()` method for objects

**Load Factor and Resizing:**
- **Load factor** = number of elements / table size
- When load factor exceeds threshold (usually 0.75), table is resized
- **Rehashing** - create new larger table and move all elements

**HashMap vs HashSet:**
- **HashMap** - stores key-value pairs
- **HashSet** - stores only keys (values are dummy)
- **LinkedHashMap** - maintains insertion order
- **TreeMap** - maintains sorted order (not a hash table)

**Applications:**
- Database indexing
- Caching (memoization)
- Symbol tables in compilers
- Counting frequencies
- Removing duplicates
- Implementation of other data structures

### Basic Usage
```java
// HashMap
HashMap<String, Integer> map = new HashMap<>();
map.put("key", 10);
int value = map.get("key");
map.remove("key");
boolean exists = map.containsKey("key");

// HashSet
HashSet<Integer> set = new HashSet<>();
set.add(10);
boolean contains = set.contains(10);
set.remove(10);

// LinkedHashMap (maintains insertion order)
LinkedHashMap<String, Integer> orderedMap = new LinkedHashMap<>();

// TreeMap (sorted keys)
TreeMap<String, Integer> sortedMap = new TreeMap<>();
```

### Custom Hash Table Implementation
```java
class HashTable<K, V> {
    private static class Entry<K, V> {
        K key;
        V value;
        Entry<K, V> next;
        
        Entry(K key, V value) {
            this.key = key;
            this.value = value;
        }
    }
    
    private Entry<K, V>[] table;
    private int size;
    private static final int DEFAULT_CAPACITY = 16;
    
    @SuppressWarnings("unchecked")
    public HashTable() {
        table = new Entry[DEFAULT_CAPACITY];
    }
    
    private int hash(K key) {
        return Math.abs(key.hashCode()) % table.length;
    }
    
    public void put(K key, V value) {
        int index = hash(key);
        Entry<K, V> entry = table[index];
        
        while (entry != null) {
            if (entry.key.equals(key)) {
                entry.value = value;
                return;
            }
            entry = entry.next;
        }
        
        Entry<K, V> newEntry = new Entry<>(key, value);
        newEntry.next = table[index];
        table[index] = newEntry;
        size++;
    }
    
    public V get(K key) {
        int index = hash(key);
        Entry<K, V> entry = table[index];
        
        while (entry != null) {
            if (entry.key.equals(key)) {
                return entry.value;
            }
            entry = entry.next;
        }
        return null;
    }
}
```

### Usage
- **Best for**: Fast lookups, caching, counting frequencies, removing duplicates
- **Avoid when**: Need sorted data, memory is extremely limited

---

## 8. Graphs

### What is a Graph?
A graph is a non-linear data structure consisting of vertices (nodes) connected by edges. It's used to represent relationships between different entities.

**Key Characteristics:**
- **Vertices (V)** - the nodes or points in the graph
- **Edges (E)** - connections between vertices
- **Non-linear structure** - no hierarchical relationship
- **Flexible connections** - any vertex can connect to any other vertex

**Types of Graphs:**
1. **Directed vs Undirected**
   - **Directed (Digraph)** - edges have direction (one-way)
   - **Undirected** - edges are bidirectional (two-way)

2. **Weighted vs Unweighted**
   - **Weighted** - edges have associated costs/weights
   - **Unweighted** - all edges are equal

3. **Cyclic vs Acyclic**
   - **Cyclic** - contains at least one cycle
   - **Acyclic** - no cycles (DAG - Directed Acyclic Graph)

4. **Connected vs Disconnected**
   - **Connected** - path exists between every pair of vertices
   - **Disconnected** - some vertices are not reachable from others

**Graph Representations:**
1. **Adjacency Matrix** - 2D array where matrix[i][j] = 1 if edge exists
   - Space: O(V²)
   - Edge lookup: O(1)
   - Good for dense graphs

2. **Adjacency List** - array of lists, each list contains neighbors
   - Space: O(V + E)
   - Edge lookup: O(degree of vertex)
   - Good for sparse graphs

**Graph Traversals:**
1. **Breadth-First Search (BFS)**
   - Explores level by level
   - Uses queue
   - Finds shortest path in unweighted graphs
   - Applications: shortest path, level-order traversal

2. **Depth-First Search (DFS)**
   - Explores as far as possible before backtracking
   - Uses stack (or recursion)
   - Applications: cycle detection, topological sorting, connected components

**Common Graph Algorithms:**
- **Dijkstra's Algorithm** - shortest path with weights
- **Bellman-Ford** - shortest path with negative weights
- **Floyd-Warshall** - all pairs shortest path
- **Kruskal's/Prim's** - minimum spanning tree
- **Topological Sort** - ordering of vertices in DAG

**Applications:**
- Social networks (friends, followers)
- Maps and navigation (roads, cities)
- Computer networks (routers, connections)
- Web page linking
- Dependency resolution
- Circuit design

### Graph Representations
```java
// Adjacency List
class Graph {
    private Map<Integer, List<Integer>> adjList;
    
    public Graph() {
        adjList = new HashMap<>();
    }
    
    void addVertex(int vertex) {
        adjList.putIfAbsent(vertex, new ArrayList<>());
    }
    
    void addEdge(int src, int dest) {
        adjList.get(src).add(dest);
        adjList.get(dest).add(src); // For undirected graph
    }
    
    // BFS
    void bfs(int start) {
        Set<Integer> visited = new HashSet<>();
        Queue<Integer> queue = new LinkedList<>();
        
        queue.offer(start);
        visited.add(start);
        
        while (!queue.isEmpty()) {
            int vertex = queue.poll();
            System.out.print(vertex + " ");
            
            for (int neighbor : adjList.get(vertex)) {
                if (!visited.contains(neighbor)) {
                    visited.add(neighbor);
                    queue.offer(neighbor);
                }
            }
        }
    }
    
    // DFS
    void dfs(int vertex, Set<Integer> visited) {
        visited.add(vertex);
        System.out.print(vertex + " ");
        
        for (int neighbor : adjList.get(vertex)) {
            if (!visited.contains(neighbor)) {
                dfs(neighbor, visited);
            }
        }
    }
}

// Adjacency Matrix
class GraphMatrix {
    private int[][] matrix;
    private int vertices;
    
    public GraphMatrix(int vertices) {
        this.vertices = vertices;
        matrix = new int[vertices][vertices];
    }
    
    void addEdge(int src, int dest) {
        matrix[src][dest] = 1;
        matrix[dest][src] = 1; // For undirected graph
    }
}
```

### Usage
- **Adjacency List**: Sparse graphs, most practical applications
- **Adjacency Matrix**: Dense graphs, quick edge existence checks

---

## 9. Trie (Prefix Tree)

### What is a Trie?
A Trie (pronounced "try") is a specialized tree data structure used for storing and searching strings efficiently. Each node represents a character, and paths from root to leaves represent complete words.

**Key Characteristics:**
- **Prefix-based structure** - common prefixes share the same path
- **Character nodes** - each node represents a character
- **Root represents empty string** - all words start from root
- **End-of-word markers** - flags to indicate complete words
- **Space-efficient for common prefixes** - shared prefixes save space

**How Trie Works:**
- Each level represents a character position in words
- Path from root to any node represents a prefix
- Complete words are marked with a boolean flag
- Children of a node represent possible next characters

**Trie Structure:**
```
        root
       /  |  \
      c   t   s
     /    |    \
    a     h     e
   / \    |     |
  r   t   e     a
      |   |
      s   m
```
Represents words: "car", "cats", "the", "them", "sea"

**Time Complexities:**
- **Insert**: O(m) where m is length of word
- **Search**: O(m) where m is length of word
- **Delete**: O(m) where m is length of word
- **Prefix Search**: O(p) where p is length of prefix

**Space Complexity:**
- Worst case: O(ALPHABET_SIZE × N × M)
- N = number of words, M = average length
- Can be optimized with compressed tries

**Advantages:**
- Fast prefix-based searches
- Efficient autocomplete functionality
- No hash collisions
- Alphabetical ordering of words

**Disadvantages:**
- High memory usage (especially for sparse tries)
- Cache locality issues
- Complex implementation compared to hash tables

**Applications:**
- Autocomplete in search engines
- Spell checkers and word games
- IP routing tables
- Phone contact searches
- Dictionary implementations
- DNA sequence analysis

### Implementation
```java
class TrieNode {
    TrieNode[] children;
    boolean isEndOfWord;
    
    public TrieNode() {
        children = new TrieNode[26]; // for lowercase letters
        isEndOfWord = false;
    }
}

class Trie {
    private TrieNode root;
    
    public Trie() {
        root = new TrieNode();
    }
    
    void insert(String word) {
        TrieNode current = root;
        for (char ch : word.toCharArray()) {
            int index = ch - 'a';
            if (current.children[index] == null) {
                current.children[index] = new TrieNode();
            }
            current = current.children[index];
        }
        current.isEndOfWord = true;
    }
    
    boolean search(String word) {
        TrieNode current = root;
        for (char ch : word.toCharArray()) {
            int index = ch - 'a';
            if (current.children[index] == null) {
                return false;
            }
            current = current.children[index];
        }
        return current.isEndOfWord;
    }
    
    boolean startsWith(String prefix) {
        TrieNode current = root;
        for (char ch : prefix.toCharArray()) {
            int index = ch - 'a';
            if (current.children[index] == null) {
                return false;
            }
            current = current.children[index];
        }
        return true;
    }
}
```

### Usage
- **Best for**: Autocomplete, spell checkers, IP routing, word games
- **Examples**: Search engines, phone contacts, dictionary lookups

---

## 10. Disjoint Set (Union-Find)

### What is a Disjoint Set?
A Disjoint Set (also called Union-Find or Merge-Find Set) is a data structure that keeps track of elements partitioned into disjoint (non-overlapping) subsets. It efficiently supports two operations: finding which subset an element belongs to, and merging two subsets.

**Key Characteristics:**
- **Disjoint subsets** - elements belong to exactly one subset
- **Dynamic connectivity** - can merge sets and query connectivity
- **Near constant time operations** - with optimizations
- **Root representative** - each set has a root element as representative

**Core Operations:**
1. **Find(x)** - determine which subset element x belongs to
2. **Union(x, y)** - merge the subsets containing x and y
3. **Connected(x, y)** - check if x and y are in the same subset

**How it Works:**
- Each element initially forms its own set
- Sets are represented as trees with root as representative
- Find operation traces parent pointers to root
- Union operation makes one root point to another

**Optimizations:**
1. **Path Compression** - during Find, make all nodes point directly to root
   - Flattens the tree structure
   - Reduces future Find operations to nearly O(1)

2. **Union by Rank** - attach smaller tree under root of larger tree
   - Keeps trees balanced
   - Prevents degenerate linear chains

**Time Complexities (with optimizations):**
- **Find**: O(α(n)) - inverse Ackermann function (practically constant)
- **Union**: O(α(n)) - inverse Ackermann function (practically constant)
- **Space**: O(n) - one parent pointer per element

**Without optimizations:**
- Both operations can be O(n) in worst case

**Applications:**
- **Kruskal's MST Algorithm** - detect cycles while building MST
- **Network Connectivity** - check if nodes are connected
- **Image Processing** - connected component labeling
- **Social Networks** - find connected groups
- **Maze Generation** - ensure all cells are connected
- **Cycle Detection** - in undirected graphs

**Real-world Examples:**
- Determining if two people are in the same friend group
- Checking if two computers are on the same network
- Finding connected regions in an image
- Detecting cycles in network routing

### Implementation
```java
class UnionFind {
    private int[] parent;
    private int[] rank;
    
    public UnionFind(int n) {
        parent = new int[n];
        rank = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            rank[i] = 0;
        }
    }
    
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // Path compression
        }
        return parent[x];
    }
    
    void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        
        if (rootX != rootY) {
            // Union by rank
            if (rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
            } else if (rank[rootX] > rank[rootY]) {
                parent[rootY] = rootX;
            } else {
                parent[rootY] = rootX;
                rank[rootX]++;
            }
        }
    }
    
    boolean connected(int x, int y) {
        return find(x) == find(y);
    }
}
```

### Usage
- **Best for**: Network connectivity, Kruskal's MST algorithm, cycle detection
- **Examples**: Social networks, clustering, maze generation

---

## 11. Important Lambda Expressions and Comparators

### Basic Comparators
```java
// Ascending order
Arrays.sort(arr, (a, b) -> a - b);
Collections.sort(list, (a, b) -> a - b);

// Descending order
Arrays.sort(arr, (a, b) -> b - a);
Collections.sort(list, Collections.reverseOrder());

// Custom object sorting
class Person {
    String name;
    int age;
    
    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

// Sort by age
Arrays.sort(people, (a, b) -> a.age - b.age);

// Sort by name
Arrays.sort(people, (a, b) -> a.name.compareTo(b.name));

// Multiple criteria sorting
Arrays.sort(people, (a, b) -> {
    if (a.age != b.age) return a.age - b.age;
    return a.name.compareTo(b.name);
});

// Using Comparator.comparing()
Arrays.sort(people, Comparator.comparing(p -> p.age));
Arrays.sort(people, Comparator.comparing(p -> p.name));

// Chaining comparators
Arrays.sort(people, 
    Comparator.comparing((Person p) -> p.age)
              .thenComparing(p -> p.name));
```

### Advanced Comparators
```java
// 2D array sorting
int[][] intervals = {{1,3}, {2,6}, {8,10}};

// Sort by start time
Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

// Sort by end time
Arrays.sort(intervals, (a, b) -> a[1] - b[1]);

// Priority Queue with custom comparator
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> {
    if (a[0] != b[0]) return a[0] - b[0];
    return a[1] - b[1];
});

// TreeMap with custom comparator
TreeMap<String, Integer> map = new TreeMap<>((a, b) -> b.compareTo(a)); // Reverse order

// Stream operations with lambdas
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Filter and collect
List<Integer> evens = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());

// Map and reduce
int sum = numbers.stream()
    .mapToInt(n -> n * n)
    .sum();

// Group by
Map<Boolean, List<Integer>> partitioned = numbers.stream()
    .collect(Collectors.partitioningBy(n -> n % 2 == 0));
```

---

## Performance Comparison Table

| Data Structure | Search | Insert | Delete | Access | Space | Best Use Case |
|----------------|--------|--------|--------|--------|-------|---------------|
| **Array** | O(n) | O(n) | O(n) | O(1) | O(n) | Random access, cache-friendly |
| **Dynamic Array** | O(n) | O(1)* | O(n) | O(1) | O(n) | Resizable arrays, most common |
| **Linked List** | O(n) | O(1) | O(1) | O(n) | O(n) | Frequent insert/delete |
| **Stack** | O(n) | O(1) | O(1) | O(n) | O(n) | LIFO operations |
| **Queue** | O(n) | O(1) | O(1) | O(n) | O(n) | FIFO operations |
| **Binary Search Tree** | O(log n)* | O(log n)* | O(log n)* | O(log n)* | O(n) | Sorted data with searches |
| **Heap** | O(n) | O(log n) | O(log n) | O(1) | O(n) | Priority operations |
| **Hash Table** | O(1)* | O(1)* | O(1)* | O(1)* | O(n) | Fast lookups, caching |
| **Trie** | O(m) | O(m) | O(m) | O(m) | O(ALPHABET_SIZE × N × M) | String prefix operations |
| **Graph (Adj List)** | O(V+E) | O(1) | O(V) | O(V) | O(V+E) | Sparse graphs |
| **Graph (Adj Matrix)** | O(1) | O(1) | O(1) | O(1) | O(V²) | Dense graphs |

**Legend:**
- \* indicates average case; worst case may be O(n)
- m = length of string (for Trie)
- V = vertices, E = edges (for Graph)
- Space complexity doesn't include input size

## When to Use Each Data Structure

### **Arrays/ArrayList**
- ✅ Need random access by index
- ✅ Cache-friendly memory access
- ✅ Simple mathematical operations
- ❌ Frequent insertions/deletions in middle

### **LinkedList**
- ✅ Frequent insertions/deletions
- ✅ Unknown size that changes often
- ✅ Memory is not contiguous
- ❌ Need random access
- ❌ Cache performance important

### **Stack**
- ✅ LIFO operations
- ✅ Function call management
- ✅ Undo operations
- ✅ Expression evaluation

### **Queue/PriorityQueue**
- ✅ FIFO operations
- ✅ BFS traversal
- ✅ Task scheduling
- ✅ Priority-based processing

### **HashMap/HashSet**
- ✅ Fast lookups
- ✅ Removing duplicates
- ✅ Counting frequencies
- ❌ Need sorted order
- ❌ Memory usage is critical

### **TreeMap/TreeSet**
- ✅ Need sorted order
- ✅ Range queries
- ✅ Finding predecessors/successors
- ❌ Just need fast lookups

### **Binary Search Tree**
- ✅ Sorted data with frequent searches
- ✅ Range queries
- ❌ Data might be heavily skewed

### **Heap/PriorityQueue**
- ✅ Finding min/max repeatedly
- ✅ Priority scheduling
- ✅ Heap sort
- ✅ Dijkstra's algorithm

### **Trie**
- ✅ Autocomplete features
- ✅ Spell checkers
- ✅ String prefix matching
- ❌ Memory usage is critical

### **Graph**
- ✅ Modeling relationships
- ✅ Network problems
- ✅ Pathfinding algorithms
- **Adjacency List**: Sparse graphs
- **Adjacency Matrix**: Dense graphs, edge queries

### **Union-Find**
- ✅ Dynamic connectivity
- ✅ Detecting cycles
- ✅ Kruskal's MST algorithm
- ✅ Social network components
