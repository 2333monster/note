# 1 inmplements and interfaces

### 1.1Hypernyms, Hyponyms, and Interface Inheritance

观察`SLList` 和 `AList` 的文档可以发现,他们非常的相似, 实际上他们支持的所有方法都一样

![image-20220914104258016](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220914104258016.png)

现在我们设计一个类`WordUtils` 它包含了一些处理字符串的方法, 其中有对`SLList`进行操作的

```java
public static String longest(SLList<String> list) {
    int maxDex = 0;
    for (int i = 0; i < list.size(); i += 1) {
        String longestString = list.get(maxDex);
        String thisString = list.get(i);
        if (thisString.length() > longestString.length()) {
            maxDex = i;
        }
    }
    return list.get(maxDex);
}
```

为了使此方法对`AList` 成立,我们只需修改一下函数签名

```java
public static String longest(AList<String> list)
```

显然这将导致大量的冗余，每次修改这个函数，必修更改两个版本。

`interface` 就是解决这种问题的一个抽象,我们可以给定几个类之间的关系

![image-20220914105256696](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220914105256696.png)

其中`list61B` 就是`interface(接口)` ,他是一种抽象,规定了一个链表必须具备那些功能,但是内有具体实现. 

> `interface summary`
>
> + 不能被实例化
> + 可以提供`abstract` or `concrete` 方法
>   + use no keyword for abstract methods
>   + use default keyword for concrete methods.
> + 可以提供 public static final variables (no instance variables)

List61B implement

```java
public interface List61B<Item> {
    public void addFirst(Item x);
    public void add Last(Item y);
    public Item getFirst();
    public Item getLast();
    public Item removeLast();
    public Item get(int i);
    public void insert(Item x, int position);
    public int size();
}
```

同时修改我们的子类,实现接口

```java
public class AList<Item> implements List61B<Item>{...}
```

### 1.2 Overriding

**Overriding(重写) VS Overloading(重载)**

> 当同一个类中的两个或多个方法具有相同的名称但参数不同时,称为Overload.
>
> 当superclass 和subclass 的方法签名(名称和参数)相同时,称为Overriding.

example

![image-20220914111317615](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220914111317615.png)

```java
public class AList<Item> implements List61B<Item>{
    ...
    @Override
   	public void addLast(Item x){
        ...
    }
}
```

事实上无论你写不写@Override ,子类的method 都会在调用时覆盖超类的默认method

标价@Override 的原因

+ Main reason: 防止拼写错误, 当你标记了@Override , 但方法名写错时,编译器会报错提醒
+ 另外就是提醒程序员,这个方法有一个超类方法

**implementation inheritance (default method)**

`inheritance` 

+ 子类继承方法签名和实现,使用`default` keyword 来标记需要子类继承实现的方法

![image-20210311230645290](https://cdn.jsdelivr.net/gh/BoL0150/imgbed@main/image-20210311230645290.png)

有些问题的是,由于子类数据结构的不同,默认的方法实现可能无法成功,此时 使用`@override` 

<img src="C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220914113153110.png" alt="image-20220914113153110" style="zoom:80%;" />

### 1.3 Dynamic Method Selection

**Static Type vs. Dynamic Type**

java 中的变量有两种格式 `"compile-time  type"` , aka. `"static type"`

同时在运行实现时,也有两种格式`"run-time type" `, aka, `"dynamic type"`

![image-20220914113910310](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220914113910310.png)

**Signature Selection,Dynamic Method Selection**

![image-20220914114145104](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220914114145104.png)

当我们调用被覆盖的函数时

使用override的 函数, 而不是默认函数, 这就是`"dynamic method selection"`

在上图的情况,我们使用`static type `和`dynamic type` 不同的示例调用函数

规则如下:

+ At compile time: 如果这个方法只有动态类型有, 而静态类型没有, 则编译不通过.
  只有两个类型都有相同的方法(签名相同, 参数不同也不行,这只是overload),才通过编译
+ At run time: 从最准确的类型(即动态类型) 开始寻找该方法, 如果动态类型重写了该方法, 就调用动态类型的版本, 如果没有重写的版本,就调用静态类型的版本.

# 2 Extends

使用`inheritance` 来定义接口与类之间的上下级关系;

使用`extend` 可以定义类与类之间的上下级关系

example1:

```java
public class RotatingSLList<Item> extends SLList<Item>{
    public void rotateRight() {
    Item x = removeLast();
    addFirst(x);
	}
}
```

定义了一个`RotatingSLList` 类, 它具有`SLList` 的所有方法,还具有一个`rotateright`

此时类之间的关系

![image-20220914123037360](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220914123037360.png)

example2:

```java
public class VengefulSLList<Item> extends SLList<Item> {
    SLList<Item> deletedItems;

    public VengefulSLList() {
        deletedItems = new SLList<Item>();
    }

    @Override
    public Item removeLast() {
        Item x = super.removeLast();
        deletedItems.addLast(x);
        return x;
    }

    /** Prints deleted items. */
    public void printLostItems() {
        deletedItems.print();
    }
}
```

`VengefulSLList` 记录被删除的元素,

在重写的`removelast` 中,为了调用superclass 的方法,使用了`super` 的关键词

事实上,不使用`super`会有一系列的麻烦出现, 直接复制粘贴superclass 的方法结构,由于其中调用的method , variations 可能为私有的, 即时为subclass 也无法调用. 所以使用super 可以避免这种情况.

**Implements vs. Extends**

+ 使用 implements 当 the hyperym 是接口, the hyponym 是类的时候  (e.g. hypernym List, hyponym AList).
+ 其他情况都使用 Extends

# 3 Encapsulation

when building large programs, our enemy is complexity

两种管理复杂度的tools:

+ Hierarchical abstraction(分层抽象)
  + 创建抽象层，具有清晰的抽象障碍
+ “Design for change” (D. Parnas)
  + 将项目围绕对象组织
  + 让对象决定怎么解决时间
  + 对其它对象不需要的信息进行隐藏

**Abstraction Barries**

>  java 的语法天然支持这种思想

+ **Module** : 一系列的同时作用为一个整体的去处理一些任务的方法
  + Module(模组) 被称为 ***Encapsulated*** 如果它的实现被完全隐藏, 只能通过Interface的来使用

`Dog interface`

![image-20220915103721667](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220915103721667.png)

​	`Dog implement`

1. 

![image-20220915103811864](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220915103811864.png)

2. ![image-20220915103842358](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220915103842358.png)

这就是抽象障碍的一种示例

**Casting**(强制类型转换)

1. 可以将父类对象赋值给子类对象

```java
SLList<Integer> sl = new VengefulSLList<Integer>();
```

+ compile-time type is right hand side(RHS) expression `VengefulSLList`;
+ A VengefulSLList is-an SLList, so assignment is allowed.

```java
VengefulSLList<Integer> vsl = new SLList<Integer>();
```

+ Compile-time type of RHS expression is SLList.
+ An SLList is not necessarily a VengefulSLList, so compilation error results.

2. problem

   > 将父类对象赋值为子类对象时编译无法通过, 但是我们可以对父类进行***Casting*** 
   > 此时, 父类对象的编译类型就是子类对象的类型, **注意!!!** 父类的run-time type 并没有改变.
   >
   > 1. 如果该对象的实际类型就是父类(`SLList s1 = new SLList();`)  ,那么运行时就会出错**classCastExpection**.
   >
   > 2. 如果该对象的实际类型时子类的类型(`VegenfulSLList s1 = new SLList();`)  此时 (VengefulSLList)s1 就可以通过编译.

3. 强制类型转换常用于使用多态时, 要调用对象特有的方法,此时通过父类对象无法调用,我们就可以用强制类型转换

# 4. **Subtype Polymorphism**

> 亚态多样性
>
> 作用与一个函数,应用于多个不同的对象

1. 写一个`通用的求最大值的函数` `max()` ,用于求不同类型的对象之间的最大值

   > 由于java 的局限性, 不能进行字符串的比较
   >
   > 并且不同的对象的最大值比较,需要用到不同的参数,
   >
   > 比如Dog , 我们比较它们的体重; Cat, 我们比较它们的长度

2. 我们首先需要导入max() 的对象都是同一类对象(有同一个接口),根据如上的问题, 我们还需要每个类有他们自己独特的比较函数

`OurCompareble`(interface)

```java
public interface OurComparable {
    // return -1 if this < o.
    // return 0 if this equals o.
    // return 1 if this > o.
    public int compareTo(Object o);
}
```

`max()`

```java
public static OurComparable max(OurComparable[] items) {
    int maxDex = 0;
    for (int i = 0; i < items.length; i += 1) {
        int cmp = items[i].compareTo(items[maxDex]);
        if (cmp > 0) {
            maxDex = i;
        }
    }
    return items[maxDex];
}
```

`Dog`

```java
public class Dog implements OurComparable {
    private String name;
    private int size;

    public Dog(String n, int s) {
        name = n;
        size = s;
    }

    public void bark() {
        System.out.println(name + " says: bark");
    }

    public int compareTo(Object o) {
        Dog uddaDog = (Dog) o;
        return this.size - uddaDog.size;
    }
}
```

此时max() 方法对传入参数的类的要求:

​	拥有一个`compareTo` 的方法, 它通过这个方法来找到这个类的最大值

3. Comparable

事实上, java内置了一个***Comparable***的接口,他和我们定义的接口十分相似

此时`Dog`的实现

```java
public class Dog implements Comparable<Dog> {
    ...
    public int compareTo(Dog uddaDog) {
        return this.size - uddaDog.size;
    }
}
```

**Subtype Polymorphism vs. Explicit Higher Order Functions**

![image-20220915123259229](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220915123259229.png)

`Dog class interface is Compareble<Dog>`

```java
public interface Comparator<T>{
    int compare(T o1, T o2);
}
```

![image-20220915123558747](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220915123558747.png)

![image-20220915123612921](C:\Users\xiguaa\AppData\Roaming\Typora\typora-user-images\image-20220915123612921.png)

Summary:

接口为我们提供了进行***callback*** 的能力：

+ 有时一个函数需要另一个可能未编写的函数的帮助
  + 例如：max() 需要compareTo
  + 这个被需要的函数(提供帮助的函数) 有时被称为***callback***
+ 有些语言使用显示传递来处理这个问题

```python
def print_larger(x,y,compare,stringify):
    if compare(x,y):
        return stringify(x)
    return stringify(y)
```

我们可以将函数作为参数传递

+ 在java 中, 我们不能将函数作为参数传递. 我们通过在接口中包装所需的函数来实现这一点

  + 例如: `Arrays.sort` 这个函数需要知道怎么去排序, 排序的规则是什么

    + 在python 中,我们只能使用函数类型作为参数, 当用户使用这个函数时,他们可以传递一个函数(特定的排序规则)来指定如何排序

    + 在 java 中，我们可以传递一个对象并在函数中调用这个对象的方法来指定规则。但是我们不知道要传入什么对象，所以我们需要一个通用的接口，来满足所有对象的要求。**传入的对象类必须实现接口并覆盖函数。**通过这种方式，我们可以使用 Array.sort 中的函数

      ![image-20210312183917712](https://cdn.jsdelivr.net/gh/BoL0150/imgbed@main/image-20210312183917712.png)

      使用接口Comparator 作为参数,传入一个实现了比较器的对象, 并在函数中调用对象的比较方法
