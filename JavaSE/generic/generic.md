### 泛型的概述

泛型：是JDK5中引入的特性，它提供了编译时类型安全检测机制，该机制允许在编译时检测到非法的类型，它的本质是**参数化类型**，也就是说所操作的数据类型被制定为一个参数

一提到参数，最熟悉的就是定义方法时有形参，然后调用此方法时传递参数。那么参数化类型怎么解释呢？

顾名思义，就是**将类型由原来的具体的类型参数化，然后使用/调用时传入具体的类型**

这种参数类型可以用在类，方法和接口中，分别成为泛型类，泛型方法，泛型接口



泛型定义格式：

- <类型>:制定一种类型的格式。这里的类型可以看成是形参
- <类型1，类型2...>:指定多种类型的格式，多种类型之间用逗号隔开。这里的类型可以看成是形参



泛型的好处：

- 把运行时期的问题提前到了编译期间
- 避免了强制类型转换

```java
package chapter01;

import java.text.CollationElementIterator;
import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;
import java.util.Objects;

public class GenericDemo {
    public static void main(String[] args) {
     //创建集合对象
//        Collection c=new ArrayList();
        Collection<String> c=new ArrayList<String>();
        //添加元素
        c.add("hello");
        c.add("world");
        c.add("java");
//        c.add(100)//ClassCastException;
        //遍历集合
        Iterator<String> it=c.iterator();
        while (it.hasNext()){
//            Object obj=it.next();
//            System.out.println(obj);

//            String s=(String)it.next();
            String s=it.next();
            System.out.println(s);

        }
    }
}

```

------





### 泛型类

**泛型的好处**

Java 语言中引入泛型是一个较大的功能增强。不仅语言、类型系统和编译器有了较大的变化，以支持泛型，而且类库也进行了大翻修，所以许多重要的类，比如集合框架，都已经成为泛型化的了。

这带来了很多好处：

1，类型安全。 泛型的主要目标是提高 Java 程序的类型安全。通过知道使用泛型定义的变量的类型限制，编译器可以在一个高得多的程度上验证类型假设。没有泛型，这些假设就只存在于程序员的头脑中（或者如果幸运的话，还存在于代码注释中）。

 

2，消除强制类型转换。 泛型的一个附带好处是，消除源代码中的许多强制类型转换。这使得代码更加可读，并且减少了出错机会。

 

3，潜在的性能收益。 泛型为较大的优化带来可能。在泛型的初始实现中，编译器将强制类型转换（没有泛型的话，程序员会指定这些强制类型转换）插入生成的字节码中。但是更多类型信息可用于编译器这一事实，为未来版本的 JVM 的优化带来可能。由于泛型的实现方式，支持泛型（几乎）不需要 JVM 或类文件更改。所有工作都在编译器中完成，编译器生成类似于没有泛型（和强制类型转换）时所写的代码，只是更能确保类型安全而已。



```java
package chapter01;

public class Teacher {
    private Integer age;
    public Integer getAge(){
        return age;
    }
    public void setAge(Integer age){
        this.age=age;
    }
}
```

```java
package chapter01;

public class Student {
  private String name;
  public String getName(){
      return name;
  }

  public void setName(String name){
      this.name=name;
  }
}

```



```java
package chapter01;

public class Generic<T> {
    private T t;

    public T getT(){
        return t;
    }

    public void setT(T t){
        this.t=t;
    }
}

```

```java
package chapter01;


public class GenericDemo {
    public static void main(String[] args) {
     Student s=new Student();
     s.setName("张三");
     System.out.println(s.getName());

     Teacher t=new Teacher();
     t.setAge(20);
//     t.setAge("20");//期望的Integer类型但是传入了String类型，出错。
     System.out.println(t.getAge());



     //使用泛型后，传入不同类型的参数不需要创建多个类了。
     Generic<String> g1=new Generic<String>();
     g1.setT("王五");
     System.out.println(g1.getT());
        Generic<Integer> g2=new Generic<Integer>();
        g2.setT(30);
        System.out.println(g2.getT());
     Generic<Boolean> g3=new Generic<Boolean>();
     g3.setT(true);
     System.out.println(g3.getT());
    }
}

```



------



### 泛型方法



泛型方法的定义格式：

- 格式：修饰符 <类型>  返回值类型方法名(类型 变量名){  }
- 范例：public <T> void show(T t){   }

```java
package chapter01;

public class  GenericMethod<T> {
    public <T> void show(T t) {
        System.out.println(t);
    }
}

```

```java
package chapter01;


public class GenericDemo {
    public static void main(String[] args) {
     Generic g=new Generic();
     g.show("韩科");
     g.show(30);
     g.show(true);
//     g.show(2.53); //报错，因为Generic类中没有提供传入浮点类型的参数的的方法


        //通过泛型调用方法
        GenericMethod G = new GenericMethod();
        g.show(33);
        g.show("韩立");
        g.show(true);
    }
}

```

------

------





### 泛型接口



泛型接口的定义格式：

- 格式： 修饰符 interface 接口名<类型>{  }

```java
package chapter01;

public interface Generic<T> {
    void show(T t);
}

```

```java
package chapter01;

public class GenericImpl<T> implements Generic<T>{
    @Override
    public void show(T t) {
        System.out.println(t);
    }
}

```

```java
package chapter01;


public class GenericDemo {
    public static void main(String[] args) {
     Generic<String> g1=new GenericImpl<String>();
     g1.show("张三");

     //不传入类型，则泛型自动转换
     Generic g2=new GenericImpl();
     g2.show(true);
    }
}

```

------

------





### 类型通配符

为了表示各种泛型List的父类，可以使用类型通配符

- 类型通配符：**<?>**
- List<?>:表示元素类型未知的List，它的元素可以匹配**任何类型**
- 这种带通配符的List仅表示它是各种泛型List的父类，并不能把元素添加到其中



   

如果说我们不希望我们不希望List<?>时任何泛型List的父类，只希望它代表某一类泛型List的父类，可以使用类型通配符的上限

- 类型通配符上限：**<?  extends  类型>**
- List<?  extends  Number>:它表示的类型是**Number或者其子类型**



除了可以指定类型通配符的上限，我们也可以指定类型通配符的下限

- 类型通配符下限：**<?  super  类型>**

- List<? super  Number>:它表示的类型是Number或者其父类型

```java
package chapter01;


import java.util.ArrayList;
import java.util.List;

public class GenericDemo {
    public static void main(String[] args) {
     //类型通配符:<?>
        List<?> list1=new ArrayList<Object>();
        List<?> list2=new ArrayList<Number>();
        List<?> list3=new ArrayList<Integer>();

        //类型通配符的上限：<? extends 类型>
       /* List<? extends Number> list4=new ArrayList<Object>();
         编译时期出错，因为类型通配符的上限是Number，而Object类是Number类的父类
       */

        List<? extends Number> list5=new ArrayList<Number>();
        List<? extends Number> list6=new ArrayList<Integer>();

        //类型通配符的下限：<? super 类型>
        List<? super Number> list7=new ArrayList<Object>();
        List<? super Number> list8=new ArrayList<Number>();
        /*List<? super Number> list9=new ArrayList<Integer>();
          报错，因为Integer类型比Number类型更小
          限定了下限为Number
         */

    }
}

```

------







------



### 







