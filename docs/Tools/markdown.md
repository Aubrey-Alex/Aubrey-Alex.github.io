## Markdown 

!!! abstract "什么是Markdown"
    Markdown 是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档，并最终生成 HTML 格式的文档。

<div class="grid cards" markdown>

-   :material-format-font:{ .lg .middle } __基础语法__

    ---
    
    === "标题"
        `#`加空格表示一级标题，共有六级标题
    
    === "列表"
        - 无序列表使用`-`加空格
        - 有序列表使用数字加`.`加空格
    
    === "引用"
        `>`加空格表示引用

-   :material-pencil:{ .lg .middle } __文本格式化__

    ---
    
    === "强调"
        - *斜体*：`*`或`_`包围文本
        - **粗体**：`**`或`__`包围文本
        - ~~删除线~~：`~~`包围文本
    
    === "代码"
        - 行内代码：\`代码\`
        - 代码块：\`\`\`语言名
    
    === "分隔线"
        3个或以上的`-`表示分隔线

</div>

### 链接与图片

<div class="grid" markdown>

=== "链接语法"
    - 基本链接：`[链接文字](链接地址)`
    - 网址：`<https://example.com>`
    - 格式化链接：`**[粗体链接](https://example.com)**`

=== "图片语法"
    - 基本图片：`![图片描述](图片地址)`
    - 带链接的图片：`[![图片描述](图片地址)](链接地址)`

</div>

### 表格与列表

<div class="grid" markdown>

=== "表格"
    ```markdown
    | 表头1 | 表头2 | 表头3 |
    | ----- | ----- | ----- |
    | 内容1 | 内容2 | 内容3 |
    ```

=== "任务列表"
    ```markdown
    - [ ] 未完成任务
    - [x] 已完成任务
    ```

=== "定义列表"
    ```markdown
    术语
    : 术语的定义
    ```

</div>

### 高级功能

<div class="grid cards" markdown>

-   :material-alert-decagram:{ .lg .middle } __警告框__

    ---

    !!! note "提示"
        这是一个提示框

    !!! warning "警告"
        这是一个警告框

-   :material-folder-multiple:{ .lg .middle } __折叠块__

    ---

    ??? "可折叠内容"
        这是折叠的内容

    ???+ "默认展开"
        这是默认展开的内容

-   :material-code-tags:{ .lg .middle } __代码高亮__

    ---

    ```python title="示例代码" linenums="1" hl_lines="2"
    def hello():
        print("Hello, World!")
    ```

</div>

### 特殊元素

<div class="grid" markdown>

=== "按钮"
    [普通按钮](#){ .md-button }
    [主要按钮](#){ .md-button .md-button--primary }

=== "表情符号"
    - 😊 `:smile:`
    - 👍 `:thumbsup:`
    [更多表情符号](https://gist.github.com/rxaviers/7360908)

=== "注释"
    这是一段带注释的文本 (1)
    { .annotate }
    
    1. :这是注释内容

</div>

### 代码块进阶

<div class="grid cards" markdown>

-   :material-code-braces:{ .lg .middle } __代码块格式化__

    ---
    
    === "添加标题"
        ```python title="Python code"
        print("Hello, world!")
        ```
    
    === "添加行号"
        ```python linenums="1"
        def hello():
            print("Hello, world!")
        ```
    
    === "高亮特定行"
        ```python hl_lines="2 3"
        def example():
            print("This line is highlighted")
            return "This line too"
        ```

-   :material-code-tags:{ .lg .middle } __代码分组展示__

    ---
    
    === "C"
        ```c
        #include <stdio.h>
        
        int main(void) {
          printf("Hello world!\n");
          return 0;
        }
        ```

    === "C++"
        ```c++
        #include <iostream>
        
        int main(void) {
          std::cout << "Hello world!" << std::endl;
          return 0;
        }
        ```

</div>

### 高级格式化

<div class="grid cards" markdown>

-   :material-format-color-highlight:{ .lg .middle } __文本修改标记__

    ---
    
    === "删除和添加"
        - 删除文本：{--已删除的文本--}
        - 添加文本：{++新添加的文本++}
        - 替换文本：{~~旧文本~>新文本~~}
    
    === "高亮和注释"
        - 高亮文本：{==重要内容==}
        - 添加注释：{>>这是一条注释<<}

-   :material-keyboard:{ .lg .middle } __快捷键标记__

    ---
    
    === "常用快捷键"
        - 保存：++ctrl+s++
        - 复制：++ctrl+c++
        - 粘贴：++ctrl+v++
        - 撤销：++ctrl+z++

-   :material-table:{ .lg .middle } __数据表格__

    ---
    
    | 方法     | 描述                               |
    | -------- | ---------------------------------- |
    | `GET`    | :material-check:     获取资源      |
    | `PUT`    | :material-check-all: 更新资源      |
    | `DELETE` | :material-close:     删除资源      |

</div>

### 警告框类型

<div class="grid cards" markdown>

-   :material-alert:{ .lg .middle } __信息类__

    ---
    
    === "基础信息"
        !!! note "注意"
            这是一个注意事项
        
        !!! info "信息"
            这是一条信息
        
        !!! tip "提示"
            这是一个提示

    === "重要信息"
        !!! warning "警告"
            这是一个警告
        
        !!! danger "危险"
            这是一个危险提示
        
        !!! error "错误"
            这是一个错误信息

-   :material-book-open:{ .lg .middle } __文档类__

    ---
    
    === "文档说明"
        !!! abstract "摘要"
            这是一个摘要说明
        
        !!! example "示例"
            这是一个示例
        
        !!! quote "引用"
            这是一段引用

    === "特殊说明"
        !!! bug "Bug"
            这是一个bug说明
        
        !!! success "成功"
            这是一个成功提示
        
        !!! failure "失败"
            这是一个失败提示

</div>

### 网格布局示例

<div class="grid cards" markdown>

-   :material-clock-fast:{ .lg .middle } __快速上手__

    ---

    5分钟快速入门Markdown基础语法
    
    [:octicons-arrow-right-24: 开始学习](#)

-   :material-text-box:{ .lg .middle } __基础语法__

    ---

    掌握Markdown最常用的基础语法
    
    [:octicons-arrow-right-24: 查看详情](#)

-   :material-format-font:{ .lg .middle } __进阶功能__

    ---

    探索Markdown的高级特性和扩展语法
    
    [:octicons-arrow-right-24: 了解更多](#)

-   :material-book-open-page-variant:{ .lg .middle } __最佳实践__

    ---

    学习Markdown的使用技巧和最佳实践
    
    [:octicons-arrow-right-24: 实践指南](#)

</div>

!!! tip "更多资源"
    - [完整的Markdown指南](https://www.markdownguide.org)
    - [Material for MkDocs文档](https://squidfunk.github.io/mkdocs-material/reference/)
    - [Markdown表情符号参考](https://gist.github.com/rxaviers/7360908)
