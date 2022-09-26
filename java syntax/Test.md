# 1 TEST

 	`调试程序` 

+ Ad Hoc Testing

```java
public class TestSort {
    /** Tests the sort method of the Sort class. */
    public static void testSort() {
        String[] input = {"i", "have", "an", "egg"};
        String[] expected = {"an", "egg", "have", "i"};
        Sort.sort(input);
        for (int i = 0; i < input.length; i += 1) {
            if (!input[i].equals(expected[i])) {
                System.out.println("Mismatch in position " + i + ", expected: " + expected + ", but got: " + input[i] + ".");
                break;
            }
        }
    }

    public static void main(String[] args) {
        testSort();
    }
}

public class Sort{
    ...
}
```

+ JUint Testing

使用org.junit 库便利简化我们的代码

修改`ad hoc test`:

```java
public static void testSort() {
    String[] input = {"i", "have", "an", "egg"};
    String[] expected = {"an", "egg", "have", "i"};
    Sort.sort(input);
    org.junit.Assert.assertArrayEquals(expected, input);
}
```

+ better JUnit
  + 把上述所有静态方法变为非静态(选择排序的方法)
  + 在测试类中导包
    import org.junit.Test;
    import static org.junit.Assert.*;
  + 在每个测试方法前添加标签`@Test` ,并删除本用于测试的main 方法
  + 将冗长的`org.junit.Assert.assertEquals(expected, actual)` 改为简单的`assertEquals(expected,actual)`

Tests provide stability and scaffolding.

+ 对每个基本单元都有绝对的正确信心
+ 专注在一项任务中