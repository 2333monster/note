# Shortest Paths

#### 1.1 Summary : DFS vs. BFS

**performance**

![image-20220926094056126](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926094056126.png)

**for Path Finding**

+ both of the two work for all graphs
+ **BFS** give better results
  + BFS is 2-for-1 deal, not only do you get paths, but you **path** are also guaranteed to **be shortest**
+ ***time efficiency*** is similar, both consider all edges twice
+ ***space efficiency***
  + DFS is worse for spindly graphs
    + calls stack gets very deep
    + computer needs $\Theta$ (V) memory to remember recursive calls
  + BFS is worse for absurdly bushy graph
    + queue gets very large , in worse case queue will require  $\Theta$ (V) memory
    + Example : 1,000,000 vertices are connected , 999,999 will be enqueued at once
  + Note: In our implementations, we have to spend Θ(V) memory anyway to track distTo and edgeTo arrays.
    + Can optimize(优化) by storing distTo and edgeTo in a map instead of an array.

**BreadthFirstSearch for Google Maps**

+ as we talked before , BFS can't handle graphs with weights
+ the shortest path it returns is only the path with least edges
+ quick example : 
  + ![image-20220926095527369](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926095527369.png)



#### 1.2 **Dijkstra’s Algorithm**

> handle the graphs with weights, find the shortest paths from source vertex s to some target vertex t



**shortest path will always be a tree**

+ ![image-20220926101150987](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926101150987.png)
+ test paths tree(SPT) vertex is V(), edges is V() - 1 



**Find SPT Algorithmically(Incorrect)**

> Algorithm begins in state below. 
>
> All vertices **unmarked**. All distances **infinite**. **No edges** in the SPT.
>
> ![image-20220926100206724](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926100206724.png)



+ #1 : perform a dfs ,when you visit v:  

  + for each edge from v to w , if w is not already part of SPT, add the edge

  + ![image-20220926100413173](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926100413173.png)
  + can't update the shortest path

+ \#2: Perform a depth first search. When you visit v:

  + For each edge from v to w, add edge to the SPT ***only if that edge yields better distance**.*

    ![image-20220926100607344](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926100607344.png)

  + need to compare all edges

+ Dijkstra's Algorithm: Perform a **best first search** (closest first). When you visit v:

  + For each from v to w, **relax that edge**.

    ![image-20220926100746485](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926100746485.png)

  + [[Dijkstra’s Algorithm Demo Link](https://docs.google.com/presentation/d/1_bw2z1ggUkquPdhl7gwdVBoTaoJmaZdpkV6MoAgxlJc/pub?start=false&loop=false&delayms=3000)]





**Dijkstra's Algorithm**

+ Dijkstra's Algorithm:
  + Visit vertices in **order of best-known distance** from source, On visit, **relax** every edge from the visited vertex.
  + Dijkstra's Algorithm is guaranteed to return a correct result if all edges are non-negative

+ Pseudocode

  + ![image-20220926101658852](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926101658852.png)

+ **证明 依赖于属性的relax 操作 总是在已经Marked 的白色节点失败**

  + At start, distTo[source] = 0, which is optimal(最佳).
  + After relaxing all edges from source, let vertex v1 be the vertex with minimum weight, i.e. that is closest to the source. Claim: distTo[v1] is optimal, and thus future relaxations will fail. Why? 
    + distTo[p]			$\geq$ distTo[v1] for all p, therefore
    + distTo[p]  + w	$\geq$ distTo[v1]
  + can use induction(归纳法) to prove that this holds for all vertices after dequeuing

  

  **Runtime**

  + ![image-20220926102914461](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926102914461.png)

  

  

#### 1.3 $A^{*}$

**$A^{*}$ vs. Dijkstra's**

+ Dijkstra's : single source , multiple targets
+ $A^{*}$ : single source, single Target
+ ![image-20220926104022197](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926104022197.png)



**Intro**

+ ![image-20220926103351386](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926103351386.png)

**Demo** [A* Demo Link](https://docs.google.com/presentation/d/177bRUTdCa60fjExdr9eO04NHm0MRfPtCzvEup1iMccM/edit#slide=id.g771336078_0_180)

+ ![image-20220926103600575](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926103600575.png)



**A^{*} Heuristic Example**

+ ![image-20220926104142273](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926104142273.png)





#### 1.4 **A\* Heuristics**

**impact of heuristic quality**

+ ![image-20220926104338090](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926104338090.png)



**Heuristics Correctness**

+ ![image-20220926104418651](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926104418651.png)





#### 1.5 summary

+ ![image-20220926104518551](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926104518551.png)
+ ![image-20220926104523169](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926104523169.png)



  