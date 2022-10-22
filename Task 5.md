## 5.1

请说出针对本章中使用的 product（商品）表执行如下 SELECT 语句所能得到的结果。

```SQL
SELECT  product_id
       ,product_name
       ,sale_price
       ,MAX(sale_price) OVER (ORDER BY product_id) AS Current_max_price
  FROM product;
```

会得到每个产品的价格, 以及当前所有产品中最贵的产品的价格. 其中每个产品按照 product_id 排序.

![](https://s1.vika.cn/space/2022/10/22/a3af79ce9af742a79e7ccb9846254be8)

## 5.2

继续使用product表，计算出按照登记日期（regist_date）升序进行排列的各日期的销售单价（sale_price）的总额。排序是需要将登记日期为NULL 的“运动 T 恤”记录排在第 1 位（也就是将其看作比其他日期都早）

```SQL
SELECT regist_date, product_name, sale_price,
	SUM(sale_price) OVER (PARTITION BY regist_date ORDER BY regist_date) AS sum_price
FROM product;
```

![](https://s1.vika.cn/space/2022/10/22/27097565395347ae887f1c7770e90f02)

## 5.3

思考题

1. 窗口函数不指定PARTITION BY的效果是什么？
2. 为什么说窗口函数只能在SELECT子句中使用？实际上，在ORDER BY 子句使用系统并不会报错.

1. 窗口函数不指定 `PARTITION BY` 的情况下, 则会默认不分类, 可以被视为对整体作用的聚合函数.
2. 因为窗口函数的作用是生成一个新的数据, 利用窗口范围的数据进行统计和作用, 如果不用 SELECT 语句的话, 那么就这个函数生成的新数据不会展示出来, 则事实上没有起到作用.

## 5.4

使用存储过程创建20个与 `shop.product` 表结构相同的表，如下图所示：

![](https://s1.vika.cn/space/2022/10/22/2def423838a44ce6b15ee77a7dbf1e17)

```SQL
DELIMITER //  # 空格后再加//不然会报错误 ①
USE shop //  #USE 'DBName' //  # 数据库名不需要用引号括起来

DROP PROCEDURE IF EXISTS create_multi_table // #过程名不需要用引号括起来不然会报错误 ①

CREATE PROCEDURE shop.create_multi_table(IN table_counts int)

BEGIN
DECLARE i int;
DECLARE table_name varchar(50);  #漏了这个会报错误 ②
SET i = 0;

  WHILE i<table_counts do

    SET table_name = CONCAT('table',i);
    SET @ctbl = CONCAT('CREATE TABLE ',table_name,' like shop.product');  #声明变量

    PREPARE create_stmt FROM @ctbl;  #预处理声明
    EXECUTE create_stmt;  #执行预处理声明
    SET i=i+1;
  END WHILE;

END//

CALL create_multi_table(20)

# DROP PROCEDURE create_multi_table;
```

![](https://s1.vika.cn/space/2022/10/23/7133a2c41a0a4794a308f18edf9ada04)
