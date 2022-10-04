# Intlist

​    简单构造

```java
public  class Intlist{
    public int first;
    public Intlist rest;
    
    public Intlist(int f, Intlist r){
        first = f;
        rest = r;
    }
}
```

​	使用起来较为麻烦

```java
Intlist L = new Intlist(5,null);
L.rest = new Intlist(10,null);
L.rest.rest = new Intlist(15,null);
// 或者
L = new Intlist(10,L);
L = new Intlist(5,L);
```

 	list method

```java
public int size();
public int iterativeSize();

/**
*Destructive 直接修改参数的值
*Non-Destructive 不该变原参数
*/
public static Intlist dcatenate(IntList A, IntList B)
```

# SLList

​	由于intlist 的难以使用, 需要使用者了解他的代码逻辑,所以设计新的类

​	首先创建新的类Intnode;

```java
public class Intnode{
    public int item;
    public Intnode next;
    
    public Intnode(int i , Intnode n){
        item = i;
        next = n;
    }
}
```

​	然后使用SLList封装这个类,从而达到优秀的简洁的使用方法

```java
public class SLList{
    public Intnode first;
    
    publice SLList(int x){
        first = new Intnode(x,null);
    }
}
```

​	简单比较

```java
Intlist L1= new Intlist(5,null);
SLList L2= new SLList(5)
```

​	显然使用者不必考虑S内部如何存储数据,只需知道如何调用她即可

SLList method

```java
public void addFirst(int x){
    first = new Intnode(x,first);
}

public int getFirst(){
    return first.item;
}
```

比较两者的内部实现

![img](https://images2018.cnblogs.com/blog/1287324/201807/1287324-20180722112256421-1313864296.png)

 错误调用, 调用first 导致无限循环

![alt](https://images2018.cnblogs.com/blog/1287324/201807/1287324-20180722112349297-1543040854.png)

此时我们就需要将first 设为私有变量

即为用户无法调用Intnode, 和使用`dot notation`直接调用`first`,

只能使用SLList 给定的函数

```java
public static class Intnode{
    ...
}
```

再次优化SLList, 加入`size`方法

```java
private static int size(Intnode p){
    if (p.next != null){
        return 1;
    }
    
    return 1 + size(p.next);
}

public int size(){
    return size(first);
}
```

由于`size`使用递归或者迭代的思想, 时间复杂度为O(n); 

为了更高效,这里用到了`储存`(caching)的思想

我们`添加一个size变量`,实时跟踪她的数值

```java
public class SLList{
    .../*Intnode declaration omitted. */
    private Intnode first;
    private int size;
    
    public SLList(int x){
        first = new Intnode(x,null);
        size = 0;
    }
    
    public void addFirst(int x){
        first = new Intnode(x , first);
        size += 1;
    }
    
    public int size(){
        return size;
    }
    ...
}
```

由此无论链表多长,我们都能在固定的时间得到`size	`

使用此法表示空列表

```java
first = null;
size = 0;
```

此时`addLast` 变得臃肿了起来

```java
public void addLast(int x) {
    size += 1;

    if (first == null) {
    first = new IntNode(x, null);
        return;
    }

    IntNode p = first;
    while (p.next != null) {
        p = p.next;
    }

    p.next = new IntNode(x, null);
}

```

​	所以在最开始的结构设计上我们解决这个问题, 不只是因为代码的简洁美观, 还有可以不必在每个特殊情况出现时,再次添加分支.一劳永逸的解决问题,也许?

​	解决方法: 添加`sentinel` 节点, 结构如下

![img](https://images2018.cnblogs.com/blog/1287324/201807/1287324-20180722112406049-1995710071.png)

当链表添加数据后的样式

![img](https://images2018.cnblogs.com/blog/1287324/201807/1287324-20180722112414498-1253563069.png)

此时的`addlast`也变的简介起来

```java
public void addLast(int x){
    size += 1;
    Intnode p = sentinel;
    while(p.next != null){
        p = p. next;
    }
     
    p.next = new Intnode(x,null);
}
```

此时,`add last` 也面临着时间复杂度的问题,同样用`储存` 的思想

添加`last` 的节点,时刻记录最后一个节点

```java
public class SLList{
    private Intnode sentinel;
    private Intnode last;
    private int size;
    
    public void addlast(int x){
        last.next = new INtnode(x,null);
        last = last.next;
        size += 1;
    }
}
```

此时内部结构

![img](https://images2018.cnblogs.com/blog/1287324/201807/1287324-20180722112547826-157869602.png)

尽管如此,SLList 还是有局限性,我们如何获取倒数第二个节点,显然我们不可能从sentinel 开始先后调用, 此时的模型结构下很难实现这种调用, 我们需要一种新的数据结构; `DLList`.

# 3. DLList

更加彻底的改造, 我们在每一个Intnode 结构中添加一个`prev` 相对于`next` 

```java
public class Intnode{
    public Intnode prev, next;
    public int item;
}
```

​	之所以称为`DLList`, 其实是 doubly linked list, 也就是`双向链表` 

结构如下 ![img](https://images2018.cnblogs.com/blog/1287324/201807/1287324-20180722112536629-1060703348.png)

​	目前, DLList 已经可以在头部和尾部快速的添加, 获取和删除操作,美中不足的时就是last的指针,有时指向sentinel, 有时指向普通数据节点, 可能在未来的操作中造成混乱;

​	改造

 + 在尾部添加一个sentinel 的空节点

   ![img](https://images2018.cnblogs.com/blog/1287324/201807/1287324-20180722112529868-343413046.png)

 + 使用`circle` 的思想 , 让首尾指针指向同一个sentinel空节点

   ![img](https://images2018.cnblogs.com/blog/1287324/201807/1287324-20180722112523965-894859567.png)

   **implement**

   ++++++========+++++++

# 4. AList

> ​	对于链表来说,进行随机访问非常慢,所以我们用数组(ArraryList) 来进行 random access

> ​	基于内置array 实现, 而不是link;

>  array 的优点, 由于array是连续存储的形式,所以不论调用任何一个内部元素,调用时间都为O(1),

`Invariants` 是十分重要的理解Alist设计理念

> Alist invariants:
>
> + 下一个插入项目的位置总是`size`
> + `size` 是Alist 中的项目总数
> + 列表中的最后一个元素的位置为`size - 1`

​	此时在让我们观察一下不同的操作带来的影响(对于这些不变量来说)

![](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220913170803365.png)

​	removelast 没有删除原本的数值,只是单纯的调整了last指针的位置, 我们发现这并不影响不变量性质 , 显然这种操作后Alist 仍然能保持她的性质.

**resizing**

> 由于arrary数组在最开始需要分配存储空间,使用完后需要扩容.
>
> 然而一个个地增加数组的容量效率极低, 成倍扩容, 摊薄成本.

```java
public void addLast(int x){
    if(size  == items.length){
        resize(size * RFACTOR);
    }
    items[size] = x;
    size += 1;
}
```

> 除了在数组容量不足时扩容外, 我们还需在使用率低下时缩小数组的容量
>
> `usage ratio`   = R (存储使用率)
>
> 当 R < 0.25.时减少数组的容量到1/2.

​	由于resize时仅仅修改了size ,原本的数据元素不会再被使用了,但是由于她的引用一直保存数组中,java只会销毁失去引用的对象, 所以为了节约空间, 我们需要主动将此空间设为null

```java
public T deleteBack(){
    T returnItem = getBack();
    items[size - 1] = null;
    size -= 1;
    return returnItem;
}
```

**implement**

```java
public class AList<Item> {
    private Item[]items;
    private int size;
    public AList(){
         items=(Item[]) new Object[100];
         size=0;
    }
    public void resize(int capacity){
            Item[]a=(Item[]) new Object[capacity];
            System.arraycopy(items,0,a,0,size);
            items=a;
    }
    public Item removeLast(){
        return items[--size];
    }
    public void addLast(Item i){
        if (size==items.length){
            resize(2*size);
        }
        items[size]=i;
        size++;
    }
    public Item getLast(){
        return items[size-1];

    }
   public Item get(int i){
        return items[i];
    }
    public int size(){
        return size;
    }
}
```



