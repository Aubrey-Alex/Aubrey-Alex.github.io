# 多态性 (Polymorphism)

!!! abstract "概述"
    多态性是面向对象编程中的核心概念，它允许对象以多种形式出现，通过统一的接口处理不同类型的对象。

## 1. 引言

多态性是面向对象编程中的一个重要概念，它允许对象以多种形式出现。通过多态性，我们可以使用统一的接口来处理不同类型的对象。

---

## 2. 子类型 (Subtyping)

在多态性中，子类可以被视为其父类的实例。

=== "重构前"
    ```cpp
    void addCD(CD &theCD);
    void addDVD(DVD &theDVD);
    ```

=== "重构后"
    ```cpp
    void addItem(Item &theItem);
    ```

### 调用示例
```cpp
DVD myDVD;
database.addItem(myDVD);
```

!!! tip "替代性"
    子类对象可以替代父类对象使用，增强了代码的灵活性和可扩展性。

---

## 3. 子类与子类型

- 📌 **类定义了类型**：类是对象的蓝图。
- 📌 **子类定义了子类型**：子类是对父类的扩展。
- 📌 **替代原则**：子类的对象可以在需要父类对象的地方使用（这被称为替代）。

---

## 4. 子类型与赋值

子类对象可以赋值给父类指针变量：

```cpp
Vehicle *v1 = new Vehicle();
Vehicle *v2 = new Car();
Vehicle *v3 = new Bicycle();
```

!!! warning "注意事项"
    确保子类与父类之间的关系是合法的，以避免运行时错误。

---

## 5. 子类型与参数传递

```cpp
public class Database {
    public void addItem(const Item &theItem) {
        // ...
    }
}

DVD dvd;
CD cd;
database.addItem(dvd);
database.addItem(cd);
```

!!! success "关键点"
    子类对象可以作为参数传递给父类方法，增强了方法的通用性。

---

## 6. 转换

公共继承应隐含替代关系：

- 如果 `B` 是 `A` 的子类，则可以在任何需要 `A` 的地方使用 `B`。
- 如果 `B` 是 `A` 的子类，那么对 `A` 成立的一切也对 `B` 成立。

给定 `D` 是 `B` 的派生类，以下转换是合法的：

- D → B
- D* → B*
- D& → B&

!!! caution "小心谨慎"
    如果替代关系不合法，可能会导致程序错误！

---

## 7. 向上转型 (Upcasting)

向上转型是将派生类对象视为基类对象的过程。

> 这就像说：学生是人类。你们是学生。所以你们是人类。

```cpp
Manager pete("Pete", "444-55-6666", "Bakery");
Employee* ep = &pete; // 向上转型
Employee& er = pete; // 向上转型
```

### 向上转型会丢失类型信息

```cpp
ep->print(cout); // 打印基类版本
```

---

## 8. 静态类型与动态类型

- **静态类型**：变量声明的类型
- **动态类型**：变量引用的对象的实际类型

```cpp
Car *c1 = new Car();     // 静态类型和动态类型都是Car
Vehicle *v1 = new Car(); // 静态类型是Vehicle，动态类型是Car
```

编译器的工作是检查静态类型违规：

```cpp
for(Item item : items) {
    item.print(); // 编译时错误，如果Item中没有定义print()
}
```

---

## 9. 多态变量

指针或引用变量是多态变量，它们可以持有声明类型的对象，或声明类型的子类型的对象。

---

## 10. 虚函数

=== "非虚函数"
    - 编译器生成静态或直接调用到声明类型
    - 执行速度更快

=== "虚函数"
    - 可以在派生类中透明地重写
    - 对象携带虚函数表
    - 编译器检查表并动态调用正确的函数
    - 如果编译器在编译时知道函数，它可以生成静态调用

```cpp
class Shape {
public:
    Shape();
    virtual ~Shape();
    virtual void render();
    void move(const XYPos&);
    virtual void resize();
protected:
    XYPos center;
};
```

### 虚函数在C++中的工作方式

```cpp
Rectangle r;
long long **vptr = (long long **)(&r);
void (*fp)() = (void (*)())vptr[0][0];
fp();
```

---

## 11. 对象切片问题

当使用对象赋值时，可能会发生对象切片：

```cpp
Ellipse elly(20F, 40F);
Circle circ(60F);
elly = circ; // 对象切片!
```

此时：

- `circ` 的 `area` 被切掉（只有适合 `elly` 的部分被复制）
- 来自 `circ` 的虚函数表被忽略
- `elly` 中的虚函数表是 `Ellipse` 的虚函数表
- `elly.render()` 调用 `Ellipse::render()`

### 使用指针时会发生什么？

```cpp
Ellipse* elly = new Ellipse(20F, 40F);
Circle* circ = new Circle(60F);
elly = circ;
```

- 原来的 `Ellipse` 对象丢失
- `elly` 和 `circ` 指向同一个 `Circle` 对象！
- `elly->render()` 调用 `Circle::render()`

---

## 12. 虚析构函数

如果类可能被继承，请使析构函数为虚函数：

```cpp
Shape *p = new Ellipse(100.0F, 200.0F);
...
delete p;
```

- 希望调用 `Ellipse::~Ellipse()`
- 必须声明 `Shape::~Shape()` 为虚函数
- 它会自动调用 `Shape::~Shape()`
- 如果 `Shape::~Shape()` 不是虚函数，只有 `Shape::~Shape()` 会被调用！

---

## 13. 重写 (Overriding)

重写重新定义虚函数的主体：

```cpp
class Base {
public:
    virtual void func();
};

class Derived : public Base {
public:
    virtual void func(); // 重写 Base::func()
};
```

### 调用链

你仍然可以调用被重写的函数：

```cpp
void Derived::func() {
    cout << "In Derived::func!";
    Base::func(); // 调用基类
}
```

这是添加新功能的常见方式，无需复制旧代码！

---

## 14. 返回类型放松

假设 `D` 是 `B` 的公共派生类：

- `D::f()` 可以返回 `B::f()` 中定义的返回类型的子类
- 适用于指针和引用类型，例如 `D&`, `D*`

```cpp
class Expr {
public:
    virtual Expr* newExpr();
    virtual Expr& clone();
    virtual Expr self();
};

class BinaryExpr : public Expr {
public:
    virtual BinaryExpr* newExpr(); // 正确
    virtual BinaryExpr& clone();   // 正确
    virtual BinaryExpr self();     // 错误!
};
```

---

## 15. 重载与虚函数

重载添加多个签名：

```cpp
class Base {
public:
    virtual void func();
    virtual void func(int);
};
```

如果重写重载函数，必须重写所有变体！

- 不能只重写一个
- 如果不全部重写，一些将被隐藏

```cpp
class Derived : public Base {
public:
    virtual void func() {
        Base::func();
    }
    virtual void func(int) { ... };
};
```

---

## 16. 虚函数使用技巧

!!! danger "禁忌"
    - 永远不要重定义继承的非虚函数
      - 非虚函数是静态绑定的
      - 没有动态分派！
    - 永远不要重定义继承的默认参数值
      - 它们也是静态绑定的

---

## 17. 虚函数与构造函数

```cpp
class A {
public:
    A() { f(); }
    virtual void f() { cout << "A::f()"; }
};

class B : public A {
public:
    B() { f(); }
    void f() { cout << "B::f()"; }
};
```

??? question "构造函数中调用虚函数会发生什么？"
    在基类构造函数中，即使函数被声明为虚函数，也只会调用基类版本，因为派生类部分尚未构造。

---

## 18. 抽象类与方法

- **抽象方法**：只有声明没有方法体的不完整方法。
- **抽象类**：包含抽象方法的类。

```cpp
class Shape {
public:
    Shape();
    virtual void render() = 0; // 将render()标记为纯虚函数
    void move(const XYPos&);
    virtual void resize();
protected:
    XYPos center;
};
```

### 抽象基类的特点

- 具有纯虚函数
- 只定义接口，没有给出函数体
- 抽象基类不能被实例化
- 必须派生新类
- 必须为所有纯虚函数提供定义才能实例化类

### 使用抽象类的原因

- **建模**：强制正确行为
- **接口设计**：定义接口而不定义实现
- 当信息不足时
- 设计接口继承时

---

## 19. 多重继承 (Multiple Inheritance)

多重继承允许一个类继承多个基类：

```cpp
class Employee {
protected:
    String name;
    EmpID id;
};

class MTS : public Employee {
protected:
    Degrees degree_info;
};

class Temporary {
protected:
    Company employer;
};

class Consultant: public MTS, public Temporary {
    // ...
};
```

### 多重继承的复杂性

- **成员复制**：成员被复制
- **派生类访问**：派生类可以访问每个基类的完整副本
- **有用场景**：多链表链接、多个输入输出流缓冲区

### 复制基类问题

```cpp
class B1 { int m_i; };
class D1 : public B1 {};
class D2 : public B1 {};
class M : public D1, public D2 {};

void main() {
    M m; // 正确
    B1* p = new M; // 错误：哪个B1?
    B1* p2 = dynamic_cast<D1*>(new M); // 正确
}
```

B1是M的重复子对象。

### 虚基类解决方案

使用虚基类可以共享：

```cpp
class B1 { int m_i; };
class D1 : virtual public B1 {};
class D2 : virtual public B1 {};
class M : public D1, public D2 {};

void main() {
    M m; // 正确
    m.m_i++; // 正确，m中只有一个B1
    B1* p = new M; // 正确
}
```

### 多重继承的复杂问题

- 名称冲突
- 优先规则
- 构造顺序
- 谁构造虚基？
- 声明虚基的时机
- 虚基中的代码被多次调用
- 编译器支持不一致

!!! warning "多重继承的建议"
    - 谨慎使用
    - 避免菱形模式（昂贵且复杂）

---

## 20. 协议/接口类

抽象基类的特殊形式：

- 所有非静态成员函数都是纯虚函数（除了析构函数）
- 虚析构函数，带空实现
- 没有非静态成员变量
- 可以包含静态成员

```cpp
// Unix字符设备接口示例
class CDevice {
public:
    virtual ~CDevice();
    virtual int read(...) = 0;
    virtual int write(...) = 0;
    virtual int open(...) = 0;
    virtual int close(...) = 0;
    virtual int ioctl(...) = 0;
};
```

---

## 21. 总结

我们学习了：

- 多态性的概念和应用
- 虚函数和重写
- 抽象函数和类
- 多重继承的使用和注意事项

多态性是面向对象编程中的核心机制，它使代码更加灵活、可扩展，能够适应更复杂的系统需求。

## 22. pdf资料

<embed src="pdfs/9 Polymorphism.pdf" type="application/pdf" width="100%" height="400px" />