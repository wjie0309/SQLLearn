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

