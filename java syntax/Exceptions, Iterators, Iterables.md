# 1. Throwing and catching

### 1.1 Throwing Exceptions

在运行的过程中，可能遇到某些错误需要中止，这时程序就会抛出一个异常。

常见情况——`IndexOutofBounds` 异常.

```java
public static void main (String[] args) {
    ArrayMap<String, Integer> am = new ArrayMap<String, Integer>();
    am.put("hello", 5);
    System.out.println(am.get("yolp"));
}
```

运行时发现,这个key值根本不存在,然后造成混乱

```makefile
$ java ExceptionDemo
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: -1
at ArrayMap.get(ArrayMap.java:38)
at ExceptionDemo.main(ExceptionDemo.java:6)
```

这是由java本身抛出的错误, 错误告诉我们`ArrayIndexOutOfBoundsException` ,但是并未明确那个数据或者步骤出了问题

使用`throw` 关键词 ,我们可以选择抛出自己的异常,这样做的好处在于

+ 我们可以有意识的决定在什么时候停止程序的流程
+ 抛出更具体的异常类型和更有用的错误消息

*example*

```java
public V get(K key){
    intlocation = findKey(key);
    if(location < 0){
        throw newIllegalArgumentException("Key " + key + " does not exist in map."\); 
	}
    return valus[findKey(key)];
}
```

此时在运行,我们就可以得知出错的位置

```
$java ExceptionDemo
Exception in thread "main" java.lang.IllegalArgumentException: Key yolp does not exist in map.
at ArrayMap.get(ArrayMap.java:40)
at ExceptionDemo.main(ExceptionDemo.java:6)
```

我们还使用**`catch`** 来防止程序崩溃

```java
Dog d = new Dog("Lucy", "Retriever", 80);
d.becomeAngry();

try {
    d.receivePat();
} catch (Exception e) {
    System.out.println("Tried to pat: " + e);
}
System.out.println(d);
```

使用后的程序结果

```
$ java ExceptionDemo
Tried to pat: java.lang.RuntimeException: grrr... snarl snarl
Lucy is a displeased Retriever weighing 80.0 standard lb units.
```

使用**exceptions** 的另外好处: 代码的逻辑变得清晰

![image-20210317232647286](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2841aba39a174aaf893ae932459f8e7e~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

