# CMPS 2200 Assignment 4
## Answers

**Name:**Natalie Gockerman






- **1a.**
- Since a full d-ary tree has N nodes given a height of h:
- N(h) = (d^(h+1) - 1)/(d - 1)
- Therefore after solving for h:
- h = O(logd N)
- In Dijkstra's algorithm N <= |V|.
- This shows that the maximum depth of a d-ary tree is O(logd |V|).


- **1b.**
- For each operation using:
-   Total Work = Cost per level x number of levels
-   Number of levels = depth = O(logd |V|)
- Considering delete-min:
-   at each level to move down, must find mimumum of d children by comparing against all --> O(d) comparisons
-   Total work = O(d) x O(logd |V|) = O(dlogd |V|)
- Considering insert:
-   at each level to move up, each element is compared with its parent --> O(1) comparisons
-   Total work = O(1) x O(logd |V|) = O(logd |V|)


- **1c.**
- |V| = number of verticies
- |E| = number of edges
- First, we must analyze how many times each operation (delete-min and insert) is called:
-   delete-min - each vertex is extraced once --> |V| calls
-   insert - each directed edge may cause insert to be called --> at most |E| operations
- Then, take the number of calls into account for total work of each operation:
-   delete-min = O(|V|dlogd |V|)
-   insert = O(|E|logd |V|)
- Now, sum these work amounts to find total work:
-   W = O(|V|dlogd |V|)) + O(|E|logd |V|) = O((|V|d + |E|)logd |V|)
-   Therefore, new bound = O((|V|d + |E|)logd |V|)



- **1d.**
- Given:
-   W = O((|V|d + |E|)logd |V|)
-   |E| = |V|^(1 + ε), ε > 0
- Therefore:
-   W = O((|V|d + |V|^(1 + ε))logd |V|)
-   Simplify this --> W = O((d|V| + |V|^(1 + ε)) x (log |V| / log d)
-   To have run-time of O(|E|), inside the work bound we need |V|^(1 + ε) to dominate the first term and the factor to be constant.
- First, set the delete-min term cost to about equal to insert:
-   d|V| = |V|^(1 + ε)
-   d = |V|^((1 + ε) - 1)
-   d = |V|^ε
- Then, calculate W with this d value, split into parenthesis term and log factor:
-   d|V| --> |V|^ε x |V| = |V|^(1 + ε) = |E|
-   |E| --> |V|^(1 + ε) = |E|
-   parenthesis term: d|V| + |E| = O(|E| + |E|) = O(|E|)
-   log factor: log |V| / log d = log |V| / log(|V|^ε)
-     simplify further --> log |V| / εlog |V| = 1/ε --> constant factor as needed
- Therfore the final bound is W = O(|E| x (1/ε) = O(|E|)
- SO... optimal value of d = |V|^ε



- **2a.**
- APSP(i,j,0):
-   (0,0,0) --> 0
-   (0,1,0) --> 1
-   (0,2,0) --> -2
-   (1,0,0) --> infinity
-   (1,1,0) --> 0
-   (1,2,0) --> 2
-   (2,0,0) --> infinity
-   (2,1,0) --> infinity
-   (2,2,0) --> 0
- APSP(i,j,1):
-   (0,0,1) --> 0
-   (0,1,1) --> 1
-   (0,2,1) --> -2
-   (1,0,1) --> infinity
-   (1,1,1) --> 0
-   (1,2,1) --> 2
-   (2,0,1) --> infinity
-   (2,1,1) --> infinity
-   (2,2,1) --> 0
- APSP(i,j,2):
-   (0,0,2) --> 0
-   (0,1,2) --> 1
-   (0,2,2) --> -2
-   (1,0,2) --> infinity
-   (1,1,2) --> 0
-   (1,2,2) --> 2
-   (2,0,2) --> infinity
-   (2,1,2) --> infinity
-   (2,2,2) --> 0


- **2b.**
- Yes, these match the Floyd-Warshall recurrence
- Therfore APSP(i,j,2) = min(APSP(i,j,1), APSP(i,2,1) + APSP(2,j,1))


- **2c.**
- Generalizing this APSP(i,j,k) = min(APSP(i,j,k-1), APSP(i,k,k-1) + APSP(k,j,k-1)
- this means either the shortest path from i to j does or does not use the vertex k 

- **2d.**
- in this there are:
-   n choices for i
-   n choices for j
-   n choices for k
-   therefore there are a total of n^3 subproblems
- considering each recurence of APSP(i,j,k) takes O(1) work:
-   total running time = O(n^3)

  

- **2e.**
- Running time of Johnson's Algorithm = O(|V||E|log|V|)
- Running time of Floyd-Warshall = O(|V|^3)
- DP is preferable when the graph is dense or when |E| is approximately |V|^2
- this would mean Johnson's runtime would be O(|V|^3logV) whereas DP would be O(V^3)
- DP is also preferrable when the graph is small
- DP is also preferrable when the graph has negative edges but no negative cycles because it supports negative weights better



- **3a.**
- Let T(MST) = min spanning tree
- Let max edge weight be M
- On the other hand, suppose another smapping tree T' has a max edge weight less than M
- In the MST when heaviest edge weight is selected:
-   Cut Property demosntrates that min edge weight must be in every MST
-   T' avoids the edge and replaces it with smaller which is impossible because if a smaller edge existed, Kruskal's algorithm would've recognized it and a smaller edge cannot exist.
- Therefore every MST is guaranteed to be an MMET.


- **3b.**
- To do this we must remove each edge one at a time and replace it with non-tree edge that reconnects the graph.
- Algorithm (english explanation):
-   T - MST
-   E(T) = edges of MST
-   E/T = non-tree edges
-   First, compute the MST using Kruskal or Prim
-   Then, for each edge (e) in E(T):
-     temporarily remove e from the MST
-     among all edges in E/T, pick the minimum-weight edge (f)
-     add each edge f to reconnect and produce spanning tree
-     compute the total weight of Te
- Finally, choose the tree within all Te that has a weight just above that of the MST


- **3c.**
- |V| = n
- |E| = m
- Computing MST --> O(mlogn)
- At each edge it is split and checked:
-   total per edge = O(n+m)
-   there are n - 1 edges --> O(nm + n^2)
- Total work = O(mlogn) + O(nm + n^2)



























