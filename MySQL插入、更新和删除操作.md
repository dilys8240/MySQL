# # MySQL插入、更新和删除操作

标签（空格分隔）： 增、删、改

------

## 一、insert语句插入数据

1.插入完整的行

如果不提供列名，必须给每个表列提供一个值；如果提供列名，必须给列出的每个列一个值。如果某列没有值，则应使用NULL值（假定表允许该列指定NULL值）。

```sql
insert into Customers
values('1000000006',
       'Toy Land',
       '123 Any Street',
       'New York',
       'NY',
       '11111',
       'USA',
        NULL,
        NULL);
```

2.插入完整的行插入行的一部分（insert into 明确给出了表的列名，可省略部分列）

省略的列需满足以下某个条件：
* 该列定义允许为null值（无值或空值）。
* 在表定义中给出默认值。

```sql
insert into Customers（cust_id,
                       cust_name,
                       cust_address,
                       cust_city,
                       cust_state,
                       cust_zip,
                       cust_country ）
values('1000000006',
       'Toy Land',
       '123 Any Street',
       'New York',
       'NY',
       '11111',
       'USA');
```       
       

3.插入检索出的数据（可插入多行 insert select）

* insert select 中可以包含where子句，以过滤插入的数据。
* insert通常只插入一行。要插入多行，可使用insert select。


```sql
insert into Customers（cust_id,
                       cust_name,
                       cust_address,
                       cust_city,
                       cust_state,
                       cust_zip,
                       cust_country ）
select cust_id,
       cust_name,
       cust_address,
       cust_city,
       cust_state,
       cust_zip,
       cust_country
from CustNew;
```

4.从一个表复制到另一个表（select into ）

* insert select 与select into 

  最重要的差别：前者是插入数据，而后者是导出数据。
  

```sql
select *
into CustCopy      --该select语句创建了一个名为CustCopy的新表，并把Customers表的整个内容复制到新表中。
from Customers;

```


## 二、更新数据

### 1.update的两种使用方式（使用一定注意，若省略where子句会更新表中所有行）
* 更新表中的特定行（使用where子句）
* 更新表中的所有行

### 2.update语句三个组成部分
* 要更新的表；
* 列名和他们的新值；
* 确定要更新哪行的过滤条件。 

```sql
update Customers
set cust_contact ='Sam Roberts',
    cust_email ='sam@toyland.com'
where cust_id =''1000000006;

```

### 3.删除某个列的值，可设置它为NULL（表定义允许NULL值时）。

```sql
update Customers
set cust_email =null
where cust_id =''1000000005;

```

## 三、删除数据

### 1.delete 的两种使用方式
从表中删除特定的行；
从表中删除所有行。
### 2.delete不需要列名或通配符。
delete删除整行而不是删除列。要删除指定的列，使用update语句。
### 3.delete删除的是表中行的行的内容，而不是表本身。

```sql
delete from Customers
where cust_id ='1000000006';

```



