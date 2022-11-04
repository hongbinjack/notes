
| 隔离级别         | 脏读可能性 | 不可重复读可能性 | 幻读可能性 | 加锁读 |
| ---------------- | :--------: | :--------------: | :--------: | :----: |
| READ UNCOMMITTED |     √      |        √         |     √      |   ×    |
| READ COMMITTED   |     ×      |        √         |     √      |   ×    |
| REPEATABLE READ  |     ×      |        ×         |     √      |   ×    |
| SERIALIZABLE     |     ×      |        ×         |     ×      |   √    |

![H(@TAVI7XCGDL X IN1SKLB](https://user-images.githubusercontent.com/106834223/199880140-fcd4a844-3798-44c1-a487-5462aaee0d36.png)
