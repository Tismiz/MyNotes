## 0. 概述

### 0.1 关系型数据库(RDBMS)

概念：建立在关系模型基础上，由多张相互连接的二维表组成的数据库

特点：

1. 使用表存储数据，格式同一，便于维护

2. 使用SQL语言擦做，标准同一，使用方便

![image-20250504212633527](C:\Users\Tismin\Documents\GitHub\MyNotes\Pictures\image-20250504212633527.png)

- 表头(header): 每一列的名称;
- 列(col): 具有相同数据类型的数据的集合;
- 行(row): 每一行用来描述某条记录的具体信息;
- 值(value): 行的具体信息, 每个值必须与该列的数据类型相同;
- **键(key)**: 键的值在当前列中具有唯一性。



### 0.2 数据模型

- **数据库:** 数据库是一些关联表的集合。
- **数据表:** 表是数据的矩阵。在一个数据库中的表看起来像一个简单的电子表格。
- **列:** 一列(数据元素) 包含了相同类型的数据, 例如邮政编码的数据。
- **行：**一行（元组，或记录）是一组相关的数据，例如一条用户订阅的数据。
- **冗余**：存储两倍数据，冗余降低了性能，但提高了数据的安全性。
- **主键**：主键是唯一的。一个数据表中只能包含一个主键。你可以使用主键来查询数据。
- **外键：**外键用于关联两个表。
- **复合键**：复合键（组合键）将多个列作为一个索引键，一般用于复合索引。
- **索引：**使用索引可快速访问数据库表中的特定信息。索引是对数据库表中一列或多列的值进行排序的一种结构。类似于书籍的目录。
- **参照完整性:** 参照的完整性要求关系中不允许引用不存在的实体。与实体完整性是关系模型必须满足的完整性约束条件，目的是保证数据的一致性。



## 1. 通用语法及分类

### 1.1 SQL通用语法

- SQL语句可以单行或多行书写，以**分号结尾**
- SQL语句可以使用空格or缩进来增强语句的可读性
- MySQL数据库的SQL语句**不区分大小写**，关键字建议使用大写
- 注释：
  - 单行注释：-- 注释内容 or # 注释内容(MySQL特有)
  - 多行注释：/* 注释内容 */



### 1.2 语法和分类

- DDL: 数据定义语言，用来定义数据库对象（数据库、表、字段）
- DML: 数据操作语言，用来对数据库表中的数据进行增删改
- DQL: 数据查询语言，用来查询数据库中表的记录
- DCL: 数据控制语言，用来创建数据库用户、控制数据库的控制权限



### 1.3 DDL(数据定义语言)

Data Definition Language,用于定义数据表结构

#### 1.3.1 数据库操作

- 查询

  ~~~mysql
  #查询所有数据库
  	SHOW DATABASES;
  	
  #查询当前数据库
  	SELECT DATABASE();
  ~~~
  
- 创建

  ~~~mysql
  #创建数据库
  	CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则];
  ~~~

- 删除

  ~~~mysql
  #删除数据库
  	DROP DATABASE [IF EXISTS] 数据库名;
  ~~~

- 使用

  ~~~mysql
  #使用数据库
  	USE 数据库名;
  ~~~

- **注意事项**

  UTF8字符集长度为3字节，有些符号占4字节，所以推荐用utf8mb4字符集

#### 1.3.2 表操作

- 查询当前数据库所有表

  ~~~mysql
  #查询所有表
  	SHOW TABLES;
  ~~~

- 查询表结构

  ~~~mysql
  DESC 表名;
  ~~~

- 查询指定表的建表语句

  ~~~mysql
  SHOW CREATE TABLE 表名;
  ~~~

- 创建

  ~~~mysql
  CREATE TABLE 表名(
  	字段1 字段1类型 [COMMENT 字段1注释],
  	字段2 字段2类型 [COMMENT 字段2注释],
  	字段3 字段3类型 [COMMENT 字段3注释],
  	...
  	字段n 字段n类型 [COMMENT 字段n注释]
  )[ COMMENT 表注释 ];
  ~~~

- 修改

  ~~~mysql
  #修改数据类型
  	ALTER TABLE 表名 MODIFY 字段名 新数据类型(长度);
  
  #修改字段名和字段类型
  	ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) [COMMENT 注释] [约束]
  
  #删除字段
  	ALTER TABLE 表名 DROP 字段名;
  
  #修改表名
  	ALTER TABLE 表名 RENAME TO 新表名;
  ~~~

  ~~~mysql
  #例子 将emp表的nickname字段修改为username，类型为varchar(30)
  	ALTER TABLE emp CHANGE nickname username varchar(30) COMMENT'昵称';
  
  #例子 将emp表的字段username删除
  	ALTER TABLE emp DROP username;
  
  #例子 将emp表名修改成employee
  	ALTER TABLE emp RENAME TO employee;
  ~~~

- 删除

  ~~~mysql
  #删除表
  	DROP TABLE [IF EXISTS] 表名;
  
  #删除指定表，并重新创建该表(删除数据留下表结构)
  	TRUNCATE TABLE 表名;
  
  #在删除表时，表中数据全会被删除
  ~~~

#### 1.3.2 数据类型及案例

- 数值类型

	| 类型         | 字节 | 有符号范围                                          | 无符号范围                                        | 说明                             |
| :----------- | :--- | :-------------------------------------------------- | :------------------------------------------------ | :------------------------------- |
	| TINYINT      | 1    | -128 ~ 127                                          | 0 ~ 255                                           | 小整数                           |
| SMALLINT     | 2    | -32,768 ~ 32,767                                    | 0 ~ 65,535                                        | 小整数                           |
| MEDIUMINT    | 3    | -8,388,608 ~ 8,388,607                              | 0 ~ 16,777,215                                    | 中等整数                         |
| INT/INTEGER  | 4    | -2,147,483,648 ~ 2,147,483,647                      | 0 ~ 4,294,967,295                                 | 标准整数                         |
| BIGINT       | 8    | -9,223,372,036,854,775,808 ~ ...                    | 0 ~ 18,446,744,073,709,551,615                    | 大整数                           |
| FLOAT        | 4    | -3.402823466E+38 ~ -1.175494351E-38                 | 1.175494351E-38 ~ 3.402823466E+38                 | 单精度浮点数，约7位有效数字      |
| DOUBLE       | 8    | -1.7976931348623157E+308 ~ -2.2250738585072014E-308 | 2.2250738585072014E-308 ~ 1.7976931348623157E+308 | 双精度浮点数，约15位有效数字     |
| DECIMAL(M,D) | 变长 | 取决于M和D                                          | 取决于M和D                                        | 精确小数，M是总位数，D是小数位数 |

- 字符串类型

  | 类型         | 最大长度          | 存储需求          | 说明               |
  | :----------- | :---------------- | :---------------- | :----------------- |
  | CHAR(n)      | 255字符           | 固定长度n字节     | 固定长度字符串     |
  | VARCHAR(n)   | 65,535字符        | 实际长度+1或2字节 | 可变长度字符串     |
  | TINYTEXT     | 255字节           | 实际长度+1字节    | 短文本字符串       |
  | TEXT         | 65,535字节        | 实际长度+2字节    | 常规文本字符串     |
  | MEDIUMTEXT   | 16,777,215字节    | 实际长度+3字节    | 中等长度文本字符串 |
  | LONGTEXT     | 4,294,967,295字节 | 实际长度+4字节    | 长文本字符串       |
  | BINARY(n)    | 255字节           | 固定长度n字节     | 固定长度二进制数据 |
  | VARBINARY(n) | 65,535字节        | 实际长度+1或2字节 | 可变长度二进制数据 |
  | TINYBLOB     | 255字节           | 实际长度+1字节    | 短二进制大对象     |
  | BLOB         | 65,535字节        | 实际长度+2字节    | 二进制大对象       |
| MEDIUMBLOB   | 16,777,215字节    | 实际长度+3字节    | 中等二进制大对象   |
  | LONGBLOB     | 4,294,967,295字节 | 实际长度+4字节    | 极大二进制数据     |

- 日期和时间类型

  | 类型      | 格式                | 范围                                      | 说明             |
  | :-------- | :------------------ | :---------------------------------------- | :--------------- |
  | DATE      | YYYY-MM-DD          | 1000-01-01 ~ 9999-12-31                   | 日期值           |
  | TIME      | HH:MM:SS            | -838:59:59 ~ 838:59:59                    | 时间值           |
  | DATETIME  | YYYY-MM-DD HH:MM:SS | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59 | 日期和时间组合   |
  | TIMESTAMP | YYYY-MM-DD HH:MM:SS | 1970-01-01 00:00:01 ~ 2038-01-19 03:14:07 | 时间戳，自动更新 |
  | YEAR      | YYYY                | 1901 ~ 2155                               | 年份值           |

### 1.4 DML(数据操作语言)

Data Mainpulation Language，用来对数据库中表的数据记录进行增删改操作

#### 1.4.1 添加数据

~~~mysql
1.#给指定字段添加数据
	INSERT INTO 表名 (字段名1,字段名2,...) VALUES(值1,值2,...);

2.#给全部字段添加数据
	INSERT INTO 表名 VALUES(值1,值2,...);
	
3.#批量添加数据
	INSERT INTO 表名(字段名1,字段名2,...) VALUES(值1,值2,,...),(值1,值2,...),(值1,值2,...);
	INSERT INTO 表名 VALUES(值1,值2,...),(值1,值2,...),(值1,值2,...);
~~~

注意：

- 插入数据时，指定字段顺序需要和值一一对应
- 字符串和日期型数据应该包含在引号里
- 插入的数据大小，应该在字段的规定范围内

#### 1.4.2 修改数据

~~~mysql
#修改数据
	UPDATE 表名 SET 字段名1=值1,字段名2=值2,...[WHERE 条件];
~~~

#### 1.4.3 删除数据

~~~mysql
#删除
	DELETE FROM 表名 [WHERE 条件];
~~~

注意：

- DELETE语句条件可以有，也可以没有，如果没有条件则会删除整张表的所有数据
- **DELETE语句不能删除某一个字段的值(可以使用UPDATE)**

 ### 1.5 DQL(数据查询语言)

Data Query Language数据查询语言，用来查询数据库中表的记录

~~~mysql
#语法结构 编写顺序

SELECT
	字段列表
FROM
	表名列表
WHERE
	条件列表
GROUP BY
	分组字段列表
HAVING
	分组后条件列表
ORDER BY
	排序字段列表
LIMIT
	分页参数
~~~

#### 1.5.1 基础查询

~~~mysql
#查询多个字段
	SELECT 字段1,字段2,字段3... FROM 表名;
	SELECT * FROM 表名;
	
#设置别名
	SELECT 字段1 [AS 别名1], 字段2 [AS 别名2] ... FROM 表名;

#去除重复记录
	SELECT DISTINCT 字段列表 FROM 表名;
~~~

#### 1.5.2 条件查询(WHERE)

~~~mysql
#语法
	SELECT 字段列表 FROM 表名 WHERE 条件列表;
~~~

| 比较运算符          | 功能                                        |
| ------------------- | ------------------------------------------- |
| >                   | 大于                                        |
| >=                  | 大于等于                                    |
| <                   | 小于                                        |
| <=                  | 小于等于                                    |
| =                   | 等于                                        |
| <> or !=            | 不等于                                      |
| BETWEEN ... AND ... | 在某个范围之内(左闭右闭)                    |
| IN(...)             | 在in之后的列表中的值                        |
| LIKE 占位符         | 模糊匹配(**_匹配单个字符,%匹配任意个字符**) |
| IS NULL             | 是NULL                                      |
| **逻辑运算符**      | **功能**                                    |
| AND or &&           | 并且，多个条件同时成立                      |
| OR or \|\|          | 或者，多个条件任意一个成立                  |
| NOT or !            | 非，不是                                    |

~~~mysql
#查询年龄等于88的员工
	SELECT * FROM emp where age > 88;
	
#查询没有身份证号信息的员工信息
	SELECT * FROM emp WHERE idcard IS NULL;
	
#查询有身份证号信息的员工信息
	SELECT * FROM emp WHERE idcard IS NOT NULL;
	
#查询年龄不等于88岁的员工信息
	SELECT * FROM emp WHERE age != 88;
	
#查询年龄在15岁到20岁之间的员工信息
	SELECT * FROM emp WHERE age >= 15 && <= 20;
    SELECT * FROM emp WHERE age >= 15 and <= 20;
    SELECT * FROM emp WHERE age BETWEEN 15 AND 20;
    
#查询性别为女且年龄小于25岁的员工信息
	SELECT * FROM emp WHERE gender = '女' AND age < 25;
	
#查询年龄等于18或20或40的员工信息
	SELECT * FROM emp WHERE age = 18 OR 20 OR 40;
	SELECT * FORM emp WHERE age in (18,20,40);
	
#查询姓名为两个字符的员工信息
	SELECT * FROM emp WHERE name like '__';
	
#查询身份证号最后一位为X的员工信息
	SELECT * FROM emp WHERE idcard like '%X';
	
~~~

#### 1.5.3 聚合函数(count,max,min,avg,sum)

将一列数据作为一个整体，进行**纵向**计算，作用于某一列

**统计计算时，NULL值不纳入统计计算范围**

~~~mysql
#统计员工数量
	SELECT count(*) FROM emp;
	SELECT count(idcard) FROM emp;
	
#统计员工的平均年龄
	SELECT avg(age) FROM emp;
	
#统计员工的最大年龄
	SELECT max(age) FROM emp;
	
#统计员工最小年龄
	SELECT min(age) FROM emp;
	
#统计西安地区员工的年龄之和
	SELECT sum(age) FROM emp WHERE city = '西安';
	
~~~

#### 1.5.4 分组查询(GROUP BY)

WHREE和HAVING 的区别

- 执行时机不同：WHERE是分组前过滤，不满足则不参与分组；而HAVING是分组后对结果过滤
- 判断条件不同：WHERE不能对聚合函数进行判断，而HAVING可以

注意

- 执行顺序:where > 聚合函数 > having
- 分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义

~~~mysql
#语法
	SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 分组字段名 [HAVING 分组后过滤条件]
	
#根据性别分组，统计男性员工和女性员工的数量
	SELECT count(*) FROM emp GROUP BY gender;
	
#根据性别分组，统计男性员工和女性员工的平均年龄
	SELECT avg(age) FROM emp GROUP BY gender;
	
#查询年龄不小于45的员工，并根据工作地址分组，获取员工数量大于等于3的工作地址
	SELECT workaddress, count(*) FROM emp where age < 45 GROUP BY workaddress HAVING coount(*) >= 3;
~~~

#### 1.5.5 排序查询(ORDER BY)

~~~mysql
#语法
	SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1,字段2 排序方式2；

#排序方式
	ASC 升序(默认值)
	DESC 降序
	
#根据年龄对员工升序排序
	SELECT * FROM emp ORDER BY age ASC;
	
#根据入职时间对员工降序排序
	SELECT * FROM emp ORDER BY entrydate DESC;
	
#根据年龄对公司的员工进行升序排序，年龄相同，再按入职时间降序排序
	SELECT * FROM emp ORDER BY age ASC, entrydate DESC;
~~~

#### 1.5.6 分页查询(LIMIT)

注意

- 起始索引从0开始，起始索引= (查询页码 - 1)*每页显示记录数
- 分页查询时数据库的方言，不同数据库有不同的实现
- 如果查询的是第一页的数据，起始索引可以省略，直接谢伟LIMIT 10;

~~~~mysql
#语法
	SELECT 字段列表 FROM 表名 LIMIT 起始索引, 查询记录数;
	
#查询第1页员工数据，每页展示10条记录
	SELECT * FROM emp LIMIT 0,10;
	
#查询第2页员工数据，每页展示10条记录
	SELECT * FROM emp LIMIT 10,10；
~~~~



~~~
select * from where gender =  and age in ();
select * from where gender = and age between 20 and 40 and name like '___';
select gender, count(*) from where age < 60 group by gender; 
select name, age from emp where age <= 35 order by age , entrydate desc;
select * from emp where gender = '男' and age between 20 and 40 order by age, entrydate desc limit(5);
~~~

#### 1.5.5 执行顺序

![image-20250507155404309](C:\Users\Tismin\Documents\GitHub\MyNotes\Pictures\image-20250507155404309.png)

### 1.6 DCL(数据控制语言)

Data Control Language，用来管理数据库用户、控制数据库的访问权限

#### 1.6.1 管理用户

注意

- 主机名可以用%通配
- 这类主要是DBA(Database Administrator 数据库管理员)使用

~~~mysql
#1.查询用户
	USE mysql;
	SELECT * FROM user;
	
#2.创建用户
	CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码'；
	
#3.修改用户密码
	ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';
	
#4.删除用户
	DROP USER '用户名'@'主机名';
~~~

~~~mysql
#创建用户itcast，只能够在当前主机localhost访问，密码123456
	CREATE USER 'idcast'@'localhost' IDENTIFIED BY '123456';
	
#创建用户heima，可以在任意主机访问该数据库，密码123456
	CREATE USER 'heima'@'%' IDENTIFIED BY '123456';
	
#修改用户heima的密码为1234
	ALTER USER 'heima'@'%' IDENTIFITED BY mysql_native_password BY '1234';
	
#删除itcast@localhost这个用户
	DROP USER '用户名'@'主机名';
~~~

#### 1.6.2 权限控制

| 权限               | 说明               |
| ------------------ | ------------------ |
| ALL,ALL PRIVILEGES | 所有权限           |
| SELECT             | 查询数据           |
| INSERT             | 插入数据           |
| UPDATE             | 修改数据           |
| DELETE             | 删除数据           |
| ALTER              | 修改表             |
| DROP               | 删除数据库/表/视图 |
| CREATE             | 创建数据库/表      |

~~~mysql
#1.查询权限
	SHOW GRANTS FOR '用户名'@'主机名';
	
#2.授予权限
	GRANT 权限列表 ON 数据库名,表名 TO '用户名'@'主机名';
	
#3.撤销权限
	REVOKE 权限列表 ON 数据库名,表名 FROM '用户名'@'主机名';
~~~

~~~mysql
#查询权限
	SHOW GRANTS FOR 'heima'@'%';
	
#授予权限
	GRANT ALL ON itcast,* TO 'heima'@'%';
	
#撤销权限
	REVOKE ALL ON itcast,* TO 'heima'@'%';
~~~



## 2. 函数

**函数**是指一段可以直接被另一段程序调用的程序或代码

### 2.1 字符串函数

| 函数名                       | 功能描述                   | 示例                                                 | 应用场景                   |
| :--------------------------- | :------------------------- | :--------------------------------------------------- | :------------------------- |
| **CONCAT(s1,s2,...sn)**      | 连接两个或多个字符串       | `CONCAT('Hello', ' ', 'World')` → 'Hello World'      | 合并姓名、生成带前缀的编号 |
| **SUBSTRING(str,start,len)** | 提取字符串的子串           | `SUBSTRING('MySQL', 2, 3)` → 'ySQ'                   | 提取身份证日期、URL解析    |
| **LENGTH(str)**              | 返回字符串的字节长度       | `LENGTH('中文')` → 6 (UTF-8)                         | 验证输入长度、计算存储空间 |
| **CHAR_LENGTH(str)**         | 返回字符串的字符数         | `CHAR_LENGTH('中文')` → 2                            | 统计实际字符数量           |
| **REPLACE()**                | 替换字符串中的子串         | `REPLACE('a-b-c', '-', ',')` → 'a,b,c'               | 数据脱敏、批量修改内容     |
| **UPPER(str)**               | 将字符串转为大写           | `UPPER('mysql')` → 'MYSQL'                           | 统一格式、不区分大小写比较 |
| **LOWER(str)**               | 将字符串转为小写           | `LOWER('MySQL')` → 'mysql'                           | 用户名标准化               |
| **TRIM(str)**                | 去除字符串两端空格         | `TRIM(' text ')` → 'text'                            | 清理用户输入、数据预处理   |
| **LPAD(str,n,pad)**          | 左侧填充字符串到指定长度   | `LPAD('5', 3, '0')` → '005'                          | 生成固定长度编号           |
| **RPAD(str,n,pad)**          | 右侧填充字符串到指定长度   | `RPAD('Hi', 5, '!')` → 'Hi!!!'                       | 格式化输出                 |
| **LOCATE(str,str)**          | 返回子串第一次出现的位置   | `LOCATE('SQL', 'MySQL')` → 3                         | 字符串解析、验证格式       |
| **GROUP_CONCAT()**           | 将多行结果合并为单个字符串 | `GROUP_CONCAT(name SEPARATOR ',')`                   | 生成标签云、合并分组结果   |
| **JSON_EXTRACT()**           | 从JSON字符串中提取数据     | `JSON_EXTRACT('{"name":"John"}', '$.name')` → "John" | 处理JSON格式数据           |

~~~mysql
#将员工id统一为5位，不足五位数的添0
	UPDATE emp SET workno = lpdd(workno, 5, '0');
~~~

### 2.2 数值函数

| 函数名                | 功能描述                 | 示例                                     | 应用场景               |
| :-------------------- | :----------------------- | :--------------------------------------- | :--------------------- |
| **ABS(x)**            | 返回数字的绝对值         | `ABS(-10) → 10`                          | 计算距离、处理差值     |
| **ROUND(x,y)**        | 四舍五入到指定小数位     | `ROUND(3.1415, 2) → 3.14`                | 金额计算、统计报表     |
| **CEIL(x)**           | 向上取整                 | `CEIL(3.2) → 4`                          | 分页计算、库存补货     |
| **FLOOR(x)**          | 向下取整                 | `FLOOR(3.9) → 3`                         | 年龄计算、批量分配     |
| **TRUNCATE(x,y)**     | 截断小数（不四舍五入）   | `TRUNCATE(3.1415, 2) → 3.14`             | 精确计算、财务处理     |
| **MOD(x,y)**          | 取模运算（返回余数）     | `MOD(10, 3) → 1`                         | 奇偶判断、循环分组     |
| **POW(x,y)**          | 幂运算（X的Y次方）       | `POW(2, 3) → 8`                          | 复利计算、科学计算     |
| **SQRT(x)**           | 计算平方根               | `SQRT(16) → 4`                           | 几何计算、标准差       |
| **RAND()**            | 生成0~1的随机数          | `RAND() → 0.123456`                      | 抽样测试、随机排序     |
| **SIGN(x)**           | 返回数字的符号（-1/0/1） | `SIGN(-5) → -1`                          | 数据分类、趋势分析     |
| **LOG(x,y)**          | 计算对数                 | `LOG(100, 10) → 2`                       | 数据缩放、增长率计算   |
| **EXP(x)**            | 计算e的X次方             | `EXP(1) → 2.718281828459045`             | 连续复利、概率计算     |
| **SIN(x)**            | 计算正弦值               | `SIN(RADIANS(30)) → 0.5`                 | 几何计算、物理模拟     |
| **COS(x)**            | 计算余弦值               | `COS(0) → 1`                             | 几何计算、物理模拟     |
| **TAN(x)**            | 计算正切值               | `TAN(RADIANS(45)) → 1`                   | 几何计算、物理模拟     |
| **RADIANS(x)**        | 角度转弧度               | `RADIANS(180) → 3.141592653589793`       | 三角函数参数预处理     |
| **DEGREES(x)**        | 弧度转角度               | `DEGREES(PI()) → 180`                    | 结果可视化             |
| **FORMAT()**          | 格式化数字（千位分隔符） | `FORMAT(1234567.89, 2) → '1,234,567.89'` | 财务报表、用户界面显示 |
| **GREATEST(x,y,...)** | 返回参数列表中的最大值   | `GREATEST(3, 5, 1) → 5`                  | 数据筛选、阈值控制     |
| **LEAST(x,y,...)**    | 返回参数列表中的最小值   | `LEAST(3, 5, 1) → 1`                     | 优惠计算、容错处理     |

~~~
#通过数据库函数，生成一个6位的随机数
	SELECT round(rand(),6) * 1000000
~~~

