# 第一章

## 三个阶段

数据管理的三个阶段：

- 人工管理阶段（数据是面向程序的，不具备独立性，更不能共享）
- 文件管理阶段（数据的存储和简单查询）
- 数据库管理阶段（实现数据共享、减少数据冗余、提高数据独立性，对数据进行统一的管理和控制）

***

## SQL 语言的四个部分

SQL（Structured Query Language，结构化查询语言）

- 数据定义语言（Data Definition Language，DDL），定义和管理数据库以及数据库中的各种对象。
- 数据操作语言（Data Manipulation Language，DML），对数据库中存储的数据进行插入、修改和删除。
- 数据查询语言（Data Query Language，DQL），对数据库中存储的数据执行各种查询操作，主要是 SELECT 语句。
- 数据控制语言（Data Control Language，DCL），用于在数据库中授予或回收队某对象的相关访问权限。

***

## 三范式

数据库设计的范式理论（xNF）

- 第一范式：表中的每一列都是不可再分的数据项，不能再拆分。
	例如，用户信息表中的 `父母` 信息一栏，需要拆分成 `父亲`和`母亲`两个栏目。
- 第二范式：在满足第一范式的基础上，消除表中的部分依赖。
	*实体的所有非主属性，必须完全依赖于主关键字，不能出现部分依赖，即只依赖于主关键字中的部分属性*。
- 第三范式：在满足第二范式的基础上，消除表中的传递依赖。
	*表中的非主关键字段都应该直接依赖于主关键字*。
	例如，学生信息表中包括 学号、姓名、年龄、所在学院、学院地点、学院电话 这些字段。学号是主键字段，姓名、年龄、所在学院三个字段是直接依赖于主键字段，而学院地点、学院电话这两个字段依赖于所在学院，出现了传递依赖，不符合第三范式。

***

## Oracle 数据库的系统结构

1. Oracle 分布式数据库系统结构
2. Oracle 客户机/服务器系统结构
3. Oracle 浏览器/服务器系统结构

***

## 零散

ORACLE_BASE 目录是 Oracle 产品安装到硬盘上的基本目录。

ORACLE_HOME 是数据库实例的路径。

OracleServiceORCL、OracleOraDb10g_gome1TNSListener 两个服务是必须启动的。

Oracle 数据库的默认用户：
- SYS 默认创建并且授予 DBA 角色，它是 Oracle 数据库中权限最大的管理员账号。
- **SYSTEM** 默认创建并且授予 DBA 角色，权限仅次于 SYS。
- SYSMAN 企业管理的超级管理员账号。
- DBSNMP 用于智能代理的用户，监控和管理数据库的相关性能。
- **SCOTT** 是 Oracle 数据库中供实验用的样例用户

***

## SQL * Plus

使用最多的链接数据库的工具

**AS SYSDBA | SYSOPER**：
SYSDBA 是数据库中的最高权限，可以启动数据库、关闭数据库、建立数据库备份和恢复数据库，以及其他数据库管理操作。
SYSOPER 可以启动和关闭数据库，但是不能建立数据库，也不能执行不完全恢复，可以进行一些基本的操作而不能查看用户的数据，不具备 DBA 角色的任何特权。

***

## Oracle 数据库的启动与关闭

用 STARTUP 命令启动数据库，可以指定三个参数：
- NOMOUNT：表示只启动一个 Oracle 实例
- MOUNT：启动一个 Oracle 实例并打开控制文件
- OPEN：启动一个 Oracle 实例，并以此打开控制文件、数据文件和重做日志文件。

用 SHUTDOWN 关闭数据库，可以指定三个参数
- NORMAL：关闭命令的默认选项。等待所有用户从数据库中退出才开始关闭数据库。
- IMMEDIATE：使用频率最高的关闭数据库的方式。使用该命令，正在处理的 SQL 语句立即中断，系统任何没有提交的数据全部回滚。
- TRANSACTIONAL：计划关闭数据库
- ABORT：在无法正常关闭数据库时才使用这种方式。立即中止，不回滚，需要恢复实例。


# 第二章

## 物理存储结构

- 数据文件
	- 存储各种数据，如表中的记录、索引数据、系统数据和临时数据等。

- 日志文件
	- 记录了用户对数据库的修改信息（如增加、删除、修改），名字通常为 REDO*.LOG 格式。
	- 对数据库执行查询操作不会产生日志。
	- 归档日志模式，自动保存；非归档日志模式，序号自动覆盖。

- 控制文件
	- 数据库的名称、数据文件和联机日志文件的名称及位置、当前的日志序号、表空间等信息。

- 初始化参数文件
	- 文本参数信息 和 服务器参数信息

## 逻辑存储结构

- 表空间

- 段

- 盘区（系统以盘区为单位给段分配新的磁盘空间）

- 数据块（Oracle 最小的存储单元，也是最基本的数据存储单位）

***

## 数据库实例的进程结构（了解）

- 用户进程
- 服务进程
- 后台进程
	- 数据写进程
	- 日志写进程
	- ...

## 内存结构（了解）

系统全局区、程序全局区

***

## 客户端和服务端配置

记住文件名：
- sqlnet.ora
- tnsnames.ora
- listener.ora

***

# 第三章

所谓的数据库用户即使用和共享数据库资源的人。

**用户的概念：**

访问 Oracle 数据库的“人”，如 DBA、开发工程师等。每个用户都有一个用户名、口令和相应的权限。

**方案的概念：**

方案（Schema）是一系列逻辑数据结构或对象的集合。

方案和用户是 **一一对应** 的关系。

***

## 创建用户

用 SQL 语句命令创建用户
```sql
CREATE USER user_name
IDENTIFIED BY password
```

创建新用户 zhangsan，密码为 a12345。
```sql
CREATE USER zhangsan IDENTIFIED BY a12345;
```

创建新用户 lisi，密码为 a12345，表空间为 users，并且在 users 表空间可以使用 10mb 的磁盘表空间。
```sql
CREATE USER lisi IDENTIFIED BY a12345
DEFAULT TABLESPACE users QUOTA 10M ON users;
```

创建新用户 tom，密码为 a12345，并且设置密码已过期，用户的状态为加锁。
```sql
CREATE USER tom IDENTIFIED BY a12345
PASSWORD expire ACCOUNT lock;
```

## 权限和角色

权限是用户对一项功能的执行权利。

例如 获得数据库建立会话的系统权限（CREATE SESSOIN 权限）或者是连接数据库的角色（CONNECT）。

- 系统权限
- 对象权限

## 系统权限的授权

```sql
GRANT system_privilege [, system_privilege] TO user_name [, user_name]
[WITH ADMIN OPTION]
```

`GRANT 权限名 TO 用户名`

授权实例：
```sql
CREATE USER lisi IDENTIFIED BY a12345;
// 为新用户 zhangsan 授予 CREATE SESSION 系统权限及管理该系统权限的权利
GRANT CREATE SESSION TO zhangsan WITH ADMIN OPTION;
// 以新用户 zhangsan 连接数据库，并继续 lisi 授予 CREATE SESSION 权限
CONNECT zhagnsan /a12345;
GRANT CREATE SESSION TO lisi;
-- 由于 zhangsan 具有对该权限的管理权利，所以授权成功
-- 以 lisi 连接数据库
CONNECT list/a12345;
```

## 系统权限的回收

`REVOKE 系统权限 [, 系统权限] FROM 用户名 [, 用户名]`

```sql
CONNECT system/a12345;
REVOKE CREATE SESSION FROM zhangsan, lisi;
```


## 对象权限的授权

```sql
GRANT 对象权限 ON 对象名称 TO 用户名称
```

例如：
```sql
CONNECT system/a12345;
GRANT SELECT, UPDATE ON scott.emp TO zhangsan, lisi;

-- 或者
CONNECT scott/tiger;
GRANT SELECT, UPDATE ON emp TO zhangsan WITH GRANT OPTION;
CONNECT zhangsan/a12345;
GRANT SELECT, UPDATE ON scott.emp TO lisi;
```

## 对象权限的回收

```sql
REVOKE 对象权限 ON 对象名称 FROM 用户名称;
```


## 角色管理

角色通常授予一类具有相同权限的用户。

**系统预定义角色：**
- DBA 数据库管理员角色
- RESOURCE 数据库资源角色
- CONNECT 数据库连接角色

**用户自定义角色：**
- 创建角色
	```sql
	CREATE ROLE 角色名称
	```
- 为角色授予权限和回收权限
	```sql
	GRANT 系统权限 TO 角色名称;
	-- 对象权限的授予同上。
	```
- 将角色授予用户
	```sql
	GRANT 角色名 TO 用户名;
	```
- 删除角色
	```sql
	DROP ROLE 角色名称;
	```


## 管理用户

**修改用户：**
```sql
ALTER USER zhangsan INDENTIFIED BY ora
DEFAULT TABLESPACE users
QUOTA UNLIMITED ON users;
```

**启用和禁用账户：**
```sql
ALTER USER 用户名 ACCOUNT LOCK;
ALTER USRE 用户名 ACCOUNT UNLOCK;
```

**删除用户：**
```sql
DROP USER 用户名;
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY1NDIxNDMyOV19
-->