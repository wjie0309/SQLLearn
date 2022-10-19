## 4.1 

找出 product 和 product2 中售价高于 500 的商品的基本信息。

```SQL
SELECT *
	FROM product
WHERE sale_price > 500

UNION

SELECT *
	FROM product2
WHERE sale_price > 500;
```

![](https://s1.vika.cn/space/2022/10/19/ea16a07f1bd946a5b582548c16e50464)

## 4.2

借助对称差的实现方式, 求product和product2的交集

```SQL
SELECT *
	FROM product
WHERE
	product_id NOT IN 
(
SELECT product_id
	FROM product
WHERE 
	product_id NOT IN (SELECT product_id FROM product2)
);
```

![](https://s1.vika.cn/space/2022/10/19/344e4b8a420644da985ef6be90b6d560)

## 4.3

每类商品中售价最高的商品都在哪些商店有售 ？

```SQL
SELECT
	SP.shop_id,
	SP.shop_name,
	MAX_price.product_type,
	MAX_price.sale_price 
FROM
	shopproduct AS SP
	INNER JOIN (
	SELECT
		product_id,
		product_name,
		product_type,
		MAX( sale_price ) as sale_price 
	FROM product
	GROUP BY product_type
	)
	as MAX_price ON SP.product_id = MAX_price.product_id
```

![](https://s1.vika.cn/space/2022/10/19/8e904c23ee544e89af74fb5d069f2fca)

## 4.4

分别使用内连结和关联子查询每一类商品中售价最高的商品

```SQL
SELECT
	product_type, product_name, sale_price
	FROM product AS P
WHERE 
	sale_price=(
	SELECT MAX(sale_price)
	FROM product AS P2
	WHERE P.product_type = P2.product_type
	GROUP BY product_type
	);
```

![](https://s1.vika.cn/space/2022/10/19/fc777e49aad747779d39375692444a95)

## 4.5

用关联子查询实现：在 product 表中，取出 product_id, product_name, sale_price, 并按照商品的售价从低到高进行排序、对售价进行累计求和

```SQL
SELECT product_id, product_name, sale_price, (SELECT SUM(sale_price) FROM product) as sum_price
	FROM product
ORDER BY sale_price ASC
```

![](https://s1.vika.cn/space/2022/10/20/3d17f925041e417b82112a49ffbdf18c)
