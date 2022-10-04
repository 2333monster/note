# 1. Hash Table

**The ubiquity of hash tables **

Hash tables are the most popular implementation for sets and maps

+ Don't require items to be comparable
+ implementations often relatively simple



**Hash tables in java**

*hash table:*

+ Data is converted into hash code
+ The hash code is then reduced to a valid index
+ Data is then stored in a bucket corresponding to that index
+ Resize when load factor N/M exceeds some constant
+ If items are spread out nicely, you get $\Theta$ (1) average runtime

![image-20220925121419191](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925121419191.png)





**Two Fundamental Challenges**

+ how do we resolve hash code collisions
  + ***collision handling***
+ how do we compute a hash code for arbitrary objects?
  + ***computing a hash code***





**Handling Collision**

> **Because java has a maximum integer**, we won't get the numbers we expect
>
> + With base 126, we will run into overflow even for short string
> + overflow can result in collisions, causing incorrect answers.

+ **solutions**: store a "bucket" of these N items at position H

  + ![image-20220925122718641](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925122718641.png)

+ **performance**

  > ![image-20220925122924027](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925122924027.png)

  + the good news : we use less memory and can now handle arbitrary data

  + the bad news : Worst case runtime is now $\Theta$ (Q), where Q is the length the longest list

  + **Q = N / M ,when M is 5, the Q is N / 5, thus runtime is $\Theta$ (N)**

+ **improving the hash table**

  + *strategy*: An increasing number of buckets M
    + the process of increasing M is "resizing"
    + N/M called the "load factor" , represents how full the hash table is
  + *resizing progress*
    + ![(image-20220925123724638)](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220925123724638.png)
    + when load factor is larger than 1.5,  resizing

+ improving performance

  + resizing takes $\Theta$ (N) time. Have to redistribute all items.
  + Most add operations will be $\Theta$ (1), some will be $\Theta$ (N) time(to resize)
    + similar to `ALists`, as long as we resize by a multiplicative factor, the average runtime will sill be $\Theta$ (1)
    +  [Amortization(摊销)]()

   

**Good HashCodes**

+ 3 case bad hash code

  + #1 return 0 is bad hashcode function
  + #2 just returning the first character of a word
  + #3 Adding chars together is bad . "ab" collides with "ba".

+ java hashcode

  + represent base 31 number

  + Stores (caches ) calculated hash code so future hashcode calls are faster

    ```java
    @Override
    public int hashCode(){
        int h = cachedHashValue;
        if(h == 0; && this.length() > 0){
            for(int i = 0; i< this.length(); i++){
                h = 31 * h + this.charAt(i);
            }
            cachedHashValue = h;
        }
        return h;
    }
    ```

+ base is a small prime

  + why prime?
    + never even: Avoids the overflow issue on previous slide.
    + Lower chance of resulting hashcode having a bad relationship with the number of buckets: See [study guide problems](https://sp21.datastructur.es/materials/lectures/lec20/lec20) and [hw3](https://sp21.datastructur.es/materials/hw/hw3/hw3.pdf).
  + why small
    + lower cost to compute