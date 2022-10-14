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
