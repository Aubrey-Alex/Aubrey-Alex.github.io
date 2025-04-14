# 高级SQL

## 介绍

<div class="grid cards" markdown>

-   :material-code-brackets:{ .lg .middle } __编程语言中访问SQL__

    ---
    本章介绍SQL的高级特性，包括：
    - 嵌入式SQL
    - ODBC和JDBC接口
    - 函数和过程构造
    - 触发器
    - 高级聚合特性
    - 联机分析处理(OLAP)

</div>

## 编程语言中访问SQL

在实际应用中，SQL查询和更新通常嵌入在通用编程语言（如C、Java、Python等）编写的程序中。SQL不提供完整的图形用户界面工具和复杂业务逻辑的控制结构，因此需要通过编程语言来弥补这些不足。

### 嵌入式SQL

<div class="grid cards" markdown>

-   :material-code-json:{ .lg .middle } __嵌入式SQL基础__

    ---
    - SQL标准定义了SQL在各种编程语言（如C、Java和Cobol）中的嵌入方式
    - 嵌入SQL的语言被称为**宿主语言**
    - 使用`EXEC SQL`语句标识嵌入SQL请求
    ```
    EXEC SQL <嵌入SQL语句> END_EXEC
    ```
    - 主要问题：宿主语言和SQL语句之间的参数和结果交换、集合与变量的处理、获取SQL语句的执行状态等

-   :material-cursor-default-outline:{ .lg .middle } __游标__

    ---
    游标允许逐个元组地处理查询结果
    
    **游标声明**
    ```sql
    EXEC SQL 
    DECLARE c CURSOR FOR
    SELECT ID, name
    FROM student
    WHERE tot_cred > :credit_amount
    END_EXEC
    ```
    
    **游标操作**
    - `OPEN`：执行查询并创建结果集
    - `FETCH`：获取下一个元组
    - `CLOSE`：释放资源

</div>

!!! example "游标使用示例(C语言)"
    ```c
    void getStudentInfo() {
        int credit_amount;
        char sId[16];
        char sName[16];
        
        EXEC SQL DECLARE c CURSOR FOR
        SELECT id, name FROM student WHERE tot_cred > :credit_amount
        END_EXEC;
        
        printf("Please input the credit amount: ");
        scanf("%d", &credit_amount);
        
        EXEC SQL OPEN c END_EXEC;
        
        while(1) {
            EXEC SQL FETCH c INTO :sId, :sName END_EXEC;
            if (!strcmp(SQLSTATE, "02000"))
                break;  // 无更多数据
            printf("%s %s\n", sId, sName);
        }
        
        EXEC SQL CLOSE c END_EXEC;
    }
    ```

游标可以用于更新已获取的元组，通过在声明游标时指定`FOR UPDATE`选项，并使用`WHERE CURRENT OF`子句。

```sql
DECLARE c CURSOR FOR
SELECT *
FROM instructor
WHERE dept_name = 'Music'
FOR UPDATE

-- 更新当前游标位置的元组
UPDATE instructor
SET salary = salary + 100
WHERE CURRENT OF c
```

### ODBC和JDBC

<div class="grid cards" markdown>

-   :material-api:{ .lg .middle } __数据库编程接口__

    ---
    API（应用程序接口）允许程序与数据库服务器交互：
    - 连接数据库服务器
    - 发送SQL命令
    - 一个一个地获取结果元组
    
    **主要接口**：
    - **ODBC**（Open Database Connectivity）：用于C、C++、C#和Visual Basic
    - **JDBC**（Java Database Connectivity）：用于Java

-   :material-database-cog:{ .lg .middle } __ODBC架构__

    ---
    **ODBC特点**：
    - 定义了应用程序与数据库服务器通信的标准
    - 提供API以打开连接、发送查询、更新和获取结果
    - 支持各种数据库系统
    - 基于驱动程序架构

    **组件**：
    - ODBC应用程序
    - ODBC管理器
    - 特定数据库系统的驱动程序

</div>

#### ODBC

ODBC（开放数据库连接）标准允许应用程序通过通用接口与各种数据库系统进行通信。

!!! info "ODBC架构"
    ```
    客户端应用程序
         ↓
      ODBC API
         ↓
     ODBC管理器
       /   |   \
    Oracle  PostgreSQL  MySQL
    驱动程序 驱动程序  驱动程序
    ```

ODBC程序的基本流程：

1. 分配SQL环境和连接句柄
2. 连接到数据库
3. 执行SQL语句
4. 处理结果
5. 断开连接并释放资源

!!! example "ODBC基本连接示例"
    ```c
    int ODBCexample() {
        RETCODE error;
        HENV env;    /* 环境句柄 */
        HDBC conn;   /* 数据库连接句柄 */
        
        SQLAllocEnv(&env);
        SQLAllocConnect(env, &conn);
        SQLConnect(conn, "MikeDSN", SQL_NTS, "avi", SQL_NTS, "avipasswd", SQL_NTS);
        
        /* 执行实际工作，通常使用语句句柄进行查询 */
        
        SQLDisconnect(conn);
        SQLFreeConnect(conn);
        SQLFreeEnv(env);
    }
    ```

**ODBC预处理语句**是一种更安全、更高效的执行SQL的方式：

```c
/* 预处理语句版本 */
SQLAllocStmt(conn, &stmt);
SQLPrepare(stmt, "SELECT id, name, tot_cred FROM student WHERE tot_cred > ?", SQL_NTS);
SQLBindParameter(stmt, 1, SQL_PARAM_INPUT, SQL_C_SLONG, SQL_INTEGER, 0, 0, 
                 &creditAmount, sizeof(int), NULL);
SQLExecute(stmt);
/* 绑定结果列并获取数据 */
```

**SQL注入风险**：始终使用预处理语句绑定用户输入，而不是直接拼接SQL字符串。

#### JDBC

JDBC (Java Database Connectivity) 是Java标准的数据库访问API。

!!! example "JDBC基本连接示例"
    ```java
    public static void JDBCexample(String dbid, String userid, String passwd) {
        try {
            Class.forName("oracle.jdbc.driver.OracleDriver");
            Connection conn = DriverManager.getConnection(
                "jdbc:oracle:thin:@db.yale.edu:2000:univdb", userid, passwd);
            Statement stmt = conn.createStatement();
            
            /* 执行实际工作 */
            
            stmt.close();
            conn.close();
        } catch (SQLException sqle) {
            System.out.println("SQLException: " + sqle);
        }
    }
    ```

**JDBC执行查询和更新**：

```java
// 执行更新
stmt.executeUpdate(
    "INSERT INTO instructor VALUES('77987', 'Kim', 'Physics', 98000)");

// 执行查询并获取结果
ResultSet rset = stmt.executeQuery(
    "SELECT dept_name, AVG(salary) FROM instructor GROUP BY dept_name");
while (rset.next()) {
    System.out.println(rset.getString("dept_name") + " " + rset.getFloat(2));
}
```

**JDBC预处理语句**：
```java
PreparedStatement pStmt = conn.prepareStatement(
    "INSERT INTO instructor VALUES(?,?,?,?)");
pStmt.setString(1, "88877");
pStmt.setString(2, "Perry");
pStmt.setString(3, "Finance");
pStmt.setInt(4, 125000);
pStmt.executeUpdate();
```

**元数据获取**：JDBC可以获取数据库和查询结果的元数据信息。

```java
// 获取ResultSet元数据
ResultSetMetaData rsmd = rs.getMetaData();
for(int i = 1; i <= rsmd.getColumnCount(); i++) {
    System.out.println(rsmd.getColumnName(i));
    System.out.println(rsmd.getColumnTypeName(i));
}

// 获取数据库元数据
DatabaseMetaData dbmd = conn.getMetaData();
ResultSet rs = dbmd.getColumns(null, "univdb", "department", "%");
```

## 函数和过程构造

SQL:1999及之后的版本支持过程化SQL，允许定义函数和存储过程，包括条件语句、循环等控制结构。

<div class="grid cards" markdown>

-   :material-function-variant:{ .lg .middle } __SQL函数__

    ---
    SQL:1999支持使用SQL或外部编程语言编写的函数：
    
    ```sql
    CREATE FUNCTION dept_count (dept_name VARCHAR(20))
    RETURNS INTEGER
    BEGIN
        DECLARE d_count INTEGER;
        SELECT COUNT(*) INTO d_count
        FROM instructor
        WHERE instructor.dept_name = dept_name;
        RETURN d_count;
    END
    ```
    
    函数调用示例：
    ```sql
    SELECT dept_name, budget
    FROM department
    WHERE dept_count(dept_name) > 12
    ```

-   :material-table-pivot:{ .lg .middle } __表值函数__

    ---
    SQL:2003添加了返回关系作为结果的函数：
    
    ```sql
    CREATE FUNCTION instructors_of(dept_name CHAR(20))
    RETURNS TABLE (
        ID VARCHAR(5),
        name VARCHAR(20),
        dept_name VARCHAR(20),
        salary NUMERIC(8,2)
    )
    RETURN TABLE (
        SELECT ID, name, dept_name, salary
        FROM instructor
        WHERE instructor.dept_name = instructors_of.dept_name
    )
    ```
    
    使用方式：
    ```sql
    SELECT *
    FROM TABLE(instructors_of('Music'))
    ```

-   :material-code-braces-box:{ .lg .middle } __存储过程__

    ---
    存储过程是可以存储在数据库中的程序：
    
    ```sql
    CREATE PROCEDURE dept_count_proc (
        IN dept_name VARCHAR(20),
        OUT d_count INTEGER
    )
    BEGIN
        SELECT COUNT(*) INTO d_count
        FROM instructor
        WHERE instructor.dept_name = dept_count_proc.dept_name;
    END
    ```
    
    调用过程：
    ```sql
    DECLARE d_count INTEGER;
    CALL dept_count_proc('Physics', d_count);
    ```

</div>

### 过程编程构造

SQL标准和各数据库系统提供了丰富的过程化编程构造，包括：

!!! example "条件语句和循环"
    **复合语句**：`BEGIN ... END`
    
    **变量声明**：`DECLARE n INTEGER DEFAULT 0;`
    
    **While循环**：
    ```sql
    WHILE n < 10 DO
        SET n = n + 1;
    END WHILE
    ```
    
    **Repeat循环**：
    ```sql
    REPEAT
        SET n = n - 1;
    UNTIL n = 0
    END REPEAT
    ```
    
    **For循环**：
    ```sql
    DECLARE n INTEGER DEFAULT 0;
    FOR r AS
        SELECT budget FROM department
        WHERE dept_name = 'Music'
    DO
        SET n = n + r.budget;
    END FOR
    ```
    
    **条件语句**：
    ```sql
    IF condition THEN
        statements;
    ELSEIF condition THEN
        statements;
    ELSE
        statements;
    END IF
    ```

### 外部语言过程和函数

SQL:1999允许使用如C、C++等外部语言编写函数和过程：

```sql
CREATE PROCEDURE dept_count_proc(IN dept_name VARCHAR(20), OUT count INTEGER)
LANGUAGE C
EXTERNAL NAME '/usr/avi/bin/dept_count_proc'
```

**优点**：对于许多操作更高效，表达能力更强。

**缺点**：可能存在安全风险，可以通过沙箱技术或单独进程执行来降低风险。

## 触发器

触发器是在数据库修改时自动执行的语句，它在SQL:1999标准中被引入。

<div class="grid cards" markdown>

-   :material-lightning-bolt:{ .lg .middle } __触发器基础__

    ---
    设计触发器机制需要指定：
    - 触发器执行的条件
    - 触发器执行时要采取的操作
    
    触发事件可以是：
    - INSERT
    - DELETE
    - UPDATE (可以限制到特定属性)

-   :material-code-tags:{ .lg .middle } __触发器语法__

    ---
    ```sql
    CREATE TRIGGER trigger_name
    BEFORE|AFTER|INSTEAD OF event
    ON table_name
    [REFERENCING OLD ROW AS old NEW ROW AS new]
    [FOR EACH ROW|STATEMENT]
    WHEN (condition)
    BEGIN
        -- 触发器操作
    END;
    ```
    
    触发器可以在事件之前或之后执行，并可以引用事件前后的值（OLD ROW和NEW ROW）。

</div>

!!! example "触发器示例：完整性约束检查"
    ```sql
    CREATE TRIGGER timeslot_check1 AFTER INSERT ON section
    REFERENCING NEW ROW AS nrow
    FOR EACH ROW
    WHEN (nrow.time_slot_id NOT IN (
        SELECT time_slot_id
        FROM time_slot
    ))
    BEGIN
        ROLLBACK;
    END;
    ```

!!! example "触发器示例：维护派生数据"
    ```sql
    CREATE TRIGGER credits_earned AFTER UPDATE OF grade ON takes
    REFERENCING NEW ROW AS nrow
    REFERENCING OLD ROW AS orow
    FOR EACH ROW
    WHEN nrow.grade <> 'F' AND nrow.grade IS NOT NULL
    AND (orow.grade = 'F' OR orow.grade IS NULL)
    BEGIN ATOMIC
        UPDATE student
        SET tot_cred = tot_cred +
            (SELECT credits
             FROM course
             WHERE course.course_id = nrow.course_id)
        WHERE student.id = nrow.id;
    END;
    ```

触发器曾经用于维护汇总数据、复制数据库等任务，但现代数据库提供了更好的方式（如物化视图、内置复制支持）。

## 递归查询

SQL:1999允许递归视图定义，这使得可以编写传递闭包等无法用非递归查询表达的查询。

!!! example "递归查询示例：先决条件传递闭包"
    ```sql
    WITH RECURSIVE rec_prereq(course_id, prereq_id) AS (
        SELECT course_id, prereq_id
        FROM prereq
        UNION
        SELECT rec_prereq.course_id, prereq.prereq_id
        FROM rec_prereq, prereq
        WHERE rec_prereq.prereq_id = prereq.course_id
    )
    SELECT *
    FROM rec_prereq;
    ```

!!! example "递归查询示例：员工-经理关系"
    ```sql
    WITH RECURSIVE empl(employee_name, manager_name) AS (
        SELECT employee_name, manager_name
        FROM manager
        UNION
        SELECT manager.employee_name, empl.manager_name
        FROM manager, empl
        WHERE manager.manager_name = empl.employee_name
    )
    SELECT *
    FROM empl;
    ```

## 高级聚合特性

SQL提供了丰富的高级聚合功能，包括排名、窗口函数等。

<div class="grid cards" markdown>

-   :material-order-numeric-ascending:{ .lg .middle } __排名函数__

    ---
    SQL排名函数与ORDER BY子句一起使用：
    
    ```sql
    SELECT ID, RANK() OVER (ORDER BY GPA DESC) AS s_rank
    FROM student_grades
    ORDER BY s_rank;
    ```
    
    排名函数类型：
    - `RANK()`：留下空缺排名（如有并列第1，则下一个排名为第3）
    - `DENSE_RANK()`：不留空缺（如有并列第1，则下一个排名为第2）
    - `ROW_NUMBER()`：分配唯一排名（对重复值处理非确定性）
    - `PERCENT_RANK()`：百分比排名
    - `CUME_DIST()`：累积分布（前面值的元组比例）

-   :material-window-maximize:{ .lg .middle } __窗口函数__

    ---
    窗口函数在一组行上执行计算，用于平滑随机变化：
    
    ```sql
    -- 计算销售移动平均值（当天、前一天和后一天）
    SELECT date, SUM(value) OVER (
        ORDER BY date
        ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING
    )
    FROM sales;
    ```
    
    窗口规范示例：
    - `ROWS UNBOUNDED PRECEDING`：从分区开始到当前行
    - `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`：同上
    - `RANGE BETWEEN 10 PRECEDING AND CURRENT ROW`：当前值减10到当前值的所有行

-   :material-swap-vertical:{ .lg .middle } __分区排名和窗口__

    ---
    排名和窗口函数可以在数据分区内应用：
    
    ```sql
    -- 每个系内学生排名
    SELECT ID, dept_name,
      RANK() OVER (PARTITION BY dept_name ORDER BY GPA DESC) AS dept_rank
    FROM dept_grades
    ORDER BY dept_name, dept_rank;
    ```
    
    ```sql
    -- 计算每个账户的余额
    SELECT account_number, date_time,
      SUM(value) OVER (
        PARTITION BY account_number
        ORDER BY date_time
        ROWS UNBOUNDED PRECEDING
      ) AS balance
    FROM transaction
    ORDER BY account_number, date_time;
    ```

</div>

## 联机分析处理(OLAP)

<div class="grid cards" markdown>

-   :material-cube-outline:{ .lg .middle } __OLAP基础__

    ---
    **OLAP**（联机分析处理）是数据的交互式分析，允许以在线方式（几乎无延迟）汇总和以不同方式查看数据。
    
    **多维数据**：可以建模为维度属性和度量属性的数据。

    - **度量属性**：度量某个值，可以聚合
    - **维度属性**：定义查看度量属性的维度

-   :material-table-pivot:{ .lg .middle } __数据立方体和交叉表__

    ---
    **交叉表（Cross-Tab）**也称为数据透视表：

    - 一个维度属性的值形成行标题
    - 另一个维度属性的值形成列标题
    - 其他维度属性列在顶部
    - 单元格中的值是指定单元格的维度属性的(聚合)值
    
    **数据立方体**是交叉表的多维概括，可以有n个维度。

-   :material-cube-scan:{ .lg .middle } __OLAP操作__

    ---
    **CUBE操作**：计算指定属性的每个子集上的GROUP BY联合
    
    ```sql
    SELECT item_name, color, size, SUM(number)
    FROM sales
    GROUP BY CUBE(item_name, color, size);
    ```
    
    **ROLLUP操作**：在指定属性列表的每个前缀上生成联合
    
    ```sql
    SELECT item_name, color, size, SUM(number)
    FROM sales
    GROUP BY ROLLUP(item_name, color, size);
    ```
    
    **其他OLAP操作**：

    - **旋转**：改变交叉表中使用的维度
    - **切片**：仅为固定值创建交叉表
    - **上卷**：从细粒度数据移动到粗粒度数据
    - **下钻**：从粗粒度数据移动到细粒度数据

-   :material-database-sync:{ .lg .middle } __OLAP实现__

    ---
    **OLAP系统类型**：

    - **MOLAP**（多维OLAP）：使用内存中的多维数组存储数据立方体
    - **ROLAP**（关系OLAP）：仅使用关系数据库功能实现OLAP
    - **HOLAP**（混合OLAP）：在内存中存储某些汇总，在关系数据库中存储基础数据和其他汇总
    
    早期OLAP系统预先计算所有可能的聚合以提供在线响应，但这需要大量空间和时间。现代系统使用各种优化技术来减少计算和存储需求。

</div>

### OLAP案例与应用

OLAP技术被广泛应用于各种商业智能和数据分析场景，如：

!!! info "OLAP应用场景"
    - 销售数据分析：按产品、地区、时间等维度分析销售情况
    - 财务报表：按不同财务指标、部门、时间周期等分析财务数据
    - 客户行为分析：按客户类型、产品类别、购买时间等分析客户行为
    - 供应链监控：按供应商、物流渠道、时间等维度分析供应链效率
    - 人力资源管理：按部门、职位等分析员工绩效和其他指标

数据立方体表示：

```
          时间维度
          /
         /
        /
       /___________ 产品维度
      /|
     / |
    /  |
地点维度
```

## 总结

SQL的高级特性极大地扩展了数据库系统的功能和灵活性：

1. **编程接口**（嵌入式SQL、ODBC和JDBC）提供了与各种应用程序集成的方式
2. **函数和存储过程**增加了过程化编程能力，允许在数据库中执行复杂逻辑
3. **触发器**提供了自动响应数据库更改的机制
4. **递归查询**使得可以表达传递闭包等复杂查询
5. **高级聚合特性**（如排名和窗口函数）提供了强大的数据分析能力
6. **OLAP功能**支持复杂的多维数据分析和数据挖掘

这些高级特性使SQL从简单的查询语言发展成为一个完整的数据处理系统，能够满足现代企业和应用程序的各种数据管理需求。 

## pdf资料

<embed src="pdfs/ch5(4).pdf" type="application/pdf" width="100%" height="400px" />