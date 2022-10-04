# Disjoint Sets

> 讲解一个数据结构实现如何演变的过程,
>
> 讨论不相交集的四个迭代: *Quick Find → Quick Union → Weighted Quick Union (WQU) → WQU with Path Compression*. 
>
> 我们将看到设计决策如何极大的影响渐进运行时和代码复杂度

### 1 Disjoint Sets 的数据结构

不相交集的数据结构有两个operations

+ connect(x,y) connext x and y 
+ isConnected(x,y) : returns true if x and y are connected (不是直接链接也可以)

**On integers**

![image-20220920101847119](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220920101847119.png)

**interface**

![image-20220920102021138](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220920102021138.png)

### 2 implements

> 实现的本质上是 connect 和 isConnected 两个方法
>
> connect 将输入的点连接起来
>
> isConnected 判断两点是否相连
>
> 接下来将主要围绕这两个操作的改进和升级

如何判断两点相连?

![image-20210315211437902](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a75bcc1c011d4ea9b73238fcc65d79b5~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

#### 2.1 Quick Find

> 直观的方法 idea #1:   
>
> List of sets of integers, e.g. [{0}, {1}, {2}, {3}, {4}, {5}, {6}]
>
> 将在相连的元素放在同一组内
>
> + In Java: List<Set<Integer>>.
> + Very intuitive idea.
> + Requires iterating through all the sets to find anything. Complicated and slow!
>   + Worst case: If nothing is connected, then isConnected(5, 6) requires iterating through N-1 sets to find 5, then N sets to find 6. Overall runtime of Θ(N).
>
> 显然直观的方法过于原始，我们对于每一次的连接和判断是否连接都需要我们从头到尾将List中的所有集合找一遍, 判断元素是否属于该集合
>
> 我们需要选择一种更好的数据结构来track 数据, 快速查找出该元素所在集合(为了判断是否相连)

**Pick Data Structure**

针对快速的随机访问，我们选取数组作为基础，

idea #2: 整数列表, 其中第i 个条目给出第i 项的集合编号(a.k.a. "id")

+ connect(p , q): Change entries that equal id[p] to id[q]
+ isConnected(p , q ) : Compare the ids of pth  item and qth item

![image-20220920104926635](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220920104926635.png)

**implement**

```java
public class QuickFindDs implements DisjoiontSets{
    private int[] id;
   
    public boolean isConnected(int p, int q ){
        return id[p] == id[q];
    }
    
    public void connect(int p, int q){
        int pid = id[p];
        int qid = id[q];
        for(int i 0; i < id.length; i++){
            if(id[i] == pid){
                id[i] = qid;
            }
        }
    }
    
    public QuickFindDS(int N){
        id = new int[N];
        for(i = 0; i < N; i++){
            id[i] = i;
        }
    }
}
```

分析:

+ connect: ⊝(1) 基于数组的存储特性,可以单位时间内查找
+ isConnected: ⊝(n)  Relatively slow: N+2 to 2N+2 array accesses

**Performance Summary**

![image-20220920110715022](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220920110715022.png)

#### 2.2 Quick Union

>Qucik Find 的问题在于链接的操作非常慢，需要一个个的去修改一个连通分支的所有的元素的值，
>
>如果我们只需修改同一个连通分支的某一个数就可以改变整个连通分支所有节点的集合，链接两个集合，就可以解决这个问题
>
>接下来的Quick Union 就是基于这个思路的实现

 **change set representation**

idea #3: 给每一个item 分配一个parent(而不是一个id), 产生一个tree-like shape

+ connect(p, q) : find the root node of the two connected elements and then merge the root node.
+ isConnected(p , q) : find the root node of the two connected elements and then compare the root nodes  

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220920111959241.png" alt="image-20220920111959241" style="zoom: 80%;" />

![image-20220920112055674](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220920112055674.png)

这个思路其实是用树结构来表示同一个集合, 只需要改变根节点就可以改变整个集合的指向

**implement**

```java
public class QuickUnionDS implements DisjointSets{
    private int[] parent;
    public QuickUnionDS(int N){
        parent = new int[N];
        for(int i = 0; i < N ;i++){
            parent[i] = -1;
        }
    }
    
    private int find(int p){
        int r = p;
        while (parent[r] >= 0){
            r = parent[r];
        }
        return r;
    }
    
    public boolean isConnected(int p, int q){
        return find(p) == find(q);
    }
    
    public void connect(int p, int q){
        int i = find(p);
        int j = find(q);
        parent[i] = j;
    }
}
```

分析:

对于两种方法,都需要先找到两个元素的根节点, 时间复杂度取决于生成树的层数

事实上, 当形成的树是不平衡的(比如深的树挂在了浅的树上,如果经常发生的话),查找根节点的速度就会变得非常慢. 此时method 的速度降低, 甚至比之前的method 还要慢变得

**Performance Summary**

![image-20220920113315598](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220920113315598.png)

#### 2.3 Weighted Quick Union

>Quick Union 的痛点在于可能的生成树的不平衡, 效率的不稳定
>
>在最好的情况下, 合并和查找的速度只需要lgN, 但在极端的情况下, 会退化成N
>
>![image-20220920114038623](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220920114038623.png)
>
>![image-20220920114055600](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220920114055600.png)
>
>所以我们接下来就是要避免极端情况: **将小的树挂在大的树上**



**implement**

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/146e66b875a6421ca7d3ea0c05bf7c14~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp" alt="image-20210316105022911" style="zoom: 50%;" />

**example**

![image-20220920114354034](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220920114354034.png)

**Implement**

```java
public class WeightedQuickUnionUF {
    private int[] parent;   // parent[i] = parent of i
    private int[] size;     // size[i] = number of elements in subtree rooted at i
    private int count;      // number of components

    public WeightedQuickUnionUF(int n) {
        count = n;
        parent = new int[n];
        size = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            size[i] = 1;
        }
    }

    public int find(int p) {
        validate(p);
        while (p != parent[p])
            p = parent[p];
        return p;
    }

    public boolean connected(int p, int q) {
        return find(p) == find(q);
    }
    
    public void union(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        if (rootP == rootQ) return;

        // make smaller root point to larger one
        if (size[rootP] < size[rootQ]) {
            parent[rootP] = rootQ;
            size[rootQ] += size[rootP];
        }
        else {
            parent[rootQ] = rootP;
            size[rootP] += size[rootQ];
        }
        count--;
    }

}

```



**Performance Summary**

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220920114513698.png" alt="image-20220920114513698" style="zoom: 67%;" />

 weight 的时间复杂度为**O(lgN)**的原因

+ 由于加权增加的结构, 我们只有在N = $2^k$ 复制原本的$2^{k-1}$的worst case添加在root下,才会增加一个高度

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220920115335897.png" alt="image-20220920115335897" style="zoom: 50%;" />

++++++++++++++++++++++++++++++++++++++

==此时我们已经达到了较为完善的程度==

+++++++++++++++++++++++++

![image-20210316115442299](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/696a8d8b928c478a8dbfe49a1d6422a7~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

#### 2.4 Path Compression

> 我们发现,每一次寻找根节点时, 沿途的节点的根节点都找到了; 那么如果我们将所有沿途的检点都直接指向根节点,这样,在下次寻找的时候,路程就会短很多

![image-20220920121518328](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220920121518328.png)



这样优化后，均摊下来，每一次connect和isConnected操作只需要查找lg*N次，这个数字几乎相当于常数。也就是说，执行M次connect和isConnected操作，只需要查找大概M次就行了。时间复杂度为O(N+Mlg *N)，几乎相当于O(N+M)

**Performance Summary**

![image-20220920121717110](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220920121717110.png)

### 3 Summary

![image-20220920121740206](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220920121740206.png)



