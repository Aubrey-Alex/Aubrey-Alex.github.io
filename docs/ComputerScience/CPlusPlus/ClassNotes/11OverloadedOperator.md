# 运算符重载 (Operator Overloading)

!!! abstract "概述"
    运算符重载允许用户自定义类型像内置类型一样使用运算符。这是C++中实现多态性的重要方式之一，使代码更加直观和易读。

## 1. 引言

运算符重载是C++中一个强大的特性，它允许我们为自定义类型定义运算符的行为。通过运算符重载，我们可以使自定义类型的使用方式更加自然和直观。

---

## 2. 可重载的运算符

<div class="grid cards" markdown>

-   :material-plus-minus:{ .lg .middle } __算术运算符__
    
    ---
    可以重载的算术运算符包括：
    
    - 单目运算符：`+`、`-`、`*`、`&`
    - 双目运算符：`+`、`-`、`*`、`/`、`%`、`^`、`&`、`|`、`~`

</div>

<div class="grid cards" markdown>

-   :material-equal:{ .lg .middle } __赋值运算符__
    
    ---
    可以重载的赋值运算符包括：
    
    - `=`、`+=`、`-=`、`*=`、`/=`
    - `%=`、`^=`、`&=`、`|=`
    - `<<=`、`>>=`、`>>>=`、`<<=`

</div>

<div class="grid cards" markdown>

-   :material-compare:{ .lg .middle } __关系运算符__
    
    ---
    可以重载的关系运算符包括：
    
    - `==`、`!=`、`<`、`>`、`<=`、`>=`
    - `!`、`&&`、`||`

</div>

<div class="grid cards" markdown>

-   :material-brackets:{ .lg .middle } __其他运算符__
    
    ---
    其他可以重载的运算符包括：
    
    - `,`、`->*`、`->`、`()`、`[]`
    - `new`、`delete`
    - `new[]`、`delete[]`

</div>

### 不可重载的运算符

以下运算符不能重载：

- `.`（成员访问）
- `.*`（成员指针访问）
- `::`（作用域解析）
- `?:`（条件运算符）
- `sizeof`（大小运算符）
- `typeid`（类型信息运算符）
- `static_cast`、`dynamic_cast`、`const_cast`、`reinterpret_cast`（类型转换运算符）

### 重载限制

1. 只能重载已有的运算符，不能创建新的运算符
2. 运算符必须重载在类或枚举类型上
3. 重载的运算符必须：
   - 保持操作数数量不变
   - 保持运算符优先级不变

---

## 3. 运算符重载的实现方式

### 成员函数方式

```cpp
class Integer {
public:
    Integer(int n = 0) : i(n) {}
    
    // 成员函数方式重载运算符
    const Integer operator+(const Integer& n) const {
        return Integer(i + n.i);
    }
    
private:
    int i;
};
```

特点：
- 隐式第一个参数（this指针）
- 不需要对接收者进行类型转换
- 必须能够访问类的定义
- 成员函数可以完全访问类的所有数据

### 全局函数方式

```cpp
class Integer {
    friend const Integer operator+(const Integer& lhs, const Integer& rhs);
    // ...
};

const Integer operator+(const Integer& lhs, const Integer& rhs) {
    return Integer(lhs.i + rhs.i);
}
```

特点：
- 显式第一个参数
- 不需要特殊访问权限
- 可能需要声明为友元
- 可以对两个参数都进行类型转换

### 选择建议

1. 单目运算符应该作为成员函数
2. `=`、`[]`、`->`、`()`、`->*`必须作为成员函数
3. 其他双目运算符应该作为非成员函数

---

## 4. 参数传递和返回值

### 参数传递

1. 如果参数是只读的，使用const引用传递（除了内置类型）
2. 对于不改变类的成员函数，使用const修饰（如布尔运算符、+、-等）
3. 对于全局函数，如果左操作数会改变，使用引用传递（如赋值运算符）

### 返回值

根据运算符的预期含义选择返回类型：

1. 对于`operator+`等需要生成新对象的运算符，返回const对象
2. 逻辑运算符应该返回bool（或旧编译器中的int）
3. 赋值运算符应该返回引用

---

## 5. 特殊运算符重载

### 自增自减运算符

```cpp
class Integer {
public:
    // 前缀形式
    const Integer& operator++() {
        *this += 1;
        return *this;
    }
    
    // 后缀形式
    const Integer operator++(int) {
        Integer old(*this);
        ++(*this);
        return old;
    }
};
```

### 关系运算符

```cpp
class Integer {
public:
    bool operator==(const Integer& rhs) const {
        return i == rhs.i;
    }
    
    bool operator!=(const Integer& rhs) const {
        return !(*this == rhs);
    }
    
    bool operator<(const Integer& rhs) const {
        return i < rhs.i;
    }
    
    bool operator>(const Integer& rhs) const {
        return rhs < *this;
    }
    
    bool operator<=(const Integer& rhs) const {
        return !(rhs < *this);
    }
    
    bool operator>=(const Integer& rhs) const {
        return !(*this < rhs);
    }
};
```

### 下标运算符

```cpp
class Vector {
public:
    int& operator[](int index) {
        return data[index];
    }
    
private:
    int* data;
};
```

### 流运算符

```cpp
// 输入流运算符
istream& operator>>(istream& is, T& obj) {
    // 读取obj的代码
    return is;
}

// 输出流运算符
ostream& operator<<(ostream& os, const T& obj) {
    // 写入obj的代码
    return os;
}
```

---

## 6. 类型转换

### 构造函数转换

```cpp
class PathName {
    string name;
public:
    PathName(const string&);
    ~PathName();
};

string abc("abc");
PathName xyz(abc);  // 隐式转换
```

### 显式转换

使用`explicit`关键字防止隐式转换：

```cpp
class PathName {
    string name;
public:
    explicit PathName(const string&);
    ~PathName();
};

string abc("abc");
PathName xyz(abc);  // 显式转换
```

### 转换运算符

```cpp
class Rational {
public:
    operator double() const;  // Rational到double的转换
};

Rational::operator double() const {
    return numerator_/(double)denominator_;
}
```

---

## 7. 最佳实践

1. 不要仅仅因为可以重载运算符就重载它
2. 重载运算符应该使代码更容易阅读和维护
3. 不要重载`&&`、`||`或`,`（逗号运算符）
4. 尽量使用显式转换而不是隐式转换
5. 对于需要类型转换的情况，优先使用成员函数而不是转换运算符

---

## 8. 总结

运算符重载是C++中实现多态性的重要方式，它允许我们为自定义类型定义运算符的行为。通过合理使用运算符重载，我们可以使代码更加直观和易读。但是，我们也需要注意运算符重载的限制和最佳实践，避免滥用导致代码难以理解和维护。

## 9. pdf资料

<embed src="pdfs/11 Overloaded Operator.pdf" type="application/pdf" width="100%" height="400px" />
