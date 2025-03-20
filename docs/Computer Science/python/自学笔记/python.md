# Python

## 一、工具的使用

### Pycharm常用快捷键

- ctrl + alt + s : 开软件设置
- ctrl + d : 复制当前行代码
- shift + alt + 上\下 : 将当前代码上移或下移
- ctrl + shift + F10 : 运行当前代码文件
- shift + F6 : 重命名文件
- ctrl + a : 全选
- ctrl + y : 删除一整行
- ctrl + c\v\x : 复制、粘贴、剪切
- ctrl + f : 搜素
- ctrl + shift + alt + j：选中所有相同的单词
- shift + alt + 鼠标拖动：光标竖着延长 

## 二、python基础

### 数据类型

#### 数据类型分类

|||
| :---: | :---: |
| 数字 | 整数、浮点数、复数、布尔 |
| 字符串 | 字符序列 |
| 列表list | 有序的可变序列 |
| 元组tuple | 有序的不可变序列 |
| 集合set | 无序的可变集合 |
| 字典dict | 无序的键值对集合 |

#### 查看数据类型

- type( )

#### 数据类型转换

- int( )

- float( )

- str( )

!!! note "注意"
	- 浮点数转字符串随便转
	- 整数转字符串随便转
	- 字符串转整数必须为整数,"11.345"报错
	- 字符串转浮点数,整数、浮点数都可以
	- 整数转浮点数加.0
	- 浮点数转整数抹去小数点后东西

### 注释

|||
| :---: | :---: |
| 单行注释 | \# 开头 |
| 多行注释 | """  """一对三引号 (一般解释python文件、类、方法)|

### 变量

- 无需定义，直接赋值

### 标识符

|||
| :---: | :---: |
| 命名规则 | 英文、中文、数字、下划线_ ; 不推荐中文; ==不可以用数字开头==; 区分大小写; 不可使用关键字 |
| 命名规范 | 见名知意、下划线命名法、英文字母全小写 |

### 运算符

=== "算术运算符"

	|||
	| :---: | :---: |
	| +  | 加法 |
	| -  | 减法 |
	| *  | 乘法 |
	| /  | 除法 |
	| // | 整除 |
	| %  | 取余 |
	| ** | 指数 |

=== "赋值运算符"

	|||
	| :---: | :---: |
	| =  | 赋值 |
	| += | 加法赋值 |
	| -= | 减法赋值 |
	| *= | 乘法赋值 |
	| /= | 除法赋值 |
	| %= | 取模赋值 |
	| **=| 幂赋值 |
	| //=| 取整赋值 |

### 字符串拓展

#### 定义方法

- 单引号定义法 ‘ ’

- 双引号定义法 ” “

- 三引号赋值法 ‘’‘  ’‘’  （可以写多行）

	> 如果不被赋值，那么就是多行注释
	>
	> 如果被赋值，那么就是字符串

#### 引号嵌套

- 单引号定义法，内含双引号
- 双引号定义法，内含单引号
- 转义字符 \ 解除效用

#### 字符串的拼接

##### 直接拼接

- 使用 + 直接进行拼接（不会自动加空格）

	```python
	name = "World"
	print("Hello," + name)
	```

	> 左右两边必须都是字符串

##### 字符串格式化

- % 占位

- 可以拼接数字以及其他

- %s 将内容变为字符串放入位置

- %d 将内容变为整数放入位置

- %f 将内容变为浮点型放入位置（默认保留六位小数）

	（精度控制四舍五入，宽度限制太小等于没有写）

	> 多个变量占位，变量要用括号括起来，并且按照占位的顺序填入

	```python
	name = "World"
	point = "."
	message = "Hello,%s%s" % (name, point)
	print(message)
	```

	```python
	name = "摸鱼小猪"
	score = 150
	math = 149.99
	message = "%s,成绩是%d,数学是%7.2f" % (name, score, math)
	print(message)
	```

##### 字符串快速格式化

- f ” 内容 { 变量 } ”

	> 不关心类型

	> 不进行精度控制

```python
name = "World"
point = "."
message = f"Hello,{name}{point}"
print(message)
```

#### 表达式的格式化

```python
print("计算结果是：%d" % (1 * 1))
print(f"计算结果是：{1 * 1}")
print("字符串的类型是：%s" % type("字符串"))
```

## 数据输入

- input（ ）默认返回值为字符串型

```python
name = input("请告诉我你是谁")
print(f"你是：{name}", type(name))
```

## 三、判断语句

### 布尔类型

- True 真，本质为1
- False 假， 本质为0

### 比较运算符

- == 相等
- != 不相等
- \> 大于
- \< 小于
- \>= 大于等于
- \<= 小于等于

### 判断语句

#### 简单的判断语句

```python
if 条件1:
	条件1满足应该做的事
elif 条件2：
	条件2满足应该做的事
else:
	所有条件都不满足应该做的事
```

> 缩进为 4 个空格，或者一个tab键

#### 判断语句的嵌套

- 嵌套的关键点：缩进
- 通过空格缩进，来决定语句间的层次关系

```
if 条件1:
	条件1满足应该做的事
	if 条件2：
		满足条件2应该做的事情
elif 条件3：
	条件3满足应该做的事
else:
	所有条件都不满足应该做的事
```

## 四、循环语句

### while 循环

#### 基础语法

```python
while 条件:
	条件满足时，做的事情1
	条件满足时，做的事情2
	······
```

#### 嵌套语法

```python
while 条件1:
	条件满足时，做的事情1
	条件满足时，做的事情2
	······
	while 条件2：
		条件满足时，做的事情1
		条件满足时，做的事情2
		······
```

#### 补充知识

- print( )语句输出后自动换行

- 使print( )语句输出不换行

```python
print("Hello", end='')
print("World")
```

- \t 相当于输 Tab 键，使多行字符串进行对齐

```python
print("Hello World")
print("Aubrey Alex")
# 对齐后
print("Hello\tWorld")
print("Aubrey\tAlex")
```

### for 循环

#### 基础语法

```python
for 临时变量 in 待处理数据集:
	循环满足条件时执行的代码
```

- 无法定义循环条件，只能取出数据处理
- 循环内的语句有空格缩进

#### 序列类型

- 字符串
- 列表
- 元组

##### range 语句

1. range(num)

	> 获取从0开始，到num结束的数字序列（不含num本身）

	> eg.range( 5 )得到的数据是：[0, 1, 2, 3, 4]

2. range(num1, num2)

	> 获得一个从num1开始，到num2结束的数字序列（不包含num2本身）

	>eg.range( 5, 10 )得到的数据是：[5, 6, 7, 8, 9]

3. range(num1, num2,step)

	> 获得一个从num1开始，到num2结束的数字序列（不包含num2本身）

	> 数字之间的步长，以step为准（step默认为1）

	> 如，range( 5, 10, 2)得到的数据是：[5, 7, 9]

#### 嵌套语法

```python
for 临时变量 in 待处理数据集（序列）:
	循环满足条件应做的事1
	循环满足条件应做的事2
	···
	for 临时变量 in 待处理数据集（序列）:
	循环满足条件应做的事1
	循环满足条件应做的事2
	···
```

### 循环中断

- continue

	> 中断本次循环，直接进入下一次循环

	> 只影响所在的循环

- break

	> 直接结束循环

	> 只影响所在的循环

## 五、函数

### 函数的定义

```python
def 函数名（传入参数）:
	函数体
	return 返回值
```

### 函数的调用

```python
函数名（参数）
```

### 函数的参数

```python
def add(x, y):
	result = x + y
	print(f"{x} + {y}的计算结果是{result}")
```

### 函数的返回值

```
def 函数（参数）:
	函数体
	return 返回值
	
变量 = 函数（参数）
```

> 无return时，返回值为None

### None

- 用在函数无返回值上

- if中None等同于False

- 声明无内容的变量

```python
name = None
```

### 函数的说明文档

```python
def func(x, y):
	"""
	函数说明
	:param x:形参x的说明
	:param y:形参y的说明
	:return:返回值的说明
	"""
	函数体
	return 返回值
```

### 函数的嵌套调用

```python
def func_b():
	print("---2---")
	
def func_a():
	print("---1---")
	func_b()
	print("---3---")
	
func_a()
```

### 变量的作用域

#### 局部变量

- 定义在函数体内部的变量
- 只在函数体内部生效

#### 全局变量

- 定义在函数外部
- 函数体内和体外都可以生效

#### 修改全局变量

=== "函数内部修改变量不对全局变量生效"

	```python
	num = 100

	def test_a():
		print(num)
		
	def test_b():
		num = 200
		print(num)
		
	test_a()
	test_b()
	print(num)
	```

	> 100
	>
	> 200
	>
	> 100

=== "使用global关键字声明修改的是全局变量"

	```python
	num = 100

	def test_a():
		print(num)
		
	def test_b():
		global num
		num = 200
		print(num)
		
	test_a()
	test_b()
	print(num)
	```

	> 100
	>
	> 200
	>
	> 200


## 六、数据容器

### list列表

### 列表的定义

- 以[ ]为标识
- 列表内每个元素用 ，分隔开

```python
# 字面量
[元素1, 元素2, 元素3, 元素4, ……]

# 定义变量
变量名称 = [元素1, 元素2, 元素3, 元素4, ……]

# 定义空列表
变量名称 = []
变量名称 = list()
```

- 可以为不同的数据类型

```python
my_list = ['Aubrey', 666, True]
```

- 支持嵌套

```python
my_list = [[1, 2, 3], [4, 5, 6]]
```

### 列表的下标索引

- 下标从0开始（第一个），从前向后每次加一

```python
my_list[0]
```

- 反向索引，从-1开始（最后一个）,从后向前每次减一

```python
my_list[-1]
```

- 取出嵌套的列表

```python
my_list = [[1, 2, 3], [4, 5, 6]]
my_list[1][1]
```

### 列表的常用操作（方法）

#### 查询

```python
列表.index(元素)
```

> 查询到后返回索引值

> 查询不到会报错ValueError

#### 修改特定位置的元素值

```python
列表[下标] = 值
```

#### 列表的修改功能

```python
列表.insert(下标, 元素)
```

=== "追加元素1"

	```python
	列表.append(元素)
	```

=== " 追加元素2"

	```python
	列表.extend(其他数据容器)
	```

	- 将其他数据容器中的内容取出，依次追加到列表尾部

=== "删除元素1（下标）"

	```python
	del 列表[下标]
	```

	- 只可删除，不可返回删除元素

=== "删除元素2（下标）"

	```python
	列表.pop(下标)
	element = 列表.pop(下标)
	```

	- 返回值为该删除元素

=== "删除元素3（元素）"

	```python
	列表.romove(元素)
	```

	- 删除匹配到的第一个元素

#### 清空列表元素

```python
列表.clear()
```

#### 统计某一个元素在列表中的数量

```python
列表.count(元素)
```

#### 统计元素个数（函数）

```python
len(列表)
```

### 列表的特点

- 可以容纳多个元素（个数上线2^63^ - 1）
- 元素可以不同类型
- 数据是有序存储（有下标序号）
- 允许重复元素的存在
- 可以修改（增加、删除等）

### 列表的遍历

=== "while循环"

	```python
	index = 0
	while index < len(列表):
		元素 = 列表[index]
		对元素进行处理
		index += 1
	```

=== "for循环"

	```python
	for 临时变量 in 数据容器:
		对临时变量进行处理
	```

### tuple元组

#### 元组的定义

- 以（ ）为标识
- 元组内每个元素用 ，分隔开

```python
# 定义元组的字面量
(元素， 元素， 元素， ……， 元素)

# 定义元组变量
变量名称 = (元素， 元素， 元素， ……， 元素)

# 定义空元组
变量名称 = ()       # 方式一
变量名称 = tuple()  # 方式二

# 定义一个元素的元组
t1 = ('Hello', )
```

- ==元组只有一个元素时，这个元素后面要加一个逗号==

- 支持 ==嵌套==

```python
t1 = ((1, 2, 3), (4, 5, 5))
```

#### 元组的下标索引

```python
t1[1][1]
```

#### 元组的常用操作（方法）

##### 查询

```python
元组.index(元素)
```

> 查询到后返回索引值

> 查询不到会报错ValueError

##### 统计某一个元素在元组中的数量

```python
元组.count(元素)
```

##### 统计元素个数（函数）

```python
len(元组)
```

#### 元组的遍历

=== "while循环"

	```python
	index = 0
	while index < len(元组):
		元素 = 元组[index]
		对元素进行处理
		index += 1
	```

=== "for循环"

	```python
	for 临时变量 in 数据容器:
		对临时变量进行处理
	```

#### 元组的特点

- 可以容纳多个元素
- 元素可以不同类型
- 数据是有序存储（有下标序号）
- 允许重复元素的存在

- ==不可以修改== 元组的内容，否则报错TypeError
- 可以修改元组内list的内容
- 支持for循环

### str字符串

#### 字符串的下标索引

- 下标从0开始（第一个），从前向后每次加一

```python
my_str[0]
```

- 反向索引，从-1开始（最后一个）,从后向前每次减一

```python
my_str[-1]
```

#### 字符串的常用操作（方法）

##### 查询

```python
字符串.index(元素)
```

> 此处元素可以为多个字符

> 查询到后返回索引值

> 查询不到会报错ValueError

##### 字符串的替换

```python
字符串.replace(字符串1， 字符串2)
```

- 将字符串内的全部字符串1替换为字符串2

> 不是修改字符串本身，而是得到一个新的字符串

##### 字符串的分割

```python
字符串.split(分隔符字符串)
```

- 按照指定的分隔符字符串，将字符串划分为多个字符串，并存入列表对象中

> 字符串本身不变，而是得到一个列表对象

> 不传参数的话则按照 ==空格==（不论有几个空格）分开

##### 字符串的规整操作

```python
字符串 = 字符串.strip()
```

- 去前后空格以及回车符

```python
字符串 = 字符串.strip(字符串)
```

```python
my_str = "12 itheima and itcast21"
print(my_str.strip("12"))
```

> itheima and itcast

- 传入的是“12”， 其实就是“1” “2”都会被移除

##### 统计某一个元素在字符串中的数量

```python
字符串.count(元素)
```

##### 统计字符个数（函数）

```python
len(字符串)
```

#### 字符串的特点

- ==只可以== 存储字符
- 长度任意（取决于内存大小）
- 支持下标索引
- 允许重复字符存在
- ==不可以== 修改（增加或删除元素等）
- 支持for循环

#### 字符串的遍历

=== "while循环"

	```python
	index = 0
	while index < len(字符串):
		元素 = 字符串[index]
		对元素进行处理
		index += 1
	```

=== "for循环"

	```python
	for 临时变量 in 数据容器:
		对临时变量进行处理
	```

### 数据容器（序列）的切片

#### 序列

- 内容连续、有序、可以用下标索引的一类数据容器
- 列表、元组、字符串

#### 切片

```python
序列[起始下标：结束下标：步长]
```

- 起始下标表示从何处开始，可以留空，留空视为从头开始
- 结束下标 ==（不含）== 表示何处结束，可以留空，留空视为截取到结尾
- 步长为负数，表示反向取

> 不会影响序列本身，而是得到一个新的序列

### set集合

#### 列表的定义

- 以{ }为标识
- 列表内每个元素用 ，分隔开

```python
# 字面量
{元素1, 元素2, 元素3, 元素4, ……}

# 定义变量
变量名称 = {元素1, 元素2, 元素3, 元素4, ……}

# 定义空集合
变量名称 = set()
```

- ==不可以== 用变量名称 = {}  定义空集合

- 可以为 ==不同== 的数据类型

```python
my_set = {'Aubrey', 666, True}
```

- ==不允许== 元素重复
- 顺序无法保证
- ==不支持== 下标索引访问

#### 集合的常用操作（方法）

##### 添加新元素

```python
集合.add(元素)
```

##### 移除元素

```python
集合.remove(元素)
```

##### 随机取出一个元素(并移除)

```python
集合.pop()
```

##### 清空集合

```python
集合.clear()
```

##### 取两个集合的差集

```python
集合3 = 集合1.difference(集合2)
```

##### 消除两个集合的差集

```python
集合1.difference_update(集合2)
```

- 集合1中删除与集合2相同的元素

##### 集合合并

```python
集合3 = 集合1.union(集合2)
```

##### 统计集合中元素数量（函数）

```python
len(集合)
```

#### 集合的特点

- 可以容纳多个数据
- 数据类型可以不一样
- 数据是 ==无序== 存储的（不支持下标索引）
- ==不允许重复== 数据存在
- 可以修改（增加或删除元素等）
- 支持for循环

#### 集合的遍历

##### ==不可使用== while循环

##### for循环

```python
for 临时变量 in 数据容器:
	对临时变量进行处理
```

### dict字典

#### 字典的定义

- 标识符为{ }
- 存储的元素是：键值对
- 不允许key重复
- 不可以使用下标索引

```python
# 字面量
{key:value, key:value, key:value, ……, key:value}

# 定义变量
变量名称 = {key:value, key:value, key:value, ……, key:value}

# 定义空字典
变量名称 = {}
变量名称 = dict()
```

#### 基于key获得对应value

```python
字典[key]
```

#### 字典的嵌套

- key和value都可以为任意数据类型（key不可以为字典）

```python
stu_score_dict = {
	"stu1":{
		"语文"：77，
		"数学"：66，
		"英语"：33
	}, "stu2":{
		"语文"：88，
		"数学"：86，
		"英语"：55
	}, "stu3":{
		"语文"：99，
		"数学"：96，
		"英语"：66
	}
}
```

#### 字典的常用操作（方法）

##### 新增元素

```python
字典[key] = value
```

- 如果key不存在的话，则会自动增加

##### 更新元素

```python
字典[key] = value
```

- 如果key存在的话，则会更新value

##### 删除元素

```python
字典.pop(key)
```

- 返回指定key对应的value，同时key的数据被删除

##### 清空字典

```python
字典.clear()
```

##### 获取全部的key

```python
字典.keys()
```

- 返回一个列表

##### ==不可使用== while循环

##### 遍历字典1：利用获取全部key

```python
for key in keys:
    进行处理
```

##### 遍历字典2：直接对字典进行for循环

```python
for key in 字典
```

- 每一次循环都是直接得到key

##### 统计字典内元素数量（函数）

```python
len(字典)
```

#### 字典的特点

- 可以容纳多个数据
- 数据可以不同类型
- 每一份数据都是有一个 ==键值对==
- 可以通过key获得value，key不可以重复
- 不支持下标索引
- 可以修改（增加或删除等）
- 支持for循环，不支持while循环

### 数据容器的对比

#### 数据容器的分类

- 是否支持下标索引
	- 支持：列表、元组、字符串  —序列类型
	- 不支持：集合、字典  —非序列类型
- 是否支持重复元素
	- 支持：列表、元组、字符串  —序列类型
	- 不支持：集合、字典  —非序列类型
- 是否可以修改
	- 可以：列表、集合、字典
	- 不可以：元组、字符串

#### 数据容器的通用操作

##### 遍历

- 都可以使用for遍历
- 集合、字典不支持while操作

##### 统计功能

- len(容器)   统计容器的元素个数
- max(容器)   统计容器的最大元素
	- 对字典中的key本身处理
- min(容器)   统计容器的最小元素
	- 对字典中的key本身处理

##### 转换功能

- list(容器)：转换为列表
	- 字典转列表：value直接略掉
- str(容器)：转换为字符串
	- 容器前后直接加字符串
- tuple(容器)：转换为元组
	- 字典转元组：value直接略掉
- set(容器)：转换为集合
	- 字典转集合：value直接略掉

##### 排序功能

- 正向排序：sorted(容器)
- 反向排序：sorted(容器，reverse=True)
- 结果变为了列表
- 字典中value略掉

# 七、函数进阶

## 函数的多返回值

- 按照返回值的顺序写对应多个变量接收即可
- 变量之间用逗号隔开
- 支持不同数据类型的return

```python
def test_return():
	return 1, "hello"
x, y = test_return ()
```

## 函数的多种传参方式

### 位置参数

- 调用函数时根据函数定义的参数位置来传递参数

### 关键字参数

- 函数调用时通过”键 = 值”的方式传递参数
- 清除了传参的顺序要求
- ==位置参数必须位于关键字实参前面==

```python
def user_name(name, age, gender):
	print(name, age, gender)
user_name("小明"， gender="男"， age=20)
```

### 缺省参数

- 定义函数时为参数提供默认值，调用函数时可以不传该参数的值
- ==所有位置参数必须出现在默认参数前，包括函数的定义和调用==

```python
def user_name(name, age, gender="男"):
	print(name, age, gender)
user_name('Tom', 20)
user_name('Tom', 20, "女")
```

### 不定长参数（可变参数）

#### 位置传递不定长

- \*args
- 会合并为一个元组

```python
def user_name(*args):
	print(args)
user_name("Tom")
user_name("Tom", 18)
```

#### 关键字传递不定长

- \*\*kwargs
- 会组成字典

```python
def user_name(**kwargs):
	print(kwargs)
user_name(name="Tom", age=18, id=110)
```

## 匿名函数

### 函数作为参数传递

- 计算逻辑的传递

```python
def test_func(compute):
	result = compute(1, 2)
	print(result)
def compute(x, y):
	return x + y

test_func(compute)
```

### lambda匿名函数

- 函数的定义：
	- def关键字，定义==带有名称==的函数
		- 可以基于名称重复使用
	- lambda关键字，定义==匿名==的函数
		- 只可==临时==使用一次

```python
lamdba 传入参数:函数体(一行代码)
```

> 只能写一行代码，不能写多行

> 可以不用写return,默认就是return

```
def test_func(compute):
	result = compute(1, 2)
	print(result)

test_func(lamdba x, y:x + y)
```

# 八、文件操作

## 文件的编码

- 常用的文件编码方式
	- UTF-8
	- GBK
	- Big5

## 文件的操作

### 打开文件

#### open语法

```python
open(name, mode, encoding)
```

> name:打开的莫表文件的字符串，可以包含文件所在的具体路径
>
> mode:设置打开文件的模式：只读、写入、追加
>
> encoding:编码格式，推荐使用UTF-8

```python
f = open("python.txt", "r", UTF-8)
# encoding 的位置不是第三位，所以不能使用位置参数
```

- 访问模式
	- r  只读
		- 默认指针放在开头
	- w  写入
		- 如果文件已存在，则打开文件，从头开始编辑，原有内容会被全部清空
		- 如果文件不存在，会创建新文件
	- a  追加
		- 如果文件已存在，新的内容会被写入已有文件之后
		- 如果文件不存在，创建新文件进行写入

#### with open语法

```python
with open("python.txt", "r") as f:
	f.readlines()
```

> 在with open语句块中对文件进行操作
>
> 可以在操作完成后自动关闭文件，避免遗忘掉close

### 读操作相关方法

#### read方法

```python
文件对象.read(num)
```

> num表示读取数据的长度（字符），回车键也算
>
> 没有传入num，表示读取整个文件的数据
>
> 返回值为一个字符串

#### readlines方法

```python
文件对象.readlines()
```

> 按照行的方式将文件中的内容进行一次性读取
>
> 返回的是一个列表，每一行的数据为一个元素

#### readline方法

```python
文件对象.readline()
```

> 调用一次读取一行
>
> 返回的是一个字符串

#### for循环读取文件行

```python
for line in open("python.txt", "r")
	print(line)
```

> 每一个line临时变量就记录了文件的一行数据

### 关闭文件

```python
f.close()
```

### 写操作相关方法

#### 内容写入

```python
f.write(内容)
```

#### 内容刷新

```python
f.flush()
```

> 直接调用write只是积攒到了缓冲区
>
> 调用flush才真正写到了硬盘中

> 为了避免频繁操作硬盘，导致效率下降

> close( )内置了flush( )功能

# 九、异常、模块与包

## 异常

### 异常的定义

- 程序运行的过程中出现了错误

### 捕获异常

- 提前假设某处会出现异常，做好提前准备，当真的出现异常时，可以有后续的手段

#### 基本捕获语法

```python
try:
	可能发生异常的代码
except:
	如果出现异常执行的代码
```

```python
try:
	f = open("python.txt", "r")
except:
	f = open("python.txt", "w")
```

#### 捕获指定异常

```python
try:
	print(name)
except NameError as e:
	print("name变量名称定义错误")
```

> e为异常信息

#### 捕获多个异常

```python
try:
	print(1/0)
except (NameError, ZeroDivisionError):
	print("ZeroDivision错误")
```

```python
try:
	print(1/0)
except (NameError, ZeroDivisionError) as e:
	print("ZeroDivision错误")
```

#### 捕获所有异常

1. 语法1（基础语法）

	```python
	try:
		可能发生异常的代码
	except:
		如果出现异常执行的代码
	```

2. 语法2

	```python
	try:
		可能发生异常的代码
	except Exception:
		如果出现异常执行的代码
	```

	```python
	try:
		可能发生异常的代码
	except Exception as e:
		如果出现异常执行的代码
	```

### 异常else(可选)

```python
try:
	print(1)
except Exception as e:
	print(e)
else:
	print("我是else,是没有异常的时候执行的代码")
```

### 异常finally(可选)

- 无论是否异常都要执行的代码

```python
try:
	f = open("python.txt", "r")
except:
	f = open("python.txt", "w")
else:
	print("没有异常")
finally:
	f.close()
```

### 异常的传递

- 函数2调用函数1时，函数1的异常不处理时会传递给函数2
- 所有函数都没有捕获异常时程序会报错
- 出现异常后直接回退到上一层函数

## 模块

### 模块的定义

- 是一个python文件，以.py结尾，模块内能定义函数、类和变量，也能包含可执行代码
- 我们可以把模块拿过来用

### 模块的导入

```python
[from 模块名] import [模块| 类 | 变量 | 函数 | *] [as 别名]
```

> [ ]表示可选项

- 常用组合形式

	- import 模块名

		```python
		import time
		time.sleep(5)
		```

	- from 模块名 import 类、变量、方法

		```python
		from time import sleep
		sleep(5)
		```

	- from 模块名 import \*

		```python
		from time import *
		sleep(5)
		```

	- import 模块名 as 别名

		```python
		import time as t
		t.sleep(5)
		```

	- from 模块名 import 功能名 as 别名

		```python
		from time import sleep as sl
		sl(5)
		```

### 自定义模块

```python
"my_moudule1.py"
def test(x, y):
	print(x + y)
	
"test.py"
import my_module1
my_module1.test(1, 2)
```

- 当导入多个模块时，且模块内有同名功能，当调用这个同名模块时，调用的是后面导入的模块的功能

### \_\_main\_\_

- 不想让模块中的测试内容被输出

```python
if __name__ == '__main__':    # 输入main敲回车即可 
    test(1, 4)
```

### \_\_all\_\_

- 对应导入模块中的\*

```python
"my_module.py"
__all__ = ['test_A']

def test_A():
	print('testA')
def test_B():
	print('testB')

"test.py"
from my_module import *
# 导入的只有函数test_A
```

## 包

### 包的定义

- 包就是一个文件夹，可以包含多个模块（.py文件）
- 还包含一个\_\_init\_\_.py文件（有就是包，没就是普通文件夹）

### 导入包

```python
import 包名.模块名
```

### \_\_all\_\_

```python
"__init__.py"
__all__ = ['module1']

"test.py"
from my_packege import *
```

### 第三方安装包的定义

- 第三方包（可以提高开发效率）
	- 科学计算常用：numpy包
	- 数据分析常用：pandas包
	- 大数据计算常用：pyspark、apache-flink包
	- 图形可视化常用：matplotlib、pyecharts包
	- 人工智能常用：tensortflow

### 第三方包的安装-pip

- 命令提示符中输入：

	```python
	pip install 包名称
	```

	```python
	# 国内网站安装
	pip install -i https://pupi.tuna.tsinghua.edu.cn/simple 包名称
	```

  # 十、数据可视化

## json数据格式

### json的定义

- 是一种轻量级的数据交互格式
- 可以按照它指定的格式组织和封装数据
- 本质是一个带有特定格式的字符串
- 中转的数据格式
- 格式
	- 将python中的字典转换成字符串
	- 将python中的列表（内含元素都是字典）转换成字符串

### Python数据和 Json数据的相互转化

```python
# 导入json模块
import json

# 准备符合json格式要求的python数据
data = [{"name":"老王", "age":16}, {"name":"张三", "age":20}]

# 通过json.dumps(data)方法把python数据转化为json数据
data = json.dumps(data)
# 要转换的内容里面有中文时，加ensure_ascii限制
data = json.dumps(data, ensure_ascii=False)

# 通过json.loads(data)方法把json数据转化为python数据
data = json.loads(data)
```

## pyecharts

### 折线图可视化

#### 基本流程

```python
# 导包，导入Line功能构建折线图对象
from pyecharts.charts import Line

# 得到折线图对象
line = Line()
# 添加x轴数据
line.add_xaxis(["中国", "美国", "英国"])
# 添加y轴数据
line.add_yaxis("GDP", [30, 20, 10])
# 生成图表
line.render()
```

#### 运用实例

```python
import json
from pyecharts.charts import Line
from pyecharts.options import TitleOpts, LabelOpts

# 读取数据
f_us = open("美国.txt", "r", encoding="UTF-8")
us_data = f_us.read()

# 去掉不合JSON规范的开头
us_data = us_data.replace("jsonp", "")

# 去掉不合JSON规范的结尾
us_data = us_data[:-2]

# JSON转Python字典
us_dict = json.loads(us_data)

# 获取trend key
us_trend_data = us_dict['data'][0]['trend']

# 获取日期数据，用于x轴，取2020年
us_x_data = us_trend_data['updataDate'][:314]

# 获取确认数据，用于y轴，取2020年
us_y_data = us_trend_data['list'][0]['data'][:314]

# 生成图表
line = Line()
# 添加x轴数据
line.add_xaxis(us_x_data)
# 添加y轴数据
line.add_yaxis("美国确诊人数", us_y_data, label_opts=LabelOpts(is_show=False))
# 设置全局配置
line.set_global_opts(
    # 标题设置
    title_opts=TitleOpts(title='2020年美国确诊人数折线图', pos_left='center', pos_bottom='1%')
)
# 调用render方法生成图表
line.render()

# 关闭文件对象
f_us.close()
```

### 配置选项

#### 全局配置

```python
set_global_opts
```

```python
# 导包，导入Line功能构建折线图对象
from pyecharts.charts import Line
from pyecharts.options import TitleOpts, LegendOpts, ToolboxOpts, VisualMapOpts

# 得到折线图对象
line = Line()
# 添加x轴数据
line.add_xaxis(["中国", "美国", "英国"])
# 添加y轴数据
line.add_yaxis("GDP", [30, 20, 10])
# 全局配置
line.set_global_opts(
    title_opts=TitleOpts(title="GDP展示", pos_left="center", pos_bottom="1%"),
    legend_opts=LegendOpts(is_show=True),
    toolbox_opts=ToolboxOpts(is_show=True),
    visualmap_opts=VisualMapOpts(is_show=True)
)
# 生成图表
line.render()
```

- 使用函数时记得导入包

#### 系列配置

- 对图中的某一部分进行设置，比如x轴，y轴等

### 地图可视化

#### 运用实例

```python
from pyecharts.charts import Map
from pyecharts.options import VisualMapOpts

# 准备地图对象
map = Map()
# 准备数据
data = [
    ("北京市", 99),
    ("上海市", 199),
    ("湖南省", 299),
    ("台湾省", 399),
    ("广东省", 499)
]
# 添加数据
map.add("测试地图", data, "china")
# 设置全局配置
map.set_global_opts(
    visualmap_opts=VisualMapOpts(
        is_show=True,
        is_piecewise=True,
        pieces=[
            {"min":1, "max":199, "label":"1-9人", "color":"#CCFFFF"},
            {"min":200, "max":399, "label":"10-99人", "color":"#FF6666"},
            {"min":400, "max":500, "label":"100-500人", "color":"#990033"}
        ]
    )
)
#绘图
map.render()
```

  ### 动态柱状图

#### 基础柱状图

```python
from pyecharts.charts import  Bar
from pyecharts.options import LabelOpts

# 构建柱状图对象
bar = Bar()

# 添加x轴数据
bar.add_xaxis(["中国", "美国", "英国"])
# 添加y轴数据
bar.add_yaxis("GDP", [30, 20, 10], label_opts=LabelOpts(position="right"))
#反转xy轴
bar.reversal_axis()

# 绘图
bar.render("render.html")
```

#### 基础时间线动态图

- Timeline( )——时间线

```python
from pyecharts.charts import  Bar, Timeline
from pyecharts.globals import ThemeType
from pyecharts.options import LabelOpts

bar1 = Bar()
bar1.add_xaxis(["中国", "美国", "英国"])
bar1.add_yaxis("GDP", [30, 20, 10], label_opts=LabelOpts(position="right"))
bar1.reversal_axis()

bar2 = Bar()
bar2.add_xaxis(["中国", "美国", "英国"])
bar2.add_yaxis("GDP", [50, 60, 30], label_opts=LabelOpts(position="right"))
bar2.reversal_axis()

bar3 = Bar()
bar3.add_xaxis(["中国", "美国", "英国"])
bar3.add_yaxis("GDP", [10, 40, 60], label_opts=LabelOpts(position="right"))
bar3.reversal_axis()

# 构建时间线对象
timeline = Timeline(
    {"theme":ThemeType.LIGHT}
)

# 在时间线内添加柱状图对象
timeline.add(bar1, "点1")
timeline.add(bar2, "点2")
timeline.add(bar3, "点3")

# 绘图是用时间线对象绘图，而不是bar对象
timeline.render("render.html")

# 设置自动播放
timeline.add_schema(
    play_interval=500,   # ms
    is_timeline_show=True,
    is_auto_play=True,
    is_loop_play=True
)
```

#### 列表的sort方法

```python
列表.sort(key=选择排序依据的函数, reverse=True|False)
```

> key 为传入一个函数，表示将列表的每一个元素都传入函数中，返回排序的依据
>
> reverse代表反转排序的结果，True表示降序，False表示升序

- 带名函数形式

	```python
	# 如下嵌套列表形式，要求对外层列表进行排序，排序的依据是内层列表的第二个元素数字
	# 以前学习的sorted函数就无法使用了，可以使用列表的sort方法
	my_list = [['a', 33], ['b', 55], ['c', 11]]
	
	# 定义排序方法
	def choose_sort_key(element):
	    return element[1]
	
	my_list.sort(key=choose_sort_key, reverse=True)
	
	print(my_list)
	```

- 匿名lambda形式

	```python
	# 如下嵌套列表形式，要求对外层列表进行排序，排序的依据是内层列表的第二个元素数字
	# 以前学习的sorted函数就无法使用了，可以使用列表的sort方法
	my_list = [['a', 33], ['b', 55], ['c', 11]]
	
	my_list.sort(key=lambda element:element[1], reverse=True)
	print(my_list)
	```

#### 运用实例

```python
# 数据集
import random
with open("GDP.csv", "a", encoding="UTF-8") as f:
    for i in range(1960, 2020):
        a1 = random.randint(100, 2001)
        f.write(f"{i}, 美国, {a1}\n")
        a2 = random.randint(100, 2001)
        f.write(f"{i}, 英国, {a2}\n")
        a3 = random.randint(100, 2001)
        f.write(f"{i}, 法国, {a3}\n")
        a4 = random.randint(100, 2001)
        f.write(f"{i}, 日本, {a4}\n")
        a5 = random.randint(100, 2001)
        f.write(f"{i}, 中国, {a5}\n")
        a6 = random.randint(100, 2001)
        f.write(f"{i}, 泰国, {a6}\n")
        a7 = random.randint(100, 2001)
        f.write(f"{i}, 印度, {a7}\n")
        a8 = random.randint(100, 2001)
        f.write(f"{i}, 巴西, {a8}\n")
        a9 = random.randint(100, 2001)
        f.write(f"{i}, 芬兰, {a9}\n")
        a10 = random.randint(100, 2001)
        f.write(f"{i}, 希腊, {a10}\n")
```

```python
# 运行代码
from pyecharts.charts import Bar, Timeline
from pyecharts.options import *
from pyecharts.globals import ThemeType

# 读取数据
f = open("GDP.csv", "r", encoding="UTF-8")
data_lines = f.readlines()

# 关闭文件
f.close()

# 删除第一条数据
data_lines.pop(0)

# 将数据转换为字典存储，格式为：
# {年份： [[国家， GDP]， [国家， GDP]，……]， 年份： [[国家， GDP]， [国家， GDP]，……]， ……}
# 先定义一个字典对象
data_dict = {}
for line in data_lines:
    year = int(line.split(",")[0])  # 年份
    country = line.split(",")[1]    # 国家
    gdp = int(line.split(",")[2]) # gdp
    # 来判断字典里有没有指定的key
    try:
        data_dict[year].append([country, gdp])
    except KeyError:
        data_dict[year] = []
        data_dict[year].append([country, gdp])
# print(data_dict)

# 创建时间线对象
timeline = Timeline({"theme":ThemeType.LIGHT})

# 排序年份（创建字典时顺序不确定）
sorted_year_list = sorted(data_dict.keys())
# print(sorted_year_list)

# 柱状图
for year in sorted_year_list:
    data_dict[year].sort(key=lambda element:element[1], reverse=True)
    # 取出本年度前8名的国家
    year_data = data_dict[year][0:8][::-1]
    x_data = []
    y_data = []
    for country_gdp in year_data:
        x_data.append(country_gdp[0]) # x轴添加国家
        y_data.append(country_gdp[1]) # y轴添加gdp数据
    # 构建柱状图
    bar = Bar()
    bar.add_xaxis(x_data)
    bar.add_yaxis("GDP", y_data, label_opts=LabelOpts(position="right"))
    # 反转x轴和y轴
    bar.reversal_axis()
    # 设置每一年的图标的标题
    bar.set_global_opts(
        title_opts=TitleOpts(title=f"{year}年全球前八GDP数据")
    )
    timeline.add(bar, str(year))

# 设置时间线自动播放
timeline.add_schema(
    play_interval=1000,
    is_timeline_show=True,
    is_auto_play=True,
    is_loop_play=True
)

# 绘图
timeline.render("render.html")
```

# 十一、面向对象

## 对象

### 使用对象组织数据

1. 设计类（class)

	```python
	class Student:
		name = None
	```

2. 创建对象

	```python
	stu1 = Student()
	stu1 = Student()
	```

3. 对象属性赋值

	```python
	stu_1.name = "1"
	stu_1.name = "2"
	```

```python
# 1.设计一个类
class Student:
	name = None
	gender = None
	nationality = None
	native_place = None
	age = None
# 2.创建一个对象
stu_1 = Student()
# 3.对象属性进行赋值
stu1.name = "摸鱼"
stu1.gender = "男"
stu1.natianality = "中国"
stu1.native_place = "山东"
stu1.age = 31
# 4.获取对象中存取的信息
print(stu1.name)
print(stu1.gender)
```

## 类

### 类的定义

```python
class 类名称:
	类的属性
	类的行为
```

- class 是关键字，表示要定义类
- 类的属性，即定义在类中的==变量==（成员变量）
- 类的行为，即定义在类中的==函数==（成员方法）
- 方法:类内部的函数

### 创建类对象

```python
对象 = 类名称
```

### 成员方法的定义语法

```python
def 方法名(self, 形参1, 形参2,……)
	方法体
```

- ==self==必须写
- 表示类对象本身
- 使用类对象调用方法时self自动传入
- 在方法内部，想要访问类的成员变量，必须使用self

```python
class Student:
	name = None
	age = None
	def say_hi(self，msg):
		print(f"Hi大家好，我是{self.name},{msg})
```

- self关键字尽管在参数列表，但是传参的时候不用理会它

## 类和对象

- 类
	- 属性
	- 行为
- 对象
	- 将类实体化
- 面向对象编程
	- 设计类，基于类，创建对象，由对象做具体的工作

## 魔术方法

### 构造方法

```python
__init__()
```

- 创建类对象（构造类）的时候，会==自动执行==
- 创建类对象（构造类）的时候，将==传入参数自动传递给\_\_init\_\_方法使用==

```python
class Student:
	name = None
	age = None
	tel = None
	def __init__(self, name, age, tel):
		self.name = name
		self.age = age
		self.tel = tel
		
stu = Student('摸鱼小猪'， 31， '18500006666')
```

### 字符串方法

```python
__str__()
```

```python
class Student:
	def __init__(self, name, age, tel):
		self.name = name
		self.age = age
		
stu = Student('摸鱼小猪', 11)
print(stu) # 会输出地址
```

```python
class Student:
	def __init__(self, name, age, tel):
		self.name = name
		self.age = age
    def __str__(self):
        return f"Student类对象，name={self.name}, age={self.age}""
		
stu = Student('摸鱼小猪', 11)
print(stu) # 会输出字符串中的内容
```

### 小于比较方法

```python
__lt__()
```

```python
class Student:
	def __init__(self, name, age):
		self.name = name
		self.age = age
		
stu1 = Student('摸鱼小猪', 11)
stu2 = Student('摸鱼', 13) 
print(stu1 < stu2) # 会报错，无法比较
```

```python
class Student:
	def __init__(self, name, age):
		self.name = name
		self.age = age
    def __lt__(self, other):
        return self.age < other.age
		
stu1 = Student('摸鱼小猪', 11)
stu2 = Student('摸鱼', 13) 
print(stu1 < stu2) # 会输出True或False
```

- 传入参数有一个==other==
- return 处必须写的是<

### 小于等于比较方法

```python
__le__()
```

```python
class Student:
	def __init__(self, name, age):
		self.name = name
		self.age = age
		
stu1 = Student('摸鱼小猪', 11)
stu2 = Student('摸鱼', 13) 
print(stu1 <= stu2) # 会报错，无法比较
```

```python
class Student:
	def __init__(self, name, age):
		self.name = name
		self.age = age
    def __le__(self, other):
        return self.age <= other.age
		
stu1 = Student('摸鱼小猪', 11)
stu2 = Student('摸鱼', 13) 
print(stu1 <= stu2) # 会输出True或False
```

- 传入参数有一个==other==
- return 处必须写的是<=

### 等于比较方法

```python
__eq__()
```

```python
class Student:
	def __init__(self, name, age):
		self.name = name
		self.age = age
		
stu1 = Student('摸鱼小猪', 11)
stu2 = Student('摸鱼', 13) 
print(stu1 <= stu2) # 会报错，无法比较
```

```python
class Student:
	def __init__(self, name, age):
		self.name = name
		self.age = age
    def __eq__(self, other):
        return self.age == other.age
		
stu1 = Student('摸鱼小猪', 11)
stu2 = Student('摸鱼', 13) 
print(stu1 == stu2) # 会输出True或False
```

## 类型注解

### 类型注解的定义

- 在代码中涉及数据交互的地方，提供数据类型的注解（显示的说明）
- 一般在无法直接看出变量的类型时会添加变量的类型注解

### 变量的类型注解

- 基础数据类型的注解

	```python
	var_1:int = 0
	```

- 类对象类型注解

	```python
	class Student:
		pass
	stu:Student = Student()
	```

- 基础容器类型注解

	```python
	my_list:list = [1, 2, 3]
	```

- 容器类型的详细注解

	```python
	my_list:tuple[str, int, bool] = ("itheima", 666, Ture)
	my_dict:dict[str, int] = {"itheima", 666}
	```

	> ==元组==类型设置类型详细注解，需要将每一个元素都标记出来
	>
	> ==字典==类型设置类型详细注解，需要两个类型，第一个是key，第二个是value

### 注释中进行类型注解

```python
# type:类型
```

```python
var_1 = random.randiant(1, 10) # type: int
```

### 函数（方法）形参列表和返回值的类型注解

```python
# 对形参进行类型注解
def 函数方法名(形参名：类型， 形参名：类型， ……):
	pass
```

```python
def add(x: int, y: int):
	return x + y
```

```python
# 对返回值进行类型注解
def 函数方法名(形参名：类型， 形参名：类型， ……)->返回值类型:
	pass
```

### Union联合类型注解

```python
from typing import Union

my_list: list[Union[str, int]] = [1, 2, "itheimi", "itcast"]
my_dict: list[str, Union[str, int]] = {"name":"摸鱼小猪", "age": 18}
```

## 面向对象的三大特性

### 封装

#### 封装的定义

- 将现实事物的
	- 属性
	- 行为
- 封装到类中，描述为
	- 成员变量
	- 成员方法

#### 成员变量

- 私有成员变量

	- 变量名以\_\_开头（两个下划线）

		```python
		__current_voltage = None
		```

- 私有成员方法

	- 方法名以\_\_开头（两个下划线）

		```python
		def __keep_single_core(self):
			print("哈哈哈哈")
		```

> 类对象不能使用私有成员变量和私有成员方法

>类的内部可以使用私有成员变量和私有成员方法

### 继承

#### 单继承

```python
class 类名（父类名）:
	类内容体
```

```python
class Phone:
    IMEI = None
    producer = "HM"

    def call_by_4g(self):
        print("4g通话")

class Phone2022(Phone):
    face_id = 10001

    def call_by_5g(self):
        print("2022新功能")

phone = Phone2022()
print(phone.producer)
phone.call_by_4g()
phone.call_by_5g()
```

#### 多继承

```python
class 类名（父类1， 父类2， ……， 父类N）:
	类内容体
```

```python
class Phone:
    IMEI = None
    producer = "HM"

    def call_by_4g(self):
        print("4g通话")

class NFCReader:
    nfc_type = "第五代"
    producer = "HM"

    def read_card(self):
        print("NFC读卡")

    def write_card(self):
        print("NFC写卡")

class RemoteControl:
    rc_type = "红外遥控"

    def control(self):
        print("红外遥控开启了")

class MyPhone(Phone, NFCReader, RemoteControl):
    pass  # 补全语法，表示这里为空

phone = MyPhone()
phone.call_by_4g()
phone.read_card()
phone.write_card()
phone.control()
```

> 多继承中，如果父类有同名方法或者属性，==先继承的优先级高于后继承==
>
> ==pass关键字==，是占位语句，来表示函数（方法）或者类定义的完整性，表示无内容，空的意思

#### 复写

- 在子类中重新定义与父类中同名的属性或方法

```python
class Phone:
    IMEI = None
    producer = "ITHEIMA"

    def call_by_5g(self):
        print("这是父类5g通话")

class MyPhone(Phone):
    producer = "ITCAST"       # 复写父类的成员属性
    
    def call_by_5g(self):
        print("这是子类5g通话") # 复写父类的成员方法

phone = MyPhone()
phone.call_by_5g()
print(phone.producer)
```

#### 调用父类同名成员

```python
# 方法一：
父类名.父类成员属性
父类名.父类成员方法(self)   # 必须加self

# 方法二：
super().父类成员属性
super().父类成员方法()     # 不加self
```

```python
class Phone:
    IMEI = None
    producer = "ITHEIMA"

    def call_by_5g(self):
        print("5g通话")

class MyPhone(Phone):
    producer = "ITCAST"

    def call_by_5g(self):
        print("开启单核处理模式")
        # 方法一：
        # print(f"父类的厂商是{Phone.producer}")
        # Phone.call_by_5g(self)
        # 方法二：
        # print(f"父类的厂商是{super().producer}")
        # super().call_by_5g()

phone = MyPhone()
phone.call_by_5g()
print(phone.producer)
```

>==只可以在子类内部==调用父类的同名成员，子类的实体类对象调用默认是调用子类复写的

### 多态

#### 定义

- 多态指的是多种状态，即完成某个行为时，使用不同的对象会得到不同的状态
- 同样的行为，传入不同的对象，得到不同的状态

#### 具体运用

```python
class Animal:
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        print("汪汪汪")

class Cat(Animal):
    def speak(self):
        print("喵喵喵")

def make_noise(animal:Animal):
    animal.speak()

dog = Dog()
cat = Cat()
make_noise(dog)
make_noise(cat)
```

- 多态常作用在继承关系上
	- 函数（方法）形参声明接受父类对象
	- 实际传入父类的子类对象进行工作
- 简单来说
	- 以父类做定义声明
	- 以子类做实际工作
	- 用以获得同一行为，不同状态

#### 抽象类（接口）

- 设计含义
	- 父亲用来确定有哪些方法
	- 具体的方法实现，由子类自行决定
- 抽象类
	- 含有抽象方法的类
- 抽象方法
	- 方法体是空实现的（pass)称之为抽象方法

```python
class AC:
    def cool_wind(self):
        '''制冷'''
        pass

    def hot_wind(self):
        '''制热'''
        pass

    def swing_l_r(self):
        '''左右摆风'''
        pass

class Midea_AC(AC):
    def cool_wind(self):
        print("美的空调制冷核心技术")

    def hot_wind(self):
        print("美的空调制热核心技术")

    def swing_l_r(self):
        print("美的空调左右摆风技术")

class GREE_AC(AC):
    def cool_wind(self):
        print("格力空调制冷核心技术")

    def hot_wind(self):
        print("格力空调制热核心技术")

    def swing_l_r(self):
        print("格力空调左右摆风技术")

def make_cool(ac:AC):
    ac.cool_wind()

midea_ac = Midea_AC()
gree_ac = GREE_AC()

make_cool(midea_ac)
make_cool(gree_ac)
```

## 运用案例

### data_define.py

```python
'''
数据定义的类
'''

class Record:

    def __init__(self, date, order_id, money, province):
        self.date = date            # 订单日期
        self.order_id = order_id    # 订单ID
        self.money = money          # 订单金额
        self.province = province    # 销售省份

    def __str__(self):
        return f"{self.date}, {self.order_id}, {self.money}, {self.province}"
```

### file_define.py

```python
'''
和文件相关的类定义
'''
from data_define import Record
import json

# 先定义一个抽象类用来做顶层设计，确定有哪些功能需要实现
class FileReader:

    def read_data(self)->list[Record]:
        '''读取文件数据，将读到的每一条数据都转换成Record对象，将他们都封装到list内返回即可'''
        pass

class TextFileReader(FileReader):

    def __init__(self, path):
        self.path = path            # 定义成员变量记录文件的路径

    # 复习父类的方法
    def read_data(self) ->list[Record]:
        f = open(self.path, "r", encoding="UTF-8")

        record_list: list[Record] = []
        for line in f.readlines():
            line = line.strip()     # 消除读取到的每一行数据中的\n
            data_list = line.split(",")
            record = Record(data_list[0], data_list[1], int(data_list[2]), data_list[3])
            record_list.append(record)

        f.close()
        return record_list

class JsonFileReader(FileReader):

    def __init__(self, path):
        self.path = path            # 定义成员变量记录文件的路径

    # 复习父类的方法
    def read_data(self) -> list[Record]:
        f = open(self.path, "r", encoding="UTF-8")

        record_list: list[Record] = []
        for line in f.readlines():
            data_dict = json.loads(line)
            record = Record(data_dict["date"], data_dict["order_id"], data_dict["money"], data_dict["province"])
            record_list.append(record)

        f.close()
        return record_list


if __name__ == '__main__':
    json1_file_reader = JsonFileReader(r"D:\文件夹汇总\课业任务\python\python_learn\销售数据分析\2011年2月销售数据JSON.txt")
    for j in json1_file_reader.read_data():
        print(j)
    text1_file_reader = TextFileReader(r"D:\文件夹汇总\课业任务\python\python_learn\销售数据分析\2011年1月销售数据.txt")
    for i in text1_file_reader.read_data():
        print(i)
```

### main.py

```python
'''
面向对象，数据分析案例，主业务逻辑代码
实现步骤：
1.设计一个类，可以完成数据的封装
2.读取一个抽象类，定义文件读取的相关功能，并使用子类实现具体的功能
3.读取文件，生产数据对象
4.进行数据需求的逻辑计算（计算每一天的销售额）
5.通过Pyecharts进行图像绘制
'''
from pyecharts.globals import ThemeType

from file_define import FileReader, TextFileReader, JsonFileReader
from data_define import Record
from pyecharts.charts import Bar
from pyecharts.options import *

text_file_reader = TextFileReader(r"D:\文件夹汇总\课业任务\python\python_learn\销售数据分析\2011年1月销售数据.txt")
json_file_reader = JsonFileReader(r"D:\文件夹汇总\课业任务\python\python_learn\销售数据分析\2011年2月销售数据JSON.txt")

jan_data: list[Record] = text_file_reader.read_data()
feb_data: list[Record] = json_file_reader.read_data()
# 将两个月份的数据合并为一个list来存储
all_data = jan_data + feb_data

# 开始进行数据计算
# {"2011-01-01": 1234, "2011-01-02": 1234, "2011-01-03": 1234}
data_dict = {}
for record in all_data:
    if record.date in data_dict:
        # 当前日期已经有记录，做累加
        data_dict[record.date] += record.money
    else:
        data_dict[record.date] = record.money

# 可视化图表开发
bar = Bar(init_opts=InitOpts(theme=ThemeType.LIGHT))

bar.add_xaxis(list(data_dict.keys()))
bar.add_yaxis("销售额", list(data_dict.values()), label_opts=LabelOpts(is_show=False))
bar.set_global_opts(
    title_opts=TitleOpts(title="每日销售额")
)

bar.render("每日销售额柱状图.html")
```

# 十二、SQL

## SQL的概述

### 定义

- 访问和处理数据库的标准的计算机语言
- 操作数据库的专用工具

### 分类

- 数据定义：DDL
	- 库的创建、表的删除等
- 数据操纵：DML
	- 新增数据、删除数据、修改数据
- 数据控制：DCL
	- 新增用户、删除用户、密码修改、权限管理
- 数据查询：DQL
	- 基于需求查询和计算数据

### 语法特征

- ==大小写不敏感==
- 可以单行书写和多行书写，以；结尾
- 支持注释
	- 单行注释：\-\- 注释内容(\-\-后面一定要加一个空格)
	- 单行注释：# 注释内容(#后面可以不加空格，建议加上)
	- 多行注释：/* 注释内容 */

## DDL

### 库管理

- 查看数据库
	- show databases;
- 使用数据库
	- use 数据库名称
- 创建数据库
	- create database 数据库名称 [charset UTF8]
- 删除数据库
	- drop database 数据库名称；
- 查看当前使用的数据库
	- select database();

### 表管理

- 查看有哪些表

	- show tables 

		> 注意要先选择数据库

- 删除表

	- drop table 表名称；
	- drop table if exists 表名称；

- 创建表

	- creat table 表名称(

		​	列名称 列类型，

		​	列名称 列类型，

		​	……

		);

>列类型有
>
>- int				            \-\- 整数
>- float                        \-\- 浮点数
>- varchar(长度)         \-\- 文本，长度为数字，做最大长度限制
>- date                        \-\- 日期类型
>- timestamp             \-\- 时间戳类型

## DML

### 数据插入INSERT

```mysql
INSERT INTO 表[列1， 列2， ……， 列N] VALUES(值1， 值2，……， 值N)[,(值1， 值2，……， 值N), ……, (值1， 值2，……， 值N)]
```

### 数据删除DELETE

```mysql
DELETE FROM 表名称 [WHERE 条件判断]
```

- 条件判断：列 操作符 值
	- 操作符：= < > <= >= != 等等

### 数据更新UPDATE

```mysql
UPDATE 表名 SET 列=值 [WHERE 条件判断];
```

 ## DQL

### 基础数据查询

```mysql
SELECT 字段列表|* FROM 表 [WHERE 条件判断]
```

### 分组聚合

```sql
SELECT 字段|聚合函数 FROM 表 [WHERE 条件] GROUP BY 列
```

- 聚合函数有：
	-  SUM(列)   求和
	- AVG(列)     求平均值
	- MIN(列)     求最小值
	- MAX(列)     求最大值
	- COUNT(列) 求数量

### 排序分页

#### 排序显示

- 对某个列进行排序

```sql
SELECT 列表|聚合函数|* FROM 表
WHERE ...
GROUP BY ...
ORDER BY ... [ASC | DESC]
```

> ASC 升序

> DESC 降序

#### 分页限制

- 对查询结果进行数量限制或者分页显示

```sql
SELECT 列表|聚合函数|* FROM 表
WHERE ...
GROUP BY ...
ORDER BY ... [ASC | DESC]
LIMIT n[, m] # 跳过前n条
```

## Python & MySQL

### 基础使用

```python
from pymysql import Connection
# 构建到MySQL的链接
conn = Connection(
    host="localhost",    # 主机名（IP）
    port=3306,           # 端口
    user="root",         # 账户
    password="888qwert"  # 密码
)

#print(conn.get_server_info())
# 获取游标对象
cursor = conn.cursor()
# 选择数据库
conn.select_db("world")
# 执行非查询性质sql
# cursor.execute("create table test_pymysql(id int);")
# 执行查询性质sql
cursor.execute("select * from student;")
# 获取查询结果
results = cursor.fetchall()
for r in results:
    print(r)
# 关闭链接
conn.close()
```

### 数据插入

#### 手动commit

```python
from pymysql import Connection
# 构建到MySQL的链接
conn = Connection(
    host="localhost",    # 主机名（IP）
    port=3306,           # 端口
    user="root",         # 账户
    password="888qwert"  # 密码
)

#print(conn.get_server_info())
# 获取游标对象
cursor = conn.cursor()
# 选择数据库
conn.select_db("world")
# 执行sql
cursor.execute("insert into student values(10001, '摸鱼小猪', 20)")
# 通过commit确认
conn.commit()
# 关闭链接
conn.close()
```

#### 自动commit

```python
from pymysql import Connection
# 构建到MySQL的链接
conn = Connection(
    host="localhost",    # 主机名（IP）
    port=3306,           # 端口
    user="root",         # 账户
    password="888qwert", # 密码
    autocommit=True      # 设置自动提交
)

#print(conn.get_server_info())
# 获取游标对象
cursor = conn.cursor()
# 选择数据库
conn.select_db("world")
# 执行sql
cursor.execute("insert into student values(10002, '摸鱼小猪', 20)")
# 关闭链接
conn.close()
```

### 运用案例

- 修改[运用案例](# 运用案例)中的main.py：

```python
'''
面向对象，数据分析案例，主业务逻辑代码
实现步骤：
1.设计一个类，可以完成数据的封装
2.读取一个抽象类，定义文件读取的相关功能，并使用子类实现具体的功能
3.读取文件，生产数据对象
4.进行数据需求的逻辑计算（计算每一天的销售额）
5.通过Pyecharts进行图像绘制
'''
from pymysql import Connection
from file_define import FileReader, TextFileReader, JsonFileReader
from data_define import Record



text_file_reader = TextFileReader(r"D:\文件夹汇总\课业任务\python\python_learn\销售数据分析\2011年1月销售数据.txt")
json_file_reader = JsonFileReader(r"D:\文件夹汇总\课业任务\python\python_learn\销售数据分析\2011年2月销售数据JSON.txt")

jan_data: list[Record] = text_file_reader.read_data()
feb_data: list[Record] = json_file_reader.read_data()
# 将两个月份的数据合并为一个list来存储
all_data = jan_data + feb_data

# 构建MySQL链接对象
conn = Connection(
    host="localhost",
    port=3306,
    user="root",
    password="888qwert",
    autocommit=True
)
# 获取游标对象
cursor = conn.cursor()
# 选择数据库
conn.select_db("py_sql")
# 组织SQL语句
for record in all_data:
    sql = f"insert into orders(order_date, order_id, money, province) "\
          f"values('{record.date}', '{record.order_id}', {record.money}, '{record.province}');"
    # 执行sql语句
    cursor.execute(sql)
```

# 十三、==Spark==(暂略)

## 基础准备

### 构建PySpark 执行环境入口对象

- PySpark 执行环境入口对象是：类SparkContext的类对象

```python
# 导包
from pyspark import SparkConf, SparkContext

# 创建SparkConf类对象
conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
# 基于SparkConf类对象创建SparkContext类对象
sc = SparkContext(conf=conf)

# 打印PySpark的运行版本
print(sc.version)

# 停止SparkContext对象的运用（停止PySpark程序）
sc.stop()
```

## 数据输入

### RDD对象

- 弹性分布式数据集

### 数据容器转RDD

```python
# 导包
from pyspark import SparkConf, SparkContext

# 创建SparkConf类对象
conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
# 基于SparkConf类对象创建SparkContext类对象
sc = SparkContext(conf=conf)

# 数据容器转RDD
rdd = sc.parallelize(数据容器对象)

# 如果要查看RDD里面有什么内容的话，需要用collect()方法
print(rdd.collect())

sc.stop()
```

>字符串会被拆分出1个个的字符，存入RDD对象
>
>字典仅有key会被存入RDD对象

### 读取文件转RDD对象

```python
# 导包
from pyspark import SparkConf, SparkContext

# 创建SparkConf类对象
conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
# 基于SparkConf类对象创建SparkContext类对象
sc = SparkContext(conf=conf)

rdd = sc.textFile(文件路径)

# 输出RDD的内容
print(rdd.collect())

sc.stop()
```

## 数据计算

### map算子

- 将RDD的数据一条条处理，返回新的RDD

# 十四、Python高阶技巧

## 闭包

```python
def outer(logo):

    def inner(msg):
        print(f"<{logo}>{msg}<{logo}>")

    return inner

fn1 = outer("黑马程序员")
fn1("大家好呀")
fn1("学Python就来")
```

- 闭包中修改外部函数变量的值，用nonlocal

	```python
	def outer(num1):
	
	    def inner(num2):
	        nonlocal num1
	        num1 += num2
	        print(num1)
	
	    return inner
	
	fn1 = outer(10)
	fn1(10)
	fn1(10)
	```

## 装饰器

- 不破坏目标函数原有的代码和功能的前提下，为目标函数增加新功能

### 一般闭包写法

```python
def outer(func):
    def inner():
        print("我要睡觉了")
        func()
        print("我要起床了")

    return inner

def sleep():
    import random
    import time
    print("睡眠中……")
    time.sleep(random.randint(1,5))

fn = outer(sleep)
fn()
```

### 语法糖写法

```python
def outer(func):
    def inner():
        print("我要睡觉了")
        func()
        print("我要起床了")

    return inner

@outer
def sleep():
    import random
    import time
    print("睡眠中……")
    time.sleep(random.randint(1,5))

sleep()

```

## 设计模式

### 单例模式

- 保证一个类只有一个实例，并提供一个访问它的全局访问点

```python
# str_tools.py
class StrTools:
	pass

str_tool = StrTools()
```

```python
# test.py
from str_tool.py import str_tool

s1 = str_tool
s2 = str_tool
```

### 工厂模式

- 基于工厂提供给的方法去创建对象形式

```python
class Person:
    pass

class Worker(Person):
    pass
class Student(Person):
    pass
class Teacher(Person):
    pass

class Factory:
    def get_person(self, p_type):
        if p_type == "w":
            return Worker()
        elif p_type == "s":
            return Student()
        else:
            return Teacher()
        
factory = Factory()
worker = factory.get_person("w")
student = factory.get_person("s")
teacher = factory.get_person("t")
```

## 多线程

### 进程、线程

- 进程
	- 一个程序，运行在系统之上，这个便称为这个程序的一个运行进程，并分配进程ID方便系统管理
- 线程
	- 线程归属进程，一个进程可以开启多个线程，执行不同的工作，是进程的实际工作的最小单位

---

- 多任务执行
	- 操作系统中运行多个进程
- 多线程运行
	- 一个进程内运行多个线程

---

- 并行执行
	- 同一时间做不同的工作

### 多线程编程

```python
import threading
from lib2to3.pgen2.tokenize import group

from Demos.win32cred_demo import target

thread_obj = threading.Thread([group [, target [, name [, args [, kwargs]]]]])
- group:暂时无用，未来功能的预留参数
- target:执行的目标任务名
- args:以元组的方式给执行的任务传参
- kwargs:以字典方式给执行任务传参
- name:线程名，一般不用设置

# 启动线程，让线程开始工作
thread_obj.start()
```

```python
import time
import threading

def sing(msg):
    while True:
        print(msg)
        time.sleep(1)

def dance(msg):
    while True:
        print(msg)
        time.sleep(1)

if __name__ == '__main__':
    # 创建一个唱歌的线程
    sing_thread = threading.Thread(target=sing, args=("我要唱歌哈哈哈",))
    # 创建一个跳舞的线程
    dance_thread = threading.Thread(target=dance, kwargs={"msg": "我在跳舞啦啦啦"})
    # 运行线程
    sing_thread.start()
    dance_thread.start()
```

## 网络编程

## Socket

- 进程之间想要进行网络通信需要Socket
- 服务端
	- 等待其他进程的连接，可以接受发来的消息、可以回复消息
- 客户端
	- 主动连接服务端，可以发送消息、可以回复消息

## 服务端

```python
'''
演示Socket服务端开发
'''
# 创建socket对象
import socket
socket_server = socket.socket()

# 绑定ip地址和端口
socket_server.bind(("localhost", 8888))

# 监听端口
socket_server.listen(1)  # listen方法内接受一个整数参数，表示接收的连接数量

# 等待客户端连接
# result: tuple = socket_server.accept()
# accept方法返回的是二元元组（链接对象， 客户端地址信息）
# accept方法是阻塞方法，等待客户端的连接，如果没有连接，就卡在这一步
# conn = result[0]         # 客户端和服务端的连接对象
# address = result[1]      # 客户端的地址信息
conn, address = socket_server.accept()
print(f"接收到了客户端的连接，客户端的信息是，{address}")
while True:
    # 接收客户端信息
    data: str = conn.recv(1024).decode("UTF-8")
    # recv接受的参数是缓冲区大小，一般给10241即可
    # recv方法的返回值是一个字节数组也就是bytes对象，可以通过decode方法通过UTF-8解码，将字节数组转换为字符串对象
    print(f"客户端发来的消息是：{data}")

    # 发送回复消息
    msg = input("请输入你要和客户端回复的消息：")
    if msg == "exit":
        break
    # encode可以将字符串编码为字节数组对象
    conn.send(msg.encode("UTF-8"))

# 关闭连接
conn.close()
socket_server.close()
```

## 客户端

```python
'''
演示Socket客户端开发
'''
import socket

# 创建socket对象
socket_client = socket.socket()

# 连接到服务器
socket_client.connect(("localhost", 8888))

while True:
    # 发送消息
    msg = input("请输入要给服务端发送的消息：")
    if msg == "exit":
        break
    socket_client.send(msg.encode("UTF-8"))
    
    # 接收返回消息
    recv_data = socket_client.recv(1024)          # 1024是缓冲区的大小，一般1024即可，recv是阻塞的
    print(f"服务端回复的消息是：{recv_data.decode('UTF-8')}")

# 关闭连接
socket_client.close()
```

## 正则表达式

- 字符串定义规则，并通过规则去验证字符串是否匹配

### 基础匹配

- re模块：match、search、findall

#### match

```python
re.match(匹配规则， 被匹配字符串)
```

- 从被匹配开头字符串开始匹配，匹配成功返回匹配对象（包含匹配信息），匹配不成功返回空

```python
import re
s = '1python itheima python itheima python itheima'

result = re.match('python', s)
print(result)
print(result.span())
print(result.group())
```

> 匹配不到

#### search

```python
re.search(匹配规则， 被匹配字符串)
```

- 搜索整个字符串，找到匹配的，从前向后找到第一个后就停止，不会继续向后，找不到返回None

```python
import re
s = '1python itheima python itheima python itheima'

result = re.search('python', s)
print(result)
print(result.span())
print(result.group())
```

#### findall

```python
re.findall(匹配规则， 被匹配字符串)
```

- 搜索整个字符串，找到所有匹配的，找不到返回None

```python
import re
s = 'python itheima python itheima python itheima'

result = re.findall('python', s)
print(result)
print(result.span())
print(result.group())
```

### 元字符匹配

#### 单字符匹配

- .	匹配任意一个字符（除了\n），\\\.匹配点本身
- []   匹配[]中列举的字符
- \d  匹配数字，即0-9
- \D  匹配非数字
- \s   匹配空白，即空格、tab键
- \S   匹配非空白
- \w  匹配单词字符，即a-z、A-Z、0-9、\_
- \W 匹配非单词字符

```python
'''
演示Python正则表达式使用元字符进行匹配
'''
import re

s = "itheima 666 i love you !!  @ hhaa18648631haha ## itcast"
# 找出全部数字
result = re.findall(r'\d', s)  # 字符串前面带上r的标记，表示字符串中转义字符无效，就是普通字符的意思
print(result)
# 找出特殊字符
result = re.findall(r'\W', s)
print(result)
# 找出全部英文字母
result = re.findall(r'[6-8]', s)
print(result)
result = re.findall(r'[abc6]', s)
print(result)
```

#### 数量匹配

- \*            匹配前一个规则的数字出现0至无数次
- \+            匹配前一个规则的字符出现1至无数次
- ？          匹配前一个规则的字符出现0或者1次
- {m}       匹配前一个规则的字符出现m次 
- {m,}      匹配前一个规则的字符最少出现m次
- {m,n}   匹配前一个规则的字符出现m至n次

#### 边界匹配

- ^           匹配字符串开头
- $           匹配字符串结尾
- \b         匹配一个单词的边界
- \B         匹配非单词边界

#### 分组匹配

- |          匹配左右任意一个
- ( )        将括号中字符作为一个分组

#### 运用案例

```python
'''
演示Python正则表达式使用元字符进行匹配
'''
import re

# 匹配账号，只能由字母和数字组成，长度限制为6-10位
r = '^[0-9a-zA-Z]{6,10}$'
s = '1234567AB_'
print(re.findall(r, s))
# 匹配QQ号，要求纯数字，长度5-11，第一位不为0
r = '^[1-9][0-9]{4,10}$'
s = '012345678'
print(re.findall(r, s))
# 匹配邮箱地址，只允许qq、163、gmail这三种邮箱地址
# {内容}.{内容}.{内容}.{内容}.{内容}.{内容}@{内容}.{内容}.{内容}
r = r'(^[\w-]+(\.[\w-]+)*@(qq|163|gmail)(\.[\w-]+)+$)'
s = '123.456.789.123@qq.com'
print(re.match(r, s).group())
```

## 递归

```python
'''
递归的使用
'''
import os.path

def get_files_recursion_from_dir(path):
    '''
    从指定文件夹中使用递归的方式，获取全部的文件列表
    :param path: 被判断的文件夹
    :return:list,包含全部的文件，如果目录不存在或者无文件就返回一个空list
    '''
    file_list = []
    if os.path.exists(path):            # 判断指定路径是否存在
        for f in os.listdir(path):      # 列出路径下的内容
            new_path = path + '/' + f
            if os.path.isdir(new_path): # 判断指定路径是否是一个文件夹
                # 这个目录是文件夹，不是文件
                file_list += get_files_recursion_from_dir(new_path)
            else:
                file_list.append(new_path)
    else:
        print(f"指定的目录{path}不存在")
        return []
    return file_list

if __name__ == '__main__':
    print(get_files_recursion_from_dir(r"D:/文件夹汇总/课业任务/python/python_learn"))
```

