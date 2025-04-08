# SQL语言

<div class="grid cards" markdown>

-   :material-history:{ .lg .middle } __SQL语言历史__

    ---
    IBM公司在San Jose研究实验室开发了Sequel语言（System R项目的一部分）
    后改名为结构化查询语言（Structured Query Language, SQL）
    ANSI和ISO标准SQL的发展历程：
    SQL-86, SQL-89, SQL-92
    SQL:1999, SQL:2003, SQL:2008, SQL:2011, SQL:2016, SQL:2023
    商业系统提供SQL-92的大部分或全部特性，以及后续标准的各种特性集和专有特性

-   :material-database:{ .lg .middle } __SQL语言组成__     
    
    ---
    SQL语言主要包含以下几个部分：
    - 数据定义语言（DDL）：用于定义数据库结构，如创建表、修改表等
    - 数据操作语言（DML）：用于查询和修改数据
    - 数据控制语言（DCL）：用于授权和访问控制
    - 事务控制语言（TCL）：用于事务控制

</div>

## 数据定义语言（DDL）

SQL的数据定义语言允许指定以下信息：

- 每个关系的模式
- 与每个属性相关的值的类型
- 完整性约束
- 每个关系要维护的索引集
- 每个关系的安全和授权信息
- 每个关系在磁盘上的物理存储结构

### 数据类型

<div class="grid cards" markdown>

-   :material-text:{ .lg .middle } __字符串类型__

    ---
    - `char(n)`：固定长度字符串，用户指定长度n
    - `varchar(n)`：可变长度字符串，用户指定最大长度n

-   :material-numeric:{ .lg .middle } __数值类型__

    ---
    - `int`：整数（机器相关的整数子集）
    - `smallint`：小整数（机器相关的整数子集）
    - `numeric(p,d)`：定点数，精度为p位数字，小数点右侧d位
    - `real`：单精度浮点数
    - `double precision`：双精度浮点数
    - `float(n)`：浮点数，至少n位精度

-   :material-calendar:{ .lg .middle } __日期和时间类型__

    ---
    - `date`：日期类型
    - `time`：时间类型
    - `timestamp`：时间戳，包含日期和时间

</div>

### 表的创建和修改

<div class="grid cards" markdown>

-   :material-table-plus:{ .lg .middle } __创建表__

    ---
    ```sql
    CREATE TABLE r (
        A1 D1,
        A2 D2,
        ...,
        An Dn,
        完整性约束1,
        ...,
        完整性约束k
    );
    ```

    示例：
    ```sql
    CREATE TABLE instructor (
        ID CHAR(5),
        name VARCHAR(20) NOT NULL,
        dept_name VARCHAR(20),
        salary NUMERIC(8,2),
        PRIMARY KEY (ID),
        FOREIGN KEY (dept_name) REFERENCES department
    );
    ```

-   :material-table-remove:{ .lg .middle } __删除表__

    ---
    ```sql
    DROP TABLE r;
    ```

-   :material-table-edit:{ .lg .middle } __修改表__

    ---
    ```sql
    ALTER TABLE r ADD A D;
    ALTER TABLE r DROP A;
    ```

-   :material-key:{ .lg .middle } __完整性约束__

    ---
    - `NOT NULL`：属性不能为空
    - `PRIMARY KEY (A1, ..., An)`：指定主键
    - `FOREIGN KEY (A1, ..., An) REFERENCES 表名`：指定外键
    - 主键声明自动确保属性非空

</div>

## 数据操作语言（DML）

SQL的数据操作语言提供了查询信息、插入、删除和更新元组的能力。

### 基本查询结构

<div class="grid cards" markdown>

-   :material-magnify:{ .lg .middle } __查询基本形式__

    ---
    ```sql
    SELECT A1, A2, ..., An
    FROM r1, r2, ..., rm
    WHERE P;
    ```

    - Ai表示属性
    - ri表示关系
    - P是谓词
    - SQL查询的结果是一个关系

-   :material-filter-outline:{ .lg .middle } __结果集重复处理__

    ---
    默认情况下，SQL查询结果不删除重复项

    使用DISTINCT关键字强制消除重复项：
    ```sql
    SELECT DISTINCT dept_name
    FROM instructor;
    ```

    使用ALL关键字明确指定不删除重复项：
    ```sql
    SELECT ALL dept_name
    FROM instructor;
    ```

-   :material-asterisk:{ .lg .middle } __查询所有属性__

    ---
    使用星号*表示选择所有属性：
    ```sql
    SELECT *
    FROM instructor;
    ```

    可以在SELECT子句中包含算术表达式：
    ```sql
    SELECT ID, name, salary * 1.1
    FROM instructor;
    ```

</div>

### WHERE子句和连接

<div class="grid cards" markdown>

-   :material-filter:{ .lg .middle } __WHERE子句__

    ---
    WHERE子句指定结果必须满足的条件

    条件可以使用逻辑连接词AND、OR和NOT组合

    比较可以应用于算术表达式的结果

    示例：
    ```sql
    SELECT name
    FROM instructor
    WHERE dept_name = 'Comp. Sci.' AND salary > 70000;
    ```

-   :material-vector-combine:{ .lg .middle } __笛卡尔积__

    ---
    FROM子句列出查询中涉及的关系

    对应关系代数的笛卡尔积操作

    示例：
    ```sql
    SELECT *
    FROM instructor, teaches;
    ```

    笛卡尔积本身不太有用，但与WHERE子句条件结合时很有用

-   :material-table-merge:{ .lg .middle } __连接操作__

    ---
    查找教师姓名和他们教授的课程ID：
    ```sql
    SELECT name, course_id
    FROM instructor, teaches
    WHERE instructor.ID = teaches.ID;
    ```

    自然连接（NATURAL JOIN）匹配所有共同属性相同值的元组：
    ```sql
    SELECT name, course_id
    FROM instructor NATURAL JOIN teaches;
    ```

    自然连接的危险：注意名称相同但无关的属性会被错误地等同

</div>

### 附加查询功能

<div class="grid cards" markdown>

-   :material-rename:{ .lg .middle } __重命名__

    ---
    SQL允许使用AS子句重命名关系和属性：
    ```sql
    SELECT T.name, S.course_id
    FROM instructor AS T, teaches AS S
    WHERE T.ID = S.ID;
    ```

    AS关键字是可选的，可以省略（Oracle中必须省略）

    重命名对于自连接特别有用：
    ```sql
    SELECT X.course_id
    FROM section X, section Y
    WHERE X.course_id = Y.course_id AND 
          X.semester = Y.semester AND
          X.year = Y.year AND
          X.sec_id <> Y.sec_id;
    ```

-   :material-format-text:{ .lg .middle } __字符串操作__

    ---
    SQL包括用于字符串比较的匹配操作符LIKE

    使用两个特殊字符：

    - 百分号（%）：匹配任何子串
    - 下划线（_）：匹配任何单个字符

    示例：
    ```sql
    SELECT name
    FROM instructor
    WHERE name LIKE 'Sm_th';

    SELECT name
    FROM instructor
    WHERE name LIKE '%son';
    ```

    模式匹配区分大小写

-   :material-order-alphabetical-ascending:{ .lg .middle } __排序__

    ---
    ORDER BY子句指定结果的排序方式：
    ```sql
    SELECT name
    FROM instructor
    WHERE dept_name = 'Physics'
    ORDER BY name;
    ```

    可以指定DESC（降序）或ASC（升序），默认为升序

    可以按多个属性排序：
    ```sql
    SELECT name, salary
    FROM instructor
    ORDER BY salary DESC, name ASC;
    ```

-   :material-compare:{ .lg .middle } __比较操作符__

    ---
    BETWEEN比较操作符：
    ```sql
    SELECT name, salary
    FROM instructor
    WHERE salary BETWEEN 90000 AND 100000;
    ```

    元组比较：
    ```sql
    SELECT name, course_id
    FROM instructor, teaches
    WHERE (instructor.ID, dept_name) = (teaches.ID, 'Biology');
    ```

</div>

### 集合操作

<div class="grid cards" markdown>

-   :material-set-all:{ .lg .middle } __集合操作概述__

    ---
    SQL支持三种基本的集合操作：
    - UNION（并集）
    - INTERSECT（交集）
    - EXCEPT（差集）

    这些操作会自动消除重复项。

-   :material-set-merge:{ .lg .middle } __并集（UNION）__

    ---
    查找2009年秋季或2010年春季开设的所有课程：
    ```sql
    (SELECT course_id
     FROM section
     WHERE semester = 'Fall' AND year = 2009)
    UNION
    (SELECT course_id
     FROM section
     WHERE semester = 'Spring' AND year = 2010);
    ```

-   :material-set-center:{ .lg .middle } __交集（INTERSECT）__

    ---
    查找2009年秋季和2010年春季都开设的课程：
    ```sql
    (SELECT course_id
     FROM section
     WHERE semester = 'Fall' AND year = 2009)
    INTERSECT
    (SELECT course_id
     FROM section
     WHERE semester = 'Spring' AND year = 2010);
    ```

-   :material-set-left:{ .lg .middle } __差集（EXCEPT）__

    ---
    查找2009年秋季开设但2010年春季未开设的课程：
    ```sql
    (SELECT course_id
     FROM section
     WHERE semester = 'Fall' AND year = 2009)
    EXCEPT
    (SELECT course_id
     FROM section
     WHERE semester = 'Spring' AND year = 2010);
    ```

-   :material-set-all:{ .lg .middle } __多重集合操作__

    ---
    保留所有重复项的多重集合版本：

    - UNION ALL
    - INTERSECT ALL
    - EXCEPT ALL

    如果元组在r中出现m次，在s中出现n次，则：

    - 在r UNION ALL s中出现m+n次
    - 在r INTERSECT ALL s中出现min(m,n)次
    - 在r EXCEPT ALL s中出现max(0,m-n)次

</div>

### NULL值处理

<div class="grid cards" markdown>

-   :material-null:{ .lg .middle } __NULL值基础__

    ---
    - 元组的属性可能有空值，用NULL表示
    - NULL表示未知值或不存在的值
    - 任何涉及NULL的算术表达式结果都是NULL
      - 例如：5 + NULL 返回 NULL
    - 使用IS NULL谓词检查NULL值：
    ```sql
    SELECT name
    FROM instructor
    WHERE salary IS NULL;
    ```

-   :material-toggle-switch:{ .lg .middle } __NULL的三值逻辑__

    ---
    与NULL的任何比较都返回UNKNOWN
    例如：5 < NULL 或 NULL <> NULL 或 NULL = NULL

    使用真值UNKNOWN的三值逻辑：

    - OR：(UNKNOWN OR TRUE) = TRUE,
      (UNKNOWN OR FALSE) = UNKNOWN,
      (UNKNOWN OR UNKNOWN) = UNKNOWN
    - AND：(TRUE AND UNKNOWN) = UNKNOWN,
      (FALSE AND UNKNOWN) = FALSE,
      (UNKNOWN AND UNKNOWN) = UNKNOWN
    - NOT：(NOT UNKNOWN) = UNKNOWN

    WHERE子句谓词结果为UNKNOWN时视为FALSE

</div>

### 聚合函数

<div class="grid cards" markdown>

-   :material-function:{ .lg .middle } __基本聚合函数__

    ---
    这些函数对关系列的值多重集合进行操作，并返回一个值：
    - avg：平均值
    - min：最小值
    - max：最大值
    - sum：值的总和
    - count：值的数量

    示例：
    ```sql
    SELECT AVG(salary)
    FROM instructor
    WHERE dept_name = 'Comp. Sci.';
    ```

-   :material-group:{ .lg .middle } __分组聚合（GROUP BY）__

    ---
    查询每个系的教师平均工资：
    ```sql
    SELECT dept_name, AVG(salary) as avg_salary
    FROM instructor
    GROUP BY dept_name;
    ```

    注意：没有教师的系不会出现在结果中

    GROUP BY子句中未出现的非聚合属性不能出现在SELECT子句中

    错误查询示例：
    ```sql
    SELECT dept_name, ID, AVG(salary)
    FROM instructor
    GROUP BY dept_name;
    ```

-   :material-filter-check:{ .lg .middle } __分组过滤（HAVING）__

    ---
    HAVING子句用于筛选分组后的结果：
    ```sql
    SELECT dept_name, AVG(salary) as avg_salary
    FROM instructor
    GROUP BY dept_name
    HAVING AVG(salary) > 42000;
    ```

    HAVING子句中的谓词在形成分组后应用
    WHERE子句中的谓词在形成分组前应用

-   :material-null:{ .lg .middle } __NULL值与聚合__

    ---
    除COUNT(*)外的所有聚合操作都忽略聚合属性上的NULL值

    如果只有NULL值的集合：

    - COUNT返回0
    - 所有其他聚合返回NULL

</div>

### 嵌套子查询

<div class="grid cards" markdown>

-   :material-layers-outline:{ .lg .middle } __子查询概述__

    ---
    - SQL提供嵌套子查询的机制
    - 子查询是嵌套在另一个查询中的SELECT-FROM-WHERE表达式
    - 子查询在WHERE子句中时，涉及集合的谓词项：
        - 属性值是否在子集中
        - 属性值是否大于子集中的某些或所有值
        - 子集是否为空
        - 子集基数是否<=1
    - 子查询在FROM子句中时，就是一个临时关系

-   :material-set-right:{ .lg .middle } __IN子查询__

    ---
    查找作为学生顾问的教师姓名：
    ```sql
    SELECT DISTINCT name
    FROM instructor
    WHERE ID IN (SELECT i_ID 
                 FROM advisor);
    ```

    查找2009年秋季和2010年春季都开设的课程：
    ```sql
    SELECT course_id
    FROM section
    WHERE semester = 'Fall' AND year = 2009 AND
          course_id IN (SELECT course_id
                        FROM section
                        WHERE semester = 'Spring' AND year = 2010);
    ```

-   :material-sort:{ .lg .middle } __比较子查询__

    ---
    使用SOME子句：
    ```sql
    SELECT name
    FROM instructor
    WHERE salary > SOME (SELECT salary
                         FROM instructor
                         WHERE dept_name = 'Biology');
    ```

    使用ALL子句：
    ```sql
    SELECT name
    FROM instructor
    WHERE salary > ALL (SELECT salary
                        FROM instructor
                        WHERE dept_name = 'Biology');
    ```

-   :material-checkbox-marked:{ .lg .middle } __EXISTS子查询__

    ---
    EXISTS构造当子查询非空时返回TRUE：
    ```sql
    SELECT course_id
    FROM section S
    WHERE semester = 'Fall' AND year = 2009 AND
          EXISTS (SELECT *
                  FROM section T
                  WHERE semester = 'Spring' AND year = 2010
                        AND S.course_id = T.course_id);
    ```

    查找2009年秋季和2010年春季都开设的课程：
    ```sql
    SELECT course_id
    FROM section S
    WHERE semester = 'Fall' AND year = 2009 AND
          EXISTS (SELECT *
                  FROM section T
                  WHERE semester = 'Spring' AND year = 2010
                        AND S.course_id = T.course_id);
    ```

    相关子查询中的属性称为相关变量

-   :material-check-all:{ .lg .middle } __UNIQUE子查询__

    ---
    UNIQUE构造测试子查询结果是否有重复元组

    查找2009年最多开设一次的所有课程：
    ```sql
    SELECT T.course_id
    FROM course T
    WHERE UNIQUE (SELECT R.course_id
                  FROM section R
                  WHERE T.course_id = R.course_id
                        AND R.year = 2009);
    ```

</div>

### 高级查询技术

<div class="grid cards" markdown>

-   :material-table-query:{ .lg .middle } __FROM子句中的子查询__

    ---
    SQL允许在FROM子句中使用子查询表达式

    查找平均工资大于$42,000的系的平均工资：
    ```sql
    SELECT dept_name, avg_salary
    FROM (SELECT dept_name, AVG(salary) as avg_salary
          FROM instructor
          GROUP BY dept_name)
    WHERE avg_salary > 42000;
    ```

    另一种写法：
    ```sql
    SELECT dept_name, AVG(salary) as avg_salary
    FROM instructor
    GROUP BY dept_name
    HAVING AVG(salary) > 42000;
    ```

-   :material-table-sync:{ .lg .middle } __LATERAL子句__

    ---
    LATERAL子句允许FROM子句的后面部分访问前面部分的相关变量：
    ```sql
    SELECT name, salary, avg_salary
    FROM instructor I,
         LATERAL (SELECT AVG(salary) as avg_salary
                  FROM instructor
                  WHERE dept_name = I.dept_name);
    ```

    LATERAL是SQL标准的一部分，但许多数据库系统不支持

-   :material-table-plus:{ .lg .middle } __WITH子句__

    ---
    WITH子句提供了一种定义临时视图的方法，该视图仅对包含WITH子句的查询有效

    查找具有最大预算的所有系：
    ```sql
    WITH max_budget(value) AS
        (SELECT MAX(budget)
         FROM department)
    SELECT department.dept_name
    FROM department, max_budget
    WHERE department.budget = max_budget.value;
    ```

    WITH子句对编写复杂查询非常有用，类似关系代数中的赋值操作

-   :material-alpha:{ .lg .middle } __标量子查询__

    ---
    标量子查询用于期望单个值的地方：
    ```sql
    SELECT dept_name,
           (SELECT COUNT(*)
            FROM instructor
            WHERE department.dept_name = instructor.dept_name)
         AS num_instructors
    FROM department;
    ```

    如果子查询返回多个结果元组，会导致运行时错误

    标量子查询不符合关系代数的概念，但在实践中经常使用

</div>

## 数据库修改操作

<div class="grid cards" markdown>

-   :material-delete:{ .lg .middle } __删除操作__

    ---
    删除指定关系中的元组：
    ```sql
    DELETE FROM r
    WHERE P;
    ```

-   :material-database-plus:{ .lg .middle } __插入操作__

    ---
    向课程表添加新元组：
    ```sql
    INSERT INTO course
    VALUES ('CS-437', 'Database Systems', 'Comp. Sci.', 4);
    ```

    向学生表添加tot_creds设为NULL的新元组：
    ```sql
    INSERT INTO student(ID, name, dept_name)
    VALUES ('3003', 'Green', 'Finance');
    ```

    将所有教师添加到学生关系，tot_creds设为0：
    ```sql
    INSERT INTO student
    SELECT ID, name, dept_name, 0
    FROM instructor;
    ```

-   :material-pencil:{ .lg .middle } __更新操作__

    ---
    将教师工资提高5%：
    ```sql
    UPDATE instructor
    SET salary = salary * 1.05;
    ```

    使用CASE语句有条件地更新：
    ```sql
    UPDATE instructor
    SET salary = CASE
                 WHEN salary <= 100000 THEN salary * 1.05
                 ELSE salary * 1.03
                 END;
    ```

    重新计算并更新所有学生的tot_creds值：
    ```sql
    UPDATE student S
    SET tot_cred = (SELECT SUM(credits)
                    FROM takes, course
                    WHERE takes.course_id = course.course_id AND
                          S.ID = takes.ID AND
                          takes.grade <> 'F' AND
                          takes.grade IS NOT NULL);
    ```

</div>

## 其他SQL功能

创建与现有表具有相同模式的表：
```sql
CREATE TABLE temp_instructor LIKE instructor;
```

SQL还提供了许多其他功能，包括：

- 视图定义
- 事务控制
- 索引创建和使用
- 授权
- 触发器
- 存储过程
- 高级数据类型（如XML、JSON）

## pdf资料

<embed src="pdfs/ch3(4).pdf" type="application/pdf" width="100%" height="400px" />