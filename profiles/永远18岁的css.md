## oblique字体和italic字体在css样式中的差别
* 区别
	* italic: 指的是一种单独的字体风格，对每个字母的结构有一些小改动，来反映外观的变化,不一定每种字体都有这种风格
	* oblique: 指的是将正常竖直文本倾斜
* 使用
	* italic：斜体，对于没有斜体变量的特殊字体，将应用oblique
	* oblique：倾斜的字体

## 如何让一段文字强制换行(适用于pre等标签)
#### 第一种方案（适合webkit内核的浏览器或移动端浏览器）
* HTML
	* 使用&lt;pre&gt;&lt;/pre&gt;包裹要换行的内容 
* css
	* 添加css

```  
white-space: pre-wrap!important;  
word-wrap: break-word!important;  
*white-space:normal!important;  
```

## 多行文本溢出显示省略号
#### 方案一
* 一个不规范的css属性 ：-webkit-line-clamp 
	* 限制在一个块元素显示的文本的行数，为了实现该效果，需要组合其他webkit属性
* 相关属性
	* display: -webkit-box; 必须结合的属性 ，将对象作为弹性伸缩盒子模型显示  
	* -webkit-box-orient 必须结合的属性 ，设置或检索伸缩盒对象的子元素的排列方式 。
	* text-overflow，可以用来多行文本的情况下，用省略号“...”隐藏超出范围的文本 。
* 完整css

```
  overflow : hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
```
#### 方案二
* 设置相对定位的容器高度，用包含省略号（。。。）的元素模拟实现

```
p { 
	position:relative;
	line-height:1.4em;
	/* 3 times the line-height to show 3 lines */
	height:4.2em;
	overflow:hidden;
	}
p::after {
	content:"...";
	font-weight:bold;
    position:absolute;
    bottom:0;
    right:0;
    padding:0 20px 1px 45px;
    background:url(http://newimg88.b0.upaiyun.com/newimg88/2014/09/ellipsis_bg.png) repeat-y;
}
```
* 注：
	1. height高度真好是line-height的3倍
	2. 结束的省略好用了半透明的png做了减淡的效果，或者设置背景颜色；
	3. IE6-7不显示content内容，所以要兼容IE6-7可以是在内容中加入一个标签，比如用`<span class="line-clamp">...</span>`去模拟；
	4. 要支持IE8，需要将::after替换成:after；

#### 方案三
* 利用float元素进行浮动

```
// HTML
<div class="css-test-phone">
    <div class="css-overflow">
        <div class="demo">
            <div class="text">
                这是一段较长的文字一二三四五六七八九十这是一段较长的文字一二三四五六七八九十
                这是一段较长的文字一二三四五六七八九十这是一段较长的文字一二三四五六七八九十
                这是一段较长的文字一二三四五六七八九十这是一段较长的文字一二三四五六七八九十
                这是一段较长的文字一二三四五六七八九十这是一段较长的文字一二三四五六七八九十
                这是一段较长的文字一二三四五六七八九十这是一段较长的文字一二三四五六七八九十
            </div>
        </div>
    </div>    
</div>

// CSS
.demo {
    max-height: 80px;
    line-height: 20px;
    text-align: left;
    background-color: #099;
    overflow: hidden;
}
.demo::before{
    float: left;
    content:'';
    width: 20px;
    height: 80px;
}
.demo .text {
    float: right;
    width: 100%;
    margin-left: -20px;
    word-break: break-all;
}
.demo::after{
    float:right;
    content:'...';
    width: 20px;
    height: 20px;
    position: relative;
    left:100%;
    background-color: red;
    transform: translate(-100%,-100%);
}
```

## 清除浏览器的默认蓝色选中框
* `outline: none;`

## 英文字符串自动折行
* `word-wrap: break-word;`

## 改变浏览器网页内容鼠标选中效果
* 代码实现

```
::selection {
	color:#ff0000;
}
::-moz-selection {
	color:#ff0000;
}
::-webkit-selection {
	color:#ff0000;
}
```
* 官方定义和用法
	* `::selection` 选择器匹配元素中被用户选中或处于高亮状态的部分。
	* `::selection` 只可以应用于少数的CSS属性：color, background, cursor,outline
* 浏览器支持
	* IE9+, Opera, Google Chrome和Safari支持 `::selection` 选择器
	* Firefox 通过其私有属性 `::-moz-selection` 支持

## 手机浏览器支持弹性滚动
* iOS页面非body元素的滚动操作会非常卡(Android不会出现此情况)，通过 `overflow-scrolling:touch` 调用Safari原生滚动来支持弹性滚动，增加页面滚动的流畅度
* 代码实现

```
body {
    -webkit-overflow-scrolling: touch;
}
.elem {
    overflow: auto;
}
```

## CSS 绝对定位的基准点
* 设置了 `top` 或 `left`  相对于border以内，padding以外为基准点
* 未设置 `top`或 `left` 相对于relative定位的content