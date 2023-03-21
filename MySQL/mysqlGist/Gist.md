# 存储过程

**对于参数 in ，out ，inout**

```mysql
create procedure p4(in score int,out result varchar(10))
begin
    if score >= 85 then
        set result := 'excellent';
        elseif score >= 60 then
        set result := 'ordinary';
        else
        set result := 'bad';
    end if;
end;

call p4(98,@result);

select @result;
```





```mysql
create procedure p4(IN input int,out output varchar(20) )
    BEGIN
        if input = 30 then
            set output := '而立之年';
        elseif input > 30 then
            set output := '不惑之年';
        else
            set  output := '小孩子';
        end if;
        select output;
    END;
```





```mysql
create procedure p5(inout score double)
begin
    set score = (1/2) * score;
end;
/**
   先要对局部变量赋值，然后再调用存储过程，最后就可以查看以200分为总分，换算实际分数为100分的结果了
 */
set @score = 99;
call p5(@score);
select @score;
```





**case**

```mysql
create procedure p6(in month int)
BEGIN
    declare result varchar(20);
    case
        when month>=1 and month<=3 then
            set result := '第一季度';

        when month>=4 and month<=6 then
            set result := '第二季度';

        when month>=7 and month<=9 then
            set result := '第三季度';

        when month>=10 and month<=12 then
            set result := '第四季度';

        else
            set result = '输入不合法';
        end case ;
    select concat('您输入的月份是：',month,'月,是',result);
end;

call p6(11);
```





**循环while:**满足条件则操作

* 计算前n项和

```mysql
create procedure p7(IN n int)
BEGIN
-- 注意，此处如果不设置total的默认值，那么会默认为空，最终无论n的值多大，得到的total值都是null
    declare total int default 0; 
    while n > 0 do
        set total := total + n;
        set n := n-1;
        end while;
    select total;
end;


call p7(3);
```





**循环repeat：**满足条件则退出循环

* repeat是有条件的循环控制语句，当满足条件的时候退出循环

语法：

```
             REPEAT
                        SQL逻辑...
                        UNTIL 条件
             END REPEAT;
```





```mysql
CREATE procedure p8(IN n int)
BEGIN
    declare total int default 0;
    REPEAT

        set total := total + n;
        set n := n-1;
    until   n <= 0

        end repeat;
    select total;
end;

```



**loop循环：**

* LOOP实现简单的循环，如果不在SQL逻辑中增加退出循环的条件，可以用其来实现简单的死循环。LOOP可以配合以下两个语句使用:
* LEAVE:配合循环使用，退出循环。

* ITERATE:必须用在循环中，作用是跳过当前循环剩下的语句，直接进入下一次循环。.



```mysql
[begin_label:] LOOP   -- 中括号是该loop循环的名字
        SQL逻辑...
END LOOP [end_label];
```

```mysql
LEAVE label; -- 退出指定标记的循环体
ITERATE label; -- 直接进入下一次循环
```



* 计算前n项和

```mysql
create procedure p9(in n int)
BEGIN
    declare total int default 0;

    sum:loop
        if n <= 0 then
            leave sum;
        end if;

    set total := total + n;
        set n = n-1;
    end loop;

    select total;
end;

call p9(10);
```



* 计算前n项的偶数和

```mysql
create procedure p10(in n int)
BEGIN
    declare total int default 0;

    sum:loop
        if n <= 0 then
            leave sum ;
        end if;

        if n%2 != 0 then
           /*
             这里如果不对n的值减去1，将会造成死循环。原因如下：
             加入此时传来的值为5，此时n的值为奇数，所以代码会直接进入下一次的循环，下面的代码就不会执行，
             所以n值还是5，之后进入下一次循环传入的n值还是5，判断不是偶数继续不执行下面的代码，进入下一次循环，
             周而复始，一直结束不了循环
            */
            set n = n-1;
            iterate sum;
        end if;

        set total := total + n;
        set n = n-1;
    end loop;

    select total;
end;

```







