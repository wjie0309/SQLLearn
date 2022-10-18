## 2.1

编写一条SQL语句，从 `product`(商品) 表中选取出“登记日期(`regist_date`)在2009年4月28日之后”的商品，查询结果要包含 `product name` 和 `regist_date` 两列

```SQL
SELECT product_name, regist_date
FROM product
WHERE regist_date > 20090428
```

![](https://s1.vika.cn/space/2022/10/18/9d49d562887a46b5959e9d1e30e9777c)

## 2.2

product 图表数据如图所示

![](https://s1.vika.cn/space/2022/10/18/dd468dacaacb4d73831fd1bd674cde70)

1. 会输出叉子行和圆珠笔行
2. 会输出除了叉子和圆珠笔外的所有行
3. 会输出空表

## 2.3

`2.2.3` 章节中的SELECT语句能够从 `product` 表中取出“销售单价（`sale_price`）比进货单价（`purchase_price`）高出500日元以上”的商品。请写出两条可以得到相同结果的SELECT语句。执行结果如下所示：

![](https://s1.vika.cn/space/2022/10/18/d567ee979a584f4eb12696105939f3a5)

```SQL
SELECT product_name, sale_price, purchase_price
FROM product
WHERE sale_price - purchase_price >= 500;

SELECT product_name, sale_price, purchase_price
FROM product
WHERE sale_price >= 500 +  purchase_price;
```

![](https://s1.vika.cn/space/2022/10/18/c2891066be144a0e84f10dd027e92606)

## 2.4

请写出一条SELECT语句，从 `product` 表中选取出满足“销售单价打九折之后利润高于 `100` 日元的办公用品和厨房用具”条件的记录。查询结果要包括 `product_name`列、`product_type` 列以及销售单价打九折之后的利润（别名设定为 `profit`）。

提示：销售单价打九折，可以通过 `sale_price` 列的值乘以0.9获得，利润可以通过该值减去 `purchase_price` 列的值获得。

```SQL
SELECT product_name, product_type, sale_price*0.9 - purchase_price AS profit
FROM product
WHERE sale_price*0.9 - purchase_price > 100
	AND ( product_type = '办公用品' OR product_type = '厨房用具');
```

![](https://s1.vika.cn/space/2022/10/18/7544f3c37e8b44e19b0ea0e01f656b5e)

## 2.5

请指出下述SELECT语句中所有的语法错误。

```SQL
SELECT product_id, SUM（product_name）
--本SELECT语句中存在错误。
  FROM product 
 GROUP BY product_type 
 WHERE regist_date > '2009-09-01';
```

1. product_name 为字符型数据, 不能使用 SUM.
2. 使用 GROUP BY 后面需要使用 HAVING, 或者把 WHERE 放在 GROUP BY 前面.
3. GROUP BY 字段需要与 SELECT 对象一致.

## 2.6

请编写一条SELECT语句，求出销售单价（ `sale_price` 列）合计值大于进货单价（ `purchase_price` 列）合计值1.5倍的商品种类。执行结果如下所示。

![](https://s1.vika.cn/space/2022/10/19/6788339b987546ab9c16fe7927b9d313)


```SQL
SELECT product_type, SUM(sale_price) AS sum, SUM(purchase_price) AS sum
	FROM product
GROUP BY product_type
HAVING SUM(sale_price) > 1.5*SUM(purchase_price)
```

![](https://s1.vika.cn/space/2022/10/19/d56c93ab66a74681a7325b8c68d0d544)

这一题标题没办法全部显示 sum, 会自动替换一个 sum1.

## 2.7

此前我们曾经使用SELECT语句选取出了product（商品）表中的全部记录。当时我们使用了 `ORDER BY` 子句来指定排列顺序，但现在已经无法记起当时如何指定的了。请根据下列执行结果，思考 `ORDER BY` 子句的内容。

![](https://s1.vika.cn/space/2022/10/19/6b011b9f3af247dca98bb77f2d3ba656)

可以观察得到, 是通过注册日期降序排列, 同时 NULL 值位于队首的排列方式. 因为 NULL 默认是比任何数字都小, 所以这种情况可以对 regist_date 的相反数来做升序排列.

可以观察除此之外, 相同日期时第二排序标准为卖出价格.

可以得到 SQL 语句为:

```SQL
SELECT *
	FROM product
ORDER BY - regist_date, sale_price;
```

![](https://s1.vika.cn/space/2022/10/19/34c246e08f9047518d63befc92fd208f)
