# 1 Multi-dimension Data

#### 1.1 **2D Range Finding and Nearest Neighbors**

**suppose** we want to perform operations on a set of Body objects in 2D space

+ **2D Range Finding** : How many objects are in the highlighted rectangle
+ **Nearest** : What is the closest objects to space horse?:carousel_horse:

![image-20221002191222270](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002191222270.png)

**suppose** the set is stored as a hash table , 

where the hash code of each Body is given below

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002192004057.png" alt="image-20221002192004057"  />

+ how would nearest(:carousel_horse:) be implemented?
+ whet would the be the runtime of nearest?



#### 1.2 Uniform Partitioning(统一分区)

+ **spatial hashing( 空间散列 )**

**#idea 1**: 将区域均匀划分为n个网格。只查找相关网格中的对象。然而这样做，当点是均匀随机分布在平面内，查找一个网格需要的复杂度仍然是O(N)。

![Uniform Partitioning](https://img-blog.csdnimg.cn/20200416151006988.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZvdXJpZXJfdHJhbnNmb3JtZXI=,size_16,color_FFFFFF,t_70)



#### 1.3 Quadtrees

**#idea 2**: 一个点相对于另一个点只有四种相对方向：左上，右上，左下，右下（边界可以自行选择归到哪类）。因此可以建一个四个分支的树形结构，就可以表示某两个节点的相对方向。

+ **[insertion Demo](https://docs.google.com/presentation/d/1vqAJkvUxSh-Eq4iIJZevjpY29nagNTjx-4N3HpDi0UQ/pub?start=false&loop=false&delayms=3000)**
+ ![image-20221002192908671](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002192908671.png)



**Quadtree  range search demo**

+ **[Range Search Demo](https://docs.google.com/presentation/d/1ZVvh_Q15Lh2D1_NnzZ4PR_aDsLBwvAU9JYQAwlSuXSM/edit?usp=sharing)**
+ ![image-20221002193809500](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002193809500.png)

**prune**

+ ![image-20221002193843044](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002193843044.png)

  

#### 1.4 Higher Dimensional Data

+ 2D -> Quadtree ; 3D -> Oct-tree(Octree)

  ![image-20221002194158994](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002194158994.png)

**Organize data on a number of dimension** 

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002194409297.png" alt="image-20221002194409297" style="zoom: 50%;" />

In these case, **KD tree** came into being (应运而生)

+ Fascinating data structure that handle arbitrary numbers of dimensions
  + k-d means "k dimensional"
+ the idea generalizes naturally ; so we use 2D data



#### 1.5 KD trees

**k-d trees example for 2-d:**

+ Basic idea, root node partitions entire space into left and right (by x).
  + 以x 轴划分成两个区域

+ All depth 1 nodes partition subspace into up and down (by y).
+ All depth 2 nodes partition subspace into left and right (by x).



**[K-d tree insertion demo](https://docs.google.com/presentation/d/1WW56RnFa3g6UJEquuIBymMcu9k2nqLrOE1ZlnTYFebg/edit?usp=sharing)**

+ Have to **break ties** somehow. We’ll say items that are equal in one dimension go off to the **right** (or up) child of each node.

+   <img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002195318819.png" alt="image-20221002195318819" style="zoom:50%;" />
+ Similar to a quadtree



**Nearest Neighbor**

Optimization(优选法) ：不检索 不可能出现比当前更好的答案 的 分支

+ example：
  + ![image-20221002195753720](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002195753720.png)
  + Answer is (1, 5).
  + [K-d tree nearest demo.](https://docs.google.com/presentation/d/1DNunK22t-4OU_9c-OBgKkMAdly9aZQkWuv_tBkDg1G4/edit?usp=sharing) 



==**Nearest Pseudocode**==

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002200039575.png" alt="image-20221002200039575" style="zoom: 67%;" />

==**Inefficient Nearest Pseudocode**==

+ 好实现，但是慢

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002200209276.png" alt="image-20221002200209276" style="zoom: 67%;" />

#### 1.6 Summary

![image-20221002200343304](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002200343304.png)

