## 初识数据库

### 数据库简介

数据库, Database Management System, 又称 DBMS. 

SQL 语言使用的数据管理系统, 被称为 RDBMS.

![](https://s1.vika.cn/space/2022/10/18/c8e5a34496ae4381bc90c4dd02ec8556)

如图所示, 这是 RDBMS 常见的结构模式, 通过客户端/服务器/数据库的形式进行联系.

## SQL 简介

### 常见SQL语句

- DDL: 数据定义语言
	- CREATE
	- DROP
	- ALTER
- DML: 数据操纵语言
	- SELECT
	- INSERT
	- UPDATE
	- DELETE
- DCL: 数据控制语言
	- COMMIT
	- ROLLBACK
	- GRANT
	- REVOKE


其中 DML 结构的语言是 SQL 使用过程中的主体, 即我们常常提到的增删改查.

### SQL 数据格式

- INTEGER
- CHAR
- VARCHAR
- DATE

### SQL 约束

约束是另一种对储存数据进行条件限制的方法.

- NOT NULL 为非空约束, 该列必须输入数据.
- PRIMARY KEY 为主键约束, 可以通过该列提取出一行的信息.

### SQL 书写规则

- 每句语句以 `;` 结尾
- 需要使用半角空格 (注意不要使用中文输入法默认的全角空格)
- 插入的数据记得需要区分大小写.
- 命名时只能使用半角英文字母、数字、下划线（`_`）, 且需要使用半角英文字母开头

### SQL 的增删改查

#### CREATE

```SQL
CREATE TABLE < 表名 >
( < 列名 1> < 数据类型 > < 该列所需约束 > ,
  < 列名 2> < 数据类型 > < 该列所需约束 > ,
  < 列名 3> < 数据类型 > < 该列所需约束 > ,
  < 列名 4> < 数据类型 > < 该列所需约束 > ,
  .
  .
  .
  < 该表的约束 1> , < 该表的约束 2> ,……);
```

#### DROP

```SQL
DROP TABLE
```

#### DELETE

```SQL
DELETE FROM ... WHERE ...

TRUNCATE TABLE TABLE_NAME; # 更快的清空方式
```

#### UPDATE

```SQL
UPDATE TABLE
SET
	...
WHERE
	...
ORDER BY ...
LIMIT ...
```

#### INSERT

```SQL
INSERT INTO <表名> (列1, 列2, 列3, ……) VALUES (值1, 值2, 值3, ……);  
```

若进行全列插入, 那么可以省略表名后的列, SQL 会默认直接插入所有列按顺序.

可以使用INSERT … SELECT 语句从其他表复制数据.

```SQL
-- 将商品表中的数据复制到商品复制表中
INSERT INTO productcopy (product_id, product_name, product_type, sale_price, purchase_price, regist_date)
SELECT product_id, product_name, product_type, sale_price, purchase_price, regist_date
	FROM Product;  
```

### 索引

索引可以大幅度增加数据的检索速度.

- MYSQL 中创建索引的方法一般有两种:

	- 创建表时直接创建索引
		```SQL
		CREATE TABLE mytable(  
		 
		ID INT NOT NULL,   
		 
		username VARCHAR(16) NOT NULL,  
		 
		INDEX [indexName] (username(length))  
		 
		);  
		```
	- 使用语句创建索引
		```SQL
		-- 方法1
		CREATE INDEX indexName ON table_name (column_name)
		
		-- 方法2
		ALTER table tableName ADD INDEX indexName(columnName)
		```

## 基础查询与排序

### SELECT

- 基本 SELECT 语法

```SQL
SELECT columns
FROM table_name
WHERE ...
```

-   星号（`*`）代表全部列的意思
-   SQL中可以随意使用换行符，不影响语句执行, 但不可插入空行
-   设定汉语别名时需要使用双引号 `"` 括起来
-   在SELECT语句中使用DISTINCT可以删除重复行

### 运算符

#### 算术运算符

简单的四则运算皆使用常用符号

#### 比较运算符

![](https://s1.vika.cn/space/2022/10/18/f6459873aa554296b9d19cb25a258654)

#### 逻辑运算符

- NOT
- AND
- OR

逻辑运算符遵循四则运算的规则, 可以用括号来规定运算优先级.

### 聚合查询

#### 聚合函数

SQL中用于汇总的函数叫做聚合函数, 以下五个是最常用的聚合函数:

- SUM
- AVG
- MAX
- MIN
- COUNT: 计算表中的记录条数

#### DISTINCT 进行删除的符合查询

使用 DISTINCT 可以去除重复的数据, 比如想要计算不重复的 COUNT 数量, 可以配合 COUNT 和 DISTINCT 使用.

#### 聚合查询规则

-   COUNT 聚合函数运算结果与参数有关，`COUNT(*) / COUNT(1)` 得到包含 NULL 值的所有行，`COUNT(<列名>)` 得到不包含 NULL 值的所有行。
-   聚合函数不处理包含 NULL 值的行，但是 `COUNT(*) `除外。

### GROUP BY

使用这个函数可以进行数据的分组汇总.

GROUP BY 的使用位置为:

```SQL
SELECT purchase_price, COUNT(*)
  FROM product
 WHERE product_type = '衣服'
 GROUP BY purchase_price;
```

- GROUP BY 不能使用别名, 因为在RDBMS系统中会先处理 GROUP BY 的语句操作.
- 使用 GROUP BY 时, 可以使用 HAVING 来配合其进行限定条件分组, 使用方法类似 WHERE.
  这里不使用 WHERE 是因为，WHERE子句只能指定记录行的条件，而不能用来指定组的条件.

### ORDER BY

ORDER BY 的使用格式为:

```SQL
SELECT <列名1>, <列名2>, <列名3>, ……
  FROM <表名>
 ORDER BY <排序基准列1> [ASC, DESC], <排序基准列2> [ASC, DESC], ……
```

其中排序格式为 ASC, DESC, 默认序列为 ASC.

#### 别名的使用

SQL 在使用 HAVING 子句时 SELECT 语句的执行顺序为：

`FROM → WHERE → GROUP BY → SELECT → HAVING → ORDER BY`

因此, ORDER BY 可以使用别名.

#### NULL 情形

在MySQL中, `NULL` 值被认为比任何 `NOT NULL` 值低, 因此, 当顺序为 ASC（升序）时,  `NULL` 值出现在第一位, 而当顺序为 DESC（降序）时, 则排序在最后.

如果想要 NULL 位于不符合寻常情况下的队首或队尾时, 可以在排序时选取对象时乘以一个 -1.

对于字符型的数据, 想要满足这种排序情况可以进行两重标准的排序.

```SQL
-- IS NULL
SELECT * FROM user 
 ORDER BY name IS NULL ASC,name ASC;
 
-- ISNULL()
SELECT * FROM user 
 ORDER BY ISNULL(name) ASC,name ASC;
```

或者也可以通过 `COALESCE` 来将空值变为指定值.

```SQL
SELECT * FROM user 
 ORDER BY COALESCE(name, 'zzzzz') ASC;
```

## 集合运算

### 表的加减法

#### UNION

UNION 可以对两个表做集合并集运算, 即在合并的同时, 去除掉重复的数据部分.

```SQL
SELECT ...
	FROM ...

UNION

SELECT ...
	FROM ...
```

如果是同一个表的查询, 可以通过使用 OR 对 WHERE 条件进行修饰来达到和 UNION 一样的效果.

- 如果需要保留重复行的数据, 就可以使用 `UNION ALL` 来进行合并.

- 在 SELECT 时选取了不同 COLOUMNS 的情况下, UNION 会进行隐式合并, 即将两个数据中不一样的 COLUMNS 合并显示.

#### 表的减法

要实现两个集合之间差集和补集的效果, 可以使用 EXCEPT 以及 NOT IN 函数. 

通过 NOT IN 实现集合之间的对称差 (独属于集合 A 的部分), 在此基础上, 还可以通过求得两个集合的对称差, 再使用 NOT IN 实现得到两个集合的交集的效果 INTERSECT.

### JOIN

连结(JOIN)就是使用某种关联条件(一般是使用相等判断谓词"="), 将其他表中的列添加过来, 进行“添加列”的集合运算.

#### INNER JOIN

```SQL
SELECT SP.shop_id
       ,SP.shop_name
       ,SP.product_id
       ,P.product_name
       ,P.product_type
       ,P.sale_price
       ,SP.quantity
  FROM shopproduct AS SP
 INNER JOIN product AS P
    ON SP.product_id = P.product_id;
```

- 语句中包含多个表
- 设定 ON 语句来确立连接条件
- 如果还会使用 WHERE 限定条件, 那么需要在 ON 之后
- `GROUP BY` 语句可以配合内连接来使用. 使用顺序可前可后. 如果是两个不同的表, 那么就需要先 `JOIN` 后再进行 `GROUP BY`.

#### OUTER JOIN

内连结会丢弃两张表中不满足 ON 条件的行,和内连结相对的就是外连结. 外连结会根据外连结的种类有选择地保留无法匹配到的行. 可以通过设定左连接, 右连接来确定连接方式.

e.g. 

```SQL
SELECT SP.shop_id
       ,SP.shop_name
       ,SP.product_id
       ,P.product_name
       ,P.sale_price
  FROM product AS P
  LEFT OUTER JOIN shopproduct AS SP
    ON SP.product_id = P.product_id;
```

使用 LEFT 时 FROM 子句中写在左侧的表是主表,使用 RIGHT 时右侧的表是主表.

## SQL 高级处理

### 窗口函数

#### 窗口函数定义

`OLAP` 函数, 即为窗口函数, 意思是对数据库做实时分析处理.

```SQL
<窗口函数> OVER ([ PARTITION BY <列名> ]
                     [ ORDER BY <排序用列名> ])  
```

其中 `PARTITION BY` 指定了窗口对象范围. `ORDER BY` 指定排序参照的对象, 排序方式等参数.

#### 常见的窗口函数

- RANK
- DENSE_RANK
- ROW_NUMBER

这三者的区别为, 当出现并列排列的items时, `RANK` 会并列排列, 然后下一位的排名分别会根据前边并列排名的个数按序号增加, 不考虑并列排名的个数只考虑 1, 2, 3... 的序列排序, 以及并列排名的item仍然按顺序进行排序,

聚合函数也可以用窗口函数的使用语法进行使用.

e.g.

```SQL
SELECT  product_id
       ,product_name
       ,sale_price
       ,SUM(sale_price) OVER (ORDER BY product_id) AS current_sum
       ,AVG(sale_price) OVER (ORDER BY product_id) AS current_avg  
  FROM product;  
```

#### 移动平均

```SQL
<窗口函数> OVER (ORDER BY <排序用列名>
                 ROWS n PRECEDING )  
                 
<窗口函数> OVER (ORDER BY <排序用列名>
                 ROWS BETWEEN n PRECEDING AND n FOLLOWING)
```

使用这种语法, 可以指定聚合函数的汇总范围.

- `PERCEDING` 指定为 截至到之前 n 行, 包括本行.
- `FOLLOWING` 指定为 截至到之后 n 行, 包括本行.

#### 注意事项

- 窗口函数只能在 `SELECT` 语句中使用
- `ORDER BY` 子句不会影响排序, 只会影响窗口函数计算排序的方式.

### ROLLUP

在使用了 `GROUP BY` 之后, 可以使用 `ROLLUP` 可以计算不同范围的合计与小计.

e.g.

```SQL
SELECT  product_type
       ,regist_date
       ,SUM(sale_price) AS sum_price
  FROM product
 GROUP BY product_type, regist_date WITH ROLLUP;  
```

就可以得到结果:

![](https://s1.vika.cn/space/2022/10/22/2bd55f17d1dd4e4185a6ea464f9b6103)

### 储存