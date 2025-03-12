# 对象

## 字符串

- \#include <string\>
- 初始化
  - string str = "hello world";
  - string str{"hello world"};
  - string str{"hello world"};

## 函数

- s.length() 返回字符串 s 的长度
- s.substr(pos, len) 返回从 pos 开始的 len 个字符的子串
- s.find(str) 返回子串 str 在 s 中第一次出现的位置，如果不存在则返回 -1
- s.replace(old, new) 返回一个新的字符串，其中所有出现的 old 都被 new 替换
- s.compare(str) 返回一个整数，表示 s 和 str 的比较结果，若 s < str，则返回 -1，若 s == str，则返回 0，若 s > str，则返回 1
- s.empty() 返回一个布尔值，表示 s 是否为空字符串
- s.push_back(c) 在 s 末尾添加字符 c
- s.pop_back() 删除 s 末尾的字符
- s.front() 返回 s 首字符
- s.back() 返回 s 末字符
- s.clear() 清空 s
- s.swap(str) 交换 s 和 str 的内容
- s.at(pos) 返回 s 中第 pos 个字符
- s.data() 返回 s 的指针
- s.c_str() 返回 s 的 C 风格字符串
- s.getline(is, delimiter) 从 is 中读取一行，并将其存入 s，直到遇到 delimiter 字符为止
- s.append(str) 将 str 追加到 s 末尾
- s.insert(pos, str) 在 s 中 pos 位置插入 str
- s.erase(pos, len) 删除 s 中从 pos 开始的 len 个字符