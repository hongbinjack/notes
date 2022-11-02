# Set集合概述和特点

**Set集合概述和特点**

- 不包含重复元素的集合
- 没有带索引的方法，所以不能使用普通for循环遍历



**Set集合练习**

```java
import java.util.HashSet;
import java.util.Set;

public class Demo {
    public static void main(String[] args) {
        //HashSet：对集合的迭代顺序不做任何保证
        Set<String> set=new HashSet<String>();

        set.add("hello");
        set.add("world");
        set.add("helloworld");
        set.add("hello");
        for(String s:set){
            System.out.println(s);
        }
    }
}
/*
运行结果：
              helloworld
              world
              hello
*/
```

