---
title: mysql基础
date: 2021-01-26 11:38:51
tags:
- mysql
---
### 数据库的查询、创建

<!--more-->

```mysql
-- 查询所有的数据库
SHOW DATABASES;

-- 查询mysql数据库的创建语句
SHOW CREATE DATABASE mysql;

-- 创建数据库 db1
CREATE DATABASE db1;

-- 创建数据库db2（判断，如果不存在则创建）
CREATE DATABASE IF NOT EXISTS db2;

-- 创建数据库并指定字符集utf8
CREATE DATABASE db3 CHARACTER SET utf8;

-- 查看db3数据库的字符集
SHOW CREATE DATABASE db3;

-- 练习：创建db4数据库、如果不存在则创建，指定字符集为gbk
CREATE DATABASE IF NOT EXISTS db4 CHARACTER SET gbk;

-- 查看db4数据库的字符集
SHOW CREATE DATABASE db4;
```

### 数据库的修改、删除、使用

```mysql
-- 修改数据库db4的字符集为utf8
ALTER DATABASE db4 CHARACTER SET utf8;

-- 查看db4数据库的字符集
SHOW CREATE DATABASE db4;

-- 删除db1数据库
DROP DATABASE db1;

-- 删除数据库db2,如果存在，则删除
DROP DATABASE IF EXISTS db2;

-- 使用db4数据库
USE db4;

-- 查询当前正在使用的数据库
SELECT DATABASE();
```

### 查询数据表

```mysql
-- 使用mysql数据库
USE mysql;

-- 查询库中所有的表
SHOW TABLES;

-- 查询user表结构
DESC USER;

-- 查看mysql数据库中user表字符集
SHOW TABLE STATUS FROM mysql LIKE 'user';
```

### 数据表的创建

```mysql
-- 创建一个product商品表（商品编号、商品名称、商品价格、商品库存、上架时间）
CREATE TABLE product(
	id INT,
	NAME VARCHAR(20),
	price DOUBLE,
	stock INT,
	insert_time DATE
);
-- 查看product表详细结构
DESC product;
```

### 数据表的修改

```mysql
-- 修改product表名为product2
ALTER TABLE product RENAME TO product2;

-- 查看db3数据库中product2数据表字符集
SHOW TABLE STATUS FROM db3 LIKE 'product2';

-- 修改product2数据表字符集
 ALTER TABLE product2 CHARACTER SET gbk;
 
 -- 给product2表添加一列color
 ALTER TABLE product2 ADD color VARCHAR(10);

-- 将color数据类型修改为int
ALTER TABLE product2 MODIFY color INT;
-- 查看product2表的详细结构
DESC product2;

-- 将color修改为address
ALTER TABLE product2 CHANGE color address VARCHAR(200);

-- 删除address列
ALTER TABLE product2 DROP address;
```

### 删除数据表

```mysql
-- 删除数据表
DROP product2;
-- 删除数据表（如果表存在就删除）
DROP TABLE IF EXISTS product2; 
```

### 新增表数据

```mysql
-- 向product表添加一条数据
INSERT INTO product (id,NAME,price,stock,insert_time) VALUE (1,'手机',1560.56,200,'2021-1-26');

-- 向product表添加指定列数据
INSERT INTO product(id,NAME,price) VALUE (2,'电脑',5999.99);

-- 默认给全部列添加数据
INSERT INTO product VALUE(3,'电视',2000.52,150,'2021-01-15');

-- 批量添加数据
INSERT INTO product VALUE(4,'洗衣机',1502,100,'2022-05-03'),(5,'电冰箱',1814,520,'2012-08-15');
```

### 修改和删除表数据

```mysql
-- 修改手机的价格为3500元
UPDATE product SET price=3500 WHERE NAME='手机'; 

-- 修改电脑的价格为5600元，库存为150个
UPDATE product SET price=5600,stock=150 WHERE NAME='电脑';

-- 删除表中电冰箱的信息
DELETE FROM product WHERE NAME='电冰箱';

-- 删除表中库存为150的数据的信息
DELETE FROM product WHERE stock='150';
```

### 查询数据表准备

```mysql
-- 创建db1数据库
CREATE DATABASE db1;

-- 使用db1数据库
USE db1; 

-- 创建数据表
CREATE TABLE product(
	id INT,			-- 商品编号
	NAME VARCHAR(20),	-- 商品名称
	price DOUBLE,		-- 商品价格
	brand VARCHAR(10),	-- 商品品牌
	stock INT,		-- 商品库存
	insert_time DATE 	-- 添加时间
);

-- 添加数据
INSERT INTO product VALUES
(1,'华为手机',3999,'华为',23,'2999-05-20'),
(2,'苹果手机',2999,'苹果',15,'2999-04-15'),
(3,'小米手机',4999,'小米',66,'2999-12-18'),
(4,'华为电脑',5999,'华为',52,'2999-05-30'),
(5,'苹果电脑',6999,'苹果',12,'2999-03-25'),
(6,'小米电脑',1999,'小米',35,'2999-11-19'),
(7,'联想电脑',7999,'联想',NULL,'2999-08-08');
```

### 查询全部

```mysql
-- 查询product中全部的数据
SELECT * FROM product;

-- 查询指定列的数据
SELECT NAME,price,insert_time FROM product;

-- 查询品牌
SELECT brand FROM product;
-- 查询品牌，去除重复
SELECT DISTINCT brand FROM product;

-- 查询商品名称和库存，库存在原有基础上加10
SELECT NAME,stock+10 FROM product;
-- 查询商品名称和库存，库存在原有基础上加10,进行null值判断
SELECT NAME,IFNULL(stock,0)+10 FROM product;
-- 查询商品名称和库存，库存在原有基础上加10,进行null值判断,起别名
SELECT NAME,IFNULL(stock,0)+10 AS getNum FROM product;
```

### 条件查询

```mysql
-- 查询库存大于20的商品信息
SELECT * FROM product WHERE stock > 20;

-- 查询品牌为华为的商品信息
SELECT * FROM product WHERE brand = '华为';

-- 查询金额在4000~6000之间的商品信息
SELECT * FROM product WHERE price >= 4000 AND price =< 6000;
SELECT * FROM product WHERE price BETWEEN 4000 AND 6000;

-- 查询库存为12,35,66的商品信息
SELECT * FROM product WHERE stock = 12 OR stock=35 OR stock=66;
SELECT * FROM product WHERE stock IN(12,35,66);

-- 查询库存为null的商品信息
SELECT * FROM product WHERE stock IS NULL;

-- 查询库存不为null的商品信息
SELECT * FROM product WHERE stock IS NOT NULL;

-- 查询名称以小米为开头的商品信息
SELECT * FROM product WHERE NAME LIKE '小米%';

-- 查询名称第二个是为的商品信息
SELECT * FROM product WHERE NAME LIKE '_为%';

-- 查询名称为四个字符的商品信息
SELECT * FROM product WHERE NAME LIKE '____';

-- 查询名称中包含电脑的商品信息
SELECT * FROM product WHERE NAME LIKE '%电脑%';

```

### 聚合函数查询

```mysql
-- 计算product表中总记录条数
SELECT COUNT(*) FROM product;

-- 获取最高价格
SELECT MAX(price) FROM product;

-- 获取最低库存
SELECT MIN(stock) FROM product;

-- 获取总库存数量
SELECT SUM(stock) FROM product;

-- 获取品牌为苹果的总库存数量
SELECT SUM(stock) FROM product WHERE brand='苹果'; 

-- 获取品牌为小米的平均商品价格
SELECT AVG(price) FROM product WHERE brand='小米';
```

### 排序查询

```mysql
-- 按照库存升序排序
SELECT * FROM product ORDER BY stock ASC;

-- 查询名称中包含手机的商品信息。按照金额降序排序
-- 先过滤条件再进行排序
SELECT * FROM product WHERE NAME LIKE '%手机%' ORDER BY price DESC;

-- 按照金额升序排序，如果金额相同，按照库存降序排序
-- 只有当排序主要条件相同的时候，才会执行次要排序条件
SELECT * FROM product ORDER BY price ASC,stock DESC;
```

### 分组查询

```mysql
-- 按照品牌分组，获取每组的总金额
SELECT brand,SUM(price) FROM product GROUP BY brand;

-- 对金额大于4000的商品，按照品牌分组，获取每组商品的总金额
SELECT brand,SUM(price) FROM product
WHERE price > 4000 
GROUP BY brand;

-- 对金额大于4000的商品，按照品牌分组，获取每组商品的总金额，只显示总金额大于7000元的
SELECT brand,SUM(price) AS getSum FROM product 
WHERE price>4000 
GROUP BY brand 
HAVING getSum > 7000;

-- 对金额大于4000的商品，按照品牌分组，获取每组商品的总金额，只显示总金额大于7000元的,并按照总金额的降序排列
SELECT brand,SUM(price) AS getSum FROM product
WHERE price>4000 
GROUP BY brand 
HAVING getSum > 7000 
ORDER BY getSum DESC;
```

### 分页查询

```mysql
-- LIMIT 当前页数，每页显示的条数
-- 公式：当前页数 = （当前页数 - 1）* 每页显示的条数
-- 每页显示3条

-- 第一页：当前页数 = （1 - 1）* 3 = 0
SELECT * FROM product LIMIT 0,3;

-- 第二页：当前页数 = （2 - 1）* 3 = 3
SELECT * FROM product LIMIT 3,3;

-- 第三页：当前页数 = （3 - 1）* 3 = 6
SELECT * FROM product LIMIT 6,3;
```

