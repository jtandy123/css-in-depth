# CSS IN DEPTH

## 层叠
CSS里的C（代表cascade，层叠）

如何将特定样式应用到目标元素？

当声明冲突时，层叠会依据三种条件解决冲突：
- 样式表的来源：样式是从哪里来的，包括你的样式和浏览器默认样式等。
- 选择器优先级：哪些选择器比另一个选择器更重要。
- 源码顺序：样式表里的声明顺序。

![cascade-rules](https://github.com/jtandy123/images/blob/master/css/cascade-rules.png?raw=true)
  
### 样式来源
样式来源（优先级从低到高）：
- 用户代理样式，即浏览器默认样式
- 用户样式表，有些浏览器允许用户定义一个用户样式表，很少见
- 作者样式表
- 作者的!important

用户代理样式在不同浏览器上稍有差异，但是大体上是在做相同的事情：为标题（&lt;h1&gt;到&lt;h6&gt;）和段落（&lt;p&gt;）添加上下外边距，为列表（&lt;ol&gt;到&lt;ul&gt;）添加左侧内边距，为链接添加颜色，为元素设置各种默认字号。

### 优先级
如果无法用来源解决冲突声明，浏览器会尝试检查它们的优先级。浏览器将优先级分为两部分：HTML的行内样式和选择器的样式。

1. 行内样式：HTML的style属性写样式，这个声明只会作用于当前元素。行内样式没有选择器。
2. 选择器优先级:
- 如果选择器的ID数量更多，则它会胜出（即它更明确）
- 如果ID数量一致，那么拥有最多类的选择器胜出
- 如果以上两次比较都一致，那么拥有最多标签名的选择器胜出  
  
说明：伪类选择器（如:hover）和属性选择器（如[type="input"]）与一个类选择器的优先级相同。通用选择器（*）和组合器（>、+、~）对优先级没有影响。

优先级标记：  
一个常用的表示优先级的方式是用数值形式来标记，通常用逗号隔开每个数。比如，“1,2,2”表示选择器由1个ID、2个类、2个标签组成。优先级最高的ID列为第一位，紧接着是类，最后是标签。
有时还会用4个数的标记，其中将最重要的位置用0或1来表示，代表一个声明是否是用行内样式添加的。
  
通常最好让优先级尽可能低，这样当需要覆盖一些样式时，才能有选择空间。

### 源码顺序  
如果两个声明的来源和优先级相同，其中一个声明在样式表中出现较晚，或者位于页面较晚引入的样式表中，则该声明胜出。

实现相同的效果有很多途径。多想些实现方法，并思考每一种方法的利弊。面对一个样式问题，首先确定哪些声明可以实现效果。其次，思考可以用哪些选择器结构，然后选择最符合需求的那个。  

链接样式和源码顺序
```
a:link {
  color: blue;
  text-decoration: none;
}

a:visited {
  color: purple;
}

a:hover {
  text-decoration: underline;
}

a:active {
  color: red;
}
```
书写顺序之所以重要是因为层叠。优先级相同时，后出现的样式会覆盖先出现的样式。如果一个元素同时处于两个或者更多状态，最后一个状态就能覆盖其他状态。

层叠值：如果一个声明在层叠中“胜出”，它就被称作一个层叠值。元素的每个属性最多只有一个层叠值。

处理层叠时有两条通用的经验法则：
- 在选择器中不要使用ID。比较难覆盖，通常找不到另一个有意义的ID进行覆盖，于是就会复制原来的选择器，然后加上另一个类，以区别于想要覆盖的选择器。
- 不要使用!important。一旦引入一个!important，就会带来更多的!important
  
  
## 继承  
如果一个元素的某个属性没有层叠值，则可能会继承某个祖先元素的值。比如通常会给&lt;body&gt;元素加上font-family，里面的所有后代元素都会继承这个字体，就不必给页面的每个元素明确指定字体了。

默认情况下，只有特定的一些属性能被继承，它们主要是：
- 文本相关的属性：color、font、font-family、font-size、font-weight、font-variant、font-style、line-height、letter-spacing、text-align、text-indent、text-transform、white-space以及word-spacing。
- 列表属性：list-style、list-style-type、list-style-position以及list-style-image。
- 表格的边框属性：border-collapse、border-spacing。

继承属性会顺序传递给后代元素，直到它被层叠值覆盖。

## 特殊值  
有两个特殊值可以赋给任意属性，用于控制层叠：inherit和initial。  
- inherit：元素会继承其父元素的值。还可以使用inherit关键字强制继承一个通常不会被继承的属性，比如边框和内边距。通常在实践中很少这么做。
- initial: 撤销作用于某个元素的样式。每一个CSS属性都有初始（默认）值。如果将initial值赋给某个属性，那么就会有效地将其重置为默认值，这种操作相当于硬复位了该值。initial重置为属性的初始值，而不是元素的初始值。

例如：
border: initial等价于border: none  
width: initial等价于width: auto
display: initial等价于display: inline

## 简写属性  
用于同时给多个属性赋值的属性。
- font: font-style font-variant font-weight font-size/line-height font-family
- background: background-color background-position background-size background-repeat background-origin background-clip background-attachment background-image
- border: border-width border-style border-color
- border-width: 上右下左
```
font: italic bold 18px/1.2 "Helvetica", "Arial", sans-serif;
```
简写属性会默默覆盖其他样式：大多简写属性可以省略一些值，只指定关注的值。省略的值会被隐式地设置为初始值。会默默覆盖在其他地方定义的样式。
在所有简写属性里，font的问题最严重，要避免在&lt;body&gt;元素的通用样式以外使用font。  

简写值的顺序：
- 上、右、下、左：margin、padding等，时钟顺时针
- 水平、垂直：background-position、box-shadow、text-shadow等，笛卡尔网格
---

## 相对单位

响应式：在CSS中指的是样式能够根据浏览器窗口的大小有不同的“响应”。  
绝对长度单位：
- px
- mm
- cm
- in
- pt（点，印刷术语，1/72英寸）
- pc（派卡，印刷术语，12点）  
  
1 in = 25.4 mm = 2.54cm = 6 pc = 72 pt = 96 px  
16 px = 12 pt  
设计师经常用点作为单位，开发人员则习惯用像素  

浏览器会根据相对单位的值计算出绝对值，称作计算值。
- em, 1em等于当前元素的字号，其准确值取决于作用的元素  
- rem  

### em
当设置padding、height、width、border-radius等属性时，使用em会很方便。当元素继承了不同的字号，或者用户改变了字体设置时，这些属性会跟着元素均匀地缩放。 

当font-size使用em声明时，会根据继承的字号来计算绝对字号。  

如果知道字号的像素值，但是想用 em 声明，可以用一个简单的公式换算：用想
要的像素大小除以父级（继承）的像素字号。比如，想要一个 10px 的字体，元素继承
的字体是 12px，则计算结果是 10/12 = 0.8333em。如果想要一个 16px 的字体，父级字
号为 12px，则计算结果是 16/12 = 1.3333em。  

对大多数浏览器来说，默认的字号为 16px。准确地说，medium 关键字
的值是 16px。  

- em同时用于字号和其他属性
   - 用 em 定义了字号（基于继承的字号）
   - 用 em 定义了其他属性，比如
padding 和 border-radius（基于当前元素的字号）。  

em 的复杂之处在于同时用它指定一个元
素的字号和其他属性。这时，浏览器必须先计算字号，然后使用这个计算值去算出其余的属性值。
这两类属性可以拥有一样的声明值，但是计算值不一样。

- 字体缩小问题：当用 em 来指定多重嵌套的元素的字号时，文字会放大或者缩小。
```
body {
 font-size: 16px;
}
ul {
 font-size: .8em;
} 

<ul>
 <li>Top level
  <ul>
   <li>Second level
    <ul>
     <li>Third level
      <ul>
       <li>Fourth level
        <ul>
         <li>Fifth level</li>
        </ul>
       </li>
      </ul>
     </li>
    </ul>
   </li>
  </ul>
 </li>
</ul>
```
解决办法：嵌套的列表应当跟其父级的字号一致
```
ul {
 font-size: .8em;
}
ul ul {
 font-size: 1em;
} 
```

### rem
根节点是所有其他元素的祖先节点。根节点有一个伪类选择器（:root），可以
用来选中它自己。  
rem 是 root em 的缩写。rem 不是相对于当前元素，而是相对于根元素的单位。不管在文档
的什么位置使用 rem，1.2rem 都会有相同的计算值：1.2 乘以根元素的字号。  

一般会用 rem 设置字号，用 px 设置边框，用 em 设置
其他大部分属性，尤其是内边距、外边距和圆角（有时用百分比设置容器宽度）。
- 停止像素思维
   - 设置一个合理的默认字号
   - 构造响应式面板：可以根据屏幕尺寸，用媒体查询改变根元素的字号

> 媒体查询，即@media 规则，可以指定某种屏幕尺寸或者媒体类型（比如，打印机
或者屏幕）下的样式。这是响应式设计的关键部分。
