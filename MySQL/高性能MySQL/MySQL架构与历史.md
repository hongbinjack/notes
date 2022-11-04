
| 隔离级别         | 脏读可能性 | 不可重复读可能性 | 幻读可能性 | 加锁读 |
| ---------------- | :--------: | :--------------: | :--------: | :----: |
| READ UNCOMMITTED |     √      |        √         |     √      |   ×    |
| READ COMMITTED   |     ×      |        √         |     √      |   ×    |
| REPEATABLE READ  |     ×      |        ×         |     √      |   ×    |
| SERIALIZABLE     |     ×      |        ×         |     ×      |   √    |

![H(@TAVI7XCGDL X IN1SKLB](https://user-images.githubusercontent.com/106834223/199880140-fcd4a844-3798-44c1-a487-5462aaee0d36.png)

**显示表的相关信息**：

```mys
show table status LIKE 'orders'  \G;
```

```mys
          Name: orders
         Engine: InnoDB
        Version: 10
     Row_format: Dynamic
           Rows: 5
 Avg_row_length: 3276
    Data_length: 16384
Max_data_length: 0
   Index_length: 16384
      Data_free: 0
 Auto_increment: 20010
    Create_time: 2022-09-14 09:28:09
    Update_time: NULL
     Check_time: NULL
      Collation: utf8mb4_0900_ai_ci
       Checksum: NULL
 Create_options:
        Comment:
1 row in set (0.05 sec)
```


</br></br></br>


**二进制日志（Binary Log）** 也可叫作变更日志（Update Log），是 MySQL 中非常重要的日志。 主要用于记录数据库的变化情况，即 SQL 语句的 DDL 和 DML 语句，不包含数据记录查询操作。

