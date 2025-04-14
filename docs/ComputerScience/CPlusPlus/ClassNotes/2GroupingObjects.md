# 分组对象

!!! abstract "C++容器"
    C++标准模板库(STL)提供了多种容器类，用于存储和管理对象集合。这些容器适用于不同的使用场景，包括顺序容器(vector, list等)和关联容器(map等)。

## 顺序容器

<div class="grid cards" markdown>

-   :material-vector-arrange-below:{ .lg .middle } __Vector__

    ---
    
    === "基本概念"
        Vector是一种动态数组，支持快速随机访问，在末尾添加和删除元素效率高。
        ```cpp
        #include <vector>
        
        // 声明vector
        vector<int> v;                  // 整数vector
        vector<string> notes;           // 字符串vector
        
        // 初始化
        vector<int> v(100);             // 创建含100个元素的vector
        vector<int> v2(v);              // 复制构造
        ```
    
    === "常用操作"
        ```cpp
        // 基本操作
        v.size();                       // 获取元素数量
        v.empty();                      // 检查是否为空
        v.begin();                      // 返回首元素迭代器
        v.end();                        // 返回尾后迭代器
        
        // 元素访问
        v[index];                       // 访问指定位置元素(无边界检查)
        v.at(index);                    // 访问指定位置元素(有边界检查)
        v.front();                      // 访问首元素
        v.back();                       // 访问尾元素
        
        // 修改
        v.push_back(e);                 // 末尾添加元素
        v.pop_back();                   // 删除末尾元素
        v.insert(pos, e);               // 在指定位置插入元素
        v.erase(pos);                   // 删除指定位置元素
        v.clear();                      // 清空所有元素
        ```
    
    === "使用示例"
        ```cpp
        #include <iostream>
        #include <vector>
        using namespace std;
        
        int main() {
            // 声明整数vector
            vector<int> x;
            
            // 添加元素
            for (int a = 0; a < 1000; a++)
                x.push_back(a);
            
            // 使用迭代器遍历
            vector<int>::iterator p;
            for (p = x.begin(); p < x.end(); p++)
                cout << *p << " ";
                
            // 基于范围的for循环(C++11)
            for (auto var : x)
                cout << var << " ";
                
            return 0;
        }
        ```

-   :material-view-list:{ .lg .middle } __List__

    ---
    
    === "基本概念"
        List是双向链表，支持在任意位置高效插入和删除，但不支持随机访问。
        ```cpp
        #include <list>
        
        // 声明list
        list<int> lst1;                 // 整数list
        list<string> strList;           // 字符串list
        ```
    
    === "常用操作"
        ```cpp
        // 基本操作
        lst.size();                     // 获取元素数量
        lst.empty();                    // 检查是否为空
        
        // 元素访问
        lst.front();                    // 访问首元素
        lst.back();                     // 访问尾元素
        
        // 修改
        lst.push_back(item);            // 末尾添加元素
        lst.push_front(item);           // 开头添加元素
        lst.pop_back();                 // 删除末尾元素
        lst.pop_front();                // 删除开头元素
        lst.insert(pos, value);         // 在迭代器位置插入
        lst.erase(pos);                 // 删除迭代器位置元素
        lst.erase(pos1, pos2);          // 删除范围内元素
        ```
    
    === "使用示例"
        ```cpp
        #include <iostream>
        #include <list>
        #include <string>
        using namespace std;
        
        int main() {
            // 声明字符串list
            list<string> s;
            
            // 添加元素
            s.push_back("hello");
            s.push_back("world");
            s.push_front("tide");
            s.push_front("crimson");
            s.push_front("alabama");
            
            // 使用迭代器遍历
            list<string>::iterator p;
            for (p = s.begin(); p != s.end(); p++)
                cout << *p << " ";
            cout << endl;
            
            // 删除第二个元素
            list<int> L;
            for(int i = 1; i <= 5; ++i)
                L.push_back(i);
            L.erase(++L.begin());       // 删除第二个元素
            
            // 注意：迭代器失效问题
            list<int>::iterator li = L.begin();
            L.erase(li);                // li现在无效
            // 正确做法：使用erase的返回值
            li = L.erase(li);           // li指向下一个元素
            
            return 0;
        }
        ```

</div>

<div class="grid cards" markdown>

-   :material-format-list-bulleted:{ .lg .middle } __其他顺序容器__

    ---
    
    === "Deque"
        双端队列(double-ended queue)，支持在两端高效插入和删除。
        ```cpp
        #include <deque>
        
        deque<int> d;
        d.push_back(item);              // 尾部添加
        d.push_front(item);             // 头部添加
        d.pop_back();                   // 尾部删除
        d.pop_front();                  // 头部删除
        ```
    
    === "Forward_list"
        单向链表，比list更节省空间，但只能向前遍历。
        ```cpp
        #include <forward_list>
        
        forward_list<int> fl;
        fl.push_front(item);            // 只支持在头部添加
        fl.pop_front();                 // 只支持在头部删除
        fl.insert_after(pos, value);    // 在pos之后插入
        ```
    
    === "Array"
        固定大小的数组，大小在编译时确定。
        ```cpp
        #include <array>
        
        array<int, 5> arr;              // 5个整数的array
        arr[0] = 10;                    // 访问元素
        ```

-   :material-sitemap:{ .lg .middle } __容器选择指南__

    ---
    
    === "顺序容器比较"
        | 容器 | 随机访问 | 头部插入/删除 | 尾部插入/删除 | 中间插入/删除 |
        |-----|---------|------------|------------|------------|
        | vector | 快 | 慢 | 快 | 慢 |
        | list | 慢(不支持) | 快 | 快 | 快 |
        | deque | 快 | 快 | 快 | 慢 |
        | forward_list | 慢(不支持) | 快 | 慢(不支持) | 快 |
    
    === "容器选择建议"
        - 需要随机访问? 选择vector或deque
        - 主要在容器中间插入/删除? 选择list
        - 需要在两端操作? 选择deque
        - 空间效率优先? 考虑forward_list
        - 需要更好的迭代器稳定性? 选择list

</div>

## 关联容器

<div class="grid cards" markdown>

-   :material-map:{ .lg .middle } __Map__

    ---
    
    === "基本概念"
        Map是关联数组，存储键值对，根据键快速查找值，键不能重复且自动排序。
        ```cpp
        #include <map>
        
        // 声明map
        map<string, float> price;       // 字符串到浮点数的映射
        map<long, int> root;            // 长整数到整数的映射
        
        // 初始化(C++11)
        map<string, int> m{
            {"CPU", 10}, 
            {"GPU", 15}, 
            {"RAM", 20}
        };
        ```
    
    === "常用操作"
        ```cpp
        // 元素访问和修改
        price["snapple"] = 0.75;        // 设置键值
        price["coke"] = 0.50;           // 设置键值
        
        // 注意：使用[]会自动创建不存在的键
        cout << m["UPS"];               // 如果"UPS"不存在，会创建并赋默认值
        
        // 判断键是否存在
        if (m.count("CPU") > 0)         // count返回键出现次数(0或1)
            cout << "找到了CPU";
        
        // 删除元素
        m.erase("GPU");                 // 删除指定键的元素
        m.clear();                      // 清空所有元素
        ```
    
    === "使用示例"
        ```cpp
        #include <map>
        #include <string>
        #include <iostream>
        using namespace std;
        
        int main() {
            // 价格表
            map<string, float> price;
            price["snapple"] = 0.75;
            price["coke"] = 0.50;
            
            // 计算总价
            string item;
            double total = 0;
            while (cin >> item)
                total += price[item];
            
            // 判断是否是完全平方数
            map<long, int> root;
            root[4] = 2;
            root[1000000] = 1000;
            
            long l;
            cin >> l;
            if (root.count(l))          // 检查键是否存在
                cout << root[l];
            else
                cout << "Not perfect square";
                
            // 使用基于范围的for循环遍历(C++11)
            for (auto entry : price) {
                cout << entry.first << ": " 
                     << entry.second << endl;
            }
            
            return 0;
        }
        ```

</div>

## 迭代器

<div class="grid" markdown>

=== "迭代器基础"
    迭代器是访问容器中元素的通用方式，类似于指针。
    ```cpp
    // 声明迭代器
    vector<int>::iterator p;            // vector迭代器
    list<int>::iterator li;             // list迭代器
    map<string, int>::iterator m_iter;  // map迭代器
    
    // 获取迭代器
    p = v.begin();                      // 指向第一个元素
    p = v.end();                        // 指向最后一个元素之后的位置
    
    // 使用迭代器
    *p;                                 // 访问元素
    ++p;                                // 移动到下一个元素
    --p;                                // 移动到上一个元素
    ```

=== "容器间操作"
    可以使用迭代器在不同容器间复制元素。
    ```cpp
    #include <algorithm>                // 需要包含此头文件
    
    list<int> L;
    vector<int> V(L.size());            // 创建足够大的vector
    
    // 将list中的元素复制到vector
    copy(L.begin(), L.end(), V.begin());
    
    // 使用ostream_iterator输出
    copy(L.begin(), L.end(), 
         ostream_iterator<int>(cout, ","));
    ```

</div>

!!! warning "常见陷阱"
    - **Vector越界**: `v[100]=1;` 如果vector大小小于101，这是未定义行为
    - **使用operator[]与不存在的键**: `if (foo["bob"]==1)` 会默默创建键"bob"
    - **迭代器失效**: 在修改容器后，某些迭代器可能失效
    ```cpp
    list<int> L;
    list<int>::iterator li = L.begin();
    L.erase(li);    // 此操作后li无效
    ++li;           // 错误!
    
    // 正确做法
    li = L.erase(li);  // 使用erase返回的新迭代器
    ```

!!! tip "C++11新特性"
    - **Auto关键字**: 自动推导类型，简化迭代器声明
      ```cpp
      auto p = v.begin();               // 代替vector<int>::iterator p
      ```
    - **基于范围的for循环**: 简化遍历
      ```cpp
      for (auto item : v) {             // 遍历元素(值拷贝)
          cout << item << " ";
      }
      
      for (auto &item : v) {            // 遍历元素(引用)
          item *= 2;                    // 可以修改原始元素
      }
      ```
    - **类型别名**: 使用using简化复杂类型
      ```cpp
      using PB = map<string, list<string>>;   // 替代typedef
      PB phonebook;
      PB::iterator finger;
      ```

## pdf资料

<embed src="pdfs/2 Grouping Objects(1).pdf" type="application/pdf" width="100%" height="400px" />