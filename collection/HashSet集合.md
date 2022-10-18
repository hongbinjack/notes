# 哈希值

<u>哈希值</u>：是JDK根据该对象的地址或者字符串或者数字算出来的int类型的数值



**Object类中有一个方法可以获取对象的hash值**

- public int hashCode():返回对象的哈希码值



**对象的哈希值特点**

- 同一个对象多次调用hashCode( )方法返回的哈希值是相同的

- 默认情况下，不同对象的哈希值是不同的。而重写hashCode( )方法，

  ```
  可以实现让不同对象的哈希值相同
  ```

```java
package chapter1;

public class Student {
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}

```

```java
package chapter1;

import java.util.HashSet;
import java.util.Set;

public class Demo {
    public static void main(String[] args) {
       Student s1=new Student("张三",22);

       //同一个对象多次调用hashCode()方法返回的哈希值是相同的
        System.out.println(s1.hashCode());//460141958
        System.out.println(s1.hashCode());//460141958

        Student s2=new Student("张三",22);
        /*默认情况下不同对象的哈希值是不同的。如果Student类中重写了hashCode()方法
          不同对象之间的哈希值是可以相同的.
       @Override
       public int hashCode(){
       return 0;//return多少哈希值就是多少
       }
         */
        System.out.println(s2.hashCode());//1163157884
        System.out.println("--------------");

        System.out.println("hello".hashCode());//99162322
        System.out.println("world".hashCode());//113318802
        System.out.println("hello".hashCode());//99162322
        System.out.println("--------------");


    }
}

```

------

</br></br><br>



**HashSet集合特点**

- 底层数据结构是哈希表
- 对集合的迭代顺序不做任何保证，也就是说不保证存储和取出的元素顺序一致
- 没有带索引的方法，所以不能使用普通for循环遍历
- 由于是Set集合，所以是不包含重复元素的集合

```java
package chapter1;

import java.util.HashSet;

public class Demo{
    public static void main(String[] args) {
        HashSet<String> hs=new HashSet<String>();
        //添加元素
        hs.add("hello");
        hs.add("world");
        hs.add("java");
        hs.add("hello");
        //遍历
        for(String s:hs){
            System.out.println(s);
        }
    }
}
/*
运行结果：
          world
          java
          hello
    和存储的结果不一样,且hello不重复
 */
```



</br></br>

------



**HashSet集合保证元素唯一性源码分析**



![241](https://user-images.githubusercontent.com/106834223/196332081-b3fb62bc-84ea-44fe-ab9c-37eccdd7b01c.png)



```java
 HashSet<String> hs=new HashSet<String>();
        //添加元素
        hs.add("hello");
        hs.add("world");
        hs.add("java");
        hs.add("hello");
===================================================================

public boolean add(E e) { //e就是元素hello
        return map.put(e, PRESENT)==null;
    }
-------------------------------------------------------------------


   public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
------------------------------------------------------------------


static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }

------------------------------------------------------------------
   
  //hash值是根据元素的hashCode( )方法相关  
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
    
    //如果哈希表未初始化，就对其进行初始化
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
    
    //根据对象的哈希值计算对象的存储位置，如果该位置没有元素，就存储元素
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            /*
            存入的元素与以前的元素比较哈希值
            如果哈希值不同，会继续向下执行，把元素添加到集合
            如果哈希值相同，会调用equals()方法比较
            如果返回false，会继续向下执行，把元素添加到集合
            如果返回true，说明元素重复，不存储
            */
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }




```

------



</br></br>

**常见数据结构之哈希表**

![242](https://user-images.githubusercontent.com/106834223/196331874-59a7459e-8ef1-441b-a8b0-b2333df94deb.png)


<u>哈希表</u>

- JDK8之前，底层采用数组+链表实现，可以说是一个元素为链表的数组
- JDK8以后，在长度比较长的时候，底层实现了优化







------

# HashSet集合存储学生对象并遍历

需求：创建一个存储学生对象的集合，存储多个学生对象，使用程序实现在控制台

​            遍历该集合

要求：学生对象的成员变量值相同我们就认为是同一个对象

```java
package chapter1;

import java.util.Objects;

public class Student {
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        Student student = (Student) o;

        if (age != student.age) return false;
        return Objects.equals(name, student.name);
    }

    @Override
    public int hashCode() {
        int result = name != null ? name.hashCode() : 0;
        result = 31 * result + age;
        return result;
    }
}

```

```java
package chapter1;

import java.util.HashSet;

public class HashSetDemo {
    public static void main(String[] args) {
        //创建HashSet集合对象
        HashSet<Student> hs=new HashSet<Student>();

        //创建学生对象
        Student s=new Student("Mark",34);
        Student ss=new Student("Jack",33);
        Student sss=new Student("Tom",35);
        Student ssss=new Student("Jack",33);
        //把学生添加到集合
        hs.add(s);
        hs.add(ss);
        hs.add(sss);
        hs.add(ssss);
        //遍历集合
        for(Student n:hs){
            System.out.println(n.getName()+","+n.getAge());
        }
    }
}
/*
运行结果：
Tom,35
Jack,33
Mark,34
Jack,33

重写hashCode()&equals()方法之后，运行结果：
Tom,35
Jack,33
Mark,34

注：需要重写两个方法，保证在对象内部存储内容一致时对象的
    哈希值相同，从而再通过hashset保证元素唯一性
 */
```

------

</br></br>

# LinkedHashSet集合概述和特点

**特点**

- 哈希表和链表实现Set接口，具有可预测的迭代次序
- 由链表保证元素有序，也就是说元素的存储和去除顺序是一致的



```java
package chapter1;

import java.util.LinkedHashSet;

public class HashSetDemo {
    public static void main(String[] args) {
        LinkedHashSet<String> lhs=new LinkedHashSet<String>();
        lhs.add("hello");
        lhs.add("world");
        lhs.add("java");

        lhs.add("world");

        for(String s:lhs){
            System.out.println(s);
        }
    }
    }
    /*
    运行结果：hello
            world
            java
     */
```


