---
title: '译 | Obsidian-Dataview文档'
date: 2022-01-26 10:09:24
tags:
---

> 翻译自官方文档  [https://blacksmithgu.github.io/obsidian-dataview/]( https://blacksmithgu.github.io/obsidian-dataview/)
> 
> 如有不同之处，请以官方文档为主。

# 概述

## 基本用法

Dataview 有两个主要组件：_注释_ 和 _查询_ 

### 注释

Dataview**索引**负责不断解析您的知识库中的 markdown 文件和其他元数据，创建一个内存索引，以便对数据进行快速查询。注释在 _markdown 页面_ 、_段落_ 和 _task_ 级别完成，可以在其中使用：

1.  **Frontmatter**，一个常见的 Markdown 扩展，允许在文档顶部添加任意YAML）：
```yaml
---
alias: "document"
last-reviewed: 2021-08-17
thoughts:
rating: 8
reviewable: false
---
```

2.  **内联字段**，一种特定于 Dataview 的方式，以直观的`Key::Value`语法提供元数据：
```text
# Markdown Page

Basic Field:: Value
**Bold Field**:: Nice!

- [ ] I am a task with [metadata::value]!
	- [X] I am another task with completed::2020-09-15
```

如果需要，您可以结合使用这两种方法。Dataview 还添加了大量“隐式”字段，例如`file.name`文件名、`file.size`大小等；

你可以在[数据注释文档](https://blacksmithgu.github.io/obsidian-dataview/data-annotation)中找到更多详细信息。

### 查询

一旦你有一些你已经注释的页面，剩下要做的就是查询它们以创建动态表、列表或 JavaScript 视图。

有四种方法可以做到这一点：

1.  **Dataview Query Language (DQL)**：类似 SQL 的语言。请参阅[[3.数据查询]]
` ```dataview
TABLE file.name AS "File", rating AS "Rating" FROM #book
``` ` 

undefined **内联表达式**：可以直接嵌入到 Markdown 中的 DQL 表达式，并将在预览模式下显示。请参阅[[3.数据查询]]

``We are on page `= this.file.name`.`` 

3.  **DataviewJS**：一个高性能的 JavaScript API，它可以完全访问 Dataview 索引和一些方便的渲染实用程序。**如果了解 JavaScript，强烈推荐，因为它比查询语言强大得多**。查看[[5.0.JavascriptAPI]]以获取更多详细信息。
` ```dataviewjs
dv.taskList(dv.pages().file.tasks.where(t => !t.completed));
``` ` 

4.  **内联 JS 表达式**：相当于内联表达式的 JavaScript，它允许您执行任意 JS 内联：
``This page was last modified at `$= dv.current().file.mtime`.``


# 数据注释

数据索引，支持的是字段。

---

## 页面

您可以通过三种不同的方式将字段添加到markdown页面：

1.  **Frontmatter**：Frontmatter 是一个常见的 Markdown 扩展，它允许将 YAML 元数据添加到页面顶部。所有 YAML 字段都将作为 Dataview 的字段：

`---
alias: "document"
last-reviewed: 2021-08-17
thoughts:
 rating: 8
 reviewable: false
---` 

2.  **内联字段**：Dataview 支持“内联的”字段，它提供了一种简单的`Key:: Value`语法，你可以直接将其嵌入到文件中：

`# Markdown Page
Basic Field:: Value
**Bold Field**:: Nice!` 

如果你想在句子中嵌入元数据，或者在同一行中嵌入多个字段，你可以使用括号语法：
`I would rate this a [rating:: 9]! It was [mood:: acceptable].` 

3.  **隐式**：Dataview 自动使用大量元数据注释页面，例如文件创建日期 (`file.cday` )、关联日期 ( `file.day`)、文件中的链接 ( `file.outlinks`)、标签 ( `file.tags`) 等。

一个简单的 Markdown 页面，有以下两种用户定义的添加元数据的方式：

`---
duration: 4 hours
reviewed: false
---`
`# Movie X
**Thoughts**:: It was decent.
**Rating**:: 6` 

#### 隐式字段

Dataview 会自动为每个页面添加大量元数据：

-   `file.name`：文件标题（字符串）。
-   `file.folder`：该文件所属文件夹的路径。
-   `file.path`：完整的文件路径（字符串）。
-   `file.link`：文件的链接（链接）。
-   `file.size`：文件的大小（以字节为单位）（数字）。
-   `file.ctime`：文件的创建日期（日期+时间）。
-   `file.cday`：文件的创建日期（日期）。
-   `file.mtime`：文件最后修改的日期（日期+时间）。
-   `file.mday`：文件最后修改的日期（日期）。
-   `file.tags`：笔记中所有唯一标签的数组。子标签按每个级别细分，因此`#Tag/1/A`将存储在数组中`[#Tag, #Tag/1, #Tag/1/A]`。
-   `file.etags`：注释中所有显式标签的数组；不和`file.tags`一样, 不包括子标签。
-   `file.inlinks`：此文件的所有传入链接的数组。
-   `file.outlinks`：此文件中所有传出链接的数组。
-   `file.aliases`：笔记的所有别名的数组。
-   `file.tasks`：此文件中所有任务（即，- [  ] blah blah blah）的数组。

如果文件在其标题（或表单`yyyy-mm-dd`或`yyyymmdd`）中有日期，或者有一个`Date`字段/内联字段，它还具有以下属性：

-   `file.day`：与文件关联的日期。

---

## 任务清单

使用内联字段语法使用元数据注释您的任务清单（即表单的行）：`- [ ] blah blah blah`
```text
- [ ] Hello, this is some [metadata:: value]!
- [X] I finished this on [completion::2021-08-15].
```

#### 字段速记

为了支持“常见用例”，Dataview 了解一些常见数据的简写，您可能希望使用以下命令注释任务：

语法

-   截止日期：`🗓️YYYY-MM-DD`
-   完成日期：`✅YYYY-MM-DD`
-   创建日期：`➕YYYY-MM-DD`

如果你不喜欢表情符号，可以对这些字段进行文本注释（`[due:: ]`、`[created:: ]`、 `[completion:: ]`）。

#### 隐式字段

与页面一样，Dataview 为每个任务添加了许多隐式字段：

-   任务从其父页面继承所有字段， 因此，如果页面中有一个字段，也可以在任务中访问它。
-   `completed`：这个 _特定的_ 任务是否已经完成；这不考虑任何子任务的完成/未完成。
-   `fullyCompleted`：此任务及其**所有**子任务是否已完成。
-   `text`: 这个任务的文本
-   `line`：此任务显示的行
-   `path`：此任务所在文件的完整路径。
-   `section`：此任务所在部分的链接。
-   `link`: 指向该任务附近最近的可链接块的链接；对于制作指向任务的链接很有用。
-   `subtasks`：此任务的所有子任务。
-   `real`: 如果是true，这是一个真正的任务；否则，它是任务上方/下方的列表元素。
-   `completion`：任务完成的日期。如果没有注释，将默认为文件修改时间。
-   `due`：任务的到期日期（如果有的话）。
-   `created`：创建任务的日期。如果没有注释，则默认为文件创建时间。
-   `annotated`：如果任务有任何自定义注释，则为真，否则为假。

---

## 字段类型

dataview 中的所有字段都有一个 _type_ ，它决定了 dataview 将如何呈现、排序和操作该字段。Dataview 了解几种不同的字段类型以涵盖常见用例：

-   **文本**：默认的。如果字段不匹配更具体的类型，则它只是纯文本。
    
    `Example:: This is some normal text.` 
    
-   **Number** : 像 '6' 和 '3.6' 这样的数字。
    
    `Example:: 6
    Example:: 2.4
    Example:: -80` 
    
-   **Boolean** : true/false，逻辑值
    
    `Example:: true
    Example:: false` 
    
-   **日期**：ISO8601日期的一般形式`YYYY-MM[-DDTHH:mm:ss.nnn+ZZ]`。月后的一切都是可选的。
    
    `Example:: 2021-04-18
    Example:: 2021-04-18T04:19:35.000
    Example:: 2021-04-18T04:19:35.000+06:30` 
    
-   **持续时间**：`<time> <unit>`表单的持续时间，例如`6 hours`或`4 minutes`。常见的英文缩写如 `6hrs`or`2m` 您可以使用可选的逗号分隔符指定多个单位：`6 hours, 4 minutes` 或 `6hr4min`.
    
    `Example:: 7 hours
    Example:: 4min
    Example:: 16 days
    Example:: 9 years, 8 months, 4 days, 16 hours, 2 minutes
    Example:: 9 yrs 8 min` 
    
-   **链接**：普通Obsidian链接，如`[[Page]]`或`[[Page|Page Display]]`。
    -   如果你在 frontmatter 中引用链接，则需要引用它，如下所示：`key: "[[Link]]"`. 这是默认的 Obsidian 支持的行为。
        
        `Example:: [[A Page]]
        Example:: [[Some Other Page|Render Text]]` 
        
-   **List**：其他数据视图字段的列表。在 YAML 中，这些被定义为正常的 YAML 列表；对于内联字段，它们只是逗号分隔的列表。
    
    `Example:: 1, 2, 3
    Example:: "yes", "or", "no"` 
    
-   **对象**：名称到数据视图字段的映射。这些只能在 YAML frontmatter 中定义，使用正常的 YAML 对象语法：
    
    `field:
      value1: 1
      value2: 2
      ...`

# 创建查询

有4种方式实现这一点，并且在库update 的时候可以重新加载

## Dataview 查询语言 (DQL)

dataview查询语言是一种简单的、类似 SQL 的语言，用于快速创建视图的语言。它支持基本的算术和比较运算，适用于基本应用程序。您使用带注释的代码块创建数据视图查询`dataview`：

` ```dataview
TABLE rating AS "Rating", summary AS "Summary" FROM #games
SORT rating DESC
``` ` 

## 内联 DQL

查询可以直接嵌入页面中
- 例如，今天的日期 `= date(today)`，或来自另一个页面的字段 `= [[Page]].value`

使用内联代码块创建内联查询：
`` `= this.file.name` `` 

内联 DQL 表达式是使用[查询语言表达式语言](https://blacksmithgu.github.io/obsidian-dataview/query/expressions)编写的。您可以在 Dataview 设置中配置内联查询以使用不同的前缀（如`dv:`或）。`~`

## Dataview JS

dataview[[5.0.JavascriptAPI]]为您提供 JavaScript 的全部功能，并提供用于拉取 Dataview 数据和执行查询的 DSL，允许您创建任意复杂的查询和视图。与查询语言类似，您可以通过带注释的代码块创建 Dataview JS 块`dataviewjs`：

` ```dataviewjs
let pages = dv.pages("#books and -#books/finished").where(b => b.rating >= 7);
for (let group of pages.groupBy(b => b.genre)) {
   dv.header(group.key);
   dv.list(group.rows.file.name);
}
``` ` 

`dv` 在 JS dataview 块中，您可以通过变量访问完整的 dataview API 

## 内联数据视图 JS

与查询语言类似，您可以编写 JS 内联查询，它可以让您直接嵌入计算出的 JS 值。您可以通过内联代码块创建 JS 内联查询：

`` `$= dv.current().file.mtime` `` 

在 inline DataviewJS 中，您可以访问`dv`变量，就像在代码块中访问`dataviewjs`，并且可以进行所有相同的调用。
结果应该被看作 JavaScript 值的结果，Dataview 将自动适当地呈现该值。


# 查询语言

## 例子

## 查询

Dataview查询语言是一种简单的、结构化的、自定义的查询语言，用于快速创建数据的视图。支持：

-   提取与标签、文件夹、链接等相关的页面。
-   通过对字段的简单操作过滤页面/数据，如比较、存在性检查等。
-   根据字段对结果进行排序。

该查询语言支持以下视图类型，描述如下：

1.  **表格(TABLE)**：传统的视图类型；每个数据为一行，字段数据为一列。
2.  **列表(LIST)**：匹配查询页面的列表。你可以为每个页面输出一个单一的关联值。
3.  **任务列表(TASK)**：匹配给定查询的任务列表。

**一般格式**

` ```dataview
TABLE|LIST|TASK <field> [AS "Column Name"], <field>, ..., <field> 
FROM <source> (like #tag or "folder") 
WHERE <expression> (like 'field = value') 
SORT <expression> 
[ASC/DESC] (like 'field ASC') ... other data commands 
``` `

- 只有select 语句是必须的
- 按照顺序运行
- 允许重复多个where
- from可以从任何source中选择

查询类型和数据命令请查看[[4.1.查询]]


## 表达式

表达式：能够产生值的量

```text
# 常规
field               (directly refer to a field)
simple-field        (refer to fields with spaces/punctuation in them like "Simple Field!")
a.b                 (if a is an object, retrieve field named 'b')
a[expr]             (if a is an object or array, retrieve field with name specified by expression 'expr')
f(a, b, ...)        (call a function called `f` on arguments a, b, ...)

# 算术运算
a + b               (addition)
a - b               (subtraction)
a * b               (multiplication)
a / b               (division)

# 比较运算
a > b               (check if a is greater than b)
a < b               (check if a is less than b)
a = b               (check if a equals b)
a != b              (check if a does not equal b)
a <= b              (check if a is less than or equal to b)
a >= b              (check if a is greater than or equal to b)

# 特殊操作
[[Link]].value      (fetch `value` from page `Link`)
```

### 表达式类型

字符串型、字面值型、算数型、比较型、数组对象索引、函数调用、lambda表达式型

**特定类型的交互和值**

大多数dataview类型都有和运算符的特定的操作，或者有额外的能被引用的属性

Date
- date.year
- date.month
- date.day
- date.hour
- date.minute
- date.second
- date.week

Durations
- duration.years
- duration.months
- duration.days
- duration.hours\\minutes\\seconds

Links
`[[Link]].value`


## 字面量(字面值)

字面量是表示常量的表达式

包括一般类型、日期类型、持续时间类型

具体情况在[[4.2.字面值]]


## source

dataview的source定义了一系列的file task 或者其他的数据对象

source 被Dataview内部引用，它支持3种source类型

1. tag `#tag`
2. 目录 `"folder"`
3. 特定文件 `"folder/file.md"`
4. 同名的目录和文件、优先选择目录，可以添加md后缀
5. 进入或者连出文件的链接
6. 要引用连接到文件的链接的话，应该使用 `[[note]]`，也可以使用`[[]]` 引用本文件
7. 引用从文件中射出的链接，使用`outgoing([[note]])`

可以使用and、or链接这些过滤器


## 函数

函数提供了更高级的功能，具体函数查看[[4.3.函数]]

# 查询

## 查询类型

### list查询

最简单的视图，只是呈现与查询匹配的页面列表（或自定义字段）。

语法

`LIST FROM <source>` 

查询

`LIST FROM #games/mobas OR #games/crpg` 

除了每个匹配文件之外，您还可以通过在 之后添加一个表达式来呈现单个计算值`LIST`：

语法

`LIST <expression> FROM <source>` 

查询

`LIST "File Path: " + file.path FROM "4. Archive"` 

##### 无 ID list

如果您不希望列表视图中包含filename/group key，您可以使用`LIST WITHOUT ID`：

语法

`LIST WITHOUT ID <expression> FROM <source>` 

查询

`LIST WITHOUT ID file.path FROM "4. Archive"` 

---

### table 查询

表格支持页面数据的表格视图。您可以通过提供要呈现的 YAML frontmatter 字段，使用逗号分隔列表来构造一个table，如下所示：

`TABLE file.day, file.mtime FROM <source>` 

使用以下`AS`语法选择标题名称来呈现计算字段：

`TABLE (file.mtime + dur(1 day)) AS next_mtime, ... FROM <source>` 

示例：

`TABLE
 time-played AS "Time Played",
 length AS "Length",
 rating AS "Rating"
FROM #game
SORT rating DESC` 

##### 无 ID table

如果您不想在输出中使用默认的“file”或“group”字段，可以使用 `TABLE WITHOUT ID`：

询问

`TABLE WITHOUT ID
 time-played AS "Time Played",
 length AS "Length",
 rating AS "Rating"
FROM #game
SORT rating DESC` 

---

### task 查询

任务视图呈现其页面与给定谓词匹配的所有任务。

语法

`TASK FROM <source>` 

查询

`TASK FROM "dataview"` 

您可以使用典型的数据视图语句根据需要在这些查询中过滤 ( `WHERE`)、分组 ( `GROUP BY`)、排序 ( `SORT`) 

语法

`TASK FROM <source>
WHERE <predicate>
...` 

查询

`TASK FROM "dataview"
WHERE !completed
GROUP BY file.folder` 


###  calendar task 查询

日历视图呈现与日历视图中的查询匹配的所有页面，使用给定的日期表达式来选择呈现页面的日期。

语法

`CALENDAR <date>
FROM <source>` 

查询

`CALENDAR file.mtime
FROM "dataview"` 

输出

输出将是一个日历，在 dataview 目录中每个文件显示一个点。点在文件被修改的日期。

## 数据指令

dataview 查询可以由不同的命令组成。命令按顺序执行，命令可以重复（例如，多个`WHERE` 块或多个`GROUP BY`块）。

### From

该`FROM`语句确定最初将收集哪些页面，并将其传递给其他命令以进一步过滤。
可以从任何[[4.0.查询语言#来源]]进行选择，可以按文件夹、标签或传入/传出链接。

-   **标签**：要从标签（及其所有子标签）中进行选择，请使用`FROM #tag`.
-   **文件夹**：要从文件夹（及其所有子文件夹）中进行选择，请使用`FROM "folder"`.
-   **链接**：您可以选择指向文件的链接，也可以选择文件中的所有链接。
-   要获取链接到的所有页面`[[note]]`，请使用`FROM [[note]]`.
-   要获取链接 FROM 的所有页面`[[note]]`（即该文件中的所有链接），使用`FROM outgoing([[note]])`.

您可以组合这些过滤器，使用`and`和`or`进一步过滤。

-   例如，`#tag and "folder"`将返回所有页面的`folder`和`#tag`.
-   `[[Food]] or [[Exercise]]`将给出任何链接到`[[Food]]`OR的页面`[[Exercise]]`。

您还可以使用以下方法“排除”来源以获取与来源不匹配的任何内容：

-   `-#tag`将排除具有给定标签的文件。
-   `#tag and -"folder"`将仅包括标记`#tag`为不在`"folder"`.

### Where

过滤字段上的页面。结果为`true`才会被选择

`WHERE <clause>` 

1.  获取最近 24 小时内修改的所有文件：
    
    `LIST WHERE file.mtime >= date(today) - dur(1 day)` 
    
2.  查找所有未标记为完成且超过一个月的项目：
    
    `LIST FROM #projects
    WHERE !completed AND file.ctime <= date(today) - dur(1 month)` 

### Sort

按一个或多个字段对所有结果进行排序。

`SORT date [ASCENDING/DESCENDING/ASC/DESC]` 

您还可以提供多个字段进行排序。将根据第一个字段进行排序。然后，如果出现相等，则将使用第二个字段对相等字段进行排序。如果仍然存在平局，则第三个排序将解决它，依此类推。

`SORT field1 [ASCENDING/DESCENDING/ASC/DESC], ..., fieldN [ASC/DESC]` 

### group by

将所有结果分组到一个字段中。每个唯一字段值产生一行，它有 2 个属性：一个对应于被分组的字段，一个`rows`数组字段包含所有匹配的页面。

`GROUP BY field
GROUP BY (computed_field) AS name` 

### flatten

在每一行中展平一个数组，为数组中的每个条目产生一个结果行。

`FLATTEN field
FLATTEN (computed_field) AS name` 

例如，将`authors`每个文献注释中的字段展平，为每位作者提供一行：

查询

`TABLE authors FROM #LiteratureNote
FLATTEN authors` 

### 限制

将结果限制为最多 N 个值。

`LIMIT 5` 

命令是按照它们编写的顺序处理的，所以下面对已经限制后的结果进行排序：

`LIMIT 5
SORT date ASCENDING`




## 字面值

是或者运算后是常量的表达式、常量

### 一般类型

|字面量|解释|
|---|---|
|0|数字0|
|123|数字123|
|-123|数字-123|
|"HelloWorld"|string类型的变量|
|`[[Link]]`| 连接到文件Link的链接 |
|[1,2,3]|数组|
|{a:1,b:2}| 对象|

### 日期类型

date也是[[4.3.函数]]

以下都是字面值：
- date(2021-11-11)
- date(today)
- date(now)
- date(tomorrow)
- date(yesterday)
- date(sow) // start of week 本周的第一天
- date(eow) // start of week 本周的最后一天
- date(som) date(eom)
- date(soy) date(eoy)

### 持续时间类型

#### 综合

dur(1 s, 2 m, 3 h)

dur(1 s 2 m 3 h)

dur(1s 2m 3h)

dur(1second 2min 3h)

#### 秒

以下给出1秒和23秒的实例

dur(1 s) 
dur(1 sec) 
dur(1 second)

dur(23 s)
dur(23 secs)
dur(23 seconds)

#### 分钟

m、min、minute

m、mins、minutes

#### 小时

h、hr、hour

h、hrs、hours

#### 日

d、day

d、days

#### 周

w、wk、week

w、wks、weeks

#### 月

mo、month

mo、months

#### 年

yr、year

yrs、years



# 函数

Dataview 函数提供了更高级的方法来操作数据。

## 函数向量化

大多数函数既可以应用于单个值（如`number`、`string`、`date`等），也可以应用于这些值的列表。如果将函数应用于列表，则在将函数应用于列表中的每个元素后，它也会返回一个列表。例如：

`lower("YES") = "yes"
lower(list("YES", "NO")) = list("yes", "no")
replace("yes", "e", "a") = "yas"
replace(list("yes", "ree"), "e", "a") = list("yas", "raa")` 

## 构造函数

创造值的构造函数。

### `object(key1, value1, ...)`

使用给定的键和值创建一个新对象。键和值应该在调用中交替，键应该始终是字符串/文本。

`object() => empty object
object("a", 6) => object which maps "a" to 6
object("a", 4, "c", "yes") => object which maps a to 4, and c to "yes"` 

### `list(value1, value2, ...)`

创建一个包含给定值的新列表。

`list() => empty list
list(1, 2, 3) => list with 1, 2, and 3
list("a", "b", "c") => list with "a", "b", and "c"` 

### `date(any)`

如果可能，从提供的字符串、日期或链接对象解析日期，否则返回 null。

`date("2020-04-18") = <date object representing April 18th, 2020>
date([[2021-04-16]]) = <date object for the given page, refering to file.day>` 

### `dur(any)`

从提供的字符串或持续时间解析持续时间，失败时返回 null。

`dur(8 minutes) = <8 minutes>
dur("8 minutes, 4 seconds") = <8 minutes, 4 seconds>
dur(dur(8 minutes)) = dur(8 minutes) = <8 minutes>` 

### `number(string)`

从给定字符串中解析出第一个数字，返回它。如果字符串中没有数字，则返回 null。

`number("18 years") = 18
number(34) = 34
number("hmm") = null` 

### `string(any)`

将任何值转换为“合理”的字符串表示形式。与直接在查询中使用值相比，这有时会产生不那么漂亮的结果——它主要用于将日期、持续时间、数字等强制转换为字符串以进行操作。

`string(18) = "18"
string(dur(8 hours)) = "8 hours"
string(date(2021-08-15)) = "August 15th, 2021"` 

### `link(path, [display])`

从给定的文件路径或名称构造一个链接对象。如果提供了两个参数，则第二个参数是链接的显示名称。

`link("Hello") => link to page named 'Hello'
link("Hello", "Goodbye") => link to page named 'Hello', displays as 'Goodbye'` 

### `elink(url, [display])`

构造一个指向外部 url 的链接（如`www.google.com`）。如果提供了两个参数，则第二个参数是链接的显示名称。

`elink("www.google.com") => link element to google.com
elink("www.google.com", "Google") => link element to google.com, displays as "Google"` 

---

## 数值运算

### `round(number, [digits])`

将数字四舍五入到给定的位数。如果未指定第二个参数，则四舍五入到最接近的整数；否则，四舍五入到给定的位数。

`round(16.555555) = 7
round(16.555555, 2) = 16.56` 

--

## 对象、数组和字符串操作

操作容器对象内部的值的操作。

### `contains(object|list|string, value)`

检查给定的容器类型是否具有给定的值。根据第一个参数是对象、列表还是字符串，此函数的行为略有不同。

-   对于对象，检查对象是否具有给定名称的键。例如，
    
    `contains(file, "ctime") = true
    contains(file, "day") = true (if file has a date in its title, false otherwise)` 
    
-   对于列表，检查是否有任何数组元素等于给定值。例如，
    
    `contains(list(1, 2, 3), 3) = true
    contains(list(), 1) = false` 
    
-   对于字符串，检查给定值是否是字符串的子字符串（即，在字符串内部）。
    
    `contains("hello", "lo") = true
    contains("yes", "no") = false` 
    

### `extract(object, key1, key2, ...)`

从一个对象中提取多个字段，创建一个仅包含这些字段的新对象。

`extract(file, "ctime", "mtime") = object("ctime", file.ctime, "mtime", file.mtime)
extract(object("test", 1)) = object()` 

### `sort(list)`

对列表进行排序，按排序顺序返回一个新列表。

`sort(list(3, 2, 1)) = list(1, 2, 3)
sort(list("a", "b", "aa")) = list("a", "aa", "b")` 

### `reverse(list)`

反转一个列表，以相反的顺序返回一个新列表。

`reverse(list(1, 2, 3)) = list(3, 2, 1)
reverse(list("a", "b", "c")) = list("c", "b", "a")` 

### `length(object|array)`

返回对象中的字段数或数组中的条目数。

`length(list()) = 0
length(list(1, 2, 3)) = 3
length(object("hello", 1, "goodbye", 2)) = 2` 

### `sum(array)`

对数组中的所有数值求和

`sum(list(1, 2, 3)) = 6` 

### `all(array)`

`true`仅当数组中的所有值都为真时才返回。您还可以将多个参数传递给此函数，在这种情况下，它`true`仅在所有参数都为真时才返回。

`all(list(1, 2, 3)) = true
all(list(true, false)) = false
all(true, false) = false
all(true, true, true) = true` 

### `any(array)`

`true`如果数组中的任何值是真实的，则返回。您还可以将多个参数传递给此函数，在这种情况下，`true`如果任何参数为真，它就会返回。

`any(list(1, 2, 3)) = true
any(list(true, false)) = true
any(list(false, false, false)) = false
all(true, false) = true
all(false, false) = false` 

### `none(array)`

`true`如果数组中没有一个值是真实的，则返回。

`none([]) = true
none([false, false]) = true
none([false, true]) = false
none([1, 2, 3]) = false` 

### `join(array)`

将数组中的元素连接到单个字符串中（即，将它们全部呈现在同一行上）。如果提供了第二个参数，则每个元素将由给定的分隔符分隔。

`join(list(1, 2, 3)) = "1, 2, 3"
join(list(1, 2, 3), " ") = "1 2 3"
join(6) = "6"
join(list()) = ""` 

### `filter(array, predicate)`

根据谓词过滤数组中的元素，返回匹配的元素的新列表。

`filter([1, 2, 3], (x) => x >= 2) = [2, 3]
filter(["yes", "no", "yas"], (x) => startswith(x, "y")) = ["yes", "yas"]` 

### `map(array, func)`

将函数应用于数组中的每个元素，返回映射结果的列表。

`map([1, 2, 3], (x) => x + 2) = [3, 4, 5]
map(["yes", "no"], (x) => x + "?") = ["yes?", "no?"]` 

---

## 字符串操作

### `regexmatch(pattern, string)`

检查给定字符串是否与给定模式匹配（使用 JavaScript 正则表达式引擎）。

`regexmatch("\w+", "hello") = true
regexmatch(".", "a") = true
regexmatch("yes|no", "maybe") = false` 

### `regexreplace(string, pattern, replacement)`

用,替换_正则表达式_ `pattern`匹配的所有实例。这在底层使用了 JavaScript 替换方法，因此您可以使用特殊字符，例如引用第一个捕获组，依此类推。`string``replacement``$1`

`regexreplace("yes", "[ys]", "a") = "aea"
regexreplace("Suite 1000", "\d+", "-") = "Suite -"` 

### `replace(string, pattern, replacement)`

`pattern`将in的所有实例替换`string`为`replacement`。

`replace("what", "wh", "h") = "hat"
replace("The big dog chased the big cat.", "big", "small") = "The small dog chased the small cat."
replace("test", "test", "no") = "no"` 

### `lower(string)`

将字符串转换为全部小写。

`lower("Test") = "test"
lower("TEST") = "test"` 

### `upper(string)`

将字符串转换为全部大写。

`upper("Test") = "TEST"
upper("test") = "TEST"` 

### `split(string, delimiter, [limit])`

在给定的分隔符字符串上拆分字符串。如果提供了第三个参数，它会限制发生的拆分次数。分隔符字符串被解释为正则表达式。如果分隔符中有捕获组，则将匹配项拼接到结果数组中，不匹配的捕获项为空字符串。

`split("hello world", " ") = list("hello", "world")
split("hello  world", "\s") = list("hello", "world")
split("hello there world", " ", 2) = list("hello", "there")
split("hello there world", "(t?here)") = list("hello ", "there", " world")
split("hello there world", "( )(x)?") = list("hello", " ", "", "there", " ", "", "world")` 

### `startswith(string, prefix)`

检查字符串是否以给定前缀开头。

`startswith("yes", "ye") = true
startswith("path/to/something", "path/") = true
startswith("yes", "no") = false` 

### `endswith(string, suffix)`

检查字符串是否以给定的后缀结尾。

`endswith("yes", "es") = true
endswith("path/to/something", "something") = true
endswith("yes", "ye") = false` 

### `padleft(string, length, [padding])`

通过在左侧添加填充将字符串填充到所需的长度。如果省略填充字符，则默认使用空格。

`padleft("hello", 7) = "  hello"
padleft("yes", 5, "!") = "!!yes"` 

### `padright(string, length, [padding])`

等效于`padleft`，但改为向右填充。

`padright("hello", 7) = "hello  "
padright("yes", 5, "!") = "yes!!"` 

## 实用功能

### `default(field, value)`

如果`field`为空，则返回`value`；否则返回`field`。对于用默认值替换空值很有用。例如，要显示尚未完成的项目，请使用`"incomplete"`它们的默认值：

`default(dateCompleted, "incomplete")` 

默认值在两个参数中都是矢量化的；如果您需要在列表参数上显式使用 default ，请使用`ldefault`，它与 default 相同，但未矢量化。

`default(list(1, 2, null), 3) = list(1, 2, 3)
ldefault(list(1, 2, null), 3) = list(1, 2, null)` 

### `choice(bool, left, right)`

原始 if 语句 - 如果第一个参数为真，则返回左；否则，返回正确。

`choice(true, "yes", "no") = "yes"
choice(false, "yes", "no") = "no"
choice(x > 4, y, z) = y if x > 4, else z` 

### `striptime(date)`

去掉日期的时间部分，只留下年、月和日。如果您不关心时间，则适合日期比较。

`striptime(file.ctime) = file.cday
striptime(file.mtime) = file.mday` 

### `localtime(date)`

将固定时区的日期转换为当前时区的日期。

### `meta(link)`

获取包含链接元数据的对象。当您访问链接上的属性时，您得到的是链接文件中的属性值。该`meta`功能可以访问链接本身的属性。

返回的对象有几个属性`meta`：

#### `meta(link).display`

获取链接的显示文本，如果链接没有定义的显示文本，则为 null。

`meta([[2021-11-01|Displayed link text]]).display = "Displayed link text"
meta([[2021-11-01]]).display = null` 

#### `meta(link).embed`

真或假取决于链接是否为嵌入。这些是以感叹号开头的链接，例如 `![[Some Link]]`.

#### `meta(link).path`

获取链接的路径部分。

`meta([[My Project]]).path = "My Project"
meta([[My Project#Next Actions]]).path = "My Project"
meta([[My Project#^9bcbe8]]).path = "My Project"` 

#### `meta(link).subpath`

获取链接的子路径。对于文件中标题的链接，子路径将是标题的文本。对于块的链接，子路径将是块 ID。如果这些情况都不适用，则子路径将为空。

`meta([[My Project#Next Actions]]).subpath = "Next Actions"
meta([[My Project#^9bcbe8]]).subpath = "9bcbe8"
meta([[My Project]]).subpath = null` 

这可用于选择特定标题下的任务。

` ```dataview
task
where meta(section).subpath = "Next Actions"
``` ` 

#### `meta(link).type`

根据链接是链接到整个文件、文件中的标题还是文件中的块，具有值“文件”、“标题”或“块”。

`meta([[My Project]]).type = "file"
meta([[My Project#Next Actions]]).type = "header"
meta([[My Project#^9bcbe8]]).type = "block"`