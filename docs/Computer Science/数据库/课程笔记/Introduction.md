# 介绍

## 数据

- 三个层级看待数据
    - 物理层（Physical level）：存储数据在磁盘上的位置、组织方式、存储介质等
    - 逻辑层（Logical level）：数据在数据库中的逻辑结构，如表、视图、索引等
    - 关系层（View level）：数据之间的联系，如主键、外键、关系等

## 数据模型

| 数据模型 | 特点 |
| --- | --- |
| 实体-联系模型（Entity-Relationship Model） | 实体（Entity）：客观存在的事物，如人、物、事等；联系（Relationship）：实体之间的联系，如夫妻、师生、合作等；属性（Attribute）：实体的特征，如姓名、年龄、性别等； |
| 关系模型（Relational Model） | 关系（Relation）：二维表格，每一行代表一个记录，每一列代表一个属性；域（Domain）：属性的取值范围，如整数、实数、字符串等；关键字（Key）：唯一标识一个记录的属性，如主键、外键等； |
| 对象模型（Object-based Model） | 对象（Object）：客观事物的抽象，如学生、课程、书籍等；属性（Attribute）：对象特征，如姓名、年龄、性别等；操作（Operation）：对象可以执行的操作，如学习、上课等； |
| 半结构化数据模型（Semistructured Data Model） | 半结构化数据（Semistructured Data）：数据结构不固定，如XML、JSON、HTML等； |

## 关系模型

|概念|解释|
|---|---|
|Data Definition Language (DDL)|数据定义语言，用于定义数据库的结构，如创建、删除、修改表、视图、索引等。|
|Data Manipulation Language (DML)|数据操纵语言，用于操作数据库中的数据，如插入、删除、更新数据等。|
|Data Query Language (DQL)|数据查询语言，用于查询数据库中的数据，如SELECT、WHERE、ORDER BY等。|
|SQL|结构化查询语言，是关系模型的标准语言，用于定义、操纵、查询关系型数据库。|

