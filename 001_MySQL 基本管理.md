# MySQL 基本管理

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

-----------

