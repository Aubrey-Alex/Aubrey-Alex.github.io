!!! note "MySQL"
    观看资源为bilibili黑马程序员的MySQL视频教程

## 相关概念
| 名称|简称 |说明|
| --- | --- |---|
| 数据库 |DataBase(DB)| 数据库是按照数据结构来组织、存储和管理数据的仓库。|
| 数据库管理系统 |Database Management System(DBMS)| 数据库管理系统是管理数据库的软件，它是数据库系统的核心，负责数据库的创建、维护、使用和管理。|
| SQL |Structured Query Language(SQL)| SQL是一种用于管理关系数据库的语言，用于存取、更新和管理关系数据库中的数据。|
|关系型数据库|Relational Database Management System（RDBMS）| 关系型数据库是指采用关系模型来组织数据，并以表的形式存储数据的数据库。主流数据库管理系统：MySQL、Oracle、SQL Server（微软）、PostgreSQL、SQLite（安卓内置）|

- MySQL启动
    - `net start mysql80`
    - `net stop mysql80`
- MySQL客户端连接
    - `mysql [-h 127.0.0.1] [-P 3306] -u root -p`

## SQL
### SQL通用语法
1. 可以单行或者多行书写，以分号结尾
2. 使用空格或者缩进来增强可读性
3. 不区分大小写，关键字建议全部大写
4. 注释：
    - 单行注释： `-- 注释内容`
    - 多行注释： `/* 注释内容 */`

### SQL分类
|分类|全称|说明|
| --- | --- |---|
|DDL(Data Definition Language)|数据定义语言|用于定义数据库对象，如数据库、表、视图、索引等。|
|DML(Data Manipulation Language)|数据操纵语言|用于操作数据库对象，如插入、删除、更新数据。|
|DQL(Data Query Language)|数据查询语言|用于查询数据库对象，如检索、搜索数据。|
|DCL(Data Control Language)|数据控制语言|用于控制数据库对象，如事务、权限管理等。|

### DDL
#### 数据库操作
- 查询：
	- `SHOW DATABASES;`查看所有数据库
	- `SELECT DATABASE();`查看当前数据库
- 创建：
	- `CREATE DATABASE [IF NOT EXISTS] 数据库名; [DEFAULT CHARACTER SET 字符集] [COLLATE 排序规则];`如果数据库不存在，则创建数据库（utf8为三个字节，utf8mb4为四字节）
- 删除：
	- `DROP DATABASE [IF EXISTS] 数据库名;`删除数据库，如果数据库不存在，则提示错误
- 使用：
	- `USE 数据库名;`使用数据库


