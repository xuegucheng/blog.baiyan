### 存储引擎

**查看数据库存储引擎**

```mysql
mysql> show engines;
```

**查看MySQL当前默认的存储引擎**

```mysql
mysql> show variables like '%storage_engine%'
```

**查看表的存储引擎**

```mysql
show table status like "table_name"
```

- **引擎类别**
  * **InnoDB**
    * InnoDB是**事务型数据库**的首选引擎
    * 如果要提供**提交、回滚、崩溃恢复能力**的事物安全（ACID兼容）能力，并要求实现并发控制，InnoDB是一个好的选择
    * **实时业务生产**(直接跟业务处理相关的数据库)mysql引擎首选
  * **MyISAM**
    * MyISAM拥有较高的插入、查询速度，但**不支持事务**
    * 如果数据表主要用来插入和查询记录，则MyISAM引擎能提供较高的处理效率
    * 多用于**非实时业务生产**，如**数据仓库**等**数据应用数据库环境**（不存在多业务表的事务绑定操作，在用实时业务数据做了清洗、计算后，直接存入对应的数据仓库表，后面直接读取使用）
  * **Memory**（内存形存储引擎） 了解即可
    - 临时存放数据，数据量不大，并且不需要较高的数据安全性，可以选择将数据保存在内存中的Memory引擎，MySQL中使用该引擎作为临时表，存放查询的中间结果
- **InnoDB**和**MyISAM**的对比
  1. **是否支持行级锁**：MyISAM 只有表级锁(table-level locking)，而InnoDB 支持行级锁(row-level locking)和表级锁,默认为行级锁
  2. **是否支持事务和崩溃后的安全恢复**：MyISAM 强调的是性能，每次查询具有原子性,其执行速度比InnoDB类型更快，但是不提供事务支持。但是InnoDB提供事务支持事务，外部键等高级数据库功能。 具有事务(commit)、回滚(rollback)和崩溃修复能力(crash recovery capabilities)的事务安全(transaction-safe (ACID compliant))型表
  3. **是否支持外键**:MyISAM不支持，而InnoDB支持
  4. **是否支持MVCC**:仅 InnoDB 支持。应对高并发事务, MVCC比单纯的加锁更高效;MVCC只在 `READ COMMITTED` 和 `REPEATABLE READ` 两个隔离级别下工作;MVCC可以使用 乐观(optimistic)锁 和 悲观(pessimistic)锁来实现;各数据库中MVCC实现并不统一
  5. 。。。。

**什么是事务**

**事务是对数据库的一系列操作,如果属于同一个事务,那么这些操作要么同时成功,要么同时失败**

**事物的四大特性(ACID)**

![äºç©çç¹æ§](https://camo.githubusercontent.com/429c7cf4d94faa9ee216b110bf3258bf2a346987/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f2545342542412538422545352538412541312545372538392542392545362538302541372e706e67)

- **原子性**:事务是不可分割的,要么同时成功,要么同时失败
- **持久性**:数据一旦回滚或提交,那么就将永久保存在硬盘上
- **隔离性**:事务之间相互独立,不互相影响
- **一致性**:事务操作前后,数据总量不变

**不考虑隔离性会产生的问题:**

- **脏读**:可以读到其他事务未提交的数据
- **丢失修改**:指在一个事务读取一个数据时，另外一个事务也访问了该数据，那么在第一个事务中修改了这个数据后，第二个事务也修改了这个数据。这样第一个事务内的修改结果就被丢失，因此称为丢失修改
- **不可重复读**:指在一个事务内多次读同一数据。在这个事务还没有结束时，另一个事务也访问该数据。那么，在第一个事务中的两次读数据之间，由于第二个事务的修改导致第一个事务两次读取的数据可能不太一样。这就发生了在一个事务内两次读到的数据是不一样的情况，因此称为不可重复读。
- **幻读**:有一个事务进行整表操作时,其他事务插入一个数据,之前事务会查不到自己的该变

**不可重复读和幻读区别：**

不可重复读的重点是修改比如多次读取一条记录发现其中某些列的值被修改，幻读的重点在于新增或者删除比如多次读取一条记录发现记录增多或减少了。

**SQL 标准定义了四个隔离级别：**

* **READ-UNCOMMITTED**：读未提交，最低的隔离级别，**可能会出现脏读、幻读、不可重复读**
* **READ-COMMITTED**：读取以提交，可以解决脏读，此级别为Oracle默认，**可能会出现幻读、不可重复读**
* **REPEATABLE READ**：可重复读，可以解决脏读，不可重复读，此级别为MySQL默认，**但幻读仍有可能发生**
* **SERIALIZABLE**：串行化，可以解决上面所有问题，但是效率很低

|     隔离级别     | 脏读 | 不可重复读 | 幻影读 |
| :--------------: | :--: | :--------: | :----: |
| READ-UNCOMMITTED |  √   |     √      |   √    |
|  READ-COMMITTED  |  ×   |     √      |   √    |
| REPEATABLE-READ  |  ×   |     ×      |   √    |
|   SERIALIZABLE   |  ×   |     ×      |   ×    |

待补充。。。。