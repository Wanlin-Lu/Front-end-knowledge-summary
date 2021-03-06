﻿# 五、页面架构

标签（空格分隔）： 页面架构

---
## 目录
* 5.1 [CSS Reset][5.1]
* 5.2 [布局解决方案][5.2]
* 5.3 [响应式布局][5.3]
* 5.4 [页面优化][5.4]
* 5.5 [规范与模块化][5.5]

---
### 5.1 CSS Reset
#### 5.1.1 CSS Reset方法和应用
##### 5.1.1.1 为什么要Reset CSS
IE/Chrome/FireFox/Opero/Sefari,每种浏览器对不同的HTML标签有不同的`初始`样式设定。而如果我们的网站要在不同的浏览器上保持同样的样式，最好的办法就是`统一`这些设定。

比如：要设定一个`<h3>`元素的样式,开发者就需要如下的操作；
```html
/* h3元素 */
<th class="m-article">
    <h3>标题</h3>
</th>

//浏览器默认的样式如下
th{
    font-weight: bold;
}
h3{
    display: block;
    font-size: 1.17em;
    -webkit-margin-before: 1em;
    -webkit-margin-after: 1em;
    -webkit-margin-start: 0px;
    -webkit-margin-end: 0px;
    font-weight: bold;
}

//开发者需要在CSS中的设定
.m-article h3{
    font-size: 16px;
    margin: 0;
    font-weight: normal;
}
```

##### 5.1.1.2 如何进行CSS Reset
* **逐条重设法**：也就是上面用的reset方法，在每个需要reset的CSS语句中进行归零设置；但是这样做，就大大增加了开发者的工作，同时大量的重设代码会影响网站的性能。
* **Reset.css** ：在一个CSS文件中，把所有的HTML标签的样式都进行`统一初始设定`，这样一个文件引入到HTML文件中后(`<link rel"stylesheet" type="text/css" href="reset.css">`)，就能够把不同浏览器的`各自初始设定`替换为Reset.css的`统一初始设定`；然后在Reset.css的基础上开发的网站应用，在不同的浏览器上有相同的样式了，同时减轻了开发者的工作量。
```css
html,body,h1,h2,h3,h4,h5,h6,div,dl,dt,dd,ul,ol,li,p,blockquote,pre,hr,figure,table,caption,th,td,form,fieldset,legend,input,button,textarea,menu{margin:0;padding:0;}
header,fotter,section,article,aside,nav,hgroup,address,figure,figcaption,menu,details{display:block;}
table{border-collapse:collapse;border-spacing:0;}
caption,th{text-align:left;font-weight:normal;}
html,body,fieldset,img,iframe,abbr{border:0;}
i,cite,em,var,address,dfn{font-style:normal;}
[hidefocus],summary{outline:0;}
li{list-style:none;}
h1,h2,h3,h4,h5,h6,small{font-size:100%;}
sup,sub{font-size:83%;}
precode,kbd,samp{font-family:inherit;}
q:brfore,q:after{content:none;}
textarea{overflow:auto;resize:none;}
label,summary{cursor:default;}
a,button{cursor:pointer;}
h1,h2,h3,h4,h5,h6,em,strong,b{font-weight:normal;}
del,ins,u,s,a,a:hover{text-decoration:none;}
body,textarea,input,button,select,keygen,legend{font:12px/1.14 arial,simsun;color:#333;outline:0;}
```
##### 5.1.1.3 本质和使用
* 不同的软件、产品都是不一样的设置，所以需要因产品因时而对reset文件进行修改，然后使用；
* 如何使用：
    - 在项目初期就进行定好默认样式；
    - 在link的第一位就引出reset.css样式表；
        - `<link rel="stylesheet" href="reset.css"/>`
        - 或者把Reset代码，放在项目CSS样式表的最上面；

### 5.2 布局解决方案
#### 5.2.1 居中布局
##### 5.2.1.1 水平居中
* `inline-block` + `text-align` 
    - 优点：兼容性好
    - 缺点：child中的文字会继承text-align:center
```html
/* html */
<div class="parent">
    <div class="child">DEMO</div>
</div>

/* css */
.child{display: inline-block;}
.parent{text-align: center;}
```
* `table` + `margin`
    - 优点：只需要对child近些设置（因为table本来就是inline-block?）
    - 缺点：
```html
/* html */
<div class="parent">
    <div class="child">DEMO</div>
</div>

/* css */
.child{display: table;margin: 0 auto;}
```
* `relative` + `absolute` + `transform`
    - 优点： 子元素脱离文档流
    - 缺点： 不能兼容IE678
```html
/* html */
<div class="parent">
    <div class="child">DEMO</div>
</div>

/* css */
.parent{position: relative;}
.child{position: absolute;left: 50%;transform: translateX(-50%);}

/* 备注：.parent在relative之后，必须要设置高度，才能有背景颜色、margin等值的显示 */
```
* `flex` + `justify-content`
    - 优点： 只需要设置父元素
    - 缺点： 不支持IE678
```html
/* html */
<div class="parent">
    <div class="child">DEMO</div>
</div>

/* css1 */
.parent{display: flex;justify-content: center;}

/* css2 */
.parent{display: flex;}
.child{margin: 0 auto;}
```
##### 5.2.1.2 垂直居中（bs:子容器和父容器的高度都不确定）
* `table-cell` + `vertical-align`
    - 优点： 兼容性比较好
```html
/* html */
<div class="parent">
    <div class="child">DEMO</div>
</div>

/* css */
.parent{display: table-cell; vertical-align: middle;}
```
* `absolute` + `transform`
    - 优点： 子元素不干扰其他元素
    - 缺点： 兼容性不好
```html
/* html */
<div class="parent">
    <div class="child">DEMO</div>
</div>

/* CSS */
.parent{position: relative;}
.child{position: absolute; top: 50%; transform: translateY(-50%);}
```
* `flex` + `align-items`
    - 优点： 只需要设置父元素
    - 缺点： 只有高版本的浏览器支持
```html
/* html */
<div class="parent">
    <div class="child">DEMO</div>
</div>

/* css */
.parent{display: flex; align-items: center;}
```
##### 5.2.1.3 水平和垂直都居中
* `inline-block` + `text-align` + `table-cell` + `vertical-align`
    - 优点： 兼容性好
```html
/* html */
<div class="parent">
    <div class="child">DEMO</div>
</div>

/* css */
.parent{text-align: center; display: table-cell; vertical-align: middle;}
.child{display: inline-block;}
```
* `absolute` + `transform`
    - 优点： 子元素不干扰其他元素
```html
/* html */
<div class="parent">
    <div class="child">DEMO</div>
</div>

/* css */
.parent{position: relative;}
.child{position: absolute; left: 50%; top: 50%; transform: translate(-50%,-50%);}
```
* `flex` + `justify-content` + `align-items`
    - 优点： 只需要设置父元素
```html
/* html */
<div class="parent">
    <div class="child">DEMO</div>
</div>

/* css */
.parent{display: flex; justify-content: center; align-items: center;}
```
* **做方案的思路**
    - 要熟练掌握CSS相关的属性和值的特性；
    - 对问题进行分解，分布解决；


#### 5.2.2 多列布局
##### 5.2.2.a 一列定宽一列自适应
* `float` + `margin`
    - 优点： 容易理解
    - 缺点： IE6支持有问题哦
```html
/* html */
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="right">
        <p>right</p>
        <p>right</p>
    </div>
</div>

/* css */
.left{float: left; width: 100px;}
.right{margin-left: 120px;}
```
* `float` + `margin` + `(fix)`  
    - 优点： 兼容性好
    - 缺点： 多了一个结构
```html
/* html */
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="right-fix">
        <div class="right">
            <p>right</p><p>right</p>
        </div>
    </div>
</div>

/* css */
.left{float: left; width: 100px; position: relative;}
.right-fix{width: 100%; margin-left: -100px; float: right;}
.right{margin-left: 120px;}
```
* `float` + `overflow`
    - 优点： 简单
    - 缺点： 不支持IE6
```html
/* html */
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="right">
        <p>right</p>
        <p>right</p>
    </div>
</div>

/* css */
.left{width: 100px; float: left; margin-right: 20px;}
.right{overflow: hidden;}
```
* `table`
    - 缺点： 代码多
```html
/* html */
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="right">
        <p>right</p>
        <p>right</p>
    </div>
</div>

/* css */
.parent{display: table; width: 100%; table-layout: fixed;}
.right,.left{display: table-cell;}
.left{width: 100px; padding-right: 20px;}
```
* `flex`
    - 缺点： 低版本不兼容、可能性能会有影响，不适合复杂的布局
```html
/* html */
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="right">
        <p>right</p>
        <p>right</p>
    </div>
</div>

/* css */
.parent{display: flex;}
.left{width: 100px; margin-right: 20px;}
.right{flex: 1;}
```
##### 5.2.2.b 两列定宽，一列自适应
* `float` + `overflow: hidden`
```html
/* html */
<div class="parent">
    <div class="left>
        <p>left</p>
    </div>
    <div class="middle">
        <p>middle</p>
    </div>
    <div class="right">
        <p>right</p>
        <p>right</p>
    </div>
</div>

/* css */
.left,.middle{width: 100px; float: left; margin-right: 20px;}
.right{overflow: hidden;}
```
##### 5.2.2.c 等分布局 `C + G = (W+G)*N`
* `float`
    - 优点： 兼容IE67
    - 缺点： 不灵活
```html
/* html */
<div class="parent">
    <div class="column"><p>1</p></div>
    <div class="column"><p>2</p></div>
    <div class="column"><p>3</p></div>
    <div class="column"><p>4</p></div>
</div>

/* css */
.parent{margin-left: -20px;}
.column{width: 25%; padding-left: 20px; float: left; box-sizing: border-box;}
```
* `table`
    - 优点： 灵活
    - 缺点： 多了一个结构
```html
/* html */
<div class="parent-fix">
    <div class="parent">
        <div class="column"><p>1</p></div>
        <div class="column"><p>2</p></div>
        <div class="column"><p>3</p></div>
        <div class="column"><p>4</p></div>
    </div>
</div>

/* css */
.parent-fix{margin-left: -20px;}
.parent{display: table; width: 100%; table-layout: fixed;}
.column{display: table-cell; padding-left: 20px;}
```
* `flex`
    - 优点： 简洁、灵活
    - 缺点： 兼容性问题
```html
/* html */
<div class="parent">
    <div class="column"><p>1</p></div>
    <div class="column"><p>2</p></div>
    <div class="column"><p>3</p></div>
    <div class="column"><p>4</p></div>
</div>

/* css */
.parent{display: flex;}
.column{flex: 1;}
.column+.column{margin-left: 20px;}
```
##### 5.2.2.d 一列定宽，另一列自适应，两列高度自适应
* `table`
```html
/* html */
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="right">
        <p>right</p>
        <p>right</p>
    </div>
</div>

/* css */
.parent{display: table; width: 100%; table-layout: fixed;}
.left,.right{display: table-cell;}
.left{width: 100px; padding-right: 20px;}

/* 备注：上面的方案貌似，并不能两行分开呀！*/

/* html2 */
<div class="parent pbox p20">
    <div class="left thec l20">
        <p class="thep">left</p>
    </div>
    <div class="mi m20"></div>
    <div class="right thec r20">
        <p class="thep">right</p>
        <p class="thep">right</p>
    </div>
</div>

/* css2 */
.p20{display: table; width: 100%; table-layout: fixed;}
.r20,.m20,.l20{display: table-cell;}
.m20{width: 20px;}
.l20{width: 100px;box-sizing: content-box;}
```
* `flex`
```html
/* html */
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="right">
        <p>right</p>
        <p>right</p>
    </div>
</div>

/* css */
.parent{display: flex;}
.left{width: 100px; margin-right: 20px;}
.right{flex: 1;}
```
* `float`
    - 优点： 兼容性好
```html
/* html */
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="right">
        <p>right</p>
        <p>right</p>
    </div>
</div>

/* css */
.parent{overflow: hidden;}
.left,.right{padding-bottom: 9999px; margin-bottom: -9999px;}
.left{float: left; width: 100px; margin-right: 20px;}
.right{overflow: hidden;}
```

#### 5.2.3 全屏布局（一般用在信息展示、监控平台上）
**特点**： 充满浏览器窗口，滚动条只出现在内容区域；
##### 5.2.3.a 需求一： 整页整体自适应；头部、尾部、侧边栏定宽度（px）；内容区自适应；
* `position`
    - 注： IE6以下不支持
```html
/* html */
<div class="parent">
    <div class="top">top</div>
    <div class="left">left</div>
    <div class="right">
        <div class="inner">right</div>
    </div>
    <div class="bottom">bottom</div>
</div>

/* css */
html,body,.parent{height: 100%; overflow: hidden;}
.top{position: absolute; top: 0; left: 0; right: 0; height: 100px;}
.left{position: absolute; left: 0; top: 100px; bottom: 50px; width: 200px;}
.right{position: absolute; overflow: auto; left: 200px; right: 0; top: 100px; bottom: 50px;}
.bottom{position: absolute; left: 0; right: 0; bottom: 0; height: 50px;}
.right .inner{min-height: 1000px;}
```
* `flex`
    - 注： IE9以下不支持
```html
/* html */
<div class="parent">
    <div class="top">top</div>
    <div class="middle">
        <div class="left">left</div>
        <div class="right">
            <div class="inner">right</div>
        </div>
    </div>
    <div class="bottom">bottom</div>
</div>

/* css */
html,body,.parent{height: 100%; overflow: hidden;}
.parent{display: fex; flex-direction: column;}
.top{height: 100px;}
.bottom{height: 50px;}
.middle{flex:1; display: flex;}
.left{width: 200px;}
.right{flex: 1; overflow: auto;}
.right .inner{min-height: 1000px;}
```
##### 5.2.3.b 需求二：整页整体自适应；头部、尾部、侧边栏定宽度（%）；内容区自适应；
* `position`
```html
/* html */
<div class="parent">
    <div class="top">top</div>
    <div class="left">left</div>
    <div class="right">
        <div class="inner">right</div>
    </div>
    <div class="bottom">bottom</div>
</div>

/* css */
html,body,.parent{height: 100%; overflow: hidden;}
.top{position: absolute; top: 0; left: 0; right: 0; height: 10%;}
.left{position: absolute; left: 0; top: 10%; bottom: 5%; width: 20%;}
.right{position: absolute; overflow: hidden; left: 20%; right: 0; top: 10%; bottom: 5%;}
.bottom{position: absolute; left: 0; right: 0; bottom: 0; height: 5%;}
.right .inner{min-height:1000px;}
```
* `flex`
```html
/* html */
<div class="parent">
    <div class="top">top</div>
    <div class="middle">
        <div class="left">left</div>
        <div class="right">
            <div class="inner">right</div>
        </div>
    </div>
    <div class="bottom">bottom</div>
</div>

/* css */
html,body,.parent{height: 100%; overflow: hidden;}
.parent{display: flex; flex-direction: column;}
.top{height: 10%;}
.middle{flex: 1; display: flex;}
.left{width: 20%;}
.right{flex: 1; overflow: auto;}
.right .inner{min-height: 1000px;}
```
##### 5.2.3.c 需求三：整页整体自适应；头部、尾部、侧边栏自适应（内容）；内容区自适应；
**分析：**`position`无法使用，只能用`flex`/`Grid`

* `flex`
```html
/* html */
<div class="parent">
    <div class="top">top</div>
    <div class="middle">
        <div class="left">left</div>
        <div class="right">
            <div class="inner">right</div>
        </div>
    </div>
    <div class="bottom">bottom</div>
</div>

/* css */
html,body,.parent{height: 100%; overflow: hidden;}
.parent{display: flex; flex-direction: column;}
.middle{flex: 1; display: flex;}
.right{flex: 1; overflow: auto;}
.right .inner{min-height: 1000px;}
```

### 5.3 响应式布局
* 需求： 满足多设备的浏览要求
    * 优点： 节约成本
    * 缺点： 有些内容会被省略
```html
/* viewport */
<meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no"/>

/* @media */
@media screen and (max-width: 320px){/* 视窗宽度<=320px */}
@meida screen and (min-width: 769px){/* 视窗宽度>=769px */}
@media screen and (min-width: 769px) and (max-width: 1000px){/* 769px < 视窗宽度 < 1000px}

/* 案例：html */
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="right">
        <p>right</p>
        <p>right</p>
    </div>
</div>

/* 案例：css */
.left{float: left; width: 100px; margin-right: 20px;}
.right{overflow: hidden;}
@media screen and (max-width: 320px){
    .left{float: none; width: auto; margin-right: 0; margin-bottom: 20px;}
}
```
### 5.4 页面优化
#### 5.4.1 页面优化的目的
* 提升网页响应速度
* 对搜索引擎、屏幕阅读器友好
* 提高可读性，可维护性

#### 5.4.2 页面优化的方法
##### 5.4.2.a 减少请求
* 图片合并
    - spirit 拼图
* CSS文件合并
    - 多个CSS文件合并为一个
    - 少量CSS样式内联
    - 避免使用@import的方式引入CSS文件

##### 5.4.2.b 减少文件的大小
* 减少图片的大小
    - 选择合适的图片格式（PNG、JPG）
    - 压缩图片（imageOptim、imageAlpha、JPEGmini)
* CSS值缩写
    - `margin`,`padding`,`border`,`border-radius`,`font`,`background`
* 省略值为零的单位
    - `margin: 0 10px;`
* 颜色值最短表示
    - `.m-box{color: #ccc;}`
* CSS选择器合并
    - `.m-logo,.m-list li,.u-tab li a{background:url(../images/sprite.png) no-repeat;}
* 文件压缩
    - YUI Compressor
    - cssmin

##### 5.4.2.c 页面性能
* 加载顺序
    - CSS引入在`head`标签中，JS引入放在`body`标签中最后面；
* 减少标签数量
    - 在HTML中能用两级标签解决的，尽量不要多嵌套一层标签；
* 选择器的长度
    - `class`,`id`选择器的命名，能多短就多短；
* 耗性能属性
    - `expression`:`.class{width:expression(this.width>100?'100px':'auto');}`
    - `filter`:`.class{filter:alpha(opacity=50);}`
    - `border-radius`:`.class{border-radius: 30px;}`
    - `box-shadow`:`.class{box-shadow: 2px 2px 5px #333;}`
    - `gradients`:
* 图片设置宽高
    - `<img src="xx.jpg" alt="alt" width="200xp" height="300px">`
* 所有的表现用css实现
    - 尽量不用js或HTML来渲染页面样式，全部用css最好；

##### 5.4.2.d 可读性，可维护性
* 规范
* 语义化
* 尽量避免Hack
* 模块化
* 注释

### 5.5 规范与模块化
#### 5.5.1 规范
* 目的：开发团队遵循同样的规范，是为了方便开发时的沟通，后期的维护，提高整体的效率；

##### 5.5.1.a 文件规范
* 文件分类
    - 通用类
        - base(reset.css)
        - lib(jQuery-ui.css)
        - ui(editor.css)
    - 业务类（一个音乐网站为例）
        - album
        - artist
        - toplist
- 文件引入
    - 行内样式：不推荐使用
    - 外联引入：`<link rel="stylesheet" href="css/style.css">`
    - 内联引入：`<style></style>`
    - 避免在CSS中使用`@import`
- 文件本身
    - 文件名：比如（由小写字母、数字、中划线组成）
    - 编码： UTF-8

##### 5.5.1.b 注释规范
```html
/* 块状注释
 * 统一缩进、在被注释对象之上
 */
.m-list{width: 500px;}

/* 单行注释文字： 文字两端留空格，在被注释对象之上； */
.m-list li em{color: #333;}

/* 行内注释： 文字两端留空格，在分号之后；*/
.m-list li a{color: #333;/* 对color的行内注释 */}
```
##### 5.5.1.c 命名规范
* 分类命名
```html
<div class="g-header">...</div>
<div class="g-section">
    <div class="m-article">
        <div class="header">...</div>
        ...
    </div>
</div>
<div class="g-footer">...</div>

/* 布局样式 */
.g-header{color:bleack;}

/* 文章样式 */
.m-article{color:blue;}
.m-article .header{font-size:12px;}
```
* 命名格式
```html
/* 建议小写 */
.list{font:20px/1.4 serif;}
.list .title{color:red;}

/* 权衡长度和可读性 */
.subnavigator{font-size:12px;}
/* 缩写名称 */
.subnav{font-size:12px;}
```
* 语义化命名:以内容语义命名
```html
/* 以结构命名 */
.top{font-size:12px;}
.top .link{color:#eee;}

/* 以内容语义命名 */
.nav{font-size:12px;}
.nav .link{color:#333;}
```
##### 5.5.1.d 书写规范
* 单行与多行
```html
/* 单行 */
.logo{width:200px;height:50px;font-size:12px;color:#666;}
.logo a{float:left;padding:0 10px;}

/* 多行 */
.logo{
    width:200px;
    height:50px;
    font-size:12px;
    color:#666;
}
.logo a{
    float:left;
    padding:0 10px;
}
```
* 空格与分号
    - 空格
        - 缩进（必须有） 
            - 2个或者4个
        - 规则内空格
            - `/* css规则内空格 */`
            - `.logo{width: 200px; height: 50px; color: #666;}`
    - 分号
        - 保留最后一个属性值的分号
- 属性顺序
    - 根据属性的重要性按顺序书写：`显示属性`->`自身属性`->`文本属性和其他修饰`
    - 显示属性：`display`,`visibility`,`position`,`float`,`clear`,`list-style`,`top`
    - 自身属性：`width`,`height`,`margin`,`padding`,`border`,`overflow`,`min-width`
    - 文本及修饰属性：`font`,`text-align`,`text-decoration`,`vertical-align`,`white-space`,`color`,`background`
- Hack方式
    - 统一各浏览器的Hack方式
        - IE6  --> `_property:value`
        - IE6/7--> `*property:value`
    - 不要滥用Hack
```html
/* IE7显示#888，IE6显示#fff,其他浏览器显示#000 */
.m-list{color:#000;*color:#888;_color:#fff;}
```
* 统一值格式
    - Color ： `white` or `#fff` or `rgb(255,255,255)`
    - Url() ： `icon.png` or `"icon.png"` or `'icon.png'`

##### 5.5.1.e 其他规范
* HTML规范
    - 文档声明：`<!DOCTYPE html>`,首行顶格开始
    - 闭合
        - 闭合标签：`<div></div>`
        - 自闭合标签：`<input>` or `<input />`
    - 属性
        - `<h1 class="logo"></h1>` or `<h1 class='logo'></h1>`
        - `<input readonly>` or `<input readonly="readonly">`
    - 层级
        - 用缩进体现层级，提高可读性；
        - 标签正确嵌套，但嵌套不宜太深；
    - 注释
        - `<!-- LOGO --><h1 class="logo"><a href="#">LOGO</a></h1><!-- /LOGO -->`
    - 大小写
        - 标签、属性均小写
* 图片规范
    - 文件名称
        - 语义
        - 长度
    - 保留源文件
    - 图片合并
        - 尽可能使用sprite技术
        - sprite图片可按模块、业务、页面来划分

#### 5.5.2 模块化
##### 5.5.2.a 什么是模块儿化
* 以模块分类命名（如：m-,md-)
* 以一个主选择器开头（模块根节点）
* 使用主选择器开头的后代选择器（模块儿子节点）
```html
<!-- 模块1 -->
<mk1>
    <zys1></zys1>
    <zys2></zys2>
</mk1>

/* 模块1 */
mk1{}
mk1 zys1{}
mk1 zys2{}
```
##### 5.5.2.b 怎么模块儿化
* 模块化
```html
<!-- NAV MODULE -->
<div class="m-nav">
    <ul>
        <li class="z-crt"><a href="#">index</a></li>
        <li><a href="#">link1</a><li>
    </ul>
</div>
<!-- /NAV MODULE -->

/* 导航模块 */
.m-nav{}/* 模块容器 */
.m-nav li,.m-nav a{}
.m-nav li{}
.m-nav a{}
.m-nav .z-crt a{}/* 交互状态变化 */
```
* 模块的扩展
```html
<!-- NAV-1 MODULE -->
<div class="m-nav m-nav-1">
    <ul>
        <li class="z-crt"><a href="#">index</a><li>
        <li><a href="#">link1</a></li>
    </ul>
    <a class="btn">Login</a>
</div>
<!-- /NAV-1 MODULE -->

/* 导航模块扩展 */
.m-nav{}
.m-nav-1{}
.m-nav-1 a{bordre-radius:5px;}
.m-nav-1 .btn{}
```
##### 5.5.2.c 模块儿化好处
* 利于多人协同开发
* 便于扩展和重用
* 可读性、可维护性好

##### 5.5.2.d 模块儿化案例

---
[5.1]:https://github.com/Wanlin-Lu/Front-end-knowledge-summary/blob/master/HCJD/5.Web-architecture.md#51-css-reset
[5.2]:https://github.com/Wanlin-Lu/Front-end-knowledge-summary/blob/master/HCJD/5.Web-architecture.md#52-布局解决方案
[5.3]:https://github.com/Wanlin-Lu/Front-end-knowledge-summary/blob/master/HCJD/5.Web-architecture.md#53-响应式布局
[5.4]:https://github.com/Wanlin-Lu/Front-end-knowledge-summary/blob/master/HCJD/5.Web-architecture.md#54-页面优化
[5.5]:https://github.com/Wanlin-Lu/Front-end-knowledge-summary/blob/master/HCJD/5.Web-architecture.md#55-规范与模块化
