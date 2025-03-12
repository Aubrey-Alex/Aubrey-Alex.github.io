# SQL 介绍

## DDL

|数据类型|解释|
|:--:|:--:|
|char(n)|定长字符串，n为字符数|
|varchar(n)|变长字符串，n为字符数|
|int|整型|
|smallint|短整型|
|numeric(p,s)|定点数，p为总位数，s为小数点后位数|
|real|单精度浮点数|
|double precision|双精度浮点数|
|date|日期|
|timestamp|时间戳|
|float(n)|浮点数,n为总位数|

## 表操作

=== "创建表"
    ```sql
        CREATE TABLE takes (      
            ID              varchar(5)  not null,        
            course_id       varchar(8),      
            sec_id          varchar(8),        
            semester        varchar(6),        
            year            numeric(4,0),        
            grade           varchar(2),        
            primary key (ID, course_id, sec_id, semester, year),        
            foreign key (ID) references  student,        
            foreign key (course_id, sec_id, semester, year) references section 
        );
    ```
    
=== "删除表"
    ```sql
        # 删除表和内容
        DROP TABLE takes;
        # 删除内容
        DELETE FROM takes;
    ```

=== "更改属性"
    ```sql
        # 增加列
        ALTER TAABLE r ADD A D;
        # 删除列
        ALTER TABLE r DROP A;
    ```

## DML

### 查询

- 查询顺序
  from -> where -> group by -> having -> select -> order by

#### 简单查询

- select:distinct来去重，默认是不去重的，select * 代表所有列
- from:指定查询的表
- where:指定条件，返回满足条件的行，可使用and、or、between and、in等运算符
  

#### 连接

1. 内连接(inner join):返回两个表中都存在的行
2. 外连接(outer join):返回两个表中存在的行，并在结果中显示表中没有的行，lest/right/full outer join

#### 更名

- from语句使用as关键字来给表或列重新命名

#### 显示次序

- order by:指定结果的显示次序，可使用asc/desc关键字指定升序/降序

#### 集合运算

- union:合并两个结果集，去重
- intersect:返回两个结果集的交集，去重
- except:返回两个结果集的差集，去重

#### 聚合函数

- 固有聚合函数：
  - count:计算行数
  - sum:求和
  - avg:平均值
  - max:最大值
  - min:最小值
!!! note "注意"
    需保证任何没有出现在group by 子句中的属性，如果出现在select/having语句中，则必须在聚集函数中。
!!! example "运用示例"
    ```sql title="查询每个部门的平均工资大于10000的员工人数"
        SELECT COUNT(distinct name) 
        FROM table_name1 
        GROUP BY dept_name 
        HAVING AVG(salary) > 10000;
    ```

#### 空值

- is null:判断某个字段是否为空
- is not null:判断某个字段是否不为空
- is unknown:判断某个字段是否未知
- is not unknown:判断某个字段是否不未知

#### 嵌套子查询

- 子查询是嵌套在另一个查询中的select - from - where 表达式。对于 where 后面任意关系可以出现的位置，都可以被替换为一个子查询。用否在子查询返回的关系中。来自外层查询中重命名的相关名称可以用在子查询中，称为相关子查询。
- 集合的比较：some + subquery ：存在某值即可；all + subquery ：所有值都满足
- 空关系测试：exists + subquery 判断是否存在某关系。如not exists(B except A) 判断关系 A 包含关系 B。
- 重复元组存在性测试：unique 判断查询结果无重复元组。
- from 中的子查询：子查询不能使用子句中其他关系的相关变量，除非用lateral 作为前缀。
- with 子句：定义临时关系，这个定义只对包含with 子句的查询有效。

### SQL修改

```sql
    # 插入 
    INSERT INTO ... VALUES(e1, e2, ...) 
    # 删除
    DELETE FROM person WHERE name != '张三' 
    # 更新
    UPDATE person 
    SET salary = case 
        WHEN name = '李四' 
            THEN salary * 2 
        ELSE salary / 2 
        end
```