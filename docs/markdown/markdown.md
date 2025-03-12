## Markdown 

Markdown 是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档，并最终生成 HTML 格式的文档。

## 标题

`#`加空格表示一级标题，共有六级标题

## 列表

无序列表使用`-`加空格，有序列表使用数字加`.`加空格

## 引用

`>`加空格表示引用

## 链接

### 添加链接

`[链接文字](链接地址)`

### 网址和电子邮件地址

`<https://www.markdownguide.org>`

### 格式化链接

`**[EFF](https://eff.org)**`

## 图片

`![图片描述](图片地址)`

## 代码块

\`\`\`三个反引号表示代码块，可以指定编程语言

## 表格

| 表头1 | 表头2 | 表头3 |
| ----- |----- | ----- |
| 单元格 | 单元格 | 单元格 |

## 转义字符

`\`加字符表示转义字符，如`\*`表示星号

## 强调

`*`或`_`加空格表示斜体，`**`或`__`加空格表示粗体

## 删除线

`~~`加空格表示删除线

## 分隔线

3个或以上的`-`表示分隔线

## 脚注

Here's a simple footnote,`[^1]` and here's a longer one.

`[^1]`: This is the first footnote.

## 锚链接

`### My Great Heading {#heading-ids}`
`[Heading IDs](#heading-ids)`

## 定义列表

`term`: definition of the term

## 任务列表

`- [ ] task 1`
`- [x] task 2`

## 表情符号

`:joy:` 😊

[表情符号查阅链接](https://gist.github.com/rxaviers/7360908)

## 警告

### 警告块

`!!! note`

!!! note
    This is a note.

### 修改标题

`!!! note "Phasellus posuere in sem ut cursus"`

## 可折叠块

### 语法

`???` 初始为折叠
`???+` 初始为展开

### 内联块

`!!! info inline end 标题` 右对齐
`!!! info inline 标题` 左对齐

## 支持类型

!!! note
    this is a note.

!!! abstract
    this is an abstract.

!!! info
    this is an info.

!!! tip
    this is a tip.

!!! success
    this is a success.

!!! question
    this is a question.

!!! warning
    this is a warning.

!!! failure
    this is a failure.

!!! danger
    this is a danger.

!!! bug
    this is a bug.    

!!! example
    this is an example.

!!! quote
    this is a quote.    

!!! cite
    this is a citation.

## 注释

`Lorem ipsum dolor sit amet, (1) consectetur adipiscing elit{ .annotate }`

`1.  :I'm an annotation!`

Lorem ipsum dolor sit amet, (1) consectetur adipiscing elit.
{ .annotate }

1.  :I'm an annotation!
   
## 按钮

`[Subscribe to our newsletter](#){ .md-button }`

[Subscribe to our newsletter](#){ .md-button }

`[Subscribe to our newsletter](#){ .md-button .md-button--primary }`

[Subscribe to our newsletter](#){ .md-button .md-button--primary }

`[Send :fontawesome-solid-paper-plane:](#){ .md-button }`

[Send 😊 ](#){ .md-button }

## 代码块

使用\`\`\`表示代码块，可以指定编程语言

### 添加标题

\`\`\` python title="Python code"

```python title="Python code"
    print("Hello, world!")
```

### 添加行号

\`\`\` python linenums="1"

### 高亮显示特定行

\`\`\` python hl_lines="2 3"

### 高亮显示内联代码块

\`\#code\`  `#!python range()`

### 嵌入外部文件

`--8<--`直接在代码块中嵌入来自其他文件的内容（包括源文件）

## 分组

`=== "C"`

`=== "C++"`

=== "C"

    ``` c
    #include <stdio.h>

    int main(void) {
      printf("Hello world!\n");
      return 0;
    }
    ```

=== "C++"

    ``` c++
    #include <iostream>

    int main(void) {
      std::cout << "Hello world!" << std::endl;
      return 0;
    }
    ```
!!! example

    === "Unordered List"

        ``` markdown
        * Sed sagittis eleifend rutrum
        * Donec vitae suscipit est
        * Nulla tempor lobortis orci
        ```

    === "Ordered List"

        ``` markdown
        1. Sed sagittis eleifend rutrum
        2. Donec vitae suscipit est
        3. Nulla tempor lobortis orci
        ```

## 数据表格

| Method   | Description                          |
| -------- | ------------------------------------ |
| `GET`    | :material-check:     Fetch resource  |
| `PUT`    | :material-check-all: Update resource |
| `DELETE` | :material-close:     Delete resource |

## 格式化

### 高亮显示更改

=== "语法"
    Text can be {\-\-deleted\-\-} and replacement text {\+\+added\+\+}. This can also be
    combined into {\~\~one\~\>a single\~\~}operation. {\=\=Highlighting\=\=} is also
    possible {\>\>and comments can be added inline<<}.

    {\=\=
    Formatting can also be applied to blocks by putting the opening and closing
    tags on separate lines and adding new lines between the tags and the content.
    \=\=}

=== "渲染效果"
    Text can be {--deleted--} and replacement text {++added++}. This can also be
    combined into {~~one~>a single~~} operation. {==Highlighting==} is also
    possible {>>and comments can be added inline<<}.

    {==

    Formatting can also be applied to blocks by putting the opening and closing
    tags on separate lines and adding new lines between the tags and the content.

    ==}

### 添加鼠标按键

\+\+ctrl\+alt\+del\+\+

++ctrl+alt+del++

## 网格

### 卡片网格

``` markdown

<div class="grid cards" markdown>

    - :fontawesome-brands-html5: __HTML__ for content and structure
    - :fontawesome-brands-js: __JavaScript__ for interactivity
    - :fontawesome-brands-css3: __CSS__ for text running out of boxes
    - :fontawesome-brands-internet-explorer: __Internet Explorer__ ... huh?

</div>

```

<div class="grid cards" markdown>

- :fontawesome-brands-html5: __HTML__ for content and structure
- :fontawesome-brands-js: __JavaScript__ for interactivity
- :fontawesome-brands-css3: __CSS__ for text running out of boxes
- :fontawesome-brands-internet-explorer: __Internet Explorer__ ... huh?

</div>

```
    <div class="grid cards" markdown>

    -   :material-clock-fast:{ .lg .middle } __Set up in 5 minutes__

        ---

        Install [`mkdocs-material`](#) with [`pip`](#) and get up
        and running in minutes

        [:octicons-arrow-right-24: Getting started](#)

    -   :fontawesome-brands-markdown:{ .lg .middle } __It's just Markdown__

        ---

        Focus on your content and generate a responsive and searchable static site

        [:octicons-arrow-right-24: Reference](#)

    -   :material-format-font:{ .lg .middle } __Made to measure__

        ---

        Change the colors, fonts, language, icons, logo and more with a few lines

        [:octicons-arrow-right-24: Customization](#)

    -   :material-scale-balance:{ .lg .middle } __Open Source, MIT__

        ---

        Material for MkDocs is licensed under MIT and available on [GitHub]

        [:octicons-arrow-right-24: License](#)

    </div>
```

<div class="grid cards" markdown>

-   :material-clock-fast:{ .lg .middle } __Set up in 5 minutes__

    ---

    Install [`mkdocs-material`](#) with [`pip`](#) and get up
    and running in minutes

    [:octicons-arrow-right-24: Getting started](#)

-   :fontawesome-brands-markdown:{ .lg .middle } __It's just Markdown__

    ---

    Focus on your content and generate a responsive and searchable static site

    [:octicons-arrow-right-24: Reference](#)

-   :material-format-font:{ .lg .middle } __Made to measure__

    ---

    Change the colors, fonts, language, icons, logo and more with a few lines

    [:octicons-arrow-right-24: Customization](#)

-   :material-scale-balance:{ .lg .middle } __Open Source, MIT__

    ---

    Material for MkDocs is licensed under MIT and available on [GitHub]

    [:octicons-arrow-right-24: License](#)

</div>

### 通用网格

``` markdown

<div class="grid" markdown>

=== "Unordered list"

    * Sed sagittis eleifend rutrum
    * Donec vitae suscipit est
    * Nulla tempor lobortis orci

=== "Ordered list"

    1. Sed sagittis eleifend rutrum
    2. Donec vitae suscipit est
    3. Nulla tempor lobortis orci

``` title="Content tabs"
=== "Unordered list"

    * Sed sagittis eleifend rutrum
    * Donec vitae suscipit est
    * Nulla tempor lobortis orci

=== "Ordered list"

    1. Sed sagittis eleifend rutrum
    2. Donec vitae suscipit est
    3. Nulla tempor lobortis orci
```

</div>