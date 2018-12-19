# MySQL 笔记

-------

## MySQL 服务管理

```bash
# 启动
systemctl start mysqld

# 重起
systemctl restart mysqld

# 停止
systemctl stop mysqld


```


## 数据库基本命令

```bash
# 登录：
mysql -uroot -p
# 输入用户密码即可


```


## 创建MySQL数据库

示例代码：

```sql
CREATE DATABASE `test` CHARACTER SET 'utf8' COLLATE 'utf8_general_ci';

```
------
#一、以下为“查”的内容

##select检索语句

1.检索单个列或多个列
```sql
select prod_name,prod_id,prod_price
from Products;

```
2.检索所有列
```sql
select *
from Products;

```
3.检索不同的值
```sql
select distinct 列名
from 表名；

```
4.限制结果输出
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

##排序检索数据

1.按单个或多个列排序
```sql
select prod_id,prod_name,prod_price
from Products
ORDER BY prod_price,prod_name;       --先按价格排序，再按姓名排序

```

2.按列位置排序
```sql
select prod_id,prod_name,prod_price
from Products
ORDER BY 2,3;         --先按第二列排序，再按第三列排序

```
3.指定排序方向(系统默认升序排序，若要降序排序，必须在该列名后指定DESC关键字)
```sql
select prod_id,prod_name,prod_price
from Products
ORDER BY prod_price DESC,prod_name;    --若需多个列降序排序，必须对每个列指定DESC关键字

```

```
order by 子句 必须是select语句中的 最后一条字句。
```


##where子句过滤数据

1.检查单个值（=、>、<、>=、<=）
```sql
select prod_name,prod_price
from Products
where prod_price=10;

```

2.不匹配检查（<>、!=）（用单引号来限定字符串）
```sql
select vend_id,prod_name,prod_price
from Products
where vend_id<>'DLL01';

```

3.范围值检查（between and）
```sql
select prod_name,prod_price
from Products
where prod_price between 5 and 10;

```

4.空值检查(is null)
```sql
select cust_name
from Customers
where cust_email is null;

```

##高级数据过滤

1.组合where字句（and操作符、or操作符）
```sql
select prod_price,prod_name
from Products
where (vend_id='DLL01'or vend_id='BRS01')
and prod_price>=10;                          --操作符求值顺序： 圆括号 > and > or

```

2.in操作符(where子句中用来指定匹配清单的关键字，功能与or相当)
```sql
select prod_price,prod_name
from Products
where vend_id in ('DLL01','BRS01');

```


3.not操作符(where子句中用来否定其后条件的关键字，功能与<>操作符相当)
```sql
select prod_name
from Products
where not vend_id='DLL01'       --not关键字用在要过滤的 列前
ORDER BY prod_name;

```

##用通配符进行过滤










