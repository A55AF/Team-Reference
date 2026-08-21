# Team Reference Document: The Black Box Manual

This is your team's complete field manual for the ECPC finals. It provides a brief description and a classic use-case for every algorithm and data structure in your vault. When you identify a problem type during the contest, use this manual to quickly verify which "black box" template you need to copy.

---

## Bit Manipulation

### 1. Bitwise Operators Formulas
**What it solves:** Common bit-twiddling hacks (e.g., getting the lowest set bit, checking if a number is a power of 2, isolating bits).
**Classic Example:** Iterating over all submasks of a given bitmask efficiently in $O(3^N)$ instead of $O(4^N)$.

---

## Data Structures

### 1. Segment Tree & Lazy Segment Tree
**What it solves:** Answers range queries (sum, min, max, gcd) and handles point or range updates in $O(\log N)$ time.
**Classic Example:** Add value $V$ to all elements in the range $[L, R]$, and query the maximum value in the range $[X, Y]$.

### 2. Persistent Segment Tree
**What it solves:** A Segment Tree that remembers its previous states. Instead of overwriting during an update, it creates a new "version" in $O(\log N)$ memory.
**Classic Example:** Querying the $K$-th smallest number in a subarray $[L, R]$ by using a prefix-sum of segment trees.

### 3. Sparse Table
**What it solves:** Answers static (no updates) Range Minimum/Maximum Queries (RMQ) in strict $O(1)$ time after an $O(N \log N)$ build.
**Classic Example:** You need to answer $10^6$ RMQ queries on a static array where an $O(\log N)$ Segment Tree query would time out.

### 4. Binary Trie
**What it solves:** A Trie specifically for the binary representations of numbers. Fast at finding max/min XOR pairs.
**Classic Example:** Given an array, find a pair of numbers $(A_i, A_j)$ that maximizes $A_i \oplus A_j$.

### 5. Ordered Set & Multiset (PBDS)
**What it solves:** The GNU policy-based data structures provide a `std::set` that supports two extra $O(\log N)$ operations: finding the $K$-th smallest element, and finding the number of elements strictly smaller than $X$.
**Classic Example:** Finding the median of a sliding window in an array.

### 6. Implicit Treap
**What it solves:** A randomized binary search tree that treats array indices as keys. It can split, merge, insert, delete, and reverse subarrays in $O(\log N)$.
**Classic Example:** "Cut the subarray from index $L$ to $R$ and paste it at the beginning of the array."

### 7. Mo's Algorithm
**What it solves:** Answers offline range queries in $O(N \sqrt{Q})$ for problems where moving endpoints (add/remove) is easy but merging two ranges is hard.
**Classic Example:** Count the number of distinct elements in the subarray $[L, R]$.

---

## Strings

### 1. Trie
**What it solves:** Stores a dictionary of strings as a tree. Fast prefix matching.
**Classic Example:** Given a list of words, find how many words have a specific string as a prefix.

### 2. String Hashing
**What it solves:** Converts a string into a polynomial hash, allowing $O(1)$ equality checks for any two substrings.
**Classic Example:** Does the substring $S[L_1 \dots R_1]$ exactly match the substring $S[L_2 \dots R_2]$?

### 3. Z-Algorithm & KMP
**What it solves:** Finds all occurrences of a pattern in a text in $O(N + M)$ time.
**Classic Example:** Count how many times the word "ECPC" appears in a $10^5$ character string.

### 4. Manacher's Algorithm
**What it solves:** Finds the length of the longest palindromic substring centered at every single character in strict $O(N)$ time.
**Classic Example:** What is the longest contiguous palindromic substring in $S$?

### 5. Suffix Array
**What it solves:** Sorts all suffixes lexicographically. With the LCP array, it exposes all substring properties.
**Classic Example:** Find the number of *distinct* substrings in a string.

### 6. Aho-Corasick Automaton
**What it solves:** Searches for multiple patterns in a text simultaneously in $O(N)$.
**Classic Example:** Given a dictionary of 1000 forbidden words and a large text, find all occurrences of any forbidden word.

---

## Graph Theory

### 1. Disjoint Set Union (DSU)
**What it solves:** Keeps track of a set of elements partitioned into a number of disjoint subsets. Nearly $O(1)$ time to merge sets and check connectivity.
**Classic Example:** Kruskal's algorithm for Minimum Spanning Tree, or finding connected components dynamically.

### 2. Lowest Common Ancestor (LCA)
**What it solves:** Finds the lowest node that is an ancestor of both nodes $U$ and $V$ in a tree. Usually done in $O(\log N)$ with Binary Lifting.
**Classic Example:** Finding the exact distance between two nodes in a tree using the formula: $dist(u) + dist(v) - 2 \times dist(LCA(u, v))$.

### 3. Heavy-Light Decomposition (HLD)
**What it solves:** Flattens a tree into paths so that queries between $U$ and $V$ can be answered using a Segment Tree in $O(\log^2 N)$.
**Classic Example:** Update node weights and query the maximum weight on the path between $U$ and $V$.

### 4. Centroid Decomposition
**What it solves:** A divide-and-conquer strategy for trees that solves problems regarding *all paths* that satisfy a condition.
**Classic Example:** Find the number of pairs of nodes whose distance is exactly $K$.

### 5. Maximum Flow (Dinic's) & MCMF
**What it solves:** Finding the maximum "flow" through a network of capacities. MCMF adds costs to the edges and minimizes the cost for the maximum flow.
**Classic Example:** Assigning $N$ workers to $M$ jobs where each worker has a specific set of jobs they can do (Bipartite Matching).

### 6. 2-SAT
**What it solves:** Solves boolean satisfiability for clauses with 2 variables.
**Classic Example:** Assigning Truth values given constraints like "If A is true, B must be false".

### 7. Bridge-Block Tree / Articulation Points
**What it solves:** Finds edges (bridges) or nodes (articulation points) whose removal increases the number of connected components.
**Classic Example:** In a road network, find all roads that, if destroyed, will disconnect two cities.

### 8. Eulerian Path
**What it solves:** Finds a path that visits every *edge* of a graph exactly once.
**Classic Example:** Reconstructing a DNA sequence from overlapping $K$-mers (De Bruijn graphs).

---

## Math & Number Theory

### 1. Sieve (Standard, Linear) & SPF
**What it solves:** Precomputes primes, Smallest Prime Factors (SPF), and multiplicative functions like Euler's Totient up to $N$.
**Classic Example:** Factoring $10^5$ numbers up to $10^7$ in $O(\log(\text{num}))$ time per query using SPF.

### 2. Combinatorics & Divisibility
**What it solves:** $nCr \pmod M$ using precomputed factorials and modular inverse. Divisibility and GCD tricks.
**Classic Example:** How many ways can you choose $K$ items from $N$ items modulo $10^9+7$?

### 3. Extended Euclidean & CRT
**What it solves:** Solves $Ax + By = \gcd(A, B)$ (Linear Diophantine). Chinese Remainder Theorem solves systems of congruences.
**Classic Example:** Finding $X$ when you only know $X \pmod{M_1}$ and $X \pmod{M_2}$.

### 4. Matrix Exponentiation
**What it solves:** Computes the $N$-th term of a linear recurrence relation in $O(K^3 \log N)$.
**Classic Example:** Find the $10^{18}$-th Fibonacci number modulo $10^9+7$.

### 5. Fast Fourier Transform (FFT)
**What it solves:** Multiplies polynomials in $O(N \log N)$.
**Classic Example:** Given sets $A$ and $B$, how many ways can you pick one item from each to sum to $S$?

### 6. Gaussian Elimination
**What it solves:** Solves a system of linear equations or finds the basis of a set of numbers under XOR.
**Classic Example:** Finding the probability of reaching state $A$ from state $B$ in a graph with cycles.

### 7. Mobius Inversion
**What it solves:** Simplifies sums involving GCDs over large ranges.
**Classic Example:** Compute $\sum_{i=1}^N \sum_{j=1}^M \gcd(i, j)$.

---

## Dynamic Programming

### 1. DP Tricks
**What it solves:** Common patterns like prefix-sum DP optimization or state-space reduction.

### 2. Convex Hull Trick (Li Chao Tree)
**What it solves:** Optimizes DP transitions of the form $DP[i] = \min(m_j \cdot x_i + c_j)$.
**Classic Example:** Partitioning an array where the cost depends linearly on the segment properties.

### 3. Divide and Conquer DP
**What it solves:** Optimizes 2D DP $dp[i][j] = \min_{k < j} (dp[i-1][k] + C(k, j))$ from $O(K \cdot N^2)$ to $O(K \cdot N \log N)$ when the optimal split point is monotonic.
**Classic Example:** Putting $N$ elements into $K$ groups to minimize a specific cost function.

### 4. Sum Over Subsets (SOS DP)
**What it solves:** Computes the sum over all submasks for every mask in $O(N \cdot 2^N)$.
**Classic Example:** For every bitmask, find the sum of array values of its submasks.

---

## Geometry

### 1. Geometry Primitives
**What it solves:** The boilerplate for Points, Vectors, cross/dot products, and intersections.
**Classic Example:** Finding if two line segments intersect.

### 2. Convex Hull
**What it solves:** Finds the smallest convex polygon enclosing a set of points.
**Classic Example:** Minimum fence length to enclose all trees.

### 3. Rotating Calipers
**What it solves:** Finds extremal properties (like diameter) of a convex polygon in $O(N)$.
**Classic Example:** Finding the furthest pair of points in a set.

### 4. Half-plane Intersection
**What it solves:** Finds the region satisfying a set of linear inequalities.
**Classic Example:** Finding the "kernel" of a polygon (the area from which the entire polygon is visible).
