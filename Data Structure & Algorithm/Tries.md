# 1. Tries

#### 1.1 Tries

> This structure is widely used in search engine prompt function

**Special Case 1: Character Keyed Map**

all keys are always ASCII characters

+ Can use just use an array
+ simple and fast

```java
public class DataIndexdCharMap<V> {
    private V[] items;
    public DataIndexdCharMap(int R){
        items = (V[]) new Object[R];
    }
    public void put(char c, V val){
        items[c] = val;
    }
    public V get(cahr c){
        return items[c];
    }
}
```

+ analysis

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002212542238.png" alt="image-20221002212542238" style="zoom:50%;" />

**Special Case 2: String Keyed Map**

all keys are always strings

+ can use a special data structure known as a Trie
+ Basic idea: Store each letter of the string as a node in a tree



Tries will have great performance on:

+ get
+ add
+ ==special string operations==



**Sets of Strings**

+ use BST or Hash Table representation 

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002212919717.png" alt="image-20221002212919717" style="zoom: 50%;" />

+ use Tries

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002213148041.png" alt="image-20221002213148041" style="zoom:50%;" />

**Tries Search**

+ Check if node exists
+ Check if it is a terminating node

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002213238765.png" alt="image-20221002213238765" style="zoom: 50%;" />

**Why calls Tries**

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002213541855.png" alt="image-20221002213541855" style="zoom:50%;" />





#### 1.2 **Trie Implementation and Performance**

**Basic Implementation**

+ base `DataIndexdCharMap`

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002213943932.png" alt="image-20221002213943932" style="zoom:50%;" />

+ the style of nodes

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002213849772.png" alt="image-20221002213849772" style="zoom:50%;" />



**Runtime analysis**

+ Regardless of the number of nodes

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002214201488.png" alt="image-20221002214201488" style="zoom: 33%;" />

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002214337525.png" alt="image-20221002214337525" style="zoom:33%;" />



**Problem**

+ Wasteful because most links are unused in real world usage



#### 1.3 **Alternate Child Tracking Strategies**

+ use Hash table or BST tracking strategies

**Hash Table Trie**

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002214653447.png" alt="image-20221002214653447" style="zoom:67%;" />

**BST Tries**

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002214721605.png" alt="image-20221002214721605" style="zoom:67%;" />

**Performance of the three Implements**

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002214909778.png" alt="image-20221002214909778" style="zoom:67%;" />

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221002214920274.png" alt="image-20221002214920274" style="zoom:33%;" />



#### 1.4 **Trie String Operations**

**prefix matching**

+ Finding all keys that match a given prefix : `keysWithPrefix("sa")`
+ Finding longest prefix of : `longestPrefixOf("sample")`



**Implement step by step**

+ `collect` Collecting Trie Keys
  + `colHelp()` : The end condition is to encounter a blue node,that is, the node where a word ends, and add the character to x
  + ![image-20221003104629740](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003104629740.png)
  + [demo (40 - 47)](https://docs.google.com/presentation/d/1XcpPT_KWUbr25d07iHGHpJN3S5DKA4cB_D4Zs5USqlM/edit#slide=id.g528d808e96_0_3539)
+ `keysWithPrefix`
  + 实现找到相同前缀的单词，就只需要找到前缀所在的节点，然后调用`colHelp`方法即可。
  + ![image-20221003104918132](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003104918132.png)



#### 1.4 Autocomplete

> 这里的 Trie 结构是 Map , 每一个 string 对应一个 value ,表示优先级(搜索次数), 当我们输入一部分字符时, 会自动调用查找相同前缀的方法, 返回 value 大小前十的 string . 

![image-20221003105710652](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003105710652.png)

+ As shown in the figure,  at the end of each string,  the value of blue node indicates the priority of the string

![image-20221003110010234](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003110010234.png)

+ for the real world , we need to collecting billions of results only to keep 10 !!!

![image-20221003110215504](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003110215504.png)

+ A more efficient method is to store an additional substring value for each node, representing the value of substring with the highest priority

![image-20221003110618949](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003110618949.png)

 #### 1.5 Summary

+ other type of string sets/ maps put there

  + Suffix Trees ([Link](https://en.wikipedia.org/wiki/Suffix_tree)).

  + DAWG ([Link](https://en.wikipedia.org/wiki/Deterministic_acyclic_finite_state_automaton)).

+ Tries

+ ![image-20221003110846884](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20221003110846884.png)

