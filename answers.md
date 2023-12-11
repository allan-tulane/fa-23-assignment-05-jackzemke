# CMPS 2200 Assignment 5
## Answers

**Name:** Jack Zemke






- **1a.**
    - The depth of a tree is dependent on the number of nodes in the tree. Given the variable $d$ as the number of children each node must have, I will denote a variable of $h$ for the depth of the heap and $n$ for the number of nodes contained within the heap. 
    - We will begin the analysis by recognizing that at any level $i$ there are $d^i$ nodes at that level. Therefore, within the entire tree, there are a total of $1+d+d^2+d^3+...+d^{h-1}$ nodes. This is a geometric series where the common term is $d$, and the sum at an arbitrary term $S$ represents the total number of nodes of *all* the levels when it is $S$ levels deep. The equation for the sum of the series (total number nodes) at the $h$-th term (depth = $h$) of this geometric series is $n = 1(\frac{d^{h}-1}{d-1})$. Now, we solve for $h$ to find the maximum depth. $n = \frac{d^{h}-1}{d-1} \rightarrow n(d-1) = d^h-1 \rightarrow n(d-1)+1 = d^h \rightarrow$ our maximum depth $h = \log_d(n(d-1)+1) $. This will become $h = \log_dn$ as $n$ asymptotically grows. 


- **1b.**

    - `delete-min`: this operation removes the root (constant work) places the rightmost 
    leaf at the root position (constant work) and then restores the heap property by 
    swapping downward. At each step in the downward traversal, $d$ work is executed. T
    his is due to the fact that at each level, one must compare the root with all $d$ 
    of its children and potentially swap the nodes. This will be done $\log_d n$ times 
    (the complete depth of the tree), so the work of `delete-min` $\in O(d \log_d n)$.
    - `insert`: this operation places the new element at the rightmost leaf (constant work) 
    and swaps upward to restore the heap property. Because the only swaps that would 
    potentionally need to be done are between the node and the parent, the comparisons
    are completed with constant work. Thus, the work of `insert` $\in O(\log_d n)$.


- **1c.**

    - The work for Dijkstra's algorithm was initially $O(|E| \log |E|)$, which accounts for the work of `heappop` $\in O(\log n)$ and the work of `heappush` $\in O(\log n)$, as well as the fact that these functions are called once per edge. 
    - The work of a $d$-ary's `delete-min` $\in O(d \log_d n)$ and the work of a $d$-ary's `insert` $\in O(\log_d n)$. 
        - To obtain the Big-O notation, we must consider the worst running time, so we will use the dominating term of $d\log_dn$. Because we consider the worst-case, we will assume that each edge in the heap is added, so we obtain a work of $O(d \log_d |E|)$
    

- **1d.**

    - To yeild a work of $O(|E|)$, we need the term $d \log_d |E| = |E|$. If we set $d = |E|$, $\log_{|E|}|E| = 1$ so $|E|\log_{|E|}|E| = O(|E|)$. Therefore, the optimal value of $d$ is $|E|$.

- **2a.**

        APSP(1,0,2) = APSP(1,0,1) = APSP(1,0,0) = -2

        APSP(0,1,1) = APSP(0,1,2) = APSP(0,1,0) = -2

        APSP(0,2,0) = 2

        APSP(0,2,2) = APSP(0,2,1) = -1

        APSP(2,0,0) = 2

        APSP(2,0,2) = APSP(2,0,1) = -1

        APSP(1,2,2) = APSP(1,2,1) = APSP(1,2,0) = 0

        APSP(2,1,1) = APSP(2,1,2) = APSP(2,1,0) = 0


- **2b.**
    - $APSP(i, j, 1) = APSP(i, j, 2)$ for all $i,j$. Because both of the edges connected to node $2$ are positively weighted, is it never more efficient to go through node $2$. 
        - $APSP(i, j, 2) = APSP(i, j, 1)$

- **2c.**
    - $APSP(i,j,k) = min \begin{cases} APSP(i,j,k-1) \\ APSP(i,k,k-1)+APSP(k,j,k-1) \end{cases}$

- **2d.**
    - Naively, we would consider $APSP(i,j,k)$ for all $i,j \in |V|$ and $k \in |E|$. This would yield $|V|^2|E|$ subproblems and a work of $O(|V|^2|E|).$
        - However, we can utilize memoization assuming this problem is always conducted on an undirected graph. We can observe that all $APSP(i,j,k) = APSP(j,i,k)$. As such, we only have to consider $|V||E|$ cases instead of the naive $|V|^2|E|$ cases. Thus, our algorithm has a work of $O(|V||E|)$.

- **2e.**
    - Johnson's algorithm has a work of $O(|V| \cdot |E| \log |E|)$, and our algorithm has a work of $O(|V| \cdot |E|)$. Our algorithm is more efficient when $|E| \lt |E| \log |E|$. This is the case when $|E| \gt 10$. Therefore, the ideal choice of algorithm depends on the number of edges, with the threshold of decision at $|E| = 10$. 



- **3a.**
    - Proof by contradiction: Suppose we have a Minimum Spanning Tree, $T$, and an Minimum Maximum Edge Tree $T'$. Our initial assumption is that $T \neq T'$. Let us consider the maximum weighted edge in $T',e$. By nature, $T \text{ and } T'$ both connect all vertices. If we assume $e$ is not in $T$, then there must be some other edge, $e'$ that connects the same vertices as $e$. If this edge is in $T$, then by nature it minimizes the total length, so it must be smaller than $e$ which violates the condition of $T'$ that the minimum edge is prioritized. Thus we have a reached a contridiction, and our initial assumption that $T \neq T'$ is negated. A solution to MST is guaranteed to be in MMET.
- **3b.**
    - One could modify Prim's algorithm to first find the most ideal MST, and then iteratively swap each edge of the MST with some other edge in the graph. The results would be tracked and sorted in a heap, and once the algorithm has run excluding each edge from the original MST, the second best solution would be returned. 


- **3c.**
    - This implimentation is much more efficient than a brute force approach, as it runs Prim's algorithm only once then executes swaps in constant time. This algorithm has the same work as Prim's algorithm, $O(|E| log |E|)$.
