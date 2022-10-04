# 1. Graphs I

#### 1.1 Tree Traversals

> tree is an special kind of graph

**Level Order**

+ visit top-to-bottom, left-to-right

**Depth First Traversals**

+ preorder
  + ![image-20220925192040614](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925192040614.png)
+ inorder
  + ![image-20220925192052894](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925192052894.png)
+ Postorder
  + ![image-20220925192109892](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925192109892.png)

**A Useful Visual Trick**

+ preorder : we trace a path around the path, from the top going counter-clockwise."visit" every time we pass the Left of a node.
+ inorder: "Visit" when you cross the bottom of a node
+ Postorder : "Visit" when you cross the right a node
  + example:
  + ![image-20220925192441567](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925192441567.png)
  + 4 7 8 5 2 9 6 3 1



#### 1.2 Graphs

**Graph Terminology(术语)**

+ ![image-20220925192749875](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925192749875.png)

  



#### 1.3 Graph Problems

**Graph queries more theoretically**

+ ![image-20220925193121810](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925193121810.png)



#### 1.4 **Graph Traversal**



**solve a classical graph problem called the s-t connectivity problem**

+ s-t Path
+ requires us to traverse the graph somehow

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925194831498.png" alt="image-20220925194831498" style="zoom:150%;" />





**DepthFirstPaths(DFS)**(深度优先)

+ also called **DFS Preorder**
+ **Action** is **before** DFS calls to neighbors
  + Our **action** was setting edgeTo
  + example : edgeTo[1] was set before
  + DFS calls to neighbors 2 and 4
+ DFS preorder for this graph :012543678
+ Equivalent to the order of ***dfs calls***



**DFS Postorder**

+ **Action** is **after** DFS calls to neighbors
+ example: `dfs(s)`:
  + mark(s)
  + for each unmarked neighbor n of s, dfs(n)
  + print(s)
+ results for dfs(0) would be :347685210
+ Equivalent to the order of ***dfs returns***



**BFS order**

act in order of distance from s

+ BFS stands for "breadth first search"
+ Analogous to "level order" , search is wide , not deep
+ 0 1 24 53 68 7







# 2 Graphs II: Graph Traversal Implementations

#### 2.1 BreadthFirstPaths

**Breadth First Search**

+ Initialize a **queue** with starting vertex s and mark that vertex
  + A queue is a list that has two operations : **enqueue** (a.k.a. addLast) and **dequeue**(a.k.a. removeFirst)
  + let's call this queue our ***fringe***
+ Repeat until queue is empty :
  + Remove vertex v from the front of the v:
  + for each unmarked neighbor n of v:
    + mark n;
    + set edgeTo[n]  =  v (and/or distTo[n] = distTo[v] + 1).
    + add n to end of queue



**Problem**

+ can't find the shortest path



#### 2.2 Graph API

> To implement our graph algorithms like BFS and DFS.
>
> we need:

+ An **API** (Application Programming Interface) for graphs
  + includes Graph methods, their signatures and behaviors
  + Defines how graph clients programmers must think
+ An underlying data structures to represent our graphs.



**Integer Vertices**

+ use  a Map<Label, Integer>

+ Map<String, Integers>:
  + Austin : 0
  + Dallas : 1
  + Houston : 2



**optional Graph API**

+ ![image-20220925205439299](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925205439299.png)
+ some features
  + number of vertices must be specified in advance(必须优先指定顶点数)
  + doesn't support weights on nodes or edges
  + has no method for getting degrees
+ base API **degree**
  + ![image-20220925210211744](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925210211744.png)
+ base API **print**
  + ![image-20220925210322235](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925210322235.png)

 #### 2.3 Graph Representations

**Representations**

+ 1 : Adjacency Matrix
  + ![image-20220925211615778](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925211615778.png)

+ 2 : Edge sets: collection of all edges
  + ![image-20220925211411362](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925211411362.png)
+ 3 : Adjacency lists
  + ![image-20220925211518401](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925211518401.png)
  + ![image-20220925212132617](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925212132617.png)

----

**runtime analysis**

+ ![image-20220925211756481](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925211756481.png)

**Undirected Graph Implementation**

+ ![image-20220925212004103](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925212004103.png)

  



#### 2.4 **Graph Traversal Implementations and Runtime**

**DFS Implementation**

+ *Common design pattern :*
+ create a graph object
  
+ Pass the graph to a graph-processing method (or constructor) in a client class.
  
+ query(查询) the client class for information

![image-20220925212559088](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925212559088.png)

+ *Example Usage:*
  start by calling : `Paths P = new Paths(G, 0)`
  + `P.hasPathTo(3); //returns true`
  + `P.pathTo(3); //returns {0, 1, 4, 3}`
+ ***Implementation***
  + ![image-20220925212926863](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925212926863.png)

​				![image-20220925213115177](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925213115177.png)

+ Runtime for DFS

  ![image-20220925213334656](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925213334656.png)

+ Graph Problems

  ![image-20220925213701056](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925213701056.png)



**DFS implementation**

+ Implementation

  ![image-20220925213755331](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925213755331.png)

+ Graph Problems

  ![image-20220925213824141](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925213824141.png)

  

#### 2.5 Summary

![image-20220925214303813](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925214303813.png)

![image-20220925214313953](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925214313953.png)

