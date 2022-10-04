# 1 **Abstract Data Types**(ADT)

> An **Abstract Data Type(ADT)** 是由它所支持的操作定义，而不是他的实现方式
>
> we know what they do , we don't know how they do

*example 1* ：Deque

![image-20220924173604794](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924173604794.png)

*example 2* ： Stack

operation：

+ push(int x ) : Puts x on top of the stack
+ int pop() : Removes and returns the top item from the stack

![image-20220924173908917](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924173908917.png)

*example 3* :   GRabbag

operation:

![image-20220924174010413](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924174010413.png)

*example 4*:   Collections(Map, list, Set)

+ Lists of things.
+ Sets of things.
+ Mappings between items, e.g. josh hug’s grade is 88.4, or Creature c’s north neighbor is a Plip.
  + Maps also known as associative arrays, associative lists (in Lisp), symbol tables, dictionaries (in Python).

![image-20220924174307349](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924174307349.png)

**Map Example(counting words)**:

![image-20220924174558193](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924174558193.png)

# 2.**Binary Search Trees**

#### 2.1**BST** 的思路：

+ Fundamental Problem: Slow search, even though it’s in order.
  ![image-20220924175100555](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924175100555.png)
+ add extra(random) links to make the search faster [skip list](http://en.wikipedia.org/wiki/Skip_list)
  ![image-20220924175628912](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924175628912.png)
+ Move pointer to middle.
  ![image-20220924175711189](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924175711189.png)
+ Move pointer to middle and flip left links. Halved search time
  ![image-20220924175732874](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924175732874.png)
+ Do better
  ![image-20220924175753015](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924175753015.png)

**BST Definitions**:

> rooted tree and rooted binary trees

> 在有根树中：
>
> + 除了根节点（root）， 每一个节点都只有一个父节点
> + 从上到下，从根节点到N
> + 没有子节点的节点称为leaf
>
> 在有根二叉树中:
>
> + 每个节点只有0, 1, 2 subtrees.

==**Binary Search Tree**==:

>  二叉查找树是有**BST property** 的有根二叉树

**BST Property**. for every node X in the tree:

+ every Key in the **left** subtree is **less** than X's key
+ every key in the **right** subtree is **greater** than X's key.

#### 2.2 BST operation : search

![image-20220924181301124](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924181301124.png)

> compare the key of the item to search the target

算法分析:

+ N is the number of nodes, search an items is $\Theta$(log N)
  ![image-20220924181804141](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924181804141.png)

#### 2.3 **BST Operations: Insert**

**progress**:

Search for key.

+ If found, do nothing.
+ If not found:
  + Create new node.
  + Set appropriate link.

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924181942850.png" alt="image-20220924181942850" style="zoom:150%;" />

==Arms Length recursion==

![image-20220924182107053](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924182107053.png)

#### 2.4 operation ： delete

**3 Cases**

+ deletion key has no children
  + just delete
+ deletion key has one children
  + Move deletion's parent's pointer to deletion's children.
+ deletion key has two children(**Hibbard deletion**)
  + ![image-20220924193718848](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924193718848.png)
  + (delete dog) you can choose "cat"  or "elf" as the new root node
  + because "cat" is the left-most while "elf" is the right-min, either of the two 
    can be the new root without breaking the BST property

**HIbbard deletion implement**

```java
// find root's min 
private Node min(Node x){
    if(x.left == null) return x;
    else return min(x.left);
}

// delete subtree's min,
private Node deleteMin(Node x){
    if(x.left == null) return x.right;
    x.left = deleteMin(x.left);
    x.size = size(x.left) + size(x.right) + 1;
    return x
}

private Node delete(Node x, Key key){
    if(x == null) return null;
    
    int cmp = key.compareTo(x.key);
    if		(cmp < 0) x.left = delete(x.left, key);
    else if	(cmp > 0) x.right= delete(x.right,key);
    else{
        if(x.right == null) return x.left;
        if(x.left == null) return x.right;
        Node t = x;
        x = min(t.right);
        x.right = deleteMin(t.right);
        x.left = t.left;
    }
    x.size = size(x.left) + size(x.right) + 1;
    return x;
}
```

+ use BST implement Set and Map
  + node of BST represents the element of set
  + node<key,value> represents Map<key,value>

# 3. Balanced BSTs (B-Tree)

+ BST height can be varies between "bushy(浓密的)" and "spindly(细长的)" trees.
  ![image-20220924200913801](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924200913801.png)

+ performance of operations on **spindly trees** can be just **as bad as a linked list.**
+ random insert items may let BST be constructed better , performance $\Theta$(lg N)



**B-trees / 2-3 trees / 2-3-4 trees**

+ invariant properties

  + all the leaf's depth is same
  + a non-leaf node with k items must have exactly k+1 children
    ![image-20220924201755082](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924201755082.png)

+ insert

  + **add** new items in existing nodes 

    ​			![image-20220924202121267](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924202121267.png)

    + add(17)
      ![image-20220924202146360](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924202146360.png)
    + add(18)
      ![image-20220924202205684](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924202205684.png)

  + if nodes are full , they **split**.

    + the limit of the node could contains is 3 

    + add(19)
      ![image-20220924202437272](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924202437272.png)

      the leaf node get too juicy

    + if any node has more than 3 items ,give an item to parent

      + pulling item out of full node splits it into left and right
      + parent node has three children

      ![image-20220924202809488](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924202809488.png)

      + add(20,21)
        ![image-20220924202914707](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924202914707.png)
      + split tree
        ![image-20220924202932299](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924202932299.png)

    + **split balance**

      + split the root , every node gets pushed down by exactly one level
      + if we split a leaf node or internal node , the height doesn't change.

  + **[insert interactive demo](https://tinyurl.com/balanceYD)**

+ **runtime analysis**

  + Height : between ~$log_{L+1}(N)$ and ~$log_{2}(N)$
    + Largest possible height is all non-leaf nodes have 1 item.
    + Smallest possible height is all nodes have L items.
    + Overall height is therefore Θ(log N). 
      ![image-20220924204044984](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924204044984.png)
  + runtime for contains
    + Worst case number of nodes to inspect: H + 1
    + Worst case number of items to inspect per node: L
    + Overall runtime: O(HL)
    + since H = $\Theta$(log N) , overall runtimes is O(L log N)
  + Runtime for add:
    + Worst case number of nodes to inspect: H + 1
    + Worst case number of items to inspect per node: L
    + Worst case number of split operations: H + 1 
    + Overall runtime: O(HL) = O(L)

+ **deletion**(not discusion)

  + [slides](https://docs.google.com/presentation/d/1NgaMi7IWs94sC_fhF7_UWx2O4LyPicvVJ9xkru9m2dU/edit#slide=id.g4fe50d0bd7_4_9)

# 4. Left Leaning Red-Black Trees(LLRB)

#### 4.1 Tree rotation

+ for N items , there are [Catalan(N)](https://en.wikipedia.org/wiki/Catalan_number) different BSTs.
+ in fact , it is possible to move to a different configuration using "rotation"
  + in general, can move any coonfiguration to any other in 2n-6 rotations
  + see [Rotation Distance, Triangulations, and Hyperbolic Geometry](https://www.cs.cmu.edu/~sleator/papers/rotation-distance.pdf) or [Amy Liu](https://medium.com/@liuamyj/its-triangles-all-the-way-down-part-1-17f932f4c438))**(see [Rotation Distance, Triangulations, and Hyperbolic Geometry](https://www.cs.cmu.edu/~sleator/papers/rotation-distance.pdf) or [Amy Liu](https://medium.com/@liuamyj/its-triangles-all-the-way-down-part-1-17f932f4c438))**

**tree rotation defination**

+ rotateLeft(G) : 
  + let  x be the right child of G, make G the **new left child** of x.
    ![image-20220924205338962](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924205338962.png)
  + preserves search trees property, no change to semantics(语义) of tree
  + ==Can think of as temporarily merging G and P, than sending G down and **left**==
  + ![image-20220924205550568](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924205550568.png)
  + 
+ rotateRight(P):
  + Let x be the left child of P. Make P the **new right child** of x.
  + Note : k was G's right child,  now it is P's left child
    ![image-20220924210221582](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924210221582.png)
+ *example*
  + give a sequece of rotation operations 
    ![image-20220924210450343](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924210450343.png)
  + ![image-20220924210501774](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924210501774.png)
+ [Rotate operations slides](https://docs.google.com/presentation/d/1pfkQENfIBwiThGGFVO5xvlVp7XAUONI2BwBqYxib0A4/edit?usp=sharing)

#### 4.2 Red-Black Trees

+ one-to-one correspondence between B-Tree and BST

**representing a 2-3 Tree as a BST**

+ A 2-3 tree with only 2-nodes is trivial
  + BST is exactly the same
+ dealing with 3-Nodes
  + ![image-20220924220400768](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924220400768.png)
  + A BST with left glue links that represents a 2-3 tree is often called a “**Left Leaning Red Black Binary Search Tree**” or **LLRB**.
    ![image-20220924220823551](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924220823551.png)
  + examples 1:
    ![image-20220924220705912](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924220705912.png)
  + examples 2:
    ![image-20220924220720398](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924220720398.png)

**LLRB Properties**

+ No node has two red links [otherwise it'd be analogous to a 4 node, which are disallowed in 2-3 trees].
+ Every path from root to a null reference has same number of **black links**[because 2-3 trees have the same number of links to every leaf]. LLRBs are therefore.

**Maintaining 1-1 Correspondence  Between LLRB and B-Tree Through Rotations** 

+ preservation of the correspondence will invole tree rotations.

**4 Cases**:

+ :one:**insert left==(insertion Color always is red)==**
  ![image-20220924222112072](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924222112072.png)
  
+ :two:**insert right** [cause a ***right leaning "3-nodes"*** , we have a **left leaning violation**]
  + rotateLeft(parent)
  
    ![image-20220924222230168](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924222230168.png)

> New rule : representation of temporary 4-nodes
>
> ![image-20220924222358987](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924222358987.png)

+ :three:double insertion on the left [there are ***two consecutive left links***, have an **Incorrect 4 node Violation**]

  ![image-20220924222626157](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924222626157.png)

+ :four:splitting temporary 4-nodes[there are any ***nodes with two red children***, we have a **Temporary 4 Node.**]

  + suppose we have the LLRB below which includes a temporary 4 node
  + what should we do next?
    + flip the colors of all edges touoching B
      ![image-20220924222939278](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220924222939278.png)

**LLRB runtime**

the runtime analysis for LLRB is simple if you trust the 2-3 tree runtime

+ LLRB has height O(log N)
+ Contains is trivially O(log N)
+ insert is O(log N)
  + O(log N) to add the new node
  + O(log N) rotation and color flip operations per insert

**LLRB implements**

```java
private Node put(Node h, Key key, Value val){
    if(h == null) {return new Node(key,val,RED);}
    
    int cmp = key.compareTo(h.key);
    if (cmp < 0)      { h.left  = put(h.left,  key, val); }
    else if (cmp > 0) { h.right = put(h.right, key, val); }
    else              { h.val   = val;                    }
 
	if (isRed(h.right) && !isRed(h.left))      { h = rotateLeft(h);  }
	if (isRed(h.left)  &&  isRed(h.left.left)) { h = rotateRight(h); }
	if (isRed(h.left)  &&  isRed(h.right))     { flipColors(h);      } 
 
	return h;

```

# 5 Search Tree Summary

+ Binary search trees are simple, but they are subject to imbalance.
+ 2-3 Trees (B trees) are balanced, but painful to implement and relatively slow
+ LLRBs insertion is simple to implement (but deletion is hard)
  + works by maintaining mathematical bijection with a 2-3 trees
+ java's TreeMap is a red-black tree(not left leaning)
  + Maintains correspondence with 2-3-4 tree(is not a 1-1 correspondence)
  + Allows glue links on either side (see [Red-Black Tree](http://en.wikipedia.org/wiki/Red%E2%80%93black_tree))
  + More complex implementation, but signigficantly(?) faster
