# 1.Basic Sort

#### 1.1 Sorting Problem

+ sort algorithm is the base of many algorithms

**Useful task . Example:**

+ Equivalent items are adjacent, allowing rapid duplicate finding.
+ Items are in increasing order, allowing binary search.
+ Can be converted into various balanced data structures (e. g. BSTs, Kdtrees).



#### 1.2 **Selection Sort and Heapsort**

==**Selection Sort**==

+ Find smallest item
+ Swap this item to the front and "fix" it
+ Repeat for unfixed items until all items are fixed

![](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003204316242.png)

![image-20221003204343504](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003204343504.png)

​							.........

![image-20221003204407233](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003204407233.png)

+ Properties:
  + $\Theta$($N^2$) time if we used array



**Naïve Heapsort: Leveraging a Max-Oriented Heap(利用面向最大值的Heap)**

> + **#idea**: Instead of rescanning entire array looking for minimum, maintain a heap so that getting the minimum is fast!
> + **Use max-oriented heap**



==**Naïve heapsorting N items:**==

**General strategy:**

+  **Insert** all items into a **max heap**, and **discard**(丢弃) input array. **Create** output array.

+ Repeat N items:

  + Delete largest item from the max heap
  + Put largest item at the end of the unused part of the output array.

+ Progress

  + **Phase 1 : Heap Creation**

  ![image-20221003205522749](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003205522749.png)

  + **Phase 2: Heap Deletion**

  ![image-20221003205610187](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003205610187.png)

  ![image-20221003205650674](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003205650674.png)

  ![image-20221003205701769](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003205701769.png)

  ![image-20221003205713219](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003205713219.png)

  ![image-20221003205811556](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003205811556.png)

  ![image-20221003205822129](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003205822129.png)

  ​			.........[Demo](https://docs.google.com/presentation/d/1HVteFyWOxBW4mmUgkDnpUoTkWexiHt7Ei30Qolbc_I4/edit#slide=id.g463de7561_042)...........

  ![image-20221003205855097](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003205855097.png)



**Heapsort Runtime Analysis**

+ **O(N log N)**

![image-20221003210032690](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003210032690.png)



==**In-place Heapsort**==(In-place 大概是只使用源数组进行操作)

+ **Alternate approach**, treat input array as a heap!

+ Use a progress known as "bottom-up heapification"(自上而下的堆积) to **convert the array to the heap**
  + To bottom-up heapify, just **sink** nodes in reverse level order



+ **progress**

  + **Phase 1: Heapification**

  ![image-20221003211106563](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003211106563.png)

  ![image-20221003211139062](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003211139062.png)

  ![image-20221003211151610](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003211151610.png)

  ![image-20221003211213358](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003211213358.png)

  sink(17) is useful ,

  ![image-20221003211230143](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003211230143.png)

  ​												.........[Demo](https://docs.google.com/presentation/d/1SzcQC48OB9agStD0dFRgccU-tyjD6m3esrSC-GLxmNc/edit#slide=id.g12a2a1b52f_0_583).........

  ![image-20221003211916482](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003211916482.png)

  到2的时候，发现2比子节点26和41都要小，所以将2和41交换。

![image-20221003211333727](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003211333727.png)

​		最后，32和41交换。从尾部开始sink的意义是：

		+ 保证以该位置作为root的树是一个heap，
		+ 也就是该位置之后全部满足heap的条件。这是我们就有了一个max-oriented heap。

![image-20221003212118486](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003212118486.png)

​		Then,  finish the heap create

- + **Phase 2 : Heap Sort**:
    + 对这个heap进行这样的操作：删除最大的。（把最大的数和heap末尾交换，并且交换末尾的数直到满足heap的定义）。直到heap为空。

  ![image-20221003212752624](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003212752624.png)



**Runtime** 

+ time complexity: **O(N log N)**

![image-20221003213013216](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003213013216.png)

+ memory complexity : **Θ(1)**



==**Mergesort**==

**General strategy:**

+ Split items into 2 roughly even pieces
+ Mergesort each half(recursive)
+ Merge the two sorted halves to form the final result
+ **[Demo](https://docs.google.com/presentation/d/1h-gS13kKWSKd_5gt2FPXLYigFY4jf5rBkNFl3qZzRRw/edit#slide=id.g12a3009c32_0_17)**
  + ![image-20221003213502041](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003213502041.png)
  + ![image-20221003213512492](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003213512492.png)
  + ![image-20221003213523107](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003213523107.png)



**Time Complexity**

+ **$\Theta$(N log N)**

![image-20221003213736567](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003213736567.png)





==**Insertion Sort**==

**General strategy:(Naïve)**

+ Starting with an empty output sequence
+ Add each item from input, inserting into output at right point

**Problem:**

+ worst cost to insert a single item is N



**More efficient method: ==(In-place Insertion Sort)==**

General strategy:

+ Repeat for i = 0 to N - 1:
  + Designate item i as the traveling item.
  + Swap item backwards until traveler is in the right place among all previously examined items.

**Example:**

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003215352192.png" alt="image-20221003215352192" style="zoom:67%;" />

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003215405944.png" alt="image-20221003215405944" style="zoom:67%;" />

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003215451007.png" alt="image-20221003215451007"  />

**Runtime**

![image-20221003215527157](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003215527157.png)

**The advantages of Insertion Sort**

+ 先搞1000000个排序好的整数, 然后改变一个,重新排序,这时选用什么排序算法效率高呢?

![image-20221003215740693](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003215740693.png)

+ 这时，用insertion sort排序比较好。因为只需遍历一遍，再加上常数次的swap，就可以实现排序。Selection sort不用说，依然需要O(N2)；Heap Sort还是需要重新建立heap，依然需要O(N log N)；Insertion Sort却只需要O(N)了。

  经研究表明，Insertion sort对于小数组和almost sorted的数组，性能最佳

  ![image-20221003215841486](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003215841486.png)



**Sorts So Far**

![image-20221003215913306](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003215913306.png)









# 2 Quicksort

#### 2.1 Backstory, Partitioning

==**Backstory**==

> 1960年代，[英国国家物理实验室](http://zh.wikipedia.org/wiki/英国国家物理实验室) ([National Physical Laboratory](http://en.wikipedia.org/wiki/National_Physical_Laboratory_(United_Kingdom))) 开始了一项新的计划：将俄文自动翻译成英文。正好霍尔有这个经历，他与俄国的机器翻译专家相识，还在“机器翻译”(Machine Translation) 上发表过论文。于是他在那里得到了一份工作。
>
> 在那个年代，俄文到英文的词汇列表是以字母顺序存储在一条长长的磁带上的。每次找一个元素只能从开头依次访问到它的位置，所以HashMap和二分法效率很低，并且当时的array 检索也不是constant time。因此，当有一段俄文句子需要翻译时，第一步是把这个句子的词按照同样的顺序排列。这样机器就可以在磁带上只走一遍就可以找到所有的翻译。霍尔意识到，他必须找出一种能在计算机上实现的排序的算法来。他想到的第一个算法是后人称作“[冒泡排序](http://zh.wikipedia.org/wiki/冒泡排序) ([bubble sort](http://en.wikipedia.org/wiki/Bubble_sort))”的算法。虽然他没有声明这个算法是他发明的，但他显然是独自得到这个算法的。他很快放弃了这个算法，因为它的速度比较慢。用[计算复杂度理论](http://zh.wikipedia.org/wiki/计算复杂度) ([Computational complexity theory](http://en.wikipedia.org/wiki/Computational_complexity_theory)) 来说，它平均需要 **O($n^2$)** 次运算。[快速排序](http://zh.wikipedia.org/wiki/快速排序) ([Quicksort](http://en.wikipedia.org/wiki/Quicksort)) 是霍尔想到的第二个算法。这个算法的计算复杂度是 **O($n log n$)** 次运算。当 *n* 特别大的时候，显然步骤要少很多。

==**Partitioning**==

To partition an array a[] on element x=a[i] is to rearrange a[] so that:

+ x(**pivot**) moves to position j
+ All entries to the left of x are <= x.
+ All entries to the right of x are >= x.

![image-20221003221033523](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003221033523.png)

+ valid partition(A B C)

![image-20221003221059704](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003221059704.png)

**Partition Sort, a. k. a. Quicksort**

+ Partition on leftmost item
+ Quicksort left half
+ Quicksort right half



**Runtime**

+ Best case : pivot always lands in the middle(pivot is the medium item)

  ![image-20221004113131205](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221004113131205.png)

  + Runtime : $\Theta(N logN)$

  ![image-20221004114928064](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221004114928064.png)

+ Worst case : 

  ![image-20221004115033871](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221004115033871.png)

  + Runtime : $\Theta(N ^ 2)$

+ Compare with Mergesort

![image-20221004115222835](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221004115222835.png)

+ However the Quicksort is the fastest sort algorithm

==**Proof that the runtime is basically  $\Theta(N logN)$**==

+ #1 : 10% case:
  ![image-20221004115653571](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221004115653571.png)
+ **#2: Quicksort is BST Sort**
  ![image-20221004115817035](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221004115817035.png)



**Quicksort Performance**

depend critically on:

+ Pivot selection.
+ Partition algorithm.
+ How we deal with avoiding the worst case (can be covered by the above choices).

#### 2.2 Avoiding the Worst Case

==**Four philosophies**==

1. **Randomness**: Pick a random pivot or shuffle before sorting.
2. **Smarter pivot selection**: Calculate or approximate the median. -> 每次选的都是medium, 那么每次就是最佳, O(N log N)
3. **Introspection**: Switch to a safer sort if recursion goes to deep. -> recursion 次数多了，就换种方法
4. **Preprocess the array**: 可以分析数组以查看快速排序是否会变慢。 但是，没有明显的方法可以做到这一点（不能只检查数组是否已排序，几乎已排序的数组几乎很慢）。

==**Quicksort vs. Mergesort(Philosophy #1 Randomness)**==

Let’s speed test Mergesort vs. Quicksort**(QuciksortL3S)** from last time, which had:

+ Pivot selection: Always use **leftmost**.
+ Partition algorithm: Make an array copy then do **three** scans for red, white, and blue items (white scan trivially finishes in one compare).
+ **Shuffle** before starting (to avoid worst case).

![image-20221004124002886](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221004124002886.png)



**Tony Hoare’s In-place Partitioning Scheme**

Hoare's strategy:

+ Left pointer loves small items.
+ Right pointer loves large items.
+ Big idea: Walk towards each other, swapping anything they don’t like.
  + End result is that things on left are “small” and things on the right are “large”.



Full details here: [Demo](https://docs.google.com/presentation/d/1DOnWS59PJOa-LaBfttPRseIpwLGefZkn450TMSSUiQY/pub?start=false&loop=false&delayms=3000)

<img src="https://img-blog.csdnimg.cn/20200429191322408.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZvdXJpZXJfdHJhbnNmb3JtZXI=,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述" style="zoom: 33%;" />

<img src="https://img-blog.csdnimg.cn/20200429191929892.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZvdXJpZXJfdHJhbnNmb3JtZXI=,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述" style="zoom:33%;" />

Now is better than Mergesort

![image-20221004124344626](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221004124344626.png)

==**Quick Select(Philosophy #2 : Smarter Pivot Selection)**==

+ **Partitioning** can be used to **find the exact median**
  + The resulting algorithm is the best known median identification algorithm(中值识别算法)

**Quick Select**

![image-20221004125059496](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221004125059496.png)

+ Worst case performance:

  ![image-20221004125242005](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221004125242005.png)

+ Expected performance

![image-20221004125317051](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221004125317051.png)

**Quicksort with Quick select**

![image-20221004125411599](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221004125411599.png)

+ Use Quick select to find Exact Median resulting algorithm is still slow
  + 做一堆分区来确定要分区的最佳项目有点奇怪。
