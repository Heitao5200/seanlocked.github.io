# 数据库的基本使用

本章节主要记录三种数据库的使用MySQL、mongoDB、Redis



## 1.MySQL

使用的话直接看基础语法即可

需要练习的朋友可以直接跳转我下面的连接，我之前整理我在自学sql网上的题目答案，不用配环境直接就可以做题很方便

https://zhuanlan.zhihu.com/p/348330806

### 1.1SQL书写规范

在写SQL语句时，要求按照如下规范进行：

+ SQL 语句要以分号（;）结尾
+ SQL 不区分关键字的大小写 ，这对于表名和列名同样适用。
+ 插入到表中的数据是区分大小写的。例如，数据Computer、COMPUTER 或computer，三者是不一样的。
+ 常数的书写方式是固定的，在SQL 语句中直接书写的字符串、日期或者数字等称为常数。常数的书写方式如下所示。

  + SQL 语句中含有字符串的时候，需要像'abc'这样，使用单引号（'）将字符串括起来，用来标识这是一个字符串。
  + SQL 语句中含有日期的时候，同样需要使用单引号将其括起来。日期的格式有很多种（'26 Jan 2010' 或者'10/01/26' 等）。
  + 在SQL 语句中书写数字的时候，不需要使用任何符号标识，直接写成1000 这样的数字即可。
+ 单词之间需要用半角空格或者换行来分隔。
+ SQL中的注释主要采用`--`和`/* ... */`的方式，第二种方式可以换行。在MySQL下，还可以通过`#`来进行注释。



### 1.2 命名规则

+ 在数据库中，只能使用半角英文字母、数字、下划线（_）作为数据库、表和列的名称 。
+ 名称必须以半角英文字母作为开头。
+ 名称不能重复，同一个数据库下不能有2张相同的表。



### 1.3. 数据类型

MySQL 支持所有标准 SQL 数值数据类型，包括：

#### （1）数值类型

数值包含的类型如下：

+ 整型数据：`TINYINT`、`INTEGER`、`SMALLINT`、`MEDIUMINT`、`DECIMAL` 、`NUMERIC` 和`BIGINT`。

+ 浮点型数据：`DECIMAL`、`FLOAT`、`REAL` 和 `DOUBLE PRECISION`)。

其中，关键字`INT`是`INTEGER`的同义词，关键字DEC是的同义词。

不同关键字的主要区别就是表示的范围或精度不一样。具体如下表：

|     类型     | 大小                                     |                        范围（有符号）                        | 范围（无符号）                                               | 用途            |
| :----------: | :--------------------------------------- | :----------------------------------------------------------: | :----------------------------------------------------------- | :-------------- |
|   TINYINT    | 1 Bytes                                  |                         (-128，127)                          | (0，255)                                                     | 小整数值        |
|   SMALLINT   | 2 Bytes                                  |                      (-32 768，32 767)                       | (0，65 535)                                                  | 大整数值        |
|  MEDIUMINT   | 3 Bytes                                  |                   (-8 388 608，8 388 607)                    | (0，16 777 215)                                              | 大整数值        |
| INT或INTEGER | 4 Bytes                                  |               (-2 147 483 648，2 147 483 647)                | (0，4 294 967 295)                                           | 大整数值        |
|    BIGINT    | 8 Bytes                                  |   (-9,223,372,036,854,775,808，9 223 372 036 854 775 807)    | (0，18 446 744 073 709 551 615)                              | 极大整数值      |
|    FLOAT     | 4 Bytes                                  | (-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38) | 0，(1.175 494 351 E-38，3.402 823 466 E+38)                  | 单精度 浮点数值 |
|    DOUBLE    | 8 Bytes                                  | (-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 双精度 浮点数值 |
|   DECIMAL    | 对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2 |                        依赖于M和D的值                        | 依赖于M和D的值                                               | 小数值          |

#### （2）日期和时间类型

表示时间值的日期和时间类型为`DATETIME`、`DATE`、`TIMESTAMP`、`TIME`和`YEAR`。具体如下表：

| 类型      | 大小 ( bytes) |                             范围                             | 格式                  | 用途                     |
| --------- | :------------ | :----------------------------------------------------------: | :-------------------- | :----------------------- |
| DATE      | 3             |                    1000-01-01/9999-12-31                     | YYYY-MM-DD            | 日期值                   |
| TIME      | 3             |                 '-838: 59: 59'/'838: 59: 59'                 | HH: MM: SS            | 时间值或持续时间         |
| YEAR      | 1             |                          1901/2155                           | YYYY                  | 年份值                   |
| DATETIME  | 8             |         1000-01-01 00: 00: 00/9999-12-31 23: 59: 59          | YYYY-MM-DD HH: MM: SS | 混合日期和时间值         |
| TIMESTAMP | 4             | 1970-01-01 00: 00: 00/2038结束时间是第 2147483647 秒，北京时间 2038-1-19 11: 14: 07，格林尼治时间 2038年1月19日 凌晨 03: 14: 07 | YYYYMMDD HHMMSS       | 混合日期和时间值，时间戳 |

#### （3）字符串类型

字符串类型指`CHAR`、`VARCHAR`、`BINARY`、`VARBINARY`、`BLOB`、`TEXT`、`ENUM`和`SET`。具体如下表：

| 类型       | 大小                  | 用途                            |
| :--------- | :-------------------- | :------------------------------ |
| CHAR       | 0-255 bytes           | 定长字符串                      |
| VARCHAR    | 0-65535 bytes         | 变长字符串                      |
| TINYBLOB   | 0-255 bytes           | 不超过 255 个字符的二进制字符串 |
| TINYTEXT   | 0-255 bytes           | 短文本字符串                    |
| BLOB       | 0-65 535 bytes        | 二进制形式的长文本数据          |
| TEXT       | 0-65 535 bytes        | 长文本数据                      |
| MEDIUMBLOB | 0-16 777 215 bytes    | 二进制形式的中等长度文本数据    |
| MEDIUMTEXT | 0-16 777 215 bytes    | 中等长度文本数据                |
| LONGBLOB   | 0-4 294 967 295 bytes | 二进制形式的极大文本数据        |
| LONGTEXT   | 0-4 294 967 295 bytes | 极大文本数据                    |

+ `char`声明的是定长字符串。若实际中字符串长度不足，则会在末尾使用空格进行填充至声明的长度。

+ `varchar`声明的是可变长字符串。存储过程中，只会按照字符串的实际长度来存储，但会多占用一位来存放实际字节的长度。



### 1.4基础语法

#### 1.4.1 数据库基本操作

##### 1.4.1.1 数据库的创建 

通过`CREATE`命令，可以创建指定名称的数据库，语法结构如下：

```mysql
CREATE DATABASE [IF NOT EXISTS] <数据库名称>;
```

MySQL 的数据存储区将以目录方式表示 MySQL 数据库，因此数据库名称必须符合操作系统的文件夹命名规则，不能以数字开头，尽量要有实际意义。

MySQL下不运行存在两个相同名字的数据库，否则会报错。如果使用`IF NOT EXISTS`（可选项），可以避免此类错误。

##### 1.4.1.2 数据库的查看

1. 查看所有存在的数据库

```MYSQL
SHOW DATABASES [LIKE '数据库名'];
```

`LIKE`从句是可选项，用于匹配指定的数据库名称。`LIKE` 从句可以部分匹配，也可以完全匹配。

##### 1.4.1.3 查看创建的数据库

```mysql
SHOW CREATE DATABASE <数据库名>;
SHOW CREATE DATABASE <数据库名> \G
```

##### 1.4.1.3 选择数据库

在操作数据库前，必须指定所要操作的数据库。通过`USE`命令，可以切换到对应的数据库下。

```mysql
USE <数据库名>
```

##### 1.4.1.4 删除数据库

通过`DROP`命令，可以将相应数据库进行删除。

```mysql
DROP DATABASE [IF EXISTS] <数据库名>
```

其中，`IF EXISTS`为可选性，用于防止数据库不存在时报错。



#### 1.4.2 表的基本操作

##### 1.4.2.1 表的创建

创建表的语法结构如下：

```mysql
CREATE TABLE <表名> （<字段1> <数据类型> <该列所需约束>,
   <字段2> <数据类型> <该列所需约束>,
   <字段3> <数据类型> <该列所需约束>,
   <字段4> <数据类型> <该列所需约束>,
   ...
   <该表的约束1>， <该表的约束2>，……）;
   
#示例
-- 创建一个名为Product的表
CREATE TABLE Product(
  product_id CHAR(4) NOT NULL,
  product_name VARCHAR(100) NOT NULL,
  product_type VARCHAR(32) NOT NULL,
  sale_price INT,
  purchase_price INT,
  regist_date DATE,
  PRIMARY KEY (product_id)
);
```

简单介绍一下该语句中出现的约束条件，约束条件在后面会详细介绍：

+ `PRIMARY KEY`：主键，表示该字段对应的内容唯一且不能为空。
+ `NOT NULL`：在 `NULL` 之前加上了表示否定的` NOT`，表示该字段不能输入空白。

通过`SHOW TABLES`命令来查看当前数据库下的所有的表名：

```mysql
SHOW TABLES;

-- 结果如下
+----------------+
| Tables_in_shop |
+----------------+
| Product        |
+----------------+
1 rows in set (0.00 sec)
```

通过`DESC <表名>`来查看表的结构：

```mysql
DESC Product;

-- 结果如下
+----------------+--------------+------+-----+---------+-------+
| Field          | Type         | Null | Key | Default | Extra |
+----------------+--------------+------+-----+---------+-------+
| product_id     | char(4)      | NO   | PRI | NULL    |       |
| product_name   | varchar(100) | NO   |     | NULL    |       |
| product_type   | varchar(32)  | NO   |     | NULL    |       |
| sale_price     | int          | YES  |     | NULL    |       |
| purchase_price | int          | YES  |     | NULL    |       |
| regist_date    | date         | YES  |     | NULL    |       |
+----------------+--------------+------+-----+---------+-------+
6 rows in set (0.00 sec)
```

##### 1.4.2.2 表的删除

删除表的语法结构如下：

```mysql
DROP TABLE <表名>;

-- 例如：DROP TABLE Product;
```

说明：通过`DROP`删除的表示无法恢复的，在删除表的时候请谨慎。

##### 1.4.2.3 表的更新

通过`ALTER TABLE`语句，我们可以对表字段进行不同的操作，下面通过示例来具体学习用法。

示例：

1. 创建一张名为Student的表

```mysql
CREATE TABLE Student(
  id INT PRIMARY KEY,
  name CHAR(15)
);
```



```mysql
DESC student;

-- 结果如下
+-------+----------+------+-----+---------+-------+
| Field | Type     | Null | Key | Default | Extra |
+-------+----------+------+-----+---------+-------+
| id    | int      | NO   | PRI | NULL    |       |
| name  | char(15) | YES  |     | NULL    |       |
+-------+----------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```

2. 更改表名

   通过`RENAME`命令，将表名从Student => Students。

```mysql
ALTER TABLE Student RENAME Students;
```

3. 插入新的字段

   通过`ADD`命令，新增字段sex和age。

```mysql
-- 不同的字段通过逗号分开
ALTER TABLE Students ADD sex CHAR(1), ADD age INT;
```

​		其它插入技巧：

```mysql
-- 通过FIRST在表首插入字段stu_num
ALTER TABLE Students ADD stu_num INT FIRST;

-- 指定在字段sex后插入字段height
ALTER TABLE Students ADD height INT AFTER sex;
```

```mysql
DESC Students;

-- 结果如下
+---------+----------+------+-----+---------+-------+
| Field   | Type     | Null | Key | Default | Extra |
+---------+----------+------+-----+---------+-------+
| stu_num | int      | YES  |     | NULL    |       |
| id      | int      | NO   | PRI | NULL    |       |
| name    | char(15) | YES  |     | NULL    |       |
| sex     | char(1)  | YES  |     | NULL    |       |
| height  | int      | YES  |     | NULL    |       |
| age     | int      | YES  |     | NULL    |       |
+---------+----------+------+-----+---------+-------+
6 rows in set (0.00 sec)
```

4. 字段的删除

   通过`DROP`命令，可以对不在需要的字段进行删除。

```mysql
-- 删除字段stu_num
ALTER TABLE Students DROP stu_num;
```

5. 字段的修改

   通过`MODIFY`修改字段的数据类型。

```mysql
-- 修改字段age的数据类型
ALTER TABLE Students MODIFY age CHAR(3);
```

​		通过`CHANGE`命令，修改字段名或类型

```mysql
-- 修改字段name为stu_name，不修改数据类型
ALTER TABLE Students CHANGE name stu_name CHAR(15);

-- 修改字段sex为stu_sex，数据类型修改为int
ALTER TABLE Students CHANGE sex stu_sex INT;
```

```mysql
DESC Students;

-- 结果如下
+----------+----------+------+-----+---------+-------+
| Field    | Type     | Null | Key | Default | Extra |
+----------+----------+------+-----+---------+-------+
| id       | int      | NO   | PRI | NULL    |       |
| stu_name | char(20) | YES  |     | NULL    |       |
| stu_sex  | int      | YES  |     | NULL    |       |
| height   | int      | YES  |     | NULL    |       |
| age      | char(3)  | YES  |     | NULL    |       |
+----------+----------+------+-----+---------+-------+
5 rows in set (0.00 sec)
```

##### 1.4.2.4 表的查询

通过`SELECT`语句，可以从表中取出所要查看的字段的内容：

```mysql
SELECT <字段名>, ……
 FROM <表名>;
```

如要直接查询表的全部字段：

```mysql
SELECT *
 FROM <表名>;
```

其中，**星号（*）**代表全部字段的意思。

示例：

1. 建表并插入数据

   在MySQL中，我们通过`INSERT`语句往表中插入数据，该语句在后面会详细介绍，该小节的重点是学会使用`SELECT`。

```mysql
-- 向Product表中插入数据
INSERT INTO Product VALUES
  ('0001', 'T恤衫', '衣服', 1000, 500, '2009-09-20'),
  ('0002', '打孔器', '办公用品', 500, 320, '2009-09-11'),
  ('0003', '运动T恤', '衣服', 4000, 2800, NULL),
  ('0004', '菜刀', '厨房用具', 3000, 2800, '2009-09-20'),
  ('0005', '高压锅', '厨房用具', 6800, 5000, '2009-01-15'),
  ('0006', '叉子', '厨房用具', 500, NULL, '2009-09-20'),
  ('0007', '擦菜板', '厨房用具', 880, 790, '2008-04-28'),
  ('0008', '圆珠笔', '办公用品', 100, NULL,'2009-11-11')
 ;
```

2. 查看表的内容

```mysql
-- 查看表的全部内容
SELECT * 
 FROM Product;
```

```mysql
-- 查看部分字段包含的内容
SELECT 
  product_id,
  product_name,
  sale_price 
 FROM Product;
```

3. 对查看的字段从新命名

   通过`AS`语句对展示的字段另起别名，这不会修改表内字段的名字。

```mysql
SELECT 
  product_id AS ID,
  product_type AS TYPE
 FROM Product;
```

​		设定汉语别名时需要使用双引号（"）括起来，英文字符则不需要。

```Mysql
SELECT  
  product_id AS "产品编号",
  product_type AS "产品类型"  
 FROM Product;
```

4. 常数的查询

   `SELECT`子句中，除了可以写字段外，还可以写常数。

```mysql
SELECT 
  '商品' AS string,
  '2009-05-24' AS date,
  product_id,
  product_name
 FROM Product;
```

5. 删除重复行

   在`SELECT`语句中使用`DISTINCT`可以去除重复行。

```mysql
SELECT 
  DISTINCT regist_date 
 FROM Product;
```

​		在使用`DISTINCT` 时，`NULL `也被视为一类数据。`NULL `存在于多行中时，会被合并为一条`NULL `数据。

​		还可以通过组合使用，来去除列组合重复的数据。`DISTINCT `关键字只能用在第一个列名之前。

```mysql
SELECT 
  DISTINCT product_type, regist_date
 FROM Product;
```

6. 指定查询条件

   首先通过`WHERE` 子句查询出符合指定条件的记录，然后再选取出` SELECT `语句指定的列，语法结构如下：

```mysql
SELECT <字段名>, ……
  FROM <表名>
 WHERE <条件表达式>;
```

​		示例：

```mysql
SELECT product_name
  FROM Product
 WHERE product_type = '衣服';
```

注意，`WHERE`子句要紧跟在`FROM`子句之后。

##### 1.4.2.5 表的复制

表的复制可以将表结构与表中的数据全部复制，或者只复制表的结构。

```mysql
-- 将整个表复制过来
CREATE TABLE Product_COPY1
	SELECT * FROM Product;

SELECT * FROM Product_COPY1;
```

```mysql
-- 通过LIKE复制表结构
CREATE TABLE Product_COPY2
	LIKe Product;

SELECT * FROM Product_COPY2;

-- 结果如下
Empty set (0.00 sec)  -- 表为空的

DESC Product_COPY2;

-- 结果如下
-- 表结构已复制过来
```

#### 1.4.3 运算符

##### 1.4.3.1 算术运算符

我们可以在`SELECT`语句中使用计算表达式：

```mysql
SELECT 
  product_name,
  sale_price,
  sale_price * 2 AS "sale_price_x2"
 FROM Product;
 
-- 结果如下
+--------------+------------+---------------+
| product_name | sale_price | sale_price_x2 |
+--------------+------------+---------------+
| T恤衫        |       1000 |          2000 |
| 打孔器       |        500 |          1000 |
| 运动T恤      |       4000 |          8000 |
| 菜刀         |       3000 |          6000 |
| 高压锅       |       6800 |         13600 |
| 叉子         |        500 |          1000 |
| 擦菜板       |        880 |          1760 |
| 圆珠笔       |        100 |           200 |
+--------------+------------+---------------+
```

+ 四则运算所使用的运算符**(+、-、*、/）**称为算术运算符。

+ 在运算表达式中，也可以使用**()**，括号中的运算表达式优先级会得到提升。

+ **NULL**的计算结果，仍然还是**NULL**。

##### 1.4.3.2 比较运算符

在 `WHERE` 子句中通过使用比较运算符可以组合出各种各样的条件表达式。

```mysql
SELECT product_name, product_type
  FROM Product
 WHERE sale_price = 500;
```

常见比较运算符如下表：

| 运算符 | 含义     |
| ------ | -------- |
| =      | 相等     |
| <>     | 不相等   |
| \>=    | 大于等于 |
| \>     | 大于     |
| <=     | 小于等于 |
| <      | 小于     |

+ 不能对**NULL**使用任何比较运算符，只能通过`IS NULL`语句来判断：

```mysql
SELECT 
   product_name,
   purchase_price
  FROM Product
 WHERE purchase_price IS NULL;
```

​		希望选取不是 NULL 的记录时，需要使用`IS NOT NULL`运算符。

+ 对字符串使用比较符

​        MySQL中字符串的排序与数字不同，典型的规则就是按照字典顺序进行比较，也就是像姓名那样，按照条目在字典中出现的顺序来进行排序。例如：

```mysql
'1'  < '10' < '11' < '2' < '222' < '3'
```



##### 1.4.3.3 逻辑运算符

1. 使用`NOT`否认某一条件：

```mysql
SELECT 
  product_name,
  product_type,
  sale_price
  FROM Product
 WHERE NOT sale_price >= 1000;
```

2. `AND`运算符合`OR`运算符

```mysql
SELECT product_type, sale_price
    FROM Product
	WHERE product_type = '厨房用具' 
	AND sale_price >= 3000;
```

```mysql
SELECT product_type, sale_price
		FROM Product
	WHERE product_type = '厨房用具'
	OR sale_price >= 3000;
```

3. 逻辑运算符和真值

+ 符**NOT**、**AND** 和 **OR** 称为逻辑运算符；
+ 真值就是值为**真（TRUE）**或**假 （FALSE）**；

+ 在查询**NULL**时，SQL中存在第三种真值，**不确定（UNKNOWN）**，**NULL**和任何值做逻辑运算结果都是不确定；
+ 考虑 **NULL** 时的条件判断也会变得异常复杂，因此尽量给字段加上**NOT NULL**的约束。

#### 1.4.4 分组查询

##### 1.4.4.1 聚合函数

通过 SQL 对数据进行某种操作或计算时需要使用函数。

+ `COUNT`：计算表中的记录数（行数）

+ `SUM`： 计算表中数值列中数据的合计值

+ `AVG`： 计算表中数值列中数据的平均值

+ `MAX`： 求出表中任意列中数据的最大值

+ `MIN`： 求出表中任意列中数据的最小值

示例：

```mysql
-- 计算全部数据的行数
SELECT COUNT(*) FROM Product;

-- 结果如下
+----------+
| COUNT(*) |
+----------+
|        8 |
+----------+
1 row in set (0.00 sec)
```

**注意点1**：除了`COUNT`可以将`*`作为参数，其它的函数均不可以。

```mysql
-- 计算最高的销售价格
SELECT MAX(sale_price) FROM Product;

-- 结果如下
+-----------------+
| MAX(sale_price) |
+-----------------+
|          680000 |
+-----------------+
1 row in set (0.00 sec)
```

**注意点2：**当将字段名作为参数传递给函数时，只会计算不包含`NULL`的行。

示例：

```mysql
-- purchase_price字段是包含NULL值的
SELECT purchase_price FROM Product;

-- 结果如下
+----------------+
| purchase_price |
+----------------+
|            500 |
|            320 |
|           2800 |
|            700 |
|           1250 |
|           NULL |
|            198 |
|           NULL |
+----------------+
8 rows in set (0.00 sec)
```

以*为参数传递给`COUNT`函数

```mysql
SELECT COUNT(*) FROM Product;

-- 结果如下
+----------+
| COUNT(*) |
+----------+
|        8 |
+----------+
1 row in set (0.00 sec)
```

以purchase_price为参数传递给`COUNT`函数

```mysql
SELECT COUNT(purchase_price) FROM Product;

-- 结果如下
+-----------------------+
| COUNT(purchase_price) |
+-----------------------+
|                     6 |
+-----------------------+
1 row in set (0.00 sec)
```

可以看到结果并不一样，函数忽略了值为**NULL**的行。

`SUM`，`AVG`函数时也一样，计算时会直接忽略，**并不会当做0来处理！**特别注意`AVG`函数，计算时分母也不会算上`NULL`行。

**注意点3**：`MAX/MIN`函数几乎适用于所有数据类型的列，包括字符和日期。`SUM/AVG`函数只适用于数值类型的列。

**注意点4**：在聚合函数删除重复值

```mysql
SELECT COUNT(DISTINCT product_type)
 FROM Product;
 
-- 结果如下
+------------------------------+
| COUNT(DISTINCT product_type) |
+------------------------------+
|                            3 |
+------------------------------+
1 row in set (0.01 sec)
```

`DISTINCT`必须写在括号中。这是因为必须要在计算行数之前删除 product_type 字段中的重复数据。



##### 1.4.4.2 对表分组

如果对Python的Pandas熟悉，那么大家应该很了解`groupby`函数，可以根据指定的列名，对表进行分组。在MySQL中，也存在同样作用的函数，即`GROUP BY`。

语法结构如下：

```mysql
SELECT <列名1>, <列名2>, <列名3>, ……
 FROM <表名>
 GROUP BY <列名1>, <列名2>, <列名3>, ……;
```

示例：

```mysql
SELECT product_type, COUNT(*)
 FROM Product
 GROUP BY product_type;
 
-- 结果如下
+--------------+----------+
| product_type | COUNT(*) |
+--------------+----------+
| 衣服         |        2 |
| 办公用品     |        2 |
| 厨房用具     |        4 |
+--------------+----------+
3 rows in set (0.01 sec)
```

1. 在该语句中，我们首先通过`GROUP BY`函数对指定的字段product_type进行分组。分组时，product_type字段中具有相同值的行会汇聚到同一组。

2. 最后通过`COUNT`函数，统计不同分组的包含的行数。

简单来理解：

+ 例如做操时，老师将不同身高的同学进行分组，相同身高的同学会被分到同一组，分组后我们又统计了每个小组的学生数。

+ 将这里的同学可以理解为表中的一行数据，身高理解为表的某一字段。
+ 分组操作就是`GROUP BY`，`GROUP BY`后面接的字段等价于按照身高分组，统计学生数就等价于在`SELECT`后用了`COUNT(*)`函数。

注意：`GROUP BY `子句的位置一定要写在`FROM` 语句之后（如果有 `WHERE` 子句的话需要写在 `WHERE` 子句之后）

```
1. SELECT → 2. FROM → 3. WHERE → 4. GROUP BY
```

当被聚合的键中，包含`NULL`时，在结果中会以“不确定”行（空行）的形式表现出来，也就是字段中为`NULL`的数据会被聚合为一组。

##### 1.4.4.3 使用WHERE语句

在对表进行分组之前，也可以是先使用`WHERE`对表进行条件过滤，然后再进行分组处理。语法结构如下：

```mysql
SELECT <列名1>, <列名2>, <列名3>, ……
 FROM <表名>
 WHERE 
 GROUP BY <列名1>, <列名2>, <列名3>, ……;
```

示例：

```mysql
-- WHERE语句先将表中类型为衣服的行筛选出来
-- 然后再按照purchase_price来进行分组
SELECT purchase_price, COUNT(*)
 FROM Product
 WHERE product_type = '衣服'
 GROUP BY purchase_price;
 
-- 结果如下
+----------------+----------+
| purchase_price | COUNT(*) |
+----------------+----------+
|            500 |        1 |
|           2800 |        1 |
+----------------+----------+
2 rows in set (0.01 sec)
```

该语法实际的执行顺序为：

```
FROM → WHERE → GROUP BY → SELECT
```

+ 使用`GROUP BY`子句时，`SELECT`子句中不能出现聚合键之外的字段名。即，若`GROUP BY`选中purchase_price字段进行分组，则在`SELECT`语句中只能选中purchase_price字段，其它字段如product_id等均不行。
+ `WHERE`语句中，不可以使用聚合函数。`WHERE`子句只能指定记录（行）的条件，而不能用来指定组的条件。即`WHERE MAX(purchase_price) > 1000`这样的语句是非法的。



##### 1.4.4.4 为聚合结果指定条件

前面提到了`WHERE`语句中不能使用聚合函数，但是实际操作时需要通过聚合函数来进行过滤怎么办呢？这就要用到`HAVING`语句了。语法结构如下：

```mysql
SELECT <列名1>, <列名2>, <列名3>, ……
 FROM <表名>
 GROUP BY <列名1>, <列名2>, <列名3>, ……
HAVING <分组结果对应的条件>
```

在`HAVING`的子句中能够使用的 3 种要素如下所示：

● 常数

● 聚合函数

●  `GROUP BY`子句中指定的字段名（即聚合键）

示例：

```mysql
-- 不使用HAVING语句
SELECT product_type, AVG(sale_price)
 FROM Product
 GROUP BY product_type;
 
-- 结果如下
+--------------+-----------------+
| product_type | AVG(sale_price) |
+--------------+-----------------+
| 衣服         |       2500.0000 |
| 办公用品     |        300.0000 |
| 厨房用具     |     279500.0000 |
+--------------+-----------------+
3 rows in set (0.00 sec)
```

```mysql
-- 使用HAVING语句
-- 通过HAVING语句将销售平均价格大于等于2500的组给保留了
SELECT product_type, AVG(sale_price)
 FROM Product
 GROUP BY product_type
HAVING AVG(sale_price) >= 2500;

-- 结果如下
+--------------+-----------------+
| product_type | AVG(sale_price) |
+--------------+-----------------+
| 衣服         |       2500.0000 |
| 厨房用具     |     279500.0000 |
+--------------+-----------------+
2 rows in set (0.00 sec)
```

可以看到使用`HAVING`语句后，输出的结果有所变化。大致流程如下：

+ 首先，`FROM`语句会选中表Product；
+ 然后，`GROUP BY`语句会选中字段product_type进行分组；
+ 之后，通过`HAVING`语句将销售平均价格大于等于2500的组保留下来；
+ 最后，通过`SELECT`语句将保留下的组的产品类型和平均价格显示出来；



如果是对**表的行**进行条件指定，`WHERE`和`HAVING`都可以生效。

```mysql
-- 下面两条语句执行结果一致
SELECT product_type, COUNT(*)
  FROM Product
  GROUP BY product_type
 HAVING product_type = '衣服';

SELECT product_type, COUNT(*)
  FROM Product
  WHERE product_type = '衣服'
 GROUP BY product_type;
 
-- 结果如下
+--------------+----------+
| product_type | COUNT(*) |
+--------------+----------+
| 衣服         |        2 |
+--------------+----------+
1 row in set (0.01 sec)
```

但是，一般而言如果是对表的行进行条件指定，最好还是使用`WHERE`语句，因为`WHERE`的执行速度更快。



##### 1.4.4.5 对表的查询结果进行排序

如果希望对表的查询结果根据某指定的字段进行排序，可以使用`ORDER BY`语句。语法结构如下：

```mysql
SELECT <列名1>, <列名2>, <列名3>, ……
 FROM <表名>
 ORDER BY <排序基准列1>, <排序基准列2>, ……
```

示例：

```mysql
SELECT product_id, product_name, sale_price, purchase_price
 FROM Product;
```

```mysql
-- 根据字段sale_price的值进行排序
SELECT product_id, product_name, sale_price, purchase_price
 FROM Product
ORDER BY sale_price;

-- 结果如下
+------------+--------------+------------+----------------+
| product_id | product_name | sale_price | purchase_price |
+------------+--------------+------------+----------------+
| 0008       | 圆珠笔       |        100 |           NULL |
| 0002       | 打孔器       |        500 |            320 |
| 0001       | T恤衫        |       1000 |            500 |
| 0003       | 运动T恤      |       4000 |           2800 |
| 0006       | 叉子         |      50000 |           NULL |
| 0007       | 擦菜板       |      88000 |            198 |
| 0004       | 菜刀         |     300000 |            700 |
| 0005       | 高压锅       |     680000 |           1250 |
+------------+--------------+------------+----------------+
8 rows in set (0.00 sec)
```

可以看到`ORDER BY`默认是按照升序的方式进行排序的，正式的书写方式应该是在字段后加上关键字`ASC`，即`ORDER BY sale_price ASC`。

如果我们希望按照降序的方式，可以通过`DESC`关键词进行指定。

```mysql
SELECT product_id, product_name, sale_price, purchase_price
 FROM Product
ORDER BY sale_price DESC;

-- 结果如下
+------------+--------------+------------+----------------+
| product_id | product_name | sale_price | purchase_price |
+------------+--------------+------------+----------------+
| 0005       | 高压锅       |     680000 |           1250 |
| 0004       | 菜刀         |     300000 |            700 |
| 0007       | 擦菜板       |      88000 |            198 |
| 0006       | 叉子         |      50000 |           NULL |
| 0003       | 运动T恤      |       4000 |           2800 |
| 0001       | T恤衫        |       1000 |            500 |
| 0002       | 打孔器       |        500 |            320 |
| 0008       | 圆珠笔       |        100 |           NULL |
+------------+--------------+------------+----------------+
8 rows in set (0.00 sec)
```

前面展示了指定一个字段来对表进行排序，实际上我们可以指定多个字段来进行排序。

示例：

```mysql
SELECT regist_date, product_id, sale_price, purchase_price
 FROM Product
ORDER BY regist_date, product_id;

-- 结果如下
+-------------+------------+------------+----------------+
| regist_date | product_id | sale_price | purchase_price |
+-------------+------------+------------+----------------+
| 2009-10-10  | 0002       |        500 |            320 |
| 2009-10-10  | 0003       |       4000 |           2800 |
| 2009-10-10  | 0004       |     300000 |            700 |
| 2009-10-10  | 0005       |     680000 |           1250 |
| 2009-10-10  | 0006       |      50000 |           NULL |
| 2009-10-10  | 0007       |      88000 |            198 |
| 2009-10-10  | 0008       |        100 |           NULL |
| 2021-10-30  | 0001       |       1000 |            500 |
+-------------+------------+------------+----------------+
```

可以看到先按照`regist_date`的大小进行排序，在字段`regist_date`中具有相同的值的行，接着会按照`product_id`进行排序。

使用含有 NULL 的列作为排序键时，NULL 会在结果的开头或末尾汇总显示。

在`ORDER BY`子句中可以使用`SELECT`子句中定义的别名。

```mysql
-- 将product_id命名为ID，然后按照ID进行排序
SELECT product_id as ID, product_name, sale_price, purchase_price
 FROM Product
ORDER BY ID;

-- 结果如下
+------+--------------+------------+----------------+
| ID   | product_name | sale_price | purchase_price |
+------+--------------+------------+----------------+
| 0001 | T恤衫        |       1000 |            500 |
| 0002 | 打孔器       |        500 |            320 |
| 0003 | 运动T恤      |       4000 |           2800 |
| 0004 | 菜刀         |     300000 |            700 |
| 0005 | 高压锅       |     680000 |           1250 |
| 0006 | 叉子         |      50000 |           NULL |
| 0007 | 擦菜板       |      88000 |            198 |
| 0008 | 圆珠笔       |        100 |           NULL |
+------+--------------+------------+----------------+
8 rows in set (0.00 sec)
```

为什么`ORDER BY`中可以使用`SELECT`定义的别名呢？

这是因为在MySQL中，`ORDER BY `的执行次序在`SELECT`之后。



#### 1.4.5 数据的插入及更新

##### 1.4.5.1 数据的插入

通过命令`INSERT`，可以向表中插入数据：

```mysql
-- 往表中插入一行数据
INSERT INTO <表名> (字段1, 字段2, 字段3, ……) VALUES (值1, 值2, 值3, ……);

-- 往表中插入多行数据
INSERT INTO <表名> (字段1, 字段2, 字段3, ……) VALUES 
	(值1, 值2, 值3, ……),
	(值1, 值2, 值3, ……),
	...
	;
```

示例：

1. 创建表并插入数据

```mysql
-- 创建表
CREATE TABLE ProductIns
(product_id CHAR(4) NOT NULL,
 product_name VARCHAR(100) NOT NULL,
 product_type VARCHAR(32) NOT NULL,
 sale_price INTEGER DEFAULT 0, -- DEFAULT 0：表示将字段sale_price的默认值设为0
 purchase_price INT ,
 regist_date DATE ,
 PRIMARY KEY (product_id));
 
-- 通过单行方式插入
INSERT INTO 
 ProductIns(product_id, product_name, product_type, sale_price, purchase_price, regist_date)
 VALUES ('0001', '打孔器', '办公用品', 500, 320, '2009-09-11');
 
-- 当对表插入全字段时，可以省略表后的字段清单
INSERT INTO ProductIns VALUES('0002', '高压锅', '厨房用具', 6800, 5000, '2009-01-15');
  
-- 通过多行方式插入
INSERT INTO ProductIns VALUES 
 ('0003', '菜刀', '厨房用具', 3000, 2800, '2009-09-20'),
 ('0004', '订书机', '办公用品', 100, 50, '2009-09-11'),
 ('0005', '裙子', '衣服', 4100, 3200, '2009-01-23'),
 ('0006', '运动T恤', '衣服', 4000, 2800, NULL),
 ('0007', '牙刷', '日用品', 20, 10, '2010-03-22');
```

```mysql
SELECT * FROM ProductIns;

-- 结果如下
+------------+--------------+--------------+------------+----------------+-------------+
| product_id | product_name | product_type | sale_price | purchase_price | regist_date |
+------------+--------------+--------------+------------+----------------+-------------+
| 0001       | 打孔器       | 办公用品     |        500 |            320 | 2009-09-11  |
| 0002       | 高压锅       | 厨房用具     |       6800 |           5000 | 2009-01-15  |
| 0003       | 菜刀         | 厨房用具     |       3000 |           2800 | 2009-09-20  |
| 0004       | 订书机       | 办公用品     |        100 |             50 | 2009-09-11  |
| 0005       | 裙子         | 衣服         |       4100 |           3200 | 2009-01-23  |
| 0006       | 运动T恤      | 衣服         |       4000 |           2800 | NULL        |
| 0007       | 牙刷         | 日用品       |         20 |             10 | 2010-03-22  |
+------------+--------------+--------------+------------+----------------+-------------+
7 rows in set (0.00 sec)
```

2. 插入NULL

   `INSERT `语句中想给某一列赋予**NULL**值时，可以直接在` VALUES`子句的值清单中写入**NULL**。

```mysql
INSERT INTO ProductIns VALUES ('0008', '叉子', '厨房用具', 500, NULL, '2009-09-20');
```

3. 插入默认值

   在前面我们创建表时，字段sale_price包含了一条约束条件，默认为0。我们在插入数据时，可以直接用`DEFAULT`对该字段赋值。前提是，该字段被指定了默认值。

```mysql
-- 通过显式方法设定默认值
INSERT INTO 
 ProductIns (product_id, product_name, product_type, sale_price, purchase_price, regist_date)
 VALUES ('0009', '擦菜板', '厨房用具', DEFAULT, 790, '2009-04-28');

-- 通过隐式方法插入默认值
INSERT INTO 
 ProductIns (product_id, product_name, product_type, purchase_price, regist_date)
 VALUES ('0010', '擦菜板', '厨房用具', 790, '2009-04-28');
```



##### 1.4.5.2 数据的删除

通过`DROP TABLE`或者`DELETE`语句，可以对表进行删除，但二者存在一定的区别。

+ `DROP TABLE` 语句可以将表完全删除。
+ `DELETE` 语句会留下表结构，而删除表中的全部数据。

无论通过哪种方式删除，数据都是难以恢复的。

1. 通过`DROP`进行删除

   语法结构为：

```mysql
DROP <表名>;
```

2. 通过`DELETE`进行删除

   语法结构如下，记得要加`FROM`：

```mysql
DELETE FROM <表名>;
```

​		同时，也可以通过`WHERE`语句来指定删除的条件：

```mysql
DELETE FROM <表名>
	WHERE <条件>;
```

​		需要注意的是，`DELETE`语句的删除对象并不是表或者列，而是记录（行）。

示例：

```mysql
SELECT * FROM Product;

-- 结果如下
mysql> SELECT * FROM Product;
+------------+--------------+--------------+------------+----------------+-------------+
| product_id | product_name | product_type | sale_price | purchase_price | regist_date |
+------------+--------------+--------------+------------+----------------+-------------+
| 0001       | T恤衫        | 衣服         |       1000 |            500 | 2009-09-20  |
| 0002       | 打孔器       | 办公用品     |        500 |            320 | 2009-09-11  |
| 0003       | 运动T恤      | 衣服         |       4000 |           2800 | NULL        |
| 0004       | 菜刀         | 厨房用具     |       3000 |           2800 | 2009-09-20  |
| 0005       | 高压锅       | 厨房用具     |       6800 |           5000 | 2009-01-15  |
| 0006       | 叉子         | 厨房用具     |        500 |           NULL | 2009-09-20  |
| 0007       | 擦菜板       | 厨房用具     |        880 |            790 | 2008-04-28  |
| 0008       | 圆珠笔       | 办公用品     |        100 |           NULL | 2009-11-11  |
+------------+--------------+--------------+------------+----------------+-------------+
8 rows in set (0.00 sec)

-- 删除销售价格大于等于4000的行
DELETE FROM Product
 WHERE sale_price >= 4000;

-- 结果如下
mysql> SELECT * FROM Product;
+------------+--------------+--------------+------------+----------------+-------------+
| product_id | product_name | product_type | sale_price | purchase_price | regist_date |
+------------+--------------+--------------+------------+----------------+-------------+
| 0001       | T恤衫        | 衣服         |       1000 |            500 | 2009-09-20  |
| 0002       | 打孔器       | 办公用品     |        500 |            320 | 2009-09-11  |
| 0004       | 菜刀         | 厨房用具     |       3000 |           2800 | 2009-09-20  |
| 0006       | 叉子         | 厨房用具     |        500 |           NULL | 2009-09-20  |
| 0007       | 擦菜板       | 厨房用具     |        880 |            790 | 2008-04-28  |
| 0008       | 圆珠笔       | 办公用品     |        100 |           NULL | 2009-11-11  |
+------------+--------------+--------------+------------+----------------+-------------+
6 rows in set (0.00 sec)
```

3. 通过`TRUNCATE`进行删除

   在MySQL中，还存在一种删除表的方式，就是利用`TRUNCATE`语句。它的功能和`DROP`类似，但是不能通过`WHERE`指定条件，优点是速度比`DROP`快得多。

```mysql
TRUNCATE Product;

-- 结果如下
mysql> SELECT * FROM Product;
Empty set (0.00 sec)

mysql> DESC Product;
+----------------+--------------+------+-----+---------+-------+
| Field          | Type         | Null | Key | Default | Extra |
+----------------+--------------+------+-----+---------+-------+
| product_id     | char(4)      | NO   | PRI | NULL    |       |
| product_name   | varchar(100) | NO   |     | NULL    |       |
| product_type   | varchar(32)  | NO   |     | NULL    |       |
| sale_price     | int          | YES  |     | NULL    |       |
| purchase_price | int          | YES  |     | NULL    |       |
| regist_date    | date         | YES  |     | NULL    |       |
+----------------+--------------+------+-----+---------+-------+
6 rows in set (0.00 sec)
```



##### 1.4.5.3 数据的更新

当我们使用`INSERT`语句插入错误的数据后，若我们不想删除后从新插入，那就要使用到`UPDATE`语句。

1. 基本用法

   `UPDATE`的语法结构如下：

```mysql
UPDATE <表名>
	SET <字段名> = <表达式>;
```

​		示例：

```mysql
-- 由于前面演示删除语句时，表Product的内容已清空
-- 所以，这里从新进行数据插入
INSERT INTO Product VALUES
  ('0001', 'T恤衫', '衣服', 1000, 500, '2009-09-20'),
  ('0002', '打孔器', '办公用品', 500, 320, '2009-09-11'),
  ('0003', '运动T恤', '衣服', 4000, 2800, NULL),
  ('0004', '菜刀', '厨房用具', 3000, 2800, '2009-09-20'),
  ('0005', '高压锅', '厨房用具', 6800, 5000, '2009-01-15'),
  ('0006', '叉子', '厨房用具', 500, NULL, '2009-09-20'),
  ('0007', '擦菜板', '厨房用具', 880, 790, '2008-04-28'),
  ('0008', '圆珠笔', '办公用品', 100, NULL,'2009-11-11')
 ;
 
-- 修改表中所有行regist_date的值
UPDATE Product
 SET regist_date = '2009-10-10';

-- 结果如下
mysql> SELECT * FROM Product;
+------------+--------------+--------------+------------+----------------+-------------+
| product_id | product_name | product_type | sale_price | purchase_price | regist_date |
+------------+--------------+--------------+------------+----------------+-------------+
| 0001       | T恤衫        | 衣服         |       1000 |            500 | 2009-10-10  |
| 0002       | 打孔器       | 办公用品     |        500 |            320 | 2009-10-10  |
| 0003       | 运动T恤      | 衣服         |       4000 |           2800 | 2009-10-10  |
| 0004       | 菜刀         | 厨房用具     |       3000 |           2800 | 2009-10-10  |
| 0005       | 高压锅       | 厨房用具     |       6800 |           5000 | 2009-10-10  |
| 0006       | 叉子         | 厨房用具     |        500 |           NULL | 2009-10-10  |
| 0007       | 擦菜板       | 厨房用具     |        880 |            790 | 2009-10-10  |
| 0008       | 圆珠笔       | 办公用品     |        100 |           NULL | 2009-10-10  |
+------------+--------------+--------------+------------+----------------+-------------+
8 rows in set (0.00 sec)
```

2. 指定条件

```mysql
UPDATE <表名>
	SET <列名> = <表达式>
	WHERE <条件>;
```

​		示例：

```mysql
UPDATE Product
 SET regist_date = '2021-10-30'
 WHERE product_id = '0001';
```

​		注意，你也可是使用**NULL**对表进行更新，不过更新的字段必须满足没有**主键**和**NOT NULL**的约束条件。

3. 多列更新

   多列更新只需要用逗号（，）连接更改的字段即可。

```mysql
UPDATE Product
 SET 
 	sale_price = sale_price * 10,
  purchase_price = purchase_price / 2
 WHERE product_type = '厨房用具';
```

