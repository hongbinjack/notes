在类中，使用 static 修饰符修饰的属性（成员变量）称为静态变量，也可以称为类变量，常量称为静态常量，方法称为静态方法或类方法，它们统称为静态成员，归整个类所有。

静态成员不依赖于类的特定实例，被类的所有实例共享，就是说 static 修饰的方法或者变量不需要依赖于对象来进行访问，只要这个类被加载，[Java](http://c.biancheng.net/java/) 虚拟机就可以根据类名找到它们。

</br>

**调用静态成员的语法形式如下：**

```java
类名.静态成员
```

- static 修饰的成员变量和方法，从属于类。
- 普通变量和方法从属于对象。
- 静态方法不能调用非静态成员，编译会报错。

**静态变量**

类的成员变量可以分为以下两种：

1. 静态变量（或称为类变量），指被 static 修饰的成员变量。
2. 实例变量，指没有被 static 修饰的成员变量。

静态变量与实例变量的区别如下：

1）静态变量

- 运行时，Java 虚拟机只为静态变量分配一次内存，在加载类的过程中完成静态变量的内存分配。
- 在类的内部，可以在任何方法内直接访问静态变量。
- 在其他类中，可以通过类名访问该类中的静态变量。

2）实例变量

- 每创建一个实例，Java 虚拟机就会为实例变量分配一次内存。
- 在类的内部，可以在非静态方法中直接访问实例变量。
- 在本类的静态方法或其他类中则需要通过类的实例对象进行访问。

静态变量在类中的作用如下：

- 静态变量可以被类的所有实例共享，因此静态变量可以作为实例之间的共享数据，增加实例之间的交互性。

- 如果类的所有实例都包含一个相同的常量属性，则可以把这个属性定义为静态常量类型，从而节省内存空间。例如，在类中定义一个静态常量 PI。

  ```java
  public static double PI = 3.14159256;
  ```

  </br>

  **例 1**

  创建一个带静态变量的类，然后在 main() 方法中访问该变量并输出结果.

  ```java
  public class StaticVar {
      public static String str1 = "Hello";
      public static void main(String[] args) {
          String str2 = "World!";
          // 直接访问str1
          String accessVar1 = str1+str2;
          System.out.println("第 1 次访问静态变量，结果为："+accessVar1);
          
          // 通过类名访问str1
          String accessVar2 = StaticVar.str1+str2;
          System.out.println("第 2 次访问静态变量，结果为："+accessVar2);
          
          // 通过对象svt1访问str1
          StaticVar svt1 = new StaticVar();
          svt1.str1 = svt1.str1+str2;
          String accessVar3 = svt1.str1;
          System.out.println("第3次访向静态变量，结果为："+accessVar3);
          
          // 通过对象svt2访问str1
          StaticVar svt2 = new StaticVar();
          String accessVar4 = svt2.str1+str2;
          System.out.println("第 4 次访问静态变量，结果为："+accessVar4);
      }
  }
  //这里第四次结果为HelloWorld！World！是因为：每个线程读取static变量到线程的工作内存中，然后再进行计算，
  //最后将修改以后的变量的值同步到static变量中
  ```

  运行该程序后的结果如下所示:

  ```java
  第 1 次访问静态变量，结果为：HelloWorld!
  第 2 次访问静态变量，结果为：HelloWorld!
  第 3 次访向静态变量，结果为：HelloWorld!
  第 4 次访问静态变量，结果为：HelloWorld!World!
  ```

  *从运行结果可以看出，在类中定义静态的属性（成员变量），在 main() 方法中可以直接访问，也可以通过类名访问，还可以通过类的实例对象来访问。*

  注意：静态变量是被多个实例所共享的。  

  

**静态方法**

与成员变量类似，成员方法也可以分为以下两种：

1. 静态方法（或称为类方法），指被 static 修饰的成员方法。
2. 实例方法，指没有被 static 修饰的成员方法。

静态方法与实例方法的区别如下：

- 静态方法不需要通过它所属的类的任何实例就可以被调用，因此在静态方法中不能使用 this 关键字，也不能直接访问所属类的实例变量和实例方法，但是可以直接访问所属类的静态变量和静态方法。另外，和 this 关键字一样，super 关键字也与类的特定实例相关，所以在静态方法中也不能使用 super 关键字。

- 在实例方法中可以直接访问所属类的静态变量、静态方法、实例变量和实例方法。

  **例 2**

  创建一个带静态变量的类，添加几个静态方法对静态变量的值进行修改，然后在 main( ) 方法中调用静态方法并输出结果。

  ```java
  public class StaticMethod {
      public static int count = 1;    // 定义静态变量count
      public int method1() {    
          // 实例方法method1
          count++;    // 访问静态变量count并赋值
          System.out.println("在静态方法 method1()中的 count="+count);    // 打印count
          return count;
      }
      public static int method2() {    
          // 静态方法method2
          count += count;    // 访问静态变量count并赋值
          System.out.println("在静态方法 method2()中的 count="+count);    // 打印count
          return count;
      }
      public static void PrintCount() {    
          // 静态方法PrintCount
          count += 2;
          System.out.println("在静态方法 PrintCount()中的 count="+count);    // 打印count
      }
      public static void main(String[] args) {
          StaticMethod sft = new StaticMethod();
          // 通过实例对象调用实例方法
          System.out.println("method1() 方法返回值 intro1="+sft.method1());
          // 直接调用静态方法
          System.out.println("method2() 方法返回值 intro1="+method2());
          // 通过类名调用静态方法，打印 count
          StaticMethod.PrintCount();
      }
  }
  ```

  运行该程序后的结果如下所示:

  ```java
  在静态方法 method1()中的 count=2
  method1() 方法返回值 intro1=2
  在静态方法 method2()中的 count=4
  method2() 方法返回值 intro1=4
  在静态方法 PrintCount()中的 count=6
  ```

  在该程序中，静态变量 count 作为实例之间的共享数据，因此在不同的方法中调用 count，值是不一样的。从该程序中可以看出，在静态方法 method1() 和 PrintCount() 中是不可以调用非静态方法 method1() 的，而在 method1() 方法中可以调用静态方法 method2() 和 PrintCount()。

  在访问非静态方法时，需要通过实例对象来访问，而在访问静态方法时，可以直接访问，也可以通过类名来访问，还可以通过实例化对象来访问。

  

  ## 静态代码块

  静态代码块指 Java 类中的 static{ } 代码块，主要用于初始化类，为类的静态变量赋初始值，提升程序性能。

  静态代码块的特点如下：

  - 静态代码块类似于一个方法，但它不可以存在于任何方法体中。
  - 静态代码块可以置于类中的任何地方，类中可以有多个静态初始化块。 
  - Java 虚拟机在加载类时执行静态代码块，所以很多时候会将一些只需要进行一次的初始化操作都放在 static 代码块中进行。
  - 如果类中包含多个静态代码块，则 Java 虚拟机将按它们在类中出现的顺序依次执行它们，每个静态代码块只会被执行一次。
  - 静态代码块与静态方法一样，不能直接访问类的实例变量和实例方法，而需要通过类的实例对象来访问。

  #### 例 3

  编写一个 Java 类，在类中定义一个静态变量，然后使用静态代码块修改静态变量的值。最后在 main() 方法中进行测试和输出。

  ```java
  public class StaticCode {
      public static int count = 0;
      {
          count++;
          System.out.println("非静态代码块 count=" + count);
      }
      static {
          count++;
          System.out.println("静态代码块1 count=" + count);
      }
      static {
          count++;
          System.out.println("静态代码块2 count=" + count);
      }
      public static void main(String[] args) {
          System.out.println("*************** StaticCode1 执行 ***************");
          StaticCode sct1 = new StaticCode();
          System.out.println("*************** StaticCode2 执行 ***************");
          StaticCode sct2 = new StaticCode();
      }
  }
  ```

  如上述示例，为了说明静态代码块只被执行一次，特地添加了非静态代码块作为对比，并在主方法中创建了两个类的实例对象。上述示例的执行结果为：

```java
静态代码块1 count=1
静态代码块2 count=2
*************** StaticCode1 执行 ***************
非静态代码块 count=3
*************** StaticCode2 执行 ***************
非静态代码块 count=4
```

上述代码中 { } 代码块为非静态代码块，非静态代码块是在创建对象时自动执行的代码，不创建对象不执行该类的非静态代码块。代码域中定义的变量都是局部的，只有域中的代码可以调用。 

