发现网页过程中很多可以用bootstrap轻松解决的都没有使用，包括标签，按钮，表格，缩略图，nav，和navbars，list group，panels等等。
```js
栅格系统：
占满行无需加栅格列的类。
Full width, single column
No grid classes are necessary for full-width elements.
```
```js
Each tier of classes scales up, meaning if you plan on setting the same widths for xs and sm, you only need to specify xs.
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
```
```js
栅格类适用于与屏幕宽度大于或等于分界点大小的设备 ， 并且针对小屏幕设备覆盖栅格类。 因此，在元素上应用任何 .col-md-* 栅格类适用于与屏幕宽度大于或等于分界点大小的设备 ， 并且针对小屏幕设备覆盖栅格类。 因此，在元素上应用任何 .col-lg-* 不存在， 也影响大屏幕设备。

也就是如果写的是md，那么在小于这个分界点min-width就不会起作用，大于max-width也不会起作用。所以当小于这个屏幕宽度时会堆叠在一起，大于或者说在这个范围中才会水平排列。
```



