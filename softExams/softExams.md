# 计算机组成与体系结构



## 6 CPU结构（运算器与控制器的组成）

![6.1](https://user-images.githubusercontent.com/106834223/222091487-f1f528a5-d54f-4b08-833c-c3dceec646fc.png)

ALU：与运算有关

AC：通用寄存器，不仅仅是加法运算，存一些运算所需要的值

DR：对内存储器来进行读写操作时，用来暂存数据的寄存器

PSW：用来存储运算的过程中相关的标志位(存储状态信息，如溢出)的

PC:存储下一条要执行的指令地址







## 7 Flynn分类法简介



![image](https://user-images.githubusercontent.com/106834223/222092966-13a338cb-e62f-4a0f-a73b-45136ae0ea54.png)

单处理系统：单片机

阵列处理机：处理数组类型的一些运算







## 8. CISC和RISC



![8.1](https://user-images.githubusercontent.com/106834223/222093266-6c2a8f87-8d2d-489d-9cff-5cd7e3373dc2.png)

**RISC全称Reduced Instruction Set Compute**，

精简指令集计算机。

**CISC全称Complex Instruction Set Computers**，

复杂指令集计算机。

CISC既有简单指令也有复杂指令，后来人们发现典型程序中80%的语句都是使用计算机中20%的指令，而这20%的指令都属于简单指令；因此花再多时间去研究复杂指令，也仅仅只有20%的使用概率，并且复杂指令会影响计算机的执行速度。既然典型程序的80%都是使用简单指令完成，那剩下的20%语句用简单语句来重新组合一下模拟这些复杂指令就行了，而不需要使用这些复杂指令，于是RISC就出现了。



**RISC的主要特点：**

1）选取使用频率较高的一些简单指令以及一些很有用但不复杂的指令，让复杂指令的功能由使用频率高的简单指令的组合来实现。

2）指令长度固定，指令格式种类少，寻址方式种类少。

3）只有取数/存数指令访问存储器，其余指令的操作都在**寄存器**内完成。

4）CPU中有多个通用寄存器（比CICS的多）

5）采用流水线技术（RISC一定采用流水线），大部分指令在一个时钟周期内完成。采用超标量超流水线技术，可使每条指令的平均时间小于一个时钟周期。

6）控制器采用组合逻辑控制，不用微程序控制。

7）采用优化的编译程序

CICS的主要特点：
1）指令系统复杂庞大，指令数目一般多达200~300条。

2）指令长度不固定，指令格式种类多，寻址方式种类多。

3）可以访存的指令不受限制（RISC只有取数/存数指令访问存储器）

4）各种指令执行时间相差很大，大多数指令需多个时钟周期才能完成。

5）控制器大多数采用微程序控制。

6）难以用优化编译生成高效的目标代码程序



**RISC与CICS的比较**

1.RISC比CICS更能提高计算机运算速度；RISC寄存器多，就可以减少访存次数，指令数和寻址方式少，因此指令译码较快。

2.RISC比CISC更便于设计，可降低成本，提高可靠性。

3.RISC能有效支持高级语言程序。

4.CICS的指令系统比较丰富，有专用指令来完成特定的功能，因此处理特殊任务效率高。





## 9. 流水线的基本概念



![9.1](https://user-images.githubusercontent.com/106834223/222093626-d0c9daf5-4193-4829-b964-b583ad2a92c7.png)



------









## 10. 流水线周期及流水线执行时间计算

![10.1](https://user-images.githubusercontent.com/106834223/222094814-360a7789-18fc-4065-b89e-eabc70e63faf.png)



------



## 11流水线吞吐率计算





k  指令分为几段，这里k分为三段，分别是取值，分析，执行

![11.1](https://user-images.githubusercontent.com/106834223/222095783-854ae5ee-ee3c-47e3-b93a-61b3b6321aed.png)



TP~max~忽略流水线建立时间比平常执行所多消耗的时间，在建立之后，每一个流水线的周期

就会完成一条指令的运行





------





## 12. 流水线加速比计算



![1677128632512](C:\Users\LiuHongBin\AppData\Roaming\Typora\typora-user-images\1677128632512.png)



------

![1677128784333](C:\Users\LiuHongBin\AppData\Roaming\Typora\typora-user-images\1677128784333.png)



------

![1677129295861](C:\Users\LiuHongBin\AppData\Roaming\Typora\typora-user-images\1677129295861.png)

------





## 13. 计算机层次化存储结构



运算器，计算器中有相应的寄存器

![1677130182235](C:\Users\LiuHongBin\AppData\Roaming\Typora\typora-user-images\1677130182235.png)

Cache分为按内容存取和按地址存取

------





## 14. Cache的基本概念



![1677140294269](C:\Users\LiuHongBin\AppData\Roaming\Typora\typora-user-images\1677140294269.png)



------





## 15. 时间局部性与空间局部性



![1677141931424](C:\Users\LiuHongBin\AppData\Roaming\Typora\typora-user-images\1677141931424.png)

- 局部性原理：计算机在处理相关的数据和程序的时候，某一个时段集中地去访问某些指令或读取某一空间的数据
- 时间局部性：如上述循环语句，需要不断循环，刚访问玩，又要接着访问，直到循环结束
- 空间局部性：如图红色数组，首先访问A数组下标为0的数据，然后又访问它临近的下标为1的数据，称之为空间局部性











