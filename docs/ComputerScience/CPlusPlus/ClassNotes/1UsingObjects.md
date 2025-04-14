# 使用对象

!!! abstract "对象概念"
    在C++中，一切皆为对象。无论是基本类型还是复杂数据结构，都可以视为对象。这一概念由Alan Kay提出。

## 字符串类

<div class="grid cards" markdown>

-   :material-text-box-outline:{ .lg .middle } __基础操作__

    ---
    
    === "声明与初始化"
        ```cpp
        // 必须包含string头文件
        #include <string>
        
        // 声明方式
        string str1;                  // 空字符串
        string str2 = "Hello";        // 赋值初始化
        string str3("Hello");         // 构造函数初始化
        string str4{"Hello"};         // 列表初始化(C++11)
        ```
    
    === "赋值与连接"
        ```cpp
        // 赋值操作 (与字符数组不同，string支持直接赋值)
        string str1, str2 = "panther";
        str1 = str2;                  // 合法操作
        
        // 字符串连接
        string str3 = str1 + str2;    // 连接两个字符串
        str1 += str2;                 // 追加字符串
        str1 += "lalala";             // 追加字符串字面量
        ```
    
    === "输入与输出"
        ```cpp
        // 基本输入输出
        cin >> str;                   // 读取一个单词
        cout << str;                  // 输出字符串
        
        // 读取整行
        getline(cin, str);            // 读取一行到str
        ```

-   :material-function:{ .lg .middle } __常用方法__

    ---
    
    === "访问与查询"
        ```cpp
        string s = "Hello";
        
        // 访问单个字符
        s[0] = 'J';                  // 修改为"Jello"
        
        // 获取长度
        int len = s.length();        // 使用点运算符访问成员函数
        
        // 查找子串
        size_t pos = s.find("ll");   // 返回子串位置，未找到返回string::npos
        ```
    
    === "创建子串"
        ```cpp
        // 构造函数创建子串
        string(const char *cp, int len);            // 从C字符串创建指定长度
        string(const string& s2, int pos);          // 从位置pos到结尾
        string(const string& s2, int pos, int len); // 从位置pos开始的len个字符
        
        // 提取子串
        string sub = s.substr(2, 3);                // 从位置2开始的3个字符
        ```
    
    === "修改字符串"
        ```cpp
        // 插入
        s.insert(5, "world");           // 在位置5插入"world"
        
        // 删除
        s.erase(5, 5);                  // 从位置5删除5个字符
        
        // 追加
        s.append(" world");             // 在末尾追加" world"
        
        // 替换
        s.replace(0, 5, "Goodbye");     // 将前5个字符替换为"Goodbye"
        ```

</div>

## 对象与指针

<div class="grid" markdown>

=== "指向对象的指针"
    ```cpp
    string s = "hello";
    string* ps = &s;           // ps指向s
    
    // 通过指针访问对象
    (*ps).length();            // 使用解引用和点运算符
    ps->length();              // 使用箭头运算符（推荐）
    ```

=== "对象与指针的区别"
    ```cpp
    string s;       // 此时s已创建并初始化
    string* ps;     // 此时ps指向的对象尚未确定
    
    // 赋值操作
    string s1, s2;
    s1 = s2;        // 复制s2的内容给s1
    
    string* ps1, *ps2;
    ps1 = ps2;      // ps1和ps2指向同一个对象
    ```

</div>

!!! info "指针操作符"
    - `&`：取地址操作符，如 `ps = &s;`
    - `*`：解引用操作符，如 `(*ps).length()`
    - `->`：成员访问操作符，如 `ps->length()`

## 字符串常用函数详解

<div class="grid cards" markdown>

-   :material-format-text:{ .lg .middle } __字符访问__

    ---
    
    - `s[i]`：访问索引i处的字符（无边界检查）
    - `s.at(i)`：访问索引i处的字符（有边界检查）
    - `s.front()`：返回首字符
    - `s.back()`：返回末字符

-   :material-magnify:{ .lg .middle } __查找功能__

    ---
    
    - `s.find(str, pos=0)`：从pos位置查找str首次出现位置
    - `s.rfind(str, pos=npos)`：从pos位置向前查找str首次出现位置
    - `s.find_first_of(chars)`：查找chars中任一字符首次出现位置
    - `s.find_last_of(chars)`：查找chars中任一字符最后出现位置

-   :material-pencil:{ .lg .middle } __修改操作__

    ---
    
    - `s.append(str)`：追加字符串
    - `s.insert(pos, str)`：在pos位置插入str
    - `s.erase(pos, len)`：删除从pos开始的len个字符
    - `s.replace(pos, len, str)`：替换从pos开始的len个字符为str
    - `s.clear()`：清空字符串内容

-   :material-compare:{ .lg .middle } __比较与转换__

    ---
    
    - `s.compare(str)`：比较s与str
    - `s.c_str()`：返回C风格字符串（以'\0'结尾）
    - `s.data()`：返回字符数组
    - `s.empty()`：检查字符串是否为空
    - `s.substr(pos, len)`：返回子串

</div>

!!! tip "字符串与内存"
    不同于C风格字符数组，C++的string类对象会自动管理内存，不需要担心内存溢出问题，并且支持直接赋值、比较和连接等操作，大大简化了字符串处理。

## pdf资料

<embed src="pdfs/1 Using Object.pdf" type="application/pdf" width="100%" height="400px" />