# MySQL 查询操作

## 一、select检索语句

1. 检索单个列或多个列

```sql
select prod_name,prod_id,prod_price
from Products;

```
2. 检索所有列

```sql
select *
from Products;

```
3. 检索不同的值

```sql
select distinct 列名
from 表名；

```
4. 限制结果输出

```sql
select 列名
from 表名
limit 5;   --返回不超过5行的数据

```

```sql
select 列名
from 表名
limit 3 offset 4;    --从第4行开始，检索3行的数据，返回的是第5、6、7行的数据

```

```sql
select 列名
from 表名
limit 4，3;          --从第4行开始，检索3行的数据，返回的是第5、6、7行的数据

```

## 二、排序检索数据

1. 按单个或多个列排序

```sql
select prod_id,prod_name,prod_price
from Products
ORDER BY prod_price,prod_name;       --先按价格排序，再按姓名排序

```

2. 按列位置排序

```sql
select prod_id,prod_name,prod_price
from Products
ORDER BY 2,3;         --先按第二列排序，再按第三列排序

```
3. 指定排序方向(系统默认升序排序，若要降序排序，必须在该列名后指定DESC关键字)

```sql
select prod_id,prod_name,prod_price
from Products
ORDER BY prod_price DESC,prod_name;    --若需多个列降序排序，必须对每个列指定DESC关键字

```

```
order by 子句 必须是select语句中的 最后一条字句。
```


## 三、where子句过滤数据

1. 检查单个值（=、>、<、>=、<=）

```sql
select prod_name,prod_price
from Products
where prod_price=10;

```

2. 不匹配检查（<>、!=）（用单引号来限定字符串）

```sql
select vend_id,prod_name,prod_price
from Products
where vend_id<>'DLL01';

```

3. 范围值检查（between and）

```sql
select prod_name,prod_price
from Products
where prod_price between 5 and 10;

```

4. 空值检查(is null)

```sql
select cust_name
from Customers
where cust_email is null;

```

## 四、高级数据过滤

1. 组合where字句（and操作符、or操作符）

```sql
select prod_price,prod_name
from Products
where (vend_id='DLL01'or vend_id='BRS01')
and prod_price>=10;                          --操作符求值顺序： 圆括号 > and > or

```

2. in操作符(where子句中用来指定匹配清单的关键字，功能与or相当)

```sql
select prod_price,prod_name
from Products
where vend_id in ('DLL01','BRS01');

```

3. not操作符(where子句中用来否定其后条件的关键字，功能与<>操作符相当)

```sql
select prod_name
from Products
where not vend_id='DLL01'       --not关键字用在要过滤的 列前
ORDER BY prod_name;

```

## 五、用通配符进行过滤（like 操作符）

### 通配符搜索只能用于文本子段（字符串），非文本数据类型子段不能使用通配符搜索

1. 百分号（%）通配符  ( % 代表搜索模式中给定位置的0个、1个或多个字符)

   ```sql
   SELECT prod_id,prod_name
   FROM Products
   WHERE prod_name LIKE 'Fish%';
   
   SELECT prod_name
   FROM Products
   WHERE prod_name LIKE 'F%y';   --检索以 F 开头、以 y 结尾的所有产品
                              --根据部分信息搜索电子邮件地址： where email like ‘b%@forta.com‘
   ```

2. 下划线（_）通配符 （ _  只匹配单个字符，而不是多个字符）

   ```sql
   SELECT prod_id,prod_name
   FROM Products
   WHERE prod_name LIKE '_ inch teddy bear';
   
   --对比
   
   SELECT prod_id,prod_name
   FROM Products
   WHERE prod_name LIKE '% inch teddy bear';
   ```

3. 方括号（[ ]）通配符 （用来指定一个《字符集》，必须匹配指定位置的一个字符）

   ```sql
   --只有微软的Access和SQL Server支持集合
   
   SELECT cust_name
   FROM Customers
   WHERE cust_name LIKE '[VF]%'     --检索以V或F开头的名字
   ORDER BY cust_name;
   ```


## 六、使用子查询

### 子查询常用于where子句的in操作符中，以及用来填充计算列。

1. 利用子查询进行过滤

   ```sql
   SELECT cust_name,cust_contact
   FROM Customers
   WHERE cust_id IN (SELECT cust_id
                     FROM Orders
   				  WHERE order_num IN (SELECT order_num
   				                      FROM OrderItems
   									  WHERE prod_id ='RGAN01'));
   									  								  
   --作为子查询的select语句，只能查询单个列
   ```

2. 作为计算子段使用子查询

   ```sql
   SELECT COUNT(*) AS orders          --使用了计算子段
   FROM Orders
   WHERE cust_id='1000000001';        --对id是1000000001的顾客的订单进行计数
   ```

   ```sql
   SELECT cust_name,cust_state,(SELECT COUNT(*)
                                FROM Orders
   							 WHERE Orders.cust_id=Customers.cust_id)AS orders  
   							 --上面where子句使用了 完全限制列名
   FROM Customers
   ORDER BY cust_name;
   ```



## 七、联结表

### 1. 联结
   
联结是一种机制，用来在一条select语句中关联表（使用完全限定列名：用一个句点分 隔表名和列名），可联结多个表，返回一组输出（where、and）。

### 2. 关系表与关系数据库
 
关系表的设计是把信息分解成多个表，一类数据一个表；
各表通过某些共同的值互相关联。

### 3. 子查询和使用联结可以实现相同的功能。

```sql
SELECT cust_name,cust_contact
FROM Customers
WHERE cust_id in (SELECT cust_id
                 FROM Orders
                 WHERE order_num in (SELECT order_num                        
                 FROM OrderItems                                         
                 WHERE prrod_id='RGAN02'));             
```

```sql
SELECT cust_name,cust_contact
FROM Customers,Orders,OrderItems
WHERE Customers.cust_id=Orders.cust_id
                AND OrderItems.order_num=Orders.order_num
                AND  prrod_id='RGAN02';
```

## 八、组合查询
### 1. 组合查询
a. 利用union操作符将多条select语句组合成一个结果集（与where or 子句具有相同的功能）。
b. union自动取消重复的行，union all包含重复的行

### 2. 适用情况
a.在一个查询中，从不同的表返回结构数据；
b.对一个表执行多个查询，按一个查询返回数据。

### 3. union规则
a.多个select语句；
b.union中每个查询必须包含相同的列、表达式或聚集函数；
a.列数据类型必须兼容。

### 4. union与where
union几乎总是完成与多个where条件相同的工作。union all为union的一种形式，它完成where子句完成不了的工作。如果确实需要每个条件的匹配行全部出现（包括重复行），就必须使用union all。
### 5. union规则对组合查询结果排序
在用union组合查询时，只能使用一条order by 子句，它必须位于最后一条select语句之后。


