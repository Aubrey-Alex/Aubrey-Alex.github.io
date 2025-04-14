# 高级SQL

<div class="grid cards" markdown>

-   :material-database-plus:{ .lg .middle } __介绍__

    ---
    本章介绍SQL的高级特性，包括：
    - 连接表达式
    - 事务
    - 视图
    - 索引
    - 完整性约束
    - SQL数据类型
    - 授权

</div>

## 连接表达式(Join Expressions)

<div class="grid cards" markdown>

-   :material-table:{ .lg .middle } __连接操作基础__

    ---
    - 连接操作接受两个关系并返回一个结果关系
    - 这些操作通常作为from子句中的子查询表达式使用
    - 连接类型：定义如何处理在另一个关系中没有匹配元组的元组
    - 连接条件：定义两个关系中哪些元组匹配，以及结果中包含哪些属性
    - 自然连接消除重复属性

-   :material-table-multiple:{ .lg .middle } __外连接(Outer Join)类型__

    ---
    外连接保留在另一个关系中没有匹配的元组：
    
    - **左外连接(Left Outer Join)**：保留左侧关系中所有元组
    - **右外连接(Right Outer Join)**：保留右侧关系中所有元组
    - **全外连接(Full Outer Join)**：保留两个关系中所有元组
    
    未匹配的元组在结果中相应属性用null填充

-   :material-code-brackets:{ .lg .middle } __连接语法示例__

    ---
    **自然连接(Natural Join)**
    ```sql
    course NATURAL RIGHT OUTER JOIN prereq
    course NATURAL FULL OUTER JOIN prereq
    ```
    
    **使用using子句**
    ```sql
    course FULL OUTER JOIN prereq USING (course_id)
    
    -- 避免自然连接潜在危险的例子
    SELECT name, title
    FROM (student NATURAL JOIN takes) JOIN course USING (course_id)
    ```
    
    **使用on子句**
    ```sql
    course INNER JOIN prereq ON course.course_id = prereq.course_id
    course LEFT OUTER JOIN prereq ON course.course_id = prereq.course_id
    ```

</div>

## 事务(Transactions)

<div class="grid cards" markdown>

-   :material-swap-horizontal:{ .lg .middle } __事务概念__

    ---
    - 事务是由一系列查询和/或更新语句组成的工作单元
    - 事务具有原子性：要么完全执行，要么回滚(就像从未发生过一样)
    - 事务在并发环境中彼此隔离

-   :material-play-circle-outline:{ .lg .middle } __事务控制__

    ---
    - 事务隐式开始
    - 事务结束方式：
      - `COMMIT WORK`：事务执行的更新在数据库中永久保存
      - `ROLLBACK WORK`：撤销事务中所有SQL语句执行的更新
    - 大多数数据库默认：每个SQL语句自动提交
      - 可以为会话关闭自动提交(通过API)
      - SQL:1999标准中可以使用：`BEGIN ATOMIC ... END`(大多数数据库不支持)

</div>

## 视图(Views)

<div class="grid cards" markdown>

-   :material-eye-outline:{ .lg .middle } __视图概念__

    ---
    - 在某些情况下，不希望所有用户都看到整个逻辑模型(即数据库中存储的所有实际关系)
    - 视图提供了一种机制，可以对某些用户隐藏某些数据
    - 视图是一种"虚拟关系"，不属于概念模型但对用户可见

-   :material-eye-plus:{ .lg .middle } __视图定义__

    ---
    使用`CREATE VIEW`语句定义视图：
    ```sql
    CREATE VIEW v AS <查询表达式>
    ```
    
    其中`<查询表达式>`是任何合法的SQL表达式，`v`代表视图名称。
    
    一旦定义了视图，视图名称可以用来引用该视图生成的虚拟关系。
    
    视图定义不同于通过评估查询表达式创建新关系，而是保存表达式，在使用视图的查询中替换该表达式。

-   :material-file-document-outline:{ .lg .middle } __视图示例__

    ---
    **不含薪资的教师视图**
    ```sql
    CREATE VIEW faculty AS
    SELECT ID, name, dept_name
    FROM instructor
    ```
    
    **查找所有生物系教师**
    ```sql
    SELECT name
    FROM faculty
    WHERE dept_name = 'Biology'
    ```
    
    **系部薪资总和视图**
    ```sql
    CREATE VIEW departments_total_salary(dept_name, total_salary) AS
    SELECT dept_name, SUM(salary)
    FROM instructor
    GROUP BY dept_name
    ```
    
    **查询薪资总和高于平均值的系**
    ```sql
    SELECT dept_name 
    FROM departments_total_salary 
    WHERE total_salary > (SELECT AVG(total_salary) FROM departments_total_salary)
    ```

-   :material-file-tree:{ .lg .middle } __视图依赖关系__

    ---
    - 一个视图可以在定义另一个视图的表达式中使用
    - 视图关系v1直接依赖于视图关系v2，如果v2用于定义v1的表达式中
    - 视图关系v1依赖于视图关系v2，如果v1直接依赖于v2或存在从v1到v2的依赖路径
    - 如果视图依赖于自身，则称为递归视图

-   :material-cog-refresh:{ .lg .middle } __视图扩展__

    ---
    定义以其他视图为基础的视图含义的方法：
    
    1. 假设视图v1由表达式e1定义，e1可能包含对视图关系的使用
    2. 视图扩展重复以下替换步骤：
       - 在e1中找到任何视图关系vi
       - 将视图关系vi替换为定义vi的表达式
       - 重复直到e1中不再存在视图关系
    
    只要视图定义不是递归的，这个循环最终会终止

-   :material-database-edit:{ .lg .middle } __视图更新__

    ---
    对视图的插入、更新和删除操作会转换为对基本关系的操作。
    
    **例如**：向之前定义的faculty视图添加新元组
    ```sql
    INSERT INTO faculty VALUES ('30765', 'Green', 'Music')
    ```
    
    这个插入必须表示为向instructor关系插入元组
    ```sql
    ('30765', 'Green', 'Music', null)
    ```
    
    大多数SQL实现仅允许对简单视图进行更新：
    - from子句只有一个数据库关系
    - select子句只包含关系的属性名称，没有表达式、聚合或distinct说明
    - 未在select子句中列出的任何属性都可以设置为null
    - 查询没有group by或having子句

-   :material-shield-check:{ .lg .middle } __带检查选项的视图__

    ---
    例如，创建历史系教师视图：
    ```sql
    CREATE VIEW history_instructors AS
    SELECT *
    FROM instructor
    WHERE dept_name = 'History'
    ```
    
    如果我们尝试插入生物系教师到历史系教师视图中：
    ```sql
    INSERT INTO history_instructors VALUES ('25566', 'Brown', 'Biology', 100000)
    ```
    
    为避免插入非历史系教师，可以在创建视图时使用WITH CHECK OPTION：
    ```sql
    CREATE VIEW history_instructors AS
    SELECT *
    FROM instructor
    WHERE dept_name = 'History'
    WITH CHECK OPTION
    ```

-   :material-database-refresh:{ .lg .middle } __物化视图__

    ---
    - 物化视图：创建一个物理表，包含定义视图的查询结果中的所有元组
    - 如果查询中使用的关系被更新，物化视图结果会过时
    - 需要维护视图，在基础关系更新时更新视图

</div>

## 索引(Index)

<div class="grid cards" markdown>

-   :material-file-search:{ .lg .middle } __索引基础__

    ---
    索引是用于加速访问具有指定索引属性值的记录的数据结构。
    
    ```sql
    CREATE TABLE student
    (ID varchar(5),
     name varchar(20) NOT NULL,
     dept_name varchar(20),
     tot_cred numeric(3,0) DEFAULT 0,
     PRIMARY KEY (ID))
    
    CREATE INDEX studentName_index ON student(name)
    ```
    
    当执行以下查询时：
    ```sql
    SELECT * FROM student WHERE name = 'Mike'
    ```
    
    可以使用索引找到所需的记录，而无需查看student的所有记录。

</div>

## 完整性约束(Integrity Constraints)

<div class="grid cards" markdown>

-   :material-shield-check:{ .lg .middle } __完整性约束概念__

    ---
    完整性约束防止数据库意外损坏，确保对数据库的授权更改不会导致数据一致性丢失。
    
    例如：
    - 支票账户必须有大于$10,000.00的余额
    - 银行员工的薪资必须至少为每小时$4.00
    - 客户必须有(非空)电话号码

-   :material-shield-half-full:{ .lg .middle } __SQL中的约束类型__

    ---
    - NOT NULL
    - PRIMARY KEY
    - UNIQUE
    - CHECK(P)，其中P是谓词

-   :material-null:{ .lg .middle } __NOT NULL约束__

    ---
    声明name和budget不能为空：
    ```sql
    name varchar(20) NOT NULL
    budget numeric(12,2) NOT NULL
    ```

-   :material-key-variant:{ .lg .middle } __UNIQUE约束__

    ---
    UNIQUE(A1, A2, ..., Am)规定属性A1, A2, ... Am形成候选键。
    
    候选键允许为空(与主键相反)。

-   :material-check-circle:{ .lg .middle } __CHECK约束__

    ---
    CHECK(P)指定谓词P，关系中的每个元组都必须满足这个谓词。
    
    例如，确保semester是fall、winter、spring或summer之一：
    ```sql
    CREATE TABLE section (
        course_id varchar(8),
        sec_id varchar(8),
        semester varchar(6),
        year numeric(4,0),
        building varchar(15),
        room_number varchar(7),
        time_slot_id varchar(4),
        PRIMARY KEY (course_id, sec_id, semester, year),
        CHECK (semester IN ('Fall', 'Winter', 'Spring', 'Summer'))
    )
    ```

-   :material-key-link:{ .lg .middle } __外键约束__

    ---
    确保在一个关系中出现的某组属性的值也出现在另一个关系中的某组属性中。
    
    例如：如果"Biology"是instructor关系中某个元组的系名，那么在department关系中必须存在"Biology"系的元组。
    
    定义：设A是一组属性。设R和S是包含属性A的两个关系，且A是S的主键。如果R中出现的A的任何值在S中也出现，则A称为R的外键。
    
    外键可以为空。
    
    **示例**：
    ```sql
    CREATE TABLE course (
        course_id char(5) PRIMARY KEY,
        title varchar(20),
        dept_name varchar(20) REFERENCES department
    )
    ```
    
    或
    
    ```sql
    CREATE TABLE course (
        course_id char(5) PRIMARY KEY,
        title varchar(20),
        dept_name varchar(20),
        FOREIGN KEY (dept_name) REFERENCES department
            ON DELETE CASCADE
            ON UPDATE CASCADE
    )
    ```
    
    CASCADE的替代操作：SET NULL, SET DEFAULT

-   :material-family-tree:{ .lg .middle } __外键的递归依赖__

    ---
    例如：
    ```sql
    CREATE TABLE person (
        ID char(10),
        name char(40),
        mother char(10),
        father char(10),
        PRIMARY KEY ID,
        FOREIGN KEY father REFERENCES person,
        FOREIGN KEY mother REFERENCES person
    )
    ```
    
    如何插入元组而不违反约束？
    - 在插入person之前插入其father和mother
    - 或者，最初将father和mother设置为null，在插入所有persons后更新(如果father和mother属性声明为NOT NULL则不可行)

-   :material-check-decagram:{ .lg .middle } __高级约束声明__

    ---
    **使用检查子句中的子查询**：
    ```sql
    CHECK (time_slot_id IN (SELECT time_slot_id FROM time_slot))
    ```
    
    不幸的是：几乎所有数据库都不支持检查子句中的子查询
    
    另一种方式：使用断言
    ```sql
    CREATE ASSERTION <assertion-name> CHECK <predicate>
    ```
    
    例如：
    - 对于student关系中的每个元组，tot_cred属性的值必须等于该学生成功完成的课程学分总和
    - 教师在同一学期的同一时间段不能在两个不同的教室授课
    
    但也不被任何数据库系统支持

</div>

## SQL数据类型

<div class="grid cards" markdown>

-   :material-calendar:{ .lg .middle } __日期和时间类型__

    ---
    - **date**：日期，包含(4位)年、月和日
      - 例如：`date '2005-7-27'`
    
    - **time**：一天中的时间，以小时、分钟和秒为单位
      - 例如：`time '09:00:30'` 或 `time '09:00:30.75'`
    
    - **timestamp**：日期加一天中的时间
      - 例如：`timestamp '2005-7-27 09:00:30.75'`
    
    - **interval**：时间段
      - 例如：`interval '4 days 3 hours 2 minutes 1 second'`
      - 从另一个日期/时间/时间戳值中减去日期/时间/时间戳值会得到间隔值
      - 间隔值可以添加到日期/时间/时间戳值

-   :material-shape:{ .lg .middle } __用户定义类型__

    ---
    SQL中的`CREATE TYPE`构造创建用户定义类型：
    
    ```sql
    CREATE TYPE Dollars AS numeric(12,2) FINAL
    ```
    
    使用示例：
    ```sql
    CREATE TABLE department (
        dept_name varchar(20),
        building varchar(15),
        budget Dollars
    )
    ```

-   :material-format-list-group:{ .lg .middle } __用户定义域__

    ---
    SQL-92中的`CREATE DOMAIN`构造创建用户定义域类型：
    
    ```sql
    CREATE DOMAIN person_name char(20) NOT NULL
    ```
    
    类型和域类似。域可以有约束，例如NOT NULL：
    
    ```sql
    CREATE DOMAIN degree_level varchar(10)
    CONSTRAINT degree_level_constraint
    CHECK (VALUE IN ('Bachelors', 'Masters', 'Doctorate'))
    ```

-   :material-file-image:{ .lg .middle } __大对象类型__

    ---
    大对象(照片、视频、CAD文件等)作为大对象存储：
    
    - **blob**：二进制大对象 - 对象是大量未解释的二进制数据(解释留给数据库系统外部的应用程序)
    
    - **clob**：字符大对象 - 对象是大量字符数据
    
    当查询返回大对象时，返回指针而不是大对象本身。

</div>

## 授权(Authorization)

<div class="grid cards" markdown>

-   :material-shield-account:{ .lg .middle } __授权类型__

    ---
    数据库部分的授权形式：
    - **读(Read)** - 允许读取但不允许修改数据
    - **插入(Insert)** - 允许插入新数据，但不允许修改现有数据
    - **更新(Update)** - 允许修改但不允许删除数据
    - **删除(Delete)** - 允许删除数据
    
    修改数据库模式的授权形式：
    - **索引(Index)** - 允许创建和删除索引
    - **资源(Resources)** - 允许创建新关系
    - **变更(Alteration)** - 允许在关系中添加或删除属性
    - **删除(Drop)** - 允许删除关系

-   :material-account-key:{ .lg .middle } __GRANT语句__

    ---
    GRANT语句用于授予授权：
    
    ```sql
    GRANT <privilege list>
    ON <relation name or view name> TO <user list>
    ```
    
    其中<user list>是：
    - 用户ID
    - PUBLIC，允许所有有效用户授予的权限
    - 角色(稍后详述)
    
    在视图上授予权限不意味着授予对基础关系的任何权限。
    
    权限的授予者必须已经持有指定项目的权限(或是数据库管理员)。

-   :material-key-variant:{ .lg .middle } __权限类型__

    ---
    - **select**：允许对关系的读访问，或使用视图查询的能力
      - 例如：授予用户U1、U2和U3对instructor关系的select授权：
      ```sql
      GRANT SELECT ON instructor TO U1, U2, U3
      ```
    
    - **insert**：插入元组的能力
    
    - **update**：使用SQL update语句更新元组或某些属性的能力
    
    - **delete**：删除元组的能力
    
    - **all privileges**：作为所有允许权限的简写形式

-   :material-key-remove:{ .lg .middle } __REVOKE语句__

    ---
    REVOKE语句用于撤销授权：
    
    ```sql
    REVOKE <privilege list>
    ON <relation name or view name> FROM <user list>
    ```
    
    例如：
    ```sql
    REVOKE SELECT ON branch FROM U1, U2, U3
    ```
    
    - <privilege-list>可以是all以撤销被撤销者可能持有的所有权限
    - 如果<revokee-list>包括public，所有用户都会失去权限，除非明确授予他们
    - 如果同一权限由不同的授予者两次授予同一用户，则在撤销后用户可能保留权限
    - 所有依赖于被撤销权限的权限也会被撤销

-   :material-account-multiple:{ .lg .middle } __角色(Roles)__

    ---
    角色是一种区分不同用户可以访问/更新数据库内容的方式。
    
    创建角色：
    ```sql
    CREATE ROLE <name>
    ```
    
    例如：
    ```sql
    CREATE ROLE instructor
    ```
    
    一旦创建了角色，我们可以使用以下方式将"用户"分配给角色：
    ```sql
    GRANT <role> TO <users>
    ```
    
    例如：
    ```sql
    GRANT instructor TO Amit; -- 将instructor(当前或将来)的权限授予Amit
    ```
    
    可以向角色授予权限：
    ```sql
    GRANT SELECT ON takes TO instructor;
    ```
    
    角色可以授予给用户，也可以授予给其他角色：
    ```sql
    CREATE ROLE teaching_assistant;
    GRANT teaching_assistant TO instructor; -- instructor继承teaching_assistant的所有权限
    ```
    
    角色链：
    ```sql
    CREATE ROLE dean;
    GRANT instructor TO dean;
    GRANT dean TO Satoshi;
    ```

-   :material-eye-settings:{ .lg .middle } __视图与授权__

    ---
    例如：
    ```sql
    CREATE VIEW geo_instructor AS
    (SELECT *
     FROM instructor
     WHERE dept_name = 'Geology');
    
    GRANT SELECT ON geo_instructor TO geo_staff;
    ```
    
    当geo_staff成员发出以下命令时：
    ```sql
    SELECT * FROM geo_instructor;
    ```
    
    如果：
    - geo_staff对instructor没有权限？
    - 视图创建者对instructor没有某些权限？

-   :material-key-chain:{ .lg .middle } __其他授权特性__

    ---
    - **references权限**用于创建外键：
      ```sql
      GRANT REFERENCE (dept_name) ON department TO Mariano;
      ```
      为什么需要这个？
    
    - **权限传递**：
      ```sql
      GRANT SELECT ON department TO Amit WITH GRANT OPTION;
      REVOKE SELECT ON department FROM Amit, Satoshi CASCADE;
      REVOKE SELECT ON department FROM Amit, Satoshi RESTRICT;
      ```

</div>

## pdf资料

<embed src="pdfs/ch4(4).pdf" type="application/pdf" width="100%" height="400px" />