# 1. Reduction

#### 1.1 **Topological Sorting**

+ 对一个有向无环图(Directed Acyclic Graph简称DAG)G进行拓扑排序，

  + 是将G中所有顶点排成一个线性序列，
  + 使得图中任意一对顶点u和v，若边<u,v>∈E(G)，则u在线性序列中出现在v之前。

  ![image-20221003132203000](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003132203000.png)



**solution**

+ Record DFS post order in a list
+ Topological ordering is given by the reverse of that list(reverse post order) 

**demo**

![image-20221003132422340](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003132422340.png)

![image-20221003132433470](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003132433470.png)

+ Record DFS post order in a list : [7,4,1,3,0,6,5,2]
+ Topological ordering is given by the reverse of that list(reverse post order):
  + [2, 5, 6, 0, 3, 1, 4, 7]



**style of the structure**

​	![image-20221003132743365](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003132743365.png)

+ The reason why it's called topological sort:
  + Can think of this process as sorting our nodes so they appear in an order consistent with edges,  e. g. [2, 5, 6, 0, 3, 1, 4, 7]
+ when node are sorted in diagram,  arrows all point rightwards.(箭头都指向右边)



**Topological sort**

+ For Topological Sort, we restarted **from** every vertex with **indegree(度数) 0** 

**Another** better topological sort algorithm:

+ Run DFS **from** an **arbitrary vertex**

+ If not all marked, pick an unmarked vertex and do it again
+ Repeat until done



**runtime**

![image-20221003133530948](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003133530948.png)



#### 1.2 **Shortest Paths on DAGs**

+ What is the shortest paths tree for the graph below, using s as the source?

  In what order will Dijkstra’s algorithm visit the vertices?

  ![image-20221003133640078](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003133640078.png)

+ 0, 1, 3, 4, 5, 2

  ![image-20221003133751226](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003133751226.png)



**The DAG SPT Algorithm: Relax in Topological Order**

+ Allows negative edges. Dijkstra's algorithm can fail

![image-20221003133915384](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003133915384.png)

+ One simple idea: ==Visit vertices in topological order==
  + On each visit, relax all outgoing edges.
  + Each vertex is visited only when all possible info about it has been used!

+ example [demo](https://docs.google.com/presentation/d/1CfnLS3FSXV8X2sXPTravZGXeBUUkcFQv7Uf2iGWGUfs/edit?usp=sharing): 

  + Step First

  ![image-20221003134424256](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003134424256.png)

  + Step Second: We have to visit all the vertices in topological order, relaxing all edges as we go. 

**runtime**

![image-20221003134736710](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003134736710.png)



#### 1.3 **The Longest Paths Problem on DAGs**

![image-20221003134915839](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003134915839.png)

+ Make the weights on the path negative

![image-20221003135049892](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003135049892.png)

![image-20221003135102011](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003135102011.png)

**runtime**

![image-20221003135134292](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003135134292.png)



#### 1.4 Reduction

+ “归约”（Reduction）在计算复杂度理论中是一个非常重要的概念。

+ 古老笑话

  > 一个物理学家和一个数学家正坐在教师休息室里。突然间，休息室里的咖啡机着火了。物理学家就拿了一个垃圾桶，把里面的垃圾清空，跑到水池前，给垃圾桶灌满水，随后扑灭了火。由于这个咖啡机着过一次火了，大家都同意把垃圾桶装满水放在这个咖啡机旁边。
  > 第二天，同样的两个人又坐在同样的休息室里，咖啡机又一次着火了。这一次，数学家站了起来，把装满水的垃圾桶拿了起来，把里面的水倒掉，又放了一些垃圾在里面，交给了物理学家。这样就把问题归约到了一个之前已经解决过的问题上

+ simple idea : 我们现在遇到了个问题，可以把它转化到一个某个已解决的问题上，而不是一定要直接解决这个问题。

**Reduction on DAG Longest Paths**

+ Since DAG-SPT can be used to solve DAG-LPT,
   we say that “DAG-LPT **reduces** to DAG-SPT”. 

![image-20221003135623362](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003135623362.png)



**Definition**

Algorithms by Dasgupta, Papadimitriou, and Vaziani 
		defines a reduction informally as follows:

+ “If any subroutine for task Q can be used to solve P, we say P reduces to Q.”

+ **对于问题 A 和问题 B ，如果存在一个可以计算的函数 f ，使得对于任意问题 A 的实例 x ，都有 A(x)=B(f(x)) 那么我们就说问题 A 可以被归约到问题 B 上**



**Simple**

+ **The Independent Set Problem**

  ![image-20221003135959892](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003135959892.png)

+ **The 3SAT Problem**

  ![image-20221003140021329](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003140021329.png)

+ **The 3SAT Problem** reduces **The Independent Set Problem**

  ![image-20221003140050080](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003140050080.png)

![image-20221003140106000](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003140106000.png)

**Reduction**

![image-20221003140126733](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003140126733.png)

