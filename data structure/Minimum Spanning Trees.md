# 1. Minimum Spanning Trees

#### 1.1 Warm-up Problem

**Given a undirected graph , determine if it contains any circles**

+ ![image-20220926201335302](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926201335302.png)

+ Approach 1: Do DFS from 0(arbitrary vertex)
  + Keep going  until you see a marked vertex
  + potential danger:
    + 1 looks back at 0 and sees marked
    + Solution: just don't count the node you came from
  + worst case runtime: O(V + E) 

+ Approach 2: Use a WeightedQuickUnionUF object
  + For each edge, check if the two vertices are connected
    + if not, union them
    + if so, there is a cycle
  + worst case runtime : O(V + E Log*V) if we have path compression



#### 1.2 Minimum Spanning Trees

**Spanning Trees**

give an undirected graph, a ***spanning tree*** T is a subgraph of G, where T:

+ is connected
+ is acyclic
+ Includes all of the vertices
  + ![image-20220926202113660](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926202113660.png)

*** minimum spanning trees(MST)***

+ is a spanning tree of minimum total weight

**MST application**

 http://www.ics.uci.edu/~eppstein/gina/mst.html



**MST vs. SPT**

+ SPT : A shortest paths tree depends on the start vertex
  + Because it tell you how to get from a source to EVERTHING
+ MST: there is no source for a MST
+ Nonetheless, the MST sometimes happens to be an SPT for a special vertex



### 1.3 Finding The MST

**Can we find MST using SPT ?**

give a valid MST for the graph below.

![image-20220926202907959](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926202907959.png)

+ Is there a node whose SPT is also the MST
+ **No !!!** MST must include only 2 of the 2 weight edges, but SPT always includes at least 3 of the 2 weight edges
+ ***we can't use the strategy of finding MST through SPT***



**Useful tool for finding : Cut Property**

+ ***cut property*** : Given any cut,  minimum weight crossing edge is in the MST

+ ![image-20220926203507342](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926203507342.png)

  

+ Nodes **cut is arbitrary**
  + ![image-20220926203919877](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926203919877.png)

**Cut Property Proof**

>  proof by contradiction(反证法) : Suppose the **minimum crossing edge** is not on the  MST. for the two parts of the graph connected by this edge, there must also be a **crossing edge** on the MST, because the MST needs to connect all the vertices. therefore , replace the **crossing edge** with the **minimum crossing edge** , and the MST's weight will be smaller, so the MST at this time is not the real MST, which is a contradiction

+ ![image-20220926205004016](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926205004016.png)





**Generic MST Finding Algorithm**

+ ![image-20220926205056104](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926205056104.png)



#### 1.4 Prim's Algorithm

**Algorithm**

![image-20220926205235468](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926205235468.png)

**[Example demo](https://docs.google.com/presentation/d/1NFLbVeCuhhaZAM1z3s9zIYGGnhT4M4PWwAc-TLmCJjc/edit#slide=id.g9a60b2f52_0_0)** (Conceptual 概念的)

**Why does prim's work?**

![img](https://lh3.googleusercontent.com/esCvZvC1c3BAs2SZyqEWsshwT3KEJk0W8pef312BIB5cOTEwnHHRJ5VuAOtD-uP9Pn03-t5Speg0Z8H5eQJwpE2eDVkm8pfbwsUTmOypiAhuEezeM4v1NcjXBL9yxdUXXgVtZSkmZTD8Sk7eJeyA6cmpqYHDiwrIa9NzgPsl29r5K0vpu82BY2HNiyA)

+ Suppose we add edge e = v ->w
+ Side 1 of cut is all vertices connected to start, side 2 is all the others
+ No crossing edge is black (all connected edges on side 1).
+ No crossing edge has lower weight (consider in increasing order).

**Realistic Implementation Demo([Link](https://docs.google.com/presentation/d/1GPizbySYMsUhnXSXKvbqV4UhPCvrt750MiqPPgU-eCY/edit#slide=id.g9a60b2f52_0_0))**



**Prim’s vs. Dijkstra’s**

+ Prime consider **"distance from the tree"**
+ Dijkstra's consider **"distance from the source"**
+ ![image-20220926210148531](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926210148531.png)



**Prim's Implementation**

+ ![image-20220926210258931](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926210258931.png)
+ ![image-20220926210303651](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926210303651.png)



**runtime**

+ ![image-20220926210349098](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926210349098.png)



#### 1.5 **Kruskal’s Algorithm**

**Kruskal’s Algorithm**

+ Kruskal's Implementation
  + 边按weight顺序排列
  + 逐个添加edge到MST中
  + 除非加入会产生cycle
+ ![image-20220926210553974](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926210553974.png)

**Proof**

+ add edge e = v -> w
  + edge 链接的两个side（两个顶点各自的集合）
+ 加入后不产生cycle 保证了目前两个side 间没有crossing edge 在MST中；
+ 按照从小到大添加， 保证了此时没有任何crossing edge 比这条edge 大
+ 所以根据之前的property ， 这条edge 一定在MST上

**Conceptual demo ([link](https://docs.google.com/presentation/d/1RhRSYs9Jbc335P24p7vR-6PLXZUl-1EmeDtqieL9ad8/edit?usp=sharing))**

**Realistic Implementation demo ([link](https://docs.google.com/presentation/d/1KpNiR7aLIEG9sm7HgX29nvf3yLD8_vdQEPa0ktQfuYc/edit?usp=sharing))**

**Kruskal's runtime**

+ ![image-20220926211503157](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926211503157.png)



#### 1.6 Summary

+ ![image-20220926211547603](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220926211547603.png)

