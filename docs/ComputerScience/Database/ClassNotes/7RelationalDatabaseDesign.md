# 关系数据库设计

## 引言

关系数据库的良好设计对于数据一致性、消除冗余和提高性能至关重要。本章介绍关系数据库设计的理论基础，包括函数依赖、范式和关系分解算法等核心概念。通过这些理论和方法，我们可以系统地评估关系模式的质量，并且在需要时进行分解以改进设计。

## 函数依赖

<div class="grid cards" markdown>

-   :material-function-variant:{ .lg .middle } __函数依赖基本概念__

    ---
    函数依赖是关系数据库中最基本的完整性约束之一，它定义了属性之间的确定关系：
    
    - 函数依赖是对合法关系集合的约束
    - 要求某一组属性的值能唯一确定另一组属性的值
    - 函数依赖是键概念的推广
    - 允许我们找到左侧不是超键的依赖关系

</div>

### 函数依赖的定义

!!! abstract "定义"
    设R是一个关系模式，X和Y是R的属性子集。如果对于R的任何合法实例r，r中任意两个元组t₁和t₂，当它们在X上的值相同时，它们在Y上的值也相同，则称函数依赖X→Y在R上成立。

    形式化表示为：t₁[X] = t₂[X] ⟹ t₁[Y] = t₂[Y]

**例如**：考虑关系模式inst_dept(ID, name, salary, dept_name, building, budget)，以下函数依赖可能成立：

- ID → name, salary, dept_name (ID是超键)
- dept_name → building, budget (dept_name不是超键)

而以下函数依赖可能不成立：

- dept_name → salary (不同系的教师可能有不同的薪水)

### 函数依赖与键的关系

<div class="grid cards" markdown>

-   :material-key-outline:{ .lg .middle } __超键与候选键__

    ---
    - **超键(Superkey)**：如果X→R，则X是关系模式R的超键
    - **候选键(Candidate Key)**：如果X→R且对于X的任何真子集Y，Y→R不成立，则X是R的候选键
    
    函数依赖是确定键的基础，通过分析函数依赖可以找出关系模式的所有候选键。

</div>

### 平凡与非平凡函数依赖

函数依赖X→Y可分为以下类型：

1. **平凡函数依赖**：如果Y⊆X，则X→Y是平凡的（所有关系实例都满足）
   - 例如：ID, name → ID 或 name → name

2. **非平凡函数依赖**：如果Y⊈X，则X→Y是非平凡的

对于非平凡函数依赖X→Y，我们关注两个方面：

- X是否为超键
- Y是否包含候选键的一部分

### 函数依赖的闭包

<div class="grid cards" markdown>

-   :material-cached:{ .lg .middle } __函数依赖闭包__

    ---
    给定一组函数依赖F，由F逻辑蕴含的所有函数依赖构成F的闭包，记为F⁺。
    
    例如：如果A→B且B→C，则可以推断出A→C（通过传递性）。
    
    F⁺是F的超集，包含F以及所有从F推导出的函数依赖。

</div>

#### Armstrong公理

计算函数依赖闭包可以使用Armstrong公理：

1. **自反性(Reflexivity)**：如果Y⊆X，则X→Y
2. **增广律(Augmentation)**：如果X→Y，则XZ→YZ（对于任意属性集Z）
3. **传递性(Transitivity)**：如果X→Y且Y→Z，则X→Z

这些规则是：

- **健全的(Sound)**：只生成实际成立的函数依赖
- **完备的(Complete)**：生成所有成立的函数依赖

**额外的推导规则**：

- **合并规则**：如果X→Y且X→Z，则X→YZ
- **分解规则**：如果X→YZ，则X→Y且X→Z
- **伪传递规则**：如果X→Y且WY→Z，则WX→Z

#### 计算函数依赖闭包的算法

```
函数依赖闭包算法：
F⁺ = F
repeat
    for each 函数依赖 f in F⁺
        将自反性和增广律应用于f
        将结果添加到F⁺
    for each 函数依赖对 f₁和f₂ in F⁺
        如果f₁和f₂可以通过传递性组合
        then 将结果添加到F⁺
until F⁺不再变化
```

### 属性闭包

<div class="grid cards" markdown>

-   :material-set-center:{ .lg .middle } __属性闭包__

    ---
    给定属性集X和函数依赖集F，X在F下的闭包（记为X⁺）是X在F下函数确定的所有属性的集合。
    
    属性闭包是一个强大的工具，可用于：

    - 测试超键：如果X⁺包含R的所有属性，则X是R的超键
    - 测试函数依赖：要检查X→Y是否在F⁺中，只需检查Y是否包含在X⁺中
    - 计算F的闭包

</div>

#### 计算属性闭包的算法

```
计算属性闭包X⁺的算法：
result := X;
while (result有变化) do
    for each 函数依赖Y→Z in F do
        if Y ⊆ result then result := result ∪ Z;
```

**例子**：

假设R = (A, B, C, G, H, I)，F = {A→B, A→C, CG→H, CG→I, B→H}

计算(AG)⁺：

1. result = AG
2. result = ABCG (A→C和A→B)
3. result = ABCGH (CG→H且CG⊆ABCG)
4. result = ABCGHI (CG→I且CG⊆ABCGH)

因此(AG)⁺ = ABCGHI

检查AG是否为候选键：

1. 是否为超键？是，因为(AG)⁺包含R的所有属性
2. 是否有AG的子集是超键？
      - (A)⁺ = ABC...（不包含所有属性）
      - (G)⁺ = G（不包含所有属性）
   
因此AG是候选键。

### 函数依赖的用途

函数依赖有多种实用价值：

1. **指定对合法关系集合的约束**
   - 我们说F在R上成立，如果R上所有合法关系都满足F中的所有函数依赖

2. **测试关系是否满足给定的函数依赖集**
   - 如果关系r满足F中的所有函数依赖，则称r满足F

3. **如果R不是良好形式，将R分解为R₁...Rₙ，使每个Rᵢ都是良好形式**

4. **在某些情况下，将多个关系合并为一个关系**

!!! note "注意"
    关系模式的特定实例可能满足某个函数依赖，即使该函数依赖在所有合法实例上都不成立。例如，instructor的特定实例可能偶然满足name→ID。

## 关系分解

当关系模式不满足所需的范式时，需要进行分解。良好的分解应该具有以下性质：

1. **无损连接分解(Lossless-join Decomposition)**：确保通过自然连接操作可以恢复原始关系中的所有信息
2. **依赖保持(Dependency Preservation)**：确保所有原始函数依赖都能在分解后的模式中得到保持和检查

### 无损连接分解

<div class="grid cards" markdown>

-   :material-link:{ .lg .middle } __无损连接分解__

    ---
    对于关系模式R分解为R₁和R₂，如果对R的任何合法实例r，都有：
    
    r = πR₁(r) ⋈ πR₂(r)
    
    则称该分解是无损连接的。
    
    **充分条件**：如果以下依赖中至少有一个在F⁺中，则分解是无损连接的：

    - R₁∩R₂ → R₁
    - R₁∩R₂ → R₂

</div>

### 合并关系

在某些情况下，我们可能需要合并关系，特别是当多个关系共享相同的键时。

例如：

- dept_building(dept_name, building)
- dept_budget(dept_name, budget)

可以合并为：

- department(dept_name, building, budget)

在这种情况下，合并不会导致冗余。

## 规范化与范式

### 第一范式

<div class="grid cards" markdown>

-   :material-numeric-1-box:{ .lg .middle } __第一范式(1NF)__

    ---
    如果关系模式R的所有属性的域都是原子的（不可分割的单位），则称R满足第一范式(1NF)。
    
    原子性实际上是域使用方式的属性，而不是域本身的内在属性。
    
    **非原子域的例子**：

    - 名称集合、复合属性
    - 可以分解的标识符（如CS101）
    
    非原子值会使存储复杂化并鼓励数据冗余存储。

</div>

### Boyce-Codd范式(BCNF)

<div class="grid cards" markdown>

-   :material-numeric-2-box:{ .lg .middle } __BCNF定义__

    ---
    如果关系模式R对函数依赖集F满足下列条件，则称R满足BCNF：对于F⁺中的任何非平凡函数依赖X→Y，X必须是R的超键。
    
    换句话说，对于每个非平凡函数依赖X→Y，如果Y⊈X，则X必须是超键。
    
    BCNF保证了没有冗余和插入/删除/更新异常。

</div>

#### BCNF的例子

考虑模式：instr_dept(ID, name, salary, dept_name, building, budget)

函数依赖：

- ID → name, salary, dept_name
- dept_name → building, budget

这个模式不满足BCNF，因为dept_name不是超键，但dept_name → building, budget成立。

#### BCNF分解

如果关系模式R因为非平凡函数依赖X→Y违反BCNF，我们可以将R分解为：

- R₁ = (X∪Y)
- R₂ = (R - (Y-X))

对于上面的例子，instr_dept被分解为：

- department(dept_name, building, budget)
- instructor(ID, name, salary, dept_name)

在这个分解中，每个关系都满足BCNF，而且分解是无损连接的。

### 第三范式(3NF)

<div class="grid cards" markdown>

-   :material-numeric-3-box:{ .lg .middle } __第三范式(3NF)__

    ---
    如果关系模式R对函数依赖集F满足下列条件，则称R满足3NF：对于F⁺中的任何非平凡函数依赖X→Y，至少满足以下条件之一：
    
    1. X是R的超键
    2. Y-X中的每个属性A都包含在R的某个候选键中
    
    3NF是BCNF的放宽，为了确保依赖保持而设计。
    
    如果关系满足BCNF，它也满足3NF。

</div>

#### 3NF的例子

考虑关系模式：dept_advisor(s_ID, i_ID, dept_name)
函数依赖：

- s_ID, dept_name → i_ID
- i_ID → dept_name

候选键：{s_ID, dept_name}和{i_ID, s_ID}

这个关系满足3NF，因为：

- 对于s_ID, dept_name → i_ID，左侧是候选键
- 对于i_ID → dept_name，dept_name包含在候选键{s_ID, dept_name}中

#### 3NF的冗余

虽然3NF允许一些冗余，但它确保了所有函数依赖都可以在不连接关系的情况下进行检查。这是3NF相对于BCNF的主要优势。

例如，在关系R = (J, K, L)上，如果F = {JK→L, L→K}，由于依赖保持的原因，我们可能更倾向于保持这个3NF关系而不进行分解。

## 函数依赖理论与算法

### 规范覆盖

函数依赖集中可能存在冗余的依赖，这些依赖可以从其他依赖推导出来。规范覆盖(Canonical Cover)是一种找出最小函数依赖集的方法。

<div class="grid cards" markdown>

-   :material-filter-outline:{ .lg .middle } __规范覆盖概念__

    ---
    函数依赖集F的规范覆盖Fc是满足以下条件的函数依赖集：
    
    1. F逻辑蕴含Fc中的所有依赖
    2. Fc逻辑蕴含F中的所有依赖
    3. Fc中没有包含多余属性的函数依赖
    4. Fc中每个函数依赖的左侧都是唯一的
    
    规范覆盖移除了冗余信息，是数据库设计和优化的重要工具。

</div>

#### 多余属性

在函数依赖X→Y中，如果某个属性是多余的，则可以移除它而不影响所表示的约束。

1. **左侧多余属性**：如果A∈X且F逻辑蕴含(X-A)→Y，则A在X中是多余的
2. **右侧多余属性**：如果A∈Y且F' = (F - {X→Y}) ∪ {X→(Y-A)}逻辑蕴含X→Y，则A在Y中是多余的

#### 计算规范覆盖的算法

```
计算F的规范覆盖的算法：
Fc = F
repeat
    使用合并规则替换Fc中任何依赖X→Y1和X→Y2为X→Y1Y2
    找出Fc中带有多余属性的函数依赖X→Y
    如果找到多余属性，则从X→Y中删除它
until Fc不再变化
```

**例子**：

R = (A, B, C)，F = {A→BC, B→C, A→B, AB→C}

1. 合并A→BC和A→B为A→BC
   - 集合变为{A→BC, B→C, AB→C}
2. A在AB→C中是多余的
   - 检查删除A后的依赖是否被其他依赖蕴含
   - 是的：B→C已经存在！
   - 集合变为{A→BC, B→C}
3. C在A→BC中是多余的
   - 检查A→C是否被A→B和其他依赖逻辑蕴含
   - 是的：使用A→B和B→C的传递性
   - 最终规范覆盖为{A→B, B→C}

### 3NF分解算法

为了将关系分解为3NF，同时保持无损连接和依赖保持，可以使用以下算法：

```
3NF分解算法：
1. 计算F的规范覆盖Fc
2. 对Fc中的每个函数依赖X→Y：
   如果没有已创建的模式包含X∪Y
   则创建一个包含X∪Y的新模式
3. 如果没有模式包含R的候选键，则额外创建一个包含某个候选键的模式
4. 删除任何被其他模式包含的模式
```

该算法确保：
- 每个生成的关系模式都满足3NF
- 分解是无损连接的
- 分解是依赖保持的

**例子**：

考虑关系模式：cust_banker_branch = (customer_id, employee_id, branch_name, type)

函数依赖：

1. customer_id, employee_id → branch_name, type
2. employee_id → branch_name
3. customer_id, branch_name → employee_id

计算规范覆盖：

- branch_name在第1个依赖的右侧是多余的
- 得到Fc = {
  customer_id, employee_id → type,
  employee_id → branch_name,
  customer_id, branch_name → employee_id
  }

应用3NF分解算法：

1. 创建(customer_id, employee_id, type)
2. 创建(employee_id, branch_name)
3. 创建(customer_id, branch_name, employee_id)
4. (customer_id, employee_id, type)包含候选键，不需要额外创建
5. (employee_id, branch_name)是(customer_id, branch_name, employee_id)的子集，可以删除

最终3NF模式：

- (customer_id, employee_id, type)
- (customer_id, branch_name, employee_id)

### BCNF分解算法

BCNF分解算法类似于3NF分解，但不保证依赖保持：

```
BCNF分解算法：
result := {R};
done := false;
compute F+;
while (not done) do
    如果result中存在不满足BCNF的模式Ri
    then
        找出一个在Ri上成立的非平凡函数依赖X→Y，其中X不是Ri的超键
        result := (result - {Ri}) ∪ {(Ri - Y), (X ∪ Y)};
    else
        done := true;
```

**例子**：

考虑关系模式：class(course_id, title, dept_name, credits, sec_id, semester, year, building, room_number, capacity, time_slot_id)

函数依赖：

- course_id → title, dept_name, credits
- building, room_number → capacity
- course_id, sec_id, semester, year → building, room_number, time_slot_id

候选键：{course_id, sec_id, semester, year}

应用BCNF分解：

1. course_id → title, dept_name, credits成立
   - course_id不是class的超键
   - 分解为：
     - course(course_id, title, dept_name, credits)
     - class-1(course_id, sec_id, semester, year, building, room_number, capacity, time_slot_id)

2. building, room_number → capacity在class-1上成立
   - {building, room_number}不是class-1的超键
   - 进一步分解为：
     - classroom(building, room_number, capacity)
     - section(course_id, sec_id, semester, year, building, room_number, time_slot_id)

最终BCNF关系：

- course(course_id, title, dept_name, credits)
- classroom(building, room_number, capacity)
- section(course_id, sec_id, semester, year, building, room_number, time_slot_id)

### BCNF和3NF的比较

<div class="grid cards" markdown>

-   :material-scale-balance:{ .lg .middle } __BCNF与3NF的比较__

    ---
    **BCNF**:
    - 消除所有冗余和异常
    - 不总是能保持依赖
    - 可能导致某些函数依赖只能通过连接才能检查
    
    **3NF**:
    - 允许一些冗余
    - 总是能保持依赖
    - 可以在不连接的情况下检查所有函数依赖
    
    **选择依据**：
    - 如果依赖保持很重要，选择3NF
    - 如果消除冗余更重要，选择BCNF

</div>

## 多值依赖与第四范式

### 多值依赖

即使关系满足BCNF，它可能仍然存在某种形式的冗余。考虑关系：

inst_info(ID, child_name, phone)，其中一个教师可能有多个孩子和多个电话号码。

这个关系没有非平凡的函数依赖，因此它满足BCNF。但是，如果我们要为ID=99999添加一个新电话981-992-3443，需要为每个孩子添加一个元组，导致冗余。

<div class="grid cards" markdown>

-   :material-relation-many-to-many:{ .lg .middle } __多值依赖定义__

    ---
    设R是一个关系模式，X和Y是R的属性子集。多值依赖X→→Y在R上成立，当且仅当对于任意两个元组t₁和t₂，如果t₁[X] = t₂[X]，则存在元组t₃和t₄满足：
    
    - t₁[X] = t₂[X] = t₃[X] = t₄[X]
    - t₃[Y] = t₁[Y]
    - t₃[R-X-Y] = t₂[R-X-Y]
    - t₄[Y] = t₂[Y]
    - t₄[R-X-Y] = t₁[R-X-Y]
    
    简单来说，如果X→→Y，则给定X值，Y的值集合与R-X-Y的值集合是独立的。

</div>

如果X→Y，则X→→Y（每个函数依赖也是多值依赖）。

在我们的例子中：

- ID →→ child_name（给定教师ID，孩子名称的集合与电话号码集合无关）
- ID →→ phone（给定教师ID，电话号码的集合与孩子名称集合无关）

### 第四范式(4NF)

<div class="grid cards" markdown>

-   :material-numeric-4-box:{ .lg .middle } __第四范式(4NF)__

    ---
    关系模式R对函数依赖和多值依赖集D满足4NF，如果对于D⁺中的每个非平凡多值依赖X→→Y，X是R的超键。
    
    如果一个关系满足4NF，它也满足BCNF。
    
    4NF消除了BCNF中仍可能存在的冗余。

</div>

#### 4NF分解算法

```
4NF分解算法：
result := {R};
done := false;
compute D+;
while (not done) do
    如果result中存在不满足4NF的模式Ri
    then
        找出一个在Ri上成立的非平凡多值依赖X→→Y，其中X不是Ri的超键
        result := (result - {Ri}) ∪ {(X ∪ Y), (X ∪ (Ri - Y))};
    else
        done := true;
```

**例子**：

将inst_info(ID, child_name, phone)分解为：

- inst_child(ID, child_name)
- inst_phone(ID, phone)

这两个关系都满足4NF，而且分解是无损连接的。

## 数据库设计过程

### 实体-关系模型与规范化

<div class="grid cards" markdown>

-   :material-database-settings:{ .lg .middle } __设计方法比较__

    ---
    **自顶向下方法（E-R设计）**:

    - 识别实体、关系和属性
    - 将E-R图转换为关系模式
    - 应用规范化理论验证设计
    
    **自底向上方法（规范化理论）**:

    - 定义单一关系包含所有属性（全局关系）
    - 应用规范化算法分解为更小的关系
    
    实际上，这两种方法常常结合使用，先用E-R设计创建初步模式，再用规范化理论进行验证和调整。

</div>

### 从E-R模型生成的关系

当一个E-R图被仔细设计，正确识别所有实体后，从E-R图生成的表通常不需要进一步规范化。

但在实际（不完美）设计中，可能存在实体内非键属性到其他属性的函数依赖：

- 例如：一个employee实体具有department_name和building属性，且存在函数依赖department_name → building
- 好的设计应该把department做成一个实体

### 反规范化考虑

有时为了性能原因，可能选择使用非规范化的模式：

1. **显示优点**：查询性能提升，避免连接开销
2. **显示缺点**：
   
      - 额外空间消耗
      - 更新操作变慢
      - 需要额外编码工作维护一致性

在现代数据库系统中，可以使用物化视图达到类似效果，兼顾性能和规范化的好处。

### 不良设计模式

规范化理论无法捕获所有不良设计，以下是应避免的一些模式：

1. **属性作为列名**：
   ```
   # 不好的设计
   company_year(company_id, earnings_2004, earnings_2005, earnings_2006)
   
   # 更好的设计
   earnings(company_id, year, amount)
   ```

2. **子类型归并到单表**：
   ```
   # 不好的设计
   person(ID, name, type, student_tot_cred, employee_salary)
   
   # 更好的设计
   person(ID, name)
   student(ID, tot_cred)  
   employee(ID, salary)
   ```

### 时态数据考虑

时态数据具有时间关联性，在设计中需要特别考虑：

1. **时态函数依赖**：如果X→Y在所有时间点的快照上都成立，则称X→Y是时态函数依赖

2. **实用设计方法**：为关系添加开始和结束时间属性
   ```
   # 时态表设计
   course(course_id, course_title, start, end)
   ```

3. **外键参考**：可能指向当前版本的数据，或特定时间点的数据
      - 例如：学生成绩单应引用课程被选修时的课程信息

## 总结

关系数据库设计是一个复杂而重要的过程，涉及多种方法和理论：

1. **函数依赖理论**提供了评估关系质量的基础
2. **规范形式**（1NF, BCNF, 3NF, 4NF）定义了好的关系模式应满足的条件
3. **分解算法**帮助我们系统地改进关系模式
4. **E-R模型和规范化**共同为数据库设计提供了完整的方法论

良好的数据库设计目标是：

   - 尽可能达到BCNF或4NF
   - 确保分解是无损连接的
   - 在可能的情况下保持依赖

当这些目标发生冲突时，设计者需要在规范化程度（减少冗余）和依赖保持（维护完整性约束）之间做出权衡，基于应用需求选择合适的设计。

## pdf资料

<embed src="pdfs/ch7(4).pdf" type="application/pdf" width="100%" height="400px" />