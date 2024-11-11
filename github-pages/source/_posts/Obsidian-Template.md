---
title: '译 | Obsidian-Template文档'
date: 2022-01-25 10:09:24
tags:
---

> 翻译自官方文档  [https://blacksmithgu.github.io/obsidian-dataview/]( https://blacksmithgu.github.io/obsidian-dataview/)
> 
> 如有不同之处，请以官方文档为主。

# 介绍

这是个模板语言，能让你在笔记中插入变量和函数。还能让你执行js代码操作这些变量和函数。

这是个例子

```js
---
creation date: 2022-01-25 18:16
modification date: 星期三 26日 一月 2022 18:09:58
---

<< [[2022-02-07]] | [[2022-02-09]] >>

# 4.命令行

> Let us sacrifice our today so that our children can have a better tomorrow.
> — <cite>A. P. J. Abdul Kalam</cite>

```

它会产生以下结果：

```js
---
creation date: 2021-01-07 17:20
modification date: Thursday 7th January 2021 17:20:43
---

<< [[2021-04-08]] | [[2021-04-10]] >>

# Test Test

> Do the best you can until you know better. Then when you know better, do better.
> &mdash; <cite>Maya Angelou</cite>

```

可以看出，尖括号中的内容都被替换了（有点像jsp）

### 安装

不再赘述。



### 一些术语

- 模板：包含命令的文件
- 命令：开始标签` 和结束标签包裹的语句 `
- 函数：在命令中调用并返回字符串的对象

我们可以使用两种函数

- 内置函数：插件中定义的
- 用户函数：自己写的，可以是系统命令或者用户脚本

例子：

```js
Yesterday: 2022-02-07
Tomorrow: 2022-02-09
```


### 语法

Templater 的所有函数都是使用命令调用的 JavaScript 对象。

命令须一个开始标签`和一个结束标签`，比如，使用`tp.date.now`内部函数：`2022-02-08`

所有的函数，无论用户函数还是内置函数，都在且只在 tp 对象下可用，因此调用方式为 `tp.xxx....`

### 设置

通过设置 `Template folder location` 来告诉templater从哪里找模板

可以为新文件创建模板，也能为某一个特定的文件夹设置模板，这样一来，在指定文件夹下面创建新文件的时候就不用手动指定模板了。

可以和 Daily note、Calendar、Review、Note refactor 等兼容



# 内置函数

调用方式 `<% tp.<module_name>.<internal_function_name> %>`

接下来介绍各种模块以及函数的用法。

##  tp.date

本模板让我们可以直接使用moment.js 的moment对象，以及其所有的函数，

官方文档 [https://momentjs.com/docs/#/displaying/](https://momentjs.com/docs/#/displaying/)

当前日期 tp.date.now
`tp.date.now(format: string = "YYYY-MM-DD", offset?: number⎮string, reference?: string, reference_format?: string)`

下一天的日期 tp.date.tomorow
`tp.date.tomorrow(format: string = "YYYY-MM-DD")`

带有周几标记的 
`tp.date.weekday(format: string = "YYYY-MM-DD", weekday: number, reference?: string, reference_format?: string)`

上一天的时间 tp.date.yesterday
`tp.date.yesterday(format: string = "YYYY-MM-DD")`

- `format`: 日期的格式，参考这里  [format reference](https://momentjs.com/docs/#/displaying/format/)
-  `offset`: 日期的偏移，-7代表上周    
- `reference`: 日期引用, 比如把本文件的标题设置为本函数的返回值
- `reference_format`: 日期引用的格式

### 举例

这里调用file的title接口有报错了，先跳过。没有列举在下面

```js
Date now: <% tp.date.now() %>
Date now with format: <% tp.date.now("Do MMMM YYYY") %>

Last week: <% tp.date.now("dddd Do MMMM YYYY", -7) %>
Today: <% tp.date.now("dddd Do MMMM YYYY, ddd") %>
Next week: <% tp.date.now("dddd Do MMMM YYYY", 7) %>

Last month: <% tp.date.now("YYYY-MM-DD", "P-1M") %>
Next year: <% tp.date.now("YYYY-MM-DD", "P1Y") %>

Date tomorrow with format: <% tp.date.tomorrow("Do MMMM YYYY") %>    

This week's monday: <% tp.date.weekday("YYYY-MM-DD", 0) %>
Next monday: <% tp.date.weekday("YYYY-MM-DD", 7) %>

Date yesterday with format: <% tp.date.yesterday("Do MMMM YYYY") %>
```

生成之后是这样：

```
Date now: 2022-01-25
Date now with format: 25日 一月 2022

Last week: 星期二 18日 一月 2022
Today: 星期二 25日 一月 2022, 周二
Next week: 星期二 1日 二月 2022

Last month: 2021-12-25
Next year: 2023-01-25

Date tomorrow with format: 26日 一月 2022    

This week's monday: 2022-01-24
Next monday: 2022-01-31

Date yesterday with format: 24日 一月 2022
```

##  tp.file

`tp.file.content`  文件的内容

`tp.file.create_new(template: TFile | string, filename?: string, open_new: boolean = false, folder?: TFolder)` 用特定的模板和特定的内容创建新文件

-   `filename`: 新文件的文件名, 默认是 "Untitled".
-   `folder`: 放置新文件的路径，默认是obsidian的默认路径
-   `open_new`: 是不是打开新文件。如果是，以后所有命令都会执行在新文件中
-   `template`: 新文件模板

`tp.file.creation_date(format: string = "YYYY-MM-DD HH:mm")` 返回文件创建日期

cursur 相关的没有写，以后可以补充

`tp.file.exists(filename: string)` 文件是否存在

`tp.file.folder(relative: boolean = false)` 文件所在的文件夹的名称

-   `relative`: 是否是相对路径

`tp.file.include(include_link: string | TFile)`  包括文件的链接内容。将解析包含内容中的模板。

-   `include_link`: 要解析的链接，e.g.  MyFile , 或者 TFile 对象.也支持文档块 e.g. MyFile#Section1

`tp.file.last_modified_date(format: string = "YYYY-MM-DD HH:mm")` 文件最后一次修改时间

`tp.file.move(new_path: string)` 移动文件。能有目录名和文件名不能有后缀。

`tp.file.path(relative: boolean = false)` 返回文件路径

`tp.file.rename(new_title: string)` 重命名，只写文件不用写后缀

`tp.file.selection()`  查找选择的文本

`tp.file.tags` 文件中的标签，返回字符串列表

`tp.file.title`  标题

### 例子

```js
File content: <% tp.file.content %>

File creation date: <% tp.file.creation_date() %>
File creation date with format: <% tp.file.creation_date("dddd Do MMMM YYYY HH:mm") %>

File creation: [[<% (await tp.file.create_new("MyFileContent", "MyFilename")).basename %>]]

File cursor: <% tp.file.cursor(1) %>

File cursor append: <% tp.file.cursor_append("Some text") %>
    
File existence: <% tp.file.exists("MyFile") %>

File find TFile: <% tp.file.find_tfile("MyFile").basename %>
    
File Folder: <% tp.file.folder() %>
File Folder with relative path: <% tp.file.folder(true) %>

File Include: <% tp.file.include("[[Template1]]") %>

File Last Modif Date: <% tp.file.last_modified_date() %>
File Last Modif Date with format: <% tp.file.last_modified_date("dddd Do MMMM YYYY HH:mm") %>

File Move: <% await tp.file.move("/A/B/" + tp.file.title) %>
File Move + Rename: <% await tp.file.move("/A/B/NewTitle") %>

File Path: <% tp.file.path() %>
File Path with relative path: <% tp.file.path(true) %>

File Rename: <% await tp.file.rename("MyNewName") %>
Append a "2": <% await tp.file.rename(tp.file.title + "2") %>

File Selection: <% tp.file.selection() %>

File tags: <% tp.file.tags %>

File title: <% tp.file.title %>
Strip the Zettelkasten ID of title (if space separated): <% tp.file.title.split(" ")[1] %>
```

例子报错了，以后再看吧，还是path的问题。

##  tp.frontmatter

元数据信息

不重要

User Guide
[https://silentvoid13.github.io/Templater/internal-functions/internal-modules/frontmatter-module.html](https://silentvoid13.github.io/Templater/internal-functions/internal-modules/frontmatter-module.html)

##  tp.obsidian

一般用不到

User Guide
[https://silentvoid13.github.io/Templater/internal-functions/internal-modules/obsidian-module.html](https://silentvoid13.github.io/Templater/internal-functions/internal-modules/obsidian-module.html)


##  tp.system

`tp.system.clipboard()` 返回用户剪贴板的内容

其他的应该用不到了

User Guide
[https://silentvoid13.github.io/Templater/internal-functions/internal-modules/system-module.html](https://silentvoid13.github.io/Templater/internal-functions/internal-modules/system-module.html)

##  tp.web

一般用不到

User Guide
[https://silentvoid13.github.io/Templater/internal-functions/internal-modules/web-module.html](https://silentvoid13.github.io/Templater/internal-functions/internal-modules/web-module.html)

## Contributing

用于自主DIY这个软件

User Guide
[https://silentvoid13.github.io/Templater/internal-functions/contribute.html](https://silentvoid13.github.io/Templater/internal-functions/contribute.html)



## 用户脚本

你可以从obsidian中调用你自己的脚本，获取其输出，但是要保证你的obsidian库能访问到脚本（**需要设置脚本路径**）

### 定义用户函数

Let's say you specified the `Scripts` folder as your script folder in Templater's settings.

假设你的自定义脚本在这里 `Scripts/my_script.js` 

脚本需要遵循  [CommonJS module specification](https://flaviocopes.com/commonjs/), 然后导出为单个函数

Let's have an example with our previous script `my_script.js`:

```js
function my_function(msg){
	console.log("Message from my script : ", msg);
}

module.exports = my_function
```

Templater 将会加载指定文件夹中的所有js文件，然后你就能调用了

`<% tp.user.my_script("Hello World!") %>` 

### 全局命名空间

在用户自定义脚本中可以访问全局命名空间中的变量，比如 `app` 和 `moment`.

只不过在使用的时候，你需要将他们当作参数传进去。

## 系统命令

### 系统命令型用户函数

这种函数让你执行系统命令获取他们的输出

**需要在设置中进行设置**

![[assets/image-20220125232545.png]]

暂时不需要这么多，先不看了

User Guide
[https://silentvoid13.github.io/Templater/user-functions/system-user-functions.html](https://silentvoid13.github.io/Templater/user-functions/system-user-functions.html)



# 命令类型

一共有三种

**开始**符号：
-   `<%`: 只显示里面命令的表达式输出
-   `<%~`: 插值，添加了一些字符转义
-   `<%*`: 执行里面的js代码，默认不输出内容

**结束**符号：都是`%>`

命令工具

除了以上三种，还有一些工具， 在命令的开始标记中声明。

包括空白控制和动态命令

## 动态命令

命令将在进入预览模式时被解析。

声明方式：把开标签换成 `<%+`

比如查看上次修改日期 `<%+ tp.file.last_modified_date() %>` 

刷新问题：是放缓存中，每次预览刷一次，不会改变

## 执行js命令

可以通过tp访问对象和所有内部变量和函数

js执行时也可以访问app和moment等

## 异步函数

不要忘了使用 `await` 异步关键字

## 如何输出

最好使用原始的插值命令

非要想输出的话，就将要输出的值append到tR的后面  `<%* tR += "test" %>` 

一些例子

```js
<%* if (tp.file.title.startsWith("Hello")) { %>
This is a hello file !
<%* } else { %>
This is a normal file !
<%* } %>
    
<%* if (tp.frontmatter.type === "seedling") { %>
This is a seedling file !
<%* } else { %>
This is a normal file !
<%* } %>
    
<%* if (tp.file.tags.contains("#todo")) { %>
This is a todo file !
<%* } else { %>
This is a finished file !
<%* } %>

<%*
function log(msg) {
    console.log(msg);
}
%>
<%* log("Title: " + tp.file.title) %>
    
<%* tR += tp.file.content.replace(/stuff/, "things"); %>
```

## 空白控制

Templater不会删除任何换行符，比如

```js
<%* if (tp.file.title == "MyFile" ) { %>
This is my file!
<%* } else { %>
This isn't my file!
<%* } %>
Some content ...

```

会变成下面这样

```js
\\换行
This isn't my file!
\\换行
Some content ...
```

命令执行完后的空行可以删除。如下。


-   开头符号 `<%_` 会删除命令前的所有空格
-   结尾符号`_%>`)  删除之后的
-   开头符号`<%-`) 会删除命令前的1个空格
-   结尾符号`-%>` 会删除后面的1个

如下
```js
<%* if (tp.file.title == "MyFile" ) { -%>
This is my file!
<%* } else { -%>
This isn't my file!
<%* } -%>
Some content ...
```

输出为：

```js
This isn't my file!
Some content ...
```