# 类

!!! note "注意"
    类的declaration和definition分开写，declaration只写类的名字、基类、成员函数的声明，而definition则写类的实现。

## 使用案例
```c++
    class Person 
    {
    private:
        string name;
        int age;
    public:
        init(string name, int age);
        void printInfo();
    };

    Person::init(string name, int age)
    {
        this->name = name;
        this->age = age;
    }

    void Person::printInfo()
    {
        cout << "Name: " << name << endl;
        cout << "Age: " << age << endl;
    }

    int main()
    {
        Person p1("Alex", 20);
        p1.printInfo();
        return 0;
    }

```


