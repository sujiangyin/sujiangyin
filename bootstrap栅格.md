发现网页过程中很多可以用bootstrap轻松解决的都没有使用，包括标签，按钮，表格，缩略图，nav，和navbars，list group，panels等等。
```js
栅格系统：
占满行无需加栅格列的类。
Full width, single column
No grid classes are necessary for full-width elements.
```
```js

Each tier of classes scales up, meaning if you plan on setting the same widths for xs and sm,
you only need to specify xs.

.col-xs-12 .col-md-8
.col-xs-6 .col-md-4
移动设备和desktop，以上每行等价，如果你想要实现xs和md的宽度效果，只要设定xs的具体值就可以。
但一般不这么用，使用方法看下面。

比如，要实现在移动设备上占一列，在平板上占两列，在桌面上占4列：
<div class="row">
    <div class="col-md-3 col-sm-6 col-xs-12">Col 1</div>
    <div class="col-md-3 col-sm-6 col-xs-12">Col 2</div>
    <div class="clearfix visible-sm"></div>
    <div class="col-md-3 col-sm-6 col-xs-12">Col 3</div>
    <div class="col-md-3 col-sm-6 col-xs-12">Col 4</div>
</div>

为什么要加<div class="clearfix visible-sm"></div>？
因为如果上面的四个子块高度不一样时，布局就会break了。
加的规则：xs，md、和lg超过12就加对应的clearfix visible-
看下面改进：
<div class="container"> 
<div class="row">
<div class="col-lg-3 col-md-4 col-sm-6" style="background-color:blue;">1</div>
<div class="col-lg-3 col-md-4 col-sm-6" style="background-color:red;height:100px;">2</div>
<div class="clearfix visible-sm"></div>
<div class="col-lg-3 col-md-4 col-sm-6" style="background-color:yellow;">3</div>
<div class="clearfix visible-md"></div>
<div class="col-lg-3 col-md-4 col-sm-6" style="background-color:green;">4</div>
<div class="clearfix visible-sm visible-lg"></div>
<div class="col-lg-3 col-md-4 col-sm-6" style="background-color:blue;height:100px;">1</div>
<div class="col-lg-3 col-md-4 col-sm-6" style="background-color:red;">2</div>
<div class="clearfix visible-sm visible-md"></div>
<div class="col-lg-3 col-md-4 col-sm-6" style="background-color:yellow;">3</div>
<div class="col-lg-3 col-md-4 col-sm-6" style="background-color:green;">4</div>
<div class="clearfix visible-sm visible-lg"></div>
</div>
</div>


这种<div class="clearfix visible-sm visible-lg"></div>只是清除lg和sm的浮动，也就是在lg和sm屏幕这个元素是可见的，但在中等屏幕它是隐藏的。
栅格是水平还是堆叠以没有被清除浮动的md为准。
```
```js
栅格类适用于与屏幕宽度大于或等于分界点大小的设备 ， 并且针对小屏幕设备覆盖栅格类。

 因此，在元素上应用任何 .col-md-* 栅格类适用于与屏幕宽度大于或等于分界点大小的设备 ， 并且针对小屏幕设备覆盖栅格类。

 因此，在元素上应用任何 .col-lg-* 不存在， 也影响大屏幕设备。

也就是如果写的是md，那么在小于这个分界点min-width就不会起作用，
大于max-width也不会起作用。

所以当小于这个屏幕宽度时会堆叠在一起，大于或者说在这个范围中才会水平排列。

但对于超小屏幕是水平排列的，它的宽度是自动的。

```
```js
col-md-offset-1 col-md-11之所以要一套使用是因为col-md-offset-1是通过对col-md-11加左边的margin。


嵌套列

为了使用内置的栅格系统将内容再次嵌套，可以通过添加一个新的 .row 元素和一系列 .col-sm-* 元素到已经存在的 .col-sm-* 元素内。被嵌套的行（row）所包含的列（column）的个数不能超过12（其实，没有要求你必须占满12列）
```
```js
列排序

通过使用 .col-md-push-* 和 .col-md-pull-* 类就可以很容易的改变列（column）的顺序。
.col-md-9 .col-md-push-3
.col-md-3 .col-md-pull-9
复制

<div class="row">
  <div class="col-md-9 col-md-push-3">.col-md-9 .col-md-push-3</div>
  <div class="col-md-3 col-md-pull-9">.col-md-3 .col-md-pull-9</div>
</div>
```


