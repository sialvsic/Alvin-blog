---
title: Html
date: 2016-12-26 23:25:10
tags:
- Html
---
# HTML

## 什么是HTML？

## HTML注释
普通注释
```
<!-- -->
```

条件注释

```
<!--[if IE 8]>
    .... some HTML here ....
<![endif]-->
```

## HTML标签

### 常用标签
#### div
`<div>`标签可以将网页分割成不同的、清晰的、独立的局部模块，然后在不同的模块中添加内容。 这样使网页布局结构更加清晰，代码更加独立，代码修改时能尽量少地影响到整体页面， 所以在网页开发中提倡使用`<div>`标签

> position:绝对定位和相对定位 (abosolute,relative)
>
height:`<div>`模块的高度
>
width:`<div>`模块的宽度
>
left:相对于窗口左边的位置
>
top:相对于窗口上边的位置
#### blockquote
`<blockquote>`标签可以将其包含起来的文字，全部向右缩进，而且加一组此标签就会向右缩进一个单位

#### p
定义段落

#### pre
`<pre>`标签修饰的内容一般会保留内容中的空格和换行符。在浏览器中显示时，会按照编辑器中预先排好的形式显示内容
#### ol li
有序列表
#### ul li
无序列表
#### dl dd dt
`<dl></dl>`用来创建一个普通的列表,	
`<dt></dt>`用来创建列表中一条内容A，
`<dd></dd>`用来创建A内容的一条子内容B，

#### a
属性
- href : 用来接收一个URL
   - External Hyperlinks : 创建超链接到其他html页面，href属性设置为以"http://"开始的URL
    href="http://www.baidu.com"
   - Relative URLs 如果href属性不以可识别的协议（如：http://）开始，而是链接本地的html页面，那么浏览器将这种超链接视为 Relative URLs
    href="index.html"
   - Internal Hyperlinks 内部链接，它可以直接跳到当前页面中一个特定的模块，创建内部链接可以使用css的样式id选择器
    href="#test"
    
- target : 表示在哪里打开链接文档
    - _self表示在当前窗口当前框架中打开（`默认`）
    - _blank 表示在一个新的窗口中打开链接文档
    - _parent 表示在当前框架的父框架中打开
    - _top 表示先把所有被包含的框架清除，然后将链接文档展示在整个窗口
    
- framename 表示在特定框架（名为framename）中打开连接文档

a 标签可以用于发送邮件，使用mailto
这是一个 mailto 链接：

```
<a href="mailto:someone@microsoft.com?cc=someoneelse@microsoft.com&bcc=andsomeoneelse2@microsoft.com&subject=Summer%20Party&body=You%20are%20invited%20to%20a%20big%20summer%20party!">发送邮件！</a>

//使用%20来代替空格
```
注意在使用时如果src中的内容为网址，必须要加上http才行，如下所示不work会跳转到当前的路径的路由下
```
<a href="www.baidu.com">百度</a>
```

#### img
加载图片，主要的两个属性为src和alt
- src： 定义图片的位置路径
- alt：指定alt 属性用来为图像定义一串预备的可替换的文本。替换文本属性的值是用户定义的

背景图片：`如果图像小于页面，图像会进行重复`

#### table
`<table>`:定义HTML文档中的表格。如果需要给表格设置边框，则`<table border=”1”>`
- th 定义表格中的表头
- tr 定义表格中的一行
- td 定义表格中的一列
- thead 定义表格的页头
- tbody 定义表格的主体
- tfoot 定义表格的页脚
``` html
 <table border="1">
    <caption>学生成绩单</caption>
    <thead>
        <tr>
            <th>姓名</th>
            <th>年龄</th>
           </tr>
       </thead>
    <tbody>
        <tr>
            <td>张三</td>
            <td>25</td>
        </tr>
        <tr>
            <td>李四</td>
            <td>23</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td>平均</td>
            <td>24</td>
        </tr>
    </tfoot>
</table>
```
**NOTE：**tfoot 标签放在tbody标签的外部

caption标签:给表格设置标题，在`<table>`标签内添加标题
colspan设置表格的占用标准表格的几列；
rowspan设置表格的占用标准表格的几行；

#### hr
hr标签定义水平线`<hr/>`

#### br
换行`<br/>`


## HTML特殊标签
### base
base 标签为页面上的所有链接规定默认地址或默认目标（target）

```
<head>
<base href="http://www.w3school.com.cn/images/" />
<base target="_blank" />
</head>
<body>
 <a href="http://www.baidu.com">no set</a>    //可以正常跳转
 <a href="#link">link   //跳转到http://www.w3school.com.cn/images/#link
</body>
```

### meta
`<meta>` 标签提供关于 HTML 文档的元数据。元数据不会显示在页面上，但是对于机器是可读的。
典型的情况是，meta 元素被用于规定页面的描述、关键词、文档的作者、最后修改时间以及其他元数据。

针对搜索引擎的关键词
一些搜索引擎会利用 meta 元素的 name 和 content 属性来索引您的页面。
下面的 meta 元素定义页面的描述：

```
<meta name="description" content="Free Web tutorials on HTML, CSS, XML" />
```

下面的 meta 元素定义页面的关键词：

```
<meta name="keywords" content="HTML, CSS, XML" />
```

name 和 content 属性的作用是描述页面的内容。

## HTML属性
> HTML标签是我们用以表达页面内容及结构的。 势必有些标签我们需要对他进行更细致的设置。所以引入了属性。属性提供了有关 HTML 元素的更多的信息

- 属性总是以名称/值对（又叫键值对）的形式出现，比如：name="value"。属性的名称通常叫做“属性名”，值叫做“属性值”。等号两边可以有空格，但是一般不用
- 属性值可以不使用引号，当值里有空格等特殊符号的时候，需要双引号或单引号引起，但是`建议使用引号`。
- 属性总是在 HTML 元素的开始标签中规定。
- 属性名和属性值对大小写不敏感
- 一个元素可以有多个属性


### HTML全局属性
有一些属性是global的,就是说每一个HTML元素都拥有这些属性

- accesskey 用来设置快速使当前元素获得焦点的快捷键(不同的操作系统下,不同的浏览器里要配以不同的功能键)
- class 用来指定当前元素使用css里定义的哪个的class,又是也只是被用来标明语义
- contenteditable 元素里的内容是否可以被修改
- contextmenu
- dir
- draggable 标明元素是否可以被拖拽
- dropzone
- hidden  隐藏
- id :元素的id,一个页面只能有一个叫做"xxx"的id
- lang
- spellcheck 拼写检查
- style 指定元素的样式,要用css语言
- tabindex
- title 标题

## HTML 字符实体
不间断空格(`&nbsp;`)；其他的详见http://www.w3school.com.cn/html/html_entities.asp



## 表单- Form
form表单：用来接收用户输入信息
### Form 属性：
> - action ：设置URL将表单数据发送到相应的服务器
> - method：设置如何发送表单数据，分为两种方式"post"和"get",默认为"get"方法 
  >  - "get"方法 浏览器与action属性中的URL建立连接后，一次传输表单中所有的数据，并且会将数据直接附在URL之后。（不安全）
  >  - "post"方法 浏览器与action属性中的URL进行连接后，浏览器将表单数据分段发送给服务器；在服务器端，需要对接收到的数据进行解码处理（服务器端会表明如何让接受数据参数）
> - accept-charset ：设置服务器用哪种字符集处理表单数据。一般常用的字符集为：(UTF-8:Unicode字符编码)、 (ISO-8859-1:拉丁字母表的字符编码)、（gb2312:简体中文字符集)
> - enctype： 设置在发送到服务器之前对表单数据的编码，默认：url-encoded
> - autocomplete ：设置是否开启表单自动填写补全功能，默认为"on"。
> - name:   表单的名字
> - novalidate ：设置提交表单时不对表单进行验证。
> - target ： 设置在何处打开action属性的URL

### Form分组的标签：
> FieldSet :  将表单中的相关内容分组
> legend ：给表单中的每个组添加描述信息
### Button
>type="button" 按钮为普通的可点击按钮 
>type="submit" 按钮为提交按钮 
>type="reset" 按钮是重置按钮

### input
input ：单行文本输入框

input属性
> size： 表示输入框可以展示字符的长度
> maxlength： 表示输入框可以展示字符的长度。
> placeholder： 文字占位符
> value： 输入框中显示的文字,(第一次设置的为初始值)
> disabled： 输入字段是禁用的，被禁用的元素是不可用和不可点击的，也是不可提交的
> autofocus ：自动将光标聚焦在已设置的输入框中
> readonly: 规定只能为只读
> type：
   - number : 数字输入框 
   - range : 范围输入框
   - checkbox : 多选框
   - radio ： 单选框
   - email：邮箱
   - tel：电话
   - date：日期
   - time：时间
   - color：颜色
   - search：搜索
   - file：文件上传
   - hidden：隐藏输入框
   - submit:：提交表单
   - button:  设置为按钮
   - url：该包含 URL 地址的输入字段
   - text:  文本
   - passowod： 密码
   - 

注意： 有些浏览器并不完全支持html5 的input 中定义的一些属性，具体的可以查阅http://caniuse.com/


### datalist
为文本框创建展示列表

``` html
<input list="cars" />
<datalist id="cars">
	<option value="BMW">
	<option value="Ford">
	<option value="Volvo">
</datalist>
```

### select
 选择输入框
``` html
 <select id="char"name="char">
    <option value="A" >A</option>
    <option value="B" >B</option>
    <option value="C" selected >C</option>
    <option value="D" >D</option>
</select>
```
select 标签属性multiple 设置显示多行选项

### textarea
多行文本的文本框
> 
autofocus 文本框自动获得焦点
> 
cols="20" 文本框的可以展示字符的宽度
>
disabled 该文本框中的内容不可编辑
>
form="form_id" 该文本框属于哪个表单
>
maxlength 文本框中可以输入的字符的最大长度
>
name 文本框的名字
>
placeholder 文本框的文字占位符
>
pattern 给文本框输入的内容设定格式
>
readonly 文本框为只读，内容不可编辑
>
required 文本框为必填的
>
rows 文本框的可见行数
>
wrap="hard"/"soft" 在提交表单时，文本内容换行的换行方式

## HTML安全色

![Alt text](http://obqvt6b56.bkt.clouddn.com/blog-html-web-safe-color.png)

## Html 加载js
使用 script 元素标签可以插入js的代码到html中，script 元素既可包含脚本语句，也可通过 src 属性指向外部脚本文件。

eg1： 

```
<script type="text/javascript"> 
function function1(){ 
  //TODO
} 
</script>
```

eg2:

```
//index.html
 <script type="text/javascript" src="./index.js"></script>

//indes.js

window.onload = function () {
    var c = document.getElementById("myCanvas");
};

或是
function test(){
  //TODO
}

test();

```

## Html 加载css
加载css有三种方式
- 外部样式表
- 内部样式表
- 内联样式

eg1: 外部样式表

```
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```

eg2: 内部样式表
```
<head>
<style type="text/css">
body {
  background-color: red
}
</style>
</head>
```

eg3: 内部样式

```
<p style="color: red; margin-left: 20px"></p>
```

## 实例
### html中不使用css添加style
``` html
<div id="test" style="position: absolute; height: 200px; width: 200px; background-color: #00aaaa; left: 30px;top: 30px;">
    <p>我是id为"test",距左边框30px,居上边框30px,高度为200px,宽度为200px的,背景颜色为#0000ff的div块
    </p>
</div>
```

## 问题

### 常见的简单的问题

- 可以添加背景颜色`<body bgcolor="yellow">`   不赞成使用的标签，废弃
- 产生粗体的html标签  `<b>`
- 电子邮件链接 `<a href="mailto:xxx@yyy">`
- 新窗口打开链接 `<a href="url" target="_blank">`
- 如何产生带有圆点列表符号的列表  `<ul>`
- 如何产生带有数字列表符号的列表  `<ol>`
- 设置文字居中`<h1 align="center"> 拥有关于对齐方式的附加信息。`   不赞成使用的标签，废弃
- 任意元素加上了title属性后，鼠标悬浮会自动的显示一个提示，但是会有一点延迟
- `<pre>`定义预格式文本，这个标签可以用于显示需要留空格的文本
 

