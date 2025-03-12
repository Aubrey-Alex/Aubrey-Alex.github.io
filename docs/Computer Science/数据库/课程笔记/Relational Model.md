# 关系型模型

## 概念

|概念|解释|
|---|---|
|superkey|一个关系中，能唯一标识一个元组的属性或属性组|
|candidate key|来自superkey的属性,并且再移除一个就不是superkey|
|primary key|从候选键中任选一个作为主键|
|foreign key|一个关系中，引用另一个关系的主键|
|referencing relation|拥有外键的关系|
|referenced relation|被外键引用的关系|

## 关系运算

### 六个基本关系

1. 选择（SELECT）
    - $\sigma_{A=B \land D > 5} (r)$ ：选出满足 $A=B$ 且 $D>5$ 的元组
2. 投影（PROJECT）
    - $\pi_{A,D} (r)$ ：只保留 $A$ 和 $D$ 两个属性
3. 并（UNION）
    - $r \cup s$ ：将 $r$ 和 $s$ 两个关系的元组合并
!!! note "注"
    冗余行会被自动去掉

4. 差（DIFFERENCE）
    - $r - s$ ：从 $r$ 中去掉 $s$ 中相同的元组
5. 笛卡尔积（CARTESIAN PRODUCT）
    - $r \times s$ ：将 $r$ 和 $s$ 两个关系的笛卡尔积
!!! note "注"
    两个关系必须是disjoint的，即没有相同的元组，如果有的话必须重命名
6. 重命名（RENAME）
    - $\rho_{B} (A)$ ：将 $A$ 重命名为 $B$

### 额外关系运算

1. 交（INTERSECTION）
    - $r \cap s$ ：取 $r$ 和 $s$ 两个关系的交集
2. 自然连接（NATURAL JOIN）
    - $r \bowtie s$ ：将 $r$ 和 $s$ 两个关系的自然连接
3. 赋值（ASSIGNMENT）
    - $d \leftarrow r$ ：将 $r$ 中的元组赋值给 $d$
4. 外连接（OUTER JOIN）
    - 左外连接（LEFT OUTER JOIN）
    - 右外连接（RIGHT OUTER JOIN）
    - 全外连接（FULL OUTER JOIN）
5. 除（DIVIDE）
    - $r / s$ ：将 $r$ 中包含 $s$ 中所有元组的元组去掉
    - 除法使用基本运算表示 
        - $temp1 \leftarrow \pi_{R-S} (r)$ 
        - $temp2 \leftarrow \pi_{R-S}((temp1\times s)-\pi_{R-S,S}(r))$ 
        - $result=temp1-temp2$ 

### 拓展关系运算

1. 广义投影（Generalized Projection）
2. 聚合函数（Aggregation Functions）
    - $\sum_{A} (r)$ ：求和
    - $\max_{A} (r)$ ：求最大值
    - $\min_{A} (r)$ ：求最小值
    - $avg_{A} (r)$ ：求平均值
    - $count_{A} (r)$ ：求个数