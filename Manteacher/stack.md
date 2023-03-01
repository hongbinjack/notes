# 栈_线程安全

![03](https://user-images.githubusercontent.com/106834223/196076202-6f99e816-d114-444a-9cbf-01eb3b2ccd53.png)

如果变量是私有的(如局部变量)不考虑线程安全，如果变量是共享的(static),要考虑线程安全。因为每次栈调用该变量的时候如果是共享的，多个线程会在自己的栈里面对变量进行操作，然后写入共享变量中。

不是共享的，每次调用该变量的时候都单独为该变量创建了一个栈帧，分配了内存。



**方法内的局部变量是否线程安全**

- 如果方法内局部变量没有逃离方法的作用范围，那么它是线程安全的。
- 如果局部变量引用了对象，并逃离了方法的作用范围，需要考虑线程安全问题。

```java
package JVM;

public class Demo {
    public static void main(String[] args) {
        StringBuilder sb=new StringBuilder();
        sb.append(4);
        sb.append(5);
        sb.append(6);
        new Thread(()->{
            m2(sb);
        }).start();//此时两个线程同时修改StringBuilder对象，就不是线程安全了。
    }

    public static void m1(){
        StringBuilder sb=new StringBuilder();
        /*
        sb对象是每个线程私有的，其他线程不可能同时访问到这个StringBuilder对象sb
         */
        sb.append(1);
        sb.append(2);
        sb.append(3);
        System.out.println(sb.toString());
    }
    public static void m2(StringBuilder sb){
        /*
        此处StringBuilder对象sb作为方法的参数传递进来，那么可能有其他线程可以访问
        进来，它就不再是线程私有的了。 可以多个线程对它进行访问，因为它对多个线程是
        共享的。
         */
        sb.append(1);
        sb.append(2);
        sb.append(3);
    }
    public static StringBuilder m3(){
        StringBuilder sb=new StringBuilder();
        sb.append(1);
        sb.append(2);
        sb.append(3);
        return sb;//此时虽然StringBuilder对象是在局部，但是它返回了，
                  //所以其他线程可以拿着这个返回的对象。线程不安全，因为逃离了方法的作用范围
    }

}
```



------

</br></br>

# 栈内存溢出



栈帧里面一般都是一些局部变量，方法参数，占用的内存都很少

  </br>

```
**导致栈溢出的原因**
```

- 一直调用栈帧，导致放入栈的栈帧太多，栈的内存不够，就会发生栈内存溢出。

  <u>如下</u>:

  ![]([picture/02.png at main · hongbinjack/picture (github.com)](https://github.com/hongbinjack/picture/blob/main/JVMpicture/02.png))

  

  ```java
  public class StackOverflow{
      private static int count;
      public static void main(String[] args){
          try{
              method1();
          }catch(Throwable e){
              e.printStackTrace();
              System.out.println(count);
          }
      }
      private static void method1(){
          count++;
          method();
      }
  }
  ```

- 一直递归地调用method()方法，导致占内存溢出。

![02](https://user-images.githubusercontent.com/106834223/196076138-580c7653-87ab-484d-bfff-bd6a2283b2f4.png)

- 栈帧过大导致占内存溢出

​        <u>如下</u>:

![01](https://user-images.githubusercontent.com/106834223/196076174-98849957-277a-4123-8692-1ff2d3d5230d.png)

## 在实际开发中，可能我们自己写的代码不会导致栈内存溢出，但是一些第三方的库可能会导致栈内存溢出







