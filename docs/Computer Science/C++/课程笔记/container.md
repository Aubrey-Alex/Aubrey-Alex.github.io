# 容器

## 容器分类

| 容器 | 特点 |
| --- | --- |
|vector|动态数组，支持随机访问，插入删除元素效率高|
|list|双向链表，支持随机访问，插入删除元素效率高|
|deque|双端队列，支持随机访问，插入删除元素效率高|
|forward_list|单向链表，支持随机访问，插入删除元素效率高|
|array|固定大小数组，支持随机访问，插入删除元素效率高|
|string|字符串，支持随机访问，插入删除元素效率低|
|map|关联容器，支持快速查找，插入删除元素效率高|

## vector

- vector<int> v; // 声明一个整数型的动态数组
- v.push_back(10); // 向数组尾部添加元素
- v.pop_back(); // 删除数组尾部元素
- v.insert(v.begin()+1, 20); // 在数组第2个位置插入元素20
- v.erase(v.begin()+1); // 删除数组第2个位置元素
- v.size(); // 获取数组大小
- v.empty(); // 判断数组是否为空
- v[0]; // 访问数组第1个元素
- v.front(); // 访问数组第一个元素
- v.back(); // 访问数组最后一个元素
- v.clear(); // 清空数组

## list

- list<int> l; // 声明一个整数型的双向链表
- l.push_front(10); // 向链表头部添加元素
- l.push_back(20); // 向链表尾部添加元素
- l.pop_front(); // 删除链表头部元素
- l.pop_back(); // 删除链表尾部元素
- l.insert(l.begin(), 30); // 在链表头部插入元素30
- l.insert(l.end(), 40); // 在链表尾部插入元素40
- l.erase(l.begin()); // 删除链表头部元素

## map

- map<string, int> m; // 声明一个字符串型的整数型的关联容器
- m["apple"] = 10; // 向容器中插入元素
- m.insert(pair<string, int>("banana", 20)); // 向容器中插入元素
- m.erase("apple"); // 删除容器中元素
- m.find("banana"); // 查找容器中元素
- m.count("banana"); // 统计容器中元素个数
- m.empty(); // 判断容器是否为空
- m.size(); // 获取容器大小

## iterator

- list<int>::iterator it; // 声明一个迭代器
- for(auto var : vec) // 使用迭代器遍历容器

