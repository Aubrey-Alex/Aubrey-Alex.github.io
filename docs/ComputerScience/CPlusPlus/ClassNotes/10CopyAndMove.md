# 复制与移动 (Copy and Move)

!!! abstract "概述"
    复制与移动是C++中对象创建和资源管理的关键机制。复制构造函数允许从现有对象创建新对象，而移动语义则提供了更高效的对象资源转移方式，特别是对于临时对象和大型资源。

## 1. 引言

在C++中，对象可以通过多种方式创建和传递，包括复制已有对象和移动资源。这些机制对于资源管理和性能优化至关重要，尤其是在处理大型对象和临时对象时。

---

## 2. 复制 (Copying)

<div class="grid cards" markdown>

-   :material-content-copy:{ .lg .middle } __复制的基本概念__

    ---
    复制是从现有对象创建新对象的过程：
    
    - 在函数调用中传递参数时
    - 在初始化新对象时
    - 在函数返回对象时
    
    C++通过复制构造函数实现复制操作，这是面向对象编程中的基本机制。

</div>

### 复制构造函数

复制构造函数是一种特殊的构造函数，用于创建一个对象的副本：

```cpp
T::T(const T&);  // 复制构造函数的签名
```

特点：

- 参数是对同类型对象的常量引用
- 如果不提供，C++会自动生成默认复制构造函数
- 默认复制构造函数会复制每个成员变量（浅复制）

=== "复制构造函数示例"
    ```cpp
    class HowMany {
    public:
        HowMany() { counter++; print("HowMany()"); }
        HowMany(const HowMany& h) { 
            counter++; 
            print("HowMany(const HowMany&)"); 
        }
        ~HowMany() { counter--; print("~HowMany()"); }
        void print(const string& msg) const {
            cout << msg << ": " << counter << endl;
        }
    private:
        static int counter;
    };
    int HowMany::counter = 0;
    ```

=== "复制构造函数调用点"
    ```cpp
    // 1. 通过值传递参数
    void func(HowMany h) { /*...*/ }
    HowMany x;
    func(x);  // 调用复制构造函数
    
    // 2. 初始化
    HowMany y = x;  // 复制构造，不是赋值
    HowMany z(x);   // 复制构造，更明确的语法
    
    // 3. 函数返回
    HowMany getHowMany() {
        HowMany h;
        return h;  // 可能调用复制构造函数
    }
    ```

!!! warning "默认复制构造函数的问题"
    对于包含指针成员的类，默认复制构造函数会导致多个对象共享同一块内存，造成严重问题：
    
    - 当一个对象被销毁时，它会释放指针指向的内存
    - 其他对象持有的指针变成悬空指针
    - 当这些对象被销毁时，会尝试再次释放已释放的内存

### 指针成员与深复制

当类包含指针成员时，有几种处理方式：

1. **共享所有权**：多个对象共享同一资源，使用引用计数（如智能指针）
2. **独占所有权**：每个对象拥有自己的资源副本（深复制）
3. **禁止复制**：将复制构造函数声明为私有，禁止复制操作

??? example "Person类实现示例"
    ```cpp
    class Person {
    public:
        // 构造函数
        Person(const char *s) {
            name = new char[strlen(s) + 1];
            strcpy(name, s);
        }
        
        // 析构函数
        ~Person() {
            delete [] name;
        }
        
        // 复制构造函数（深复制）
        Person(const Person& p) {
            name = new char[strlen(p.name) + 1];
            strcpy(name, p.name);
        }
        
        void print() {
            cout << "Name: " << name << endl;
        }
        
    private:
        char *name;  // 使用char*而非string
    };
    ```

!!! tip "深复制 vs 浅复制"
    - **浅复制**：默认复制构造函数执行的成员级复制，仅复制指针值
    - **深复制**：不仅复制指针，还分配新内存并复制指针指向的内容
    
    当类管理资源（如内存、文件描述符等）时，通常需要深复制。

### 复制构造函数的调用时机

1. **按值传递参数**：
   ```cpp
   void roster(Person p);  // 函数声明
   Person child("Ruby");   // 创建对象
   roster(child);          // 调用复制构造函数
   ```

2. **对象初始化**：
   ```cpp
   Person baby_a("Fred");
   Person baby_b = baby_a;  // 复制构造，不是赋值
   Person baby_c(baby_a);   // 复制构造，更明确的语法
   ```

3. **函数返回值**：
   ```cpp
   Person captain() {
       Person player("George");
       return player;  // 调用复制构造函数
   }
   ```

!!! note "编译器优化"
    现代编译器可以在安全的情况下"优化掉"不必要的复制。例如：
    
    ```cpp
    // 可能调用复制构造函数
    Person copy_func(char *who) {
        Person local(who);
        return local;  // 可能被优化
    }
    
    // 不需要复制
    Person nocopy_func(char *who) {
        return Person(who);  // 返回值优化
    }
    ```

### 复制构造 vs 赋值操作

重要区别：

- 每个对象只构造一次，可以被多次赋值
- 复制构造函数初始化新对象
- 赋值操作符修改现有对象

```cpp
Person a("Alice");
Person b("Bob");

Person c = a;  // 复制构造函数
b = a;         // 赋值操作符
```

---

## 3. 移动语义 (Move Semantics)

<div class="grid cards" markdown>

-   :material-swap-horizontal:{ .lg .middle } __移动语义基本概念__

    ---
    移动语义允许资源从一个对象"窃取"到另一个对象，而不是复制：
    
    - 特别适用于临时对象和右值
    - 避免不必要的内存分配和复制
    - 通过移动构造函数和移动赋值运算符实现
    
    C++11引入的移动语义显著提高了性能并简化了资源管理。

</div>

### 右值引用

C++11引入了右值引用（使用`&&`表示），专门用于绑定到临时对象（右值）：

```cpp
T&& // 右值引用
```

右值分类：

- **纯右值**：临时对象、字面常量
- **将亡值**：即将被销毁、资源可以被移动的对象

### 移动构造函数

移动构造函数从另一个对象"窃取"资源，而不是复制：

```cpp
T::T(T&& other);  // 移动构造函数的签名
```

移动构造函数的实现要点：

- 接管other的资源（指针、句柄等）
- 将other的指针设为nullptr，防止资源被释放两次
- 通常使用`noexcept`标记，表示不会抛出异常

=== "移动构造函数示例"
    ```cpp
    class HasPtrMem {
    public:
        // 构造函数
        HasPtrMem() : d(new int(0)) {
            cout << "Construct: " << ++n_cstr << endl;
        }
        
        // 复制构造函数
        HasPtrMem(const HasPtrMem& h) : d(new int(*h.d)) {
            cout << "Copy construct: " << ++n_cptr << endl;
        }
        
        // 移动构造函数
        HasPtrMem(HasPtrMem&& h) : d(h.d) {
            h.d = nullptr;  // 防止h析构时释放内存
            cout << "Move construct: " << ++n_mptr << endl;
        }
        
        // 析构函数
        ~HasPtrMem() {
            delete d;
            cout << "Destruct: " << ++n_dstr << endl;
        }
        
    private:
        int* d;
        static int n_cstr;
        static int n_dstr;
        static int n_cptr;
        static int n_mptr;
    };
    ```

=== "动态数组移动实现"
    ```cpp
    template <typename T>
    class DynamicArray {
    private:
        T* data;
        size_t size;
    
    public:
        // 移动构造函数
        DynamicArray(DynamicArray&& other) noexcept
            : data(other.data), size(other.size) {
            // 窃取资源后将other置为安全状态
            other.data = nullptr;
            other.size = 0;
        }
        
        // 移动赋值运算符
        DynamicArray& operator=(DynamicArray&& other) noexcept {
            if (this != &other) {
                // 释放自身资源
                delete[] data;
                
                // 窃取other的资源
                data = other.data;
                size = other.size;
                
                // 将other置为安全状态
                other.data = nullptr;
                other.size = 0;
            }
            return *this;
        }
    };
    ```

!!! danger "移动语义注意事项"
    移动操作必须修改源对象（被移动的对象）的状态：
    
    - 将指针成员置为nullptr
    - 确保源对象处于安全可析构状态
    - 源对象在移动后状态可能未定义，不应再使用其值

### std::move

`std::move`是一个模板函数，将左值强制转换为右值引用，从而启用移动语义：

```cpp
vector<int> v1{1, 2, 3, 4};
vector<int> v2 = v1;           // 复制构造
vector<int> v3 = std::move(v1); // 移动构造
// v1现在处于有效但未指定的状态
```

`std::move`的主要用途：

- 将左值转换为右值引用，启用移动语义
- 用于显式表明对象可以被移动
- 在实现移动操作中转移成员资源

!!! warning "使用std::move的风险"
    使用`std::move`后，被移动对象进入一个有效但未指定的状态：
    
    - 可以对其赋新值
    - 可以安全地销毁
    - 不应该再使用其值（除非重新赋值）
    
    ```cpp
    string str1 = "Hello";
    string str2 = std::move(str1); // str1现在可能为空
    cout << str1;                  // 危险操作
    ```

### 移动赋值运算符

移动赋值运算符提供了与移动构造函数类似的功能，但用于已存在的对象：

```cpp
T& T::operator=(T&& other) noexcept;
```

移动赋值运算符的实现要点：

- 首先释放自身的资源
- 接管other的资源
- 将other的指针设为nullptr
- 返回`*this`引用

```cpp
HasPtrMem& operator=(HasPtrMem&& h) noexcept {
    if (this != &h) {
        delete d;      // 释放当前资源
        d = h.d;       // 获取h的资源
        h.d = nullptr; // 防止h析构时释放资源
    }
    return *this;
}
```

---

## 4. 完美转发 (Perfect Forwarding)

<div class="grid cards" markdown>

-   :material-cast:{ .lg .middle } __完美转发概念__

    ---
    完美转发允许函数模板将参数不变地传递给另一个函数：
    
    - 保留参数的所有类型信息（左值/右值、const/非const）
    - 避免不必要的复制和转换
    - 利用C++11的引用折叠规则和std::forward实现
    
    在模板编程和通用库开发中特别有用。

</div>

### 引用折叠规则

C++11引入了"引用折叠"规则，用于处理引用的引用：

| 类型组合 | 折叠结果 |
|---------|---------|
| T& &    | T&      |
| T& &&   | T&      |
| T&& &   | T&      |
| T&& &&  | T&&     |

简单规则：如果任一引用是左值引用，结果就是左值引用；否则是右值引用。

### std::forward

`std::forward`是一个条件转换，根据模板参数的类型保留参数的值类别（左值/右值）：

```cpp
template<typename T>
void wrapper(T&& arg) {
    // 如果arg是左值引用，保持为左值引用
    // 如果arg是右值引用，转换为右值引用
    someFunction(std::forward<T>(arg));
}
```

完美转发示例：

```cpp
template<typename T, typename... Args>
unique_ptr<T> make_unique(Args&&... args) {
    return unique_ptr<T>(new T(std::forward<Args>(args)...));
}
```

!!! info "为什么需要完美转发"
    - 仅使用值传递会有额外的复制开销
    - 仅使用左值引用不能接受右值
    - 仅使用右值引用会把左值转为右值，可能不安全
    - 完美转发保留原始参数的所有类型信息

---

## 5. 初始化与初始化列表

C++提供多种初始化对象的方式，C++11进一步扩展了初始化语法。

### 初始化形式

1. **小括号初始化**：
   ```cpp
   string str("hello");
   ```

2. **等号初始化**：
   ```cpp
   string str = "hello";
   ```

3. **大括号初始化**（C++11）：
   ```cpp
   string str{"hello"};
   ```

=== "POD类型初始化"
    ```cpp
    struct Student {
        char* name;
        int age;
    };
    
    Student s = {"John", 18};                // 单个对象初始化
    Student sArr[] = {{"John", 18}, {"Jane", 19}}; // 数组初始化
    ```

=== "列表初始化"
    ```cpp
    class Test {
        int a;
        int b;
    public:
        Test(int i, int j) : a(i), b(j) {}
    };
    
    Test t{0, 0};           // 等价于Test t(0, 0)
    Test* pT = new Test{1, 2}; // 等价于Test* pT = new Test(1, 2)
    int* a = new int[3]{1, 2, 0}; // 数组初始化
    ```

=== "容器初始化"
    ```cpp
    // C++11容器初始化器
    vector<string> vs = {"first", "second", "third"};
    map<string, string> singers = {
        {"Lady Gaga", "+1 (212) 555-7890"},
        {"Beyonce", "+1 (212) 555-0987"}
    };
    ```

!!! tip "统一初始化"
    C++11的大括号初始化（也称为统一初始化）提供了一致的语法，适用于几乎所有情况，包括：
    
    - 基本类型的变量
    - 类对象
    - 动态分配的对象
    - 容器和数组
    - 防止窄化转换（如将浮点数赋给整数）

---

## 6. 其他相关知识点

### std::swap的实现

使用移动语义可以高效实现swap操作：

=== "传统实现"
    ```cpp
    template<typename T>
    void swap(T& a, T& b) {
        T tmp{a};  // 调用复制构造函数
        a = b;     // 复制赋值
        b = tmp;   // 复制赋值
    }
    ```

=== "移动语义实现"
    ```cpp
    template<typename T>
    void swap(T& a, T& b) {
        T tmp{std::move(a)};  // 移动构造
        a = std::move(b);     // 移动赋值
        b = std::move(tmp);   // 移动赋值
    }
    ```

### using函数声明

派生类可以使用`using`声明引入基类的函数，这在重载时特别有用：

```cpp
class Base {
public:
    void f() { /*...*/ }
};

class Child : public Base {
public:
    using Base::f;       // 引入Base::f
    void f(int i) { /*...*/ }  // 重载f
};
```

### C++的六个特殊成员函数

C++类可以定义六个特殊成员函数：

1. **默认构造函数**：`T::T()`
2. **析构函数**：`T::~T()`
3. **复制构造函数**：`T::T(const T&)`
4. **复制赋值运算符**：`T& T::operator=(const T&)`
5. **移动构造函数**（C++11）：`T::T(T&&)`
6. **移动赋值运算符**（C++11）：`T& T::operator=(T&&)`

!!! warning "规则"
    - 如果定义了任何自定义复制/移动操作，编译器不会自动生成其他默认版本
    - 一般来说，这些特殊成员函数应该"同时提供，或者同时不提供"
    - Rule of Three/Five/Zero：如果需要自定义其中一个，通常需要自定义多个

---

## 7. 函数参数与返回值

### 参数传递

选择合适的参数传递方式：

1. **值传递**：`void f(Student s)`
   - 创建新对象，适合小型对象
   - 可以添加`const`：`void f(const Student s)`

2. **指针传递**：`void f(Student* p)`
   - 传递对象地址，可能为null
   - 可以添加`const`：`void f(const Student* p)`

3. **引用传递**：`void f(Student& s)`
   - 传递别名，不创建新对象
   - 可以添加`const`：`void f(const Student& s)`

!!! tip "最佳实践"
    - 传入对象准备存储时使用值传递
    - 只需要读取值时使用const指针或const引用
    - 需要修改对象时使用非const引用或指针
    - 对于小型对象（如int）优先使用值传递

### 返回值

选择合适的返回方式：

1. **值返回**：`Student f()`
      - 返回新创建的对象
      - 可能启用返回值优化

2. **指针返回**：`Student* f()`
      - 返回对象地址，调用者负责管理生命周期
      - 避免返回局部变量的地址

3. **引用返回**：`Student& f()`
      - 返回对象的别名
      - 避免返回局部变量的引用

!!! danger "避免返回局部资源"
    ```cpp
    char* foo() {
        char* p = new char[10];
        strcpy(p, "something");
        return p;  // 危险：返回动态分配的内存
    }
    
    void bar() {
        char* p = foo();
        printf("%s", p);
        delete p;  // 调用者必须记得释放资源
    }
    ```
    更好的做法是使用智能指针或让调用者传入缓冲区。

!!! success "函数参数和返回值指南"
    - 如果想存储对象，传入一个对象
    - 如果只想获取值，传入const指针或引用
    - 如果想修改对象，传入指针或引用
    - 如果在函数中创建对象，返回该对象
    - 仅返回传入对象的指针或引用
    - 避免new一个对象并返回指针

---

## 8. 总结

我们学习了：

- 复制构造函数及其工作原理
- 浅复制与深复制的区别
- 移动语义及其性能优势
- 右值引用和std::move的使用
- 完美转发和std::forward
- 初始化列表和各种初始化方式
- 函数参数和返回值的最佳实践

复制和移动机制使C++能够有效管理资源，同时提供高性能和安全性。C++11引入的移动语义显著提高了涉及临时对象和资源转移的操作效率。

## 9. pdf资料

<embed src="pdfs/10 Copy and Move.pdf" type="application/pdf" width="100%" height="400px" />
