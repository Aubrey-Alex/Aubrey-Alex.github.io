# 内部对象

## 访问控制(acces control)

- public: 对所有类可见
- private: 只对本类可见
- protected: 对本类和派生类可见(暂时不用)

!!! note "注意"
    对于private，针对的是类而不是对象，所以同一类的不同对象之间是可以互相访问私有变量的。

## 友元(friend)

- 授权global function访问private成员
- 授权类访问private成员
- 授权成员函数访问private成员

!!! example "友元举例"
    ```cpp
        struct X 
        {
        private:
            int i;
        public:
            void initialize(); 
            friend void g(X*, int); // 授权global function访问private成员
            friend void Y::ff(X*); // 授权成员函数访问private成员
            friend struct Z; // 授权类访问private成员
            friend void h(); // 授权global function访问private成员
        }
    ```

## class vs struct

- class: 默认private
- struct: 默认public

## 本地变量(local object)

- 作用域：局部作用域
- 生命周期：直到离开作用域

|术语|特点|
|:--:|:--:|
|字段(field)|类中定义的变量，使用this指针访问，作用域为类的内部，生命周期为对象整个生命周期|
|参数(parameter)|函数中定义的变量，作用域为函数内部，生命周期为函数调用结束|
|局部变量(local variable)|函数中定义的变量，作用域为函数内部，生命周期为函数调用结束|

## 静态成员(static member)

!!! note "静态成员"
    如果一个类中有静态成员变量，那么在对应的cpp里面要写一句该成员变量的定义（因为静态成员变量与对象无关，定义对象时静态变量不会被定义），但是定义时不能再加static关键字。
