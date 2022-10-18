## 1.1

编写一条 CREATE TABLE 语句，用来创建一个包含表 1-A 中所列各项的表 Addressbook （地址簿），并为 regist_no （注册编号）列设置主键约束.

```SQL
CREATE TABLE Addressbook
(
`regist_no` INTEGER NOT NULL PRIMARY KEY,
`name` VARCHAR(128) NOT NULL,
`address` VARCHAR(256) NOT NULL,
`tel_no` INTEGER(10),
`mail_address` INTEGER(20)
);
```

![](https://s1.vika.cn/space/2022/10/18/a89d7fe419db449d8c25bbd5c1781fa4)

## 1.2

假设在创建练习1.1中的 Addressbook 表时忘记添加如下一列 postal_code （邮政编码）了，请编写 SQL 把此列添加到 Addressbook 表中。

列名 ： postal_code

数据类型 ：定长字符串类型（长度为 8）

约束 ：不能为 NULL

```SQL
ALTER TABLE Addressbook ADD COLUMN postal_code CHAR(8) NOT NULL;
```

![](https://s1.vika.cn/space/2022/10/18/219b899e2a174b40831aded405045189)

## 1.3

请补充如下 SQL 语句来删除 Addressbook 表

```SQL
DROP TABLE Addressbook
```

![](https://s1.vika.cn/space/2022/10/18/591984f8ee924796ac643314df67df2e)
