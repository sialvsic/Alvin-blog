---
title: html5
date: 2016-12-27 09:42:08
tags:
- html5
- html
- front-end
- web
---

# HTML5

## HTML5的出现的目的

- Web浏览器之间的兼容性很低
- 文档结构不够明确，过多的使用`<div>`
- web应用程序的功能受到了限制

## HTML5版本于HTML4的区别
- IE 6,7,8 不支持HTML5
- 语法的改变
- 内容类型
> text/html

- DOCTYPE 文档类型
> `<!DOCTYPE html>`

- 指定字符编码
> `<meta charset="utf=8">`

- 省略引号
**属性值可以不写双引号**

- 带有默认值
> checked="checked"   //选中
> checked=""   //选中
> check  //选中
> check="unchecked"   //未选中

## 新增元素
- 新增的结构元素
section  ， article  ，aside，  header  ，footer ，nav  ，figure,  main
- 新增的其他元素
audio  ， video  ，canvas(画布)  ，mark ，time ，command ，source ，menu
- 新增的input元素类型
email ， url  ，number ，range 

## 废除的元素
不再使用frame框架   仅支持iframe

## 新增的属性

## 废除的属性


## 全局属性—HTML5中新增的特性
**可以对全局对象都适用的属性**
- contentEditable 属性  指定元素可编辑 ，必须是那种可以获取到鼠标焦点的
value：true false
- designMode 属性 指定页面是否可以被编辑
value：on  off
- hidden 属性  隐藏元素
- spellcheck属性  针对input和textarea输入框  新增的输入拼写检查
- tabindex属性   用于指定tab的显示顺序，可以让不能获取焦点的标签获取到焦点
value：tabindex="x"

## 标签
### 常用的标签
#### section
`<div>`和`<section>`两者都有对网页进行布局的作用。`<div>`本身没有任何语义，仅用于页面布局或者给模块添加样式

`<section>`顾名思义就是一个章节，它适用于对文档进行分块，将文档划分为章节（常用于修饰文档大纲），或者对一篇文章进行分块，将整篇文章分成不同的内容块。

**note：**注意：一个`<section>`元素不能是`<address>`元素的子节点。

#### article
**`<article>`标签是特殊的`<section>`标签**，与section不同，`<article>`标签是将文档或文章中的内容划分为相对独立的模块（如，将整本书的内容划分成多篇文章，或者将一篇文章划分成相对独立的不同的小结等），当然，我们所说的独立是指同一级别的article标签中的内容是相互独立的，所以一般在article标签内一般包含header和footer标签。

#### section vs article
``` html
Version 1:
<article>
    <header>
        <h1>大家好，我是美食爱好者小丽</h1>
    </header>
    <p>本文介绍一下美食爱好者小丽喜欢的水果。。。</p>
    <section>
        <h2>小丽喜欢的第一种水果是石榴</h2>
        <p>美味的石榴营养丰富，富含维C。。。</p>
    </section>
    <section>
        <h2>小丽喜欢的第二种水果是葡萄</h2>
        <p>葡萄可以美容养颜。。。</p>
    </section>
    <footer>
        <p>版权出自小丽</p>
    </footer>
</article>

Version 2:
<section>
    <h2>新闻.国内专题</h2>
    <article>
        <h3>2014年南方暴雨</h3>
        <p>2014年南方暴雨详情</p>
    </article>
    <article>
        <h3>山西官场地震</h3>
        <p>山西官场地震详情介绍</p>
    </article>
</section>
```
关于两者做一下总结：一般来说,互为兄弟关系的两个相邻的`<section>`标签里面的内容一般是相关联的，而对于两个相邻的`<article>`标签而言，它们里面的内容是没有关联的，是独立的。 由上面的例子可以更加深刻地理解section和article,从语义上讲，article主要强调独立完整性，而section之间可能会有关联，每个section内容也不是完整， 正如上面的代码看到的，article是独立完整的一篇文章或者文档，而section可能有所关联且不完整。从理论上讲，article适用的内容，都可以使用section，但如果符合使用article的要求就不要使用section。
#### aside
标签布局与当前文档内容相关却在此文档内容之外的内容
#### header
#### footer
#### nav
`<nav>`元素代表一个部分包含链接，链接到其他页面或者同一页面的其他部分，主要定义了文档中的导航部分


## Canvas
HTML5 的 canvas 元素使用 JavaScript 在网页上绘制图像。
画布是一个矩形区域，您可以控制其每一像素。



创建：

```
<canvas id="myCanvas" width="200" height="100"></canvas>
```

**canvas 元素本身是没有绘图能力的。所有的绘制工作必须在 JavaScript 内部完成**
```
<script type="text/javascript">
	var c=document.getElementById("myCanvas");
	//getContext("2d") 对象是内建的 HTML5 对象，拥有多种绘制路径、矩形、圆形、字符以及添加图像的方法
	var cxt=c.getContext("2d");
	cxt.fillStyle="#FF0000";
	cxt.fillRect(0,0,150,75);
</script>
```


## SVG
HTML5 内联 SVG

什么是SVG？
- SVG 指可伸缩矢量图形 (Scalable Vector Graphics)
- SVG 用于定义用于网络的基于矢量的图形
- SVG 使用 XML 格式定义图形
- SVG 图像在放大或改变尺寸的情况下其图形质量不会有损失
- SVG 是万维网联盟的标准

## 地理位置
getCurrentPosition() 方法来获得用户的位置

## HTML5 拖放
拖放（Drag 和 Drop）是很常见的特性。它指的是您抓取某物并拖入不同的位置。
拖放是 HTML5 标准的组成部分：任何元素都是可拖放的。

如何拖放：
> Step1：把元素设置为可拖放，为了把一个元素设置为可拖放，请把 draggable 属性设置为 true：

```
<img id="drag1" src="./gift.png" draggable="true" ondragstart="drag(event)" width="336" height="269">
```

> Step2：然后规定当元素被拖动时发生的事情
ondragstart 和 setData()
- ondragstart() 可以调用一个函数（drag（event））规定拖动什么数据
- dataTransfer.setData() 方法设置被拖动数据的数据类型和值

```
function drag(ev) {
    ev.dataTransfer.setData("text", ev.target.id);
}
//数据类型是 "text"，而值是这个可拖动元素的 id ("drag1")
//实质上是移动了这个元素到新的位置
```

> Step3: 拖到何处 - ondragover

ondragover 事件规定被拖动的数据能够被放置到何处。
默认地，数据/元素无法被放置到其他元素中。为了实现拖放，我们必须阻止元素的这种默认的处理方式。
这个任务由 ondragover 事件的 event.preventDefault() 方法完成：

```
event.preventDefault()
```

Step4: 进行放置 - ondrop
当放开被拖数据时，会发生 drop 事件, ondrop 属性调用了一个函数，drop(event)：

```
function drop(ev) {
    ev.preventDefault();
    var data = ev.dataTransfer.getData("text");
    ev.target.appendChild(document.getElementById(data));
}
```

解释：
- 调用 preventDefault() 来阻止数据的浏览器默认处理方式（drop 事件的默认行为是以链接形式打开）
- 通过 dataTransfer.getData() 方法获得被拖的数据。该方法将返回在 setData() 方法中设置为相同类型的任何数据
- 被拖数据是被拖元素的 id ("drag1")
- 把被拖元素追加到放置元素中

重点：
- DataTransfer 对象：拖拽对象用来传递的媒介，使用一般为Event.dataTransfer。
- draggable 属性：就是标签元素要设置draggable=true，否则不会有效果，例如：

```
<div title="拖拽我" draggable="true">列表1</div>
```

- ondragstart 事件：当拖拽元素开始被拖拽的时候触发的事件，此事件作用在被拖曳元素上
- ondragenter 事件：当拖曳元素进入目标元素的时候触发的事件，此事件作用在目标元素上
- ondragover 事件：拖拽元素在目标元素上移动的时候触发的事件，此事件作用在目标元素上
- ondrop 事件：被拖拽的元素在目标元素上同时鼠标放开触发的事件，此事件作用在目标元素上
- ondragend 事件：当拖拽完成后触发的事件，此事件作用在被拖曳元素上
- ondragleave 事件： 当拖拽的元素离开时触发，此事件作用在被拖拽元素上
- Event.preventDefault() 方法：阻止默认的些事件方法等执行。在ondragover中一定要执行preventDefault()，否则ondrop事件不会被触发。另外，如果是从其他应用软件或是文件中拖东西进来，尤其是图片的时候，默认的动作是显示这个图片或是相关信息，并不是真的执行drop。此时需要用用document的ondragover事件把它直接干掉。
- Event.effectAllowed 属性：就是拖拽的效果。

## html5 web 存储
HTML 本地存储：优于 cookies。

什么是 HTML 本地存储？
通过本地存储（Local Storage），web 应用程序能够在用户浏览器中对数据进行本地的存储。
在 HTML5 之前，应用程序数据只能存储在 cookie 中，包括每个服务器请求。本地存储则更安全，并且可在不影响网站性能的前提下将大量数据存储于本地。
与 cookie 不同，存储限制要大得多（至少5MB），并且信息不会被传输到服务器。
本地存储经由起源地（origin）（经由域和协议）。所有页面，从起源地，能够存储和访问相同的数据。


HTML 本地存储对象
HTML 本地存储提供了两个在客户端存储数据的对象：
window.localStorage - 存储没有截止日期的数据
window.sessionStorage - 针对一个 session 来存储数据（当关闭浏览器标签页时数据会丢失）

### localStorage 对象
localStorage 对象存储的是没有截止日期的数据。当浏览器被关闭时数据不会被删除，在下一天、周或年中，都是可用的。


eg：

```
// 存储
localStorage.setItem("lastname", "Gates");
或是
localStorage.lastname = "Gates";


// 取回
localStorage.getItem("lastname");
或是
localStorage.lastname

//删除
localStorage.removeItem("keyname")
```

localStorage如何存取，删除对象

```
var testObject = { 'one': 1, 'two': 2, 'three': 3 };

// Put the object into storage
localStorage.setItem('testObject', JSON.stringify(testObject));

// Retrieve the object from storage
var retrievedObject = localStorage.getItem('testObject');

console.log('retrievedObject: ', JSON.parse(retrievedObject));
```

**注意：亲测表明localStorge仅仅对于同源的网站下是可用的**

比如，对于在此链接(`http://stackoverflow.com/questions/2010892/storing-objects-in-html5-localstorage`)下设置的localStorage，那么在`www.baidu.com`下是访问不到的。但是在http://stackoverflow中是可以访问到的。

### sessionStorage对象
sessionStorage 对象等同 localStorage 对象，不同之处在于只对一个 session 存储数据。如果用户关闭具体的浏览器标签页，数据也会被删除。

## HTML事件属性
具体的事件描述参考：http://www.w3school.com.cn/tags/html_ref_eventattributes.asp



