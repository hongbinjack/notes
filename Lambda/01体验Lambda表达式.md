```java
public class MyRunnable implements Runnable{
    @Override
    public void run() {
        System.out.println("多线程启动了");
    }
}
```
```java
public class LambdaDemo {
/*
    需求：启动一个线程，在控制台输出一句话，多线程启动了
 */

    public static void main(String[] args) {
       //实现类的方式实现需求
        MyRunnable my =new MyRunnable();
        Thread t=new Thread(my);
        t.start();


        //匿名内部类的方式改进
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("多线程启动了");
            }
        }).start();


        //Lambda表达式方式改进
        new Thread(() -> {
            System.out.println("多线程启动了");
        }).start();
    }
}
```



**Lambda表达式的代码分析**

- ()    里面没有内容，可以看成是方法形式参数为空

- ->   用箭头指向后面要做的事情

- { }   包含一段代码，我们称之为代码块，可以看成是方法体中的内容

  ​           

**组成Lambda表达式的三要素：**<font color=red>形式参数，箭头，代码块</font>



------

**Lambda表达式的标准格式**

- **格式：**<font color=red>(形式参数) -> {代码块}</font>

- 形式参数:  如果多个参数，参数之间用逗号隔开；如果没有参数，留空既可

- **->**    由英文中画线和大于符号组成，固定写法。代表指向动作

- **代码块**    是我们具体要做的事情，也就是以前我们写的方法体内  



</br></br></br>

#                    Lambda表达式的练习01(抽象方法无参无返回值)

Lambda表达式的使用前提

- 有一个接口

- 接口中有且仅有一个抽象方法

  ------

  

**代码：**

```java
public interface Eatable {
    void eat();
}
```

```java
public class EatableImpl implements Eatable{
    @Override
    public void eat() {
        System.out.println("一天一个苹果");
    }
}

```

```java
public class EatableDemo {
    /*Lambda表达式的格式：(形式参数) -> {代码块}*/
    public static void main(String[] args) {
        //在主方法中调用use Eatable方法
        Eatable e=new EatableImpl();
        useEatable(e);

        //优化：匿名内部类
        useEatable(new Eatable() {
            @Override
            public void eat() {
                System.out.println("一天两个苹果");
            }
        });

        //再优化：Lambda表达式
        useEatable(new Eatable() {
            @Override
            public void eat() {
                System.out.println("一天三个苹果");
            }
        });
    }
    private static void useEatable(Eatable e){
        e.eat();
    }
}

```

------

</br></br></br>

# Lambda表达式练习02(抽象方法带参无返回值)

```java
public interface Flyable {
    void fly(String s);
}

```

```java
public class FlyableDemo {
    public static void main(String[] args) {
        //在主方法中调用useFlyable方法
        //匿名内部类
        useFlyable(new Flyable() {
            @Override
            public void fly(String s) {
                System.out.println(s);
                System.out.println("飞机自驾游");
            }
        });
        System.out.println("-------------------");

        //Lambda表达式
        useFlyable((String s)->{
            System.out.println(s);
            System.out.println("开车自驾游");
        });
    }
    private static void useFlyable(Flyable f){
        f.fly("风和日丽，晴空万里");
    }
}

```

</br></br></br>

------



# Lambda表达式练习03(抽象方法带参带返回值)

```java
public interface Addable {
    int add(int x,int y);
}

```

```java
public class AddableDemo {
    public static void main(String[] args) {
        //在主方法中调用Addable方法
         useAddable((int x,int y)->{
             return x+y;
         });

    }


    private static void useAddable(Addable a){
        int sum=a.add(10,89);
        System.out.println(sum);
    }
}

```



</br></br></br>

------



# Lambda表达式的省略模式

**省略规则**

- 参数类型可以省略，但是有多个参数的情况下，不能只省略一个
- 如果参数有且仅有一个，那么小括号可以省略
- 如果代码块的语句只有一条，可以省略大括号，甚至是return

```java
public interface Addable {
    int add(int x,int y);
}

```

```java
public interface Flyable {
    void fly(String s);
}

```

```java
public class LambdaDemo {
    public static void main(String[] args) {
     useAddable((int x,int y)->{
         return x+y;
     });
     //参数的类型可以省略,但是有多个参数的情况下不能只省略一个如：int x,y
        useAddable((x,y)->{
            return x+y;
        });


       useFlyable((String s)->{
       System.out.println(s);
        });
         //以下是省略参数类型的情况
        useFlyable((s)->{
            System.out.println(s);
        });
        useFlyable(s->{  //如果参数只有一个，连小括号也可以省略
            System.out.println(s);
        });

//如果代码块的语句只有一条，可以省略大括号和分号。如果有return，return也要省略，否则报错
        useFlyable(s-> System.out.println(s));
        useAddable((x,y) -> x+y);//useAddable((x,y)-> return x+y);省略return
    }



    private static void useFlyable(Flyable f){
        f.fly("风和日丽，晴空万里");
    }

    private static void useAddable(Addable a){
        int sum=a.add(23,99);
        System.out.println(sum);
    }
}

```



</br></br></br>

# Lambda表达式的注意事项

**注意事项**

- 使用Lambda必须要有接口，并且接口中有且仅有一个抽象方法

- 必须有上下文环境，才能推导出Lambda对应的接口

  ​      根据局部变量的赋值得知Lambda对应的接口：Runnable r=() -> System.out.println("Lambda表达式");

        根据调用方法的参数得知Lambda对应的接口：new Thread(() -> System.out.println("Lambda表达式")).start();

