p元素在字体下加了margin。

```js
对齐

通过文本对齐类，可以简单方便的将文字重新对齐。

<p class="text-left">Left aligned text.</p>
<p class="text-center">Center aligned text.</p>
<p class="text-right">Right aligned text.</p>
<p class="text-justify">Justified text.</p>
<p class="text-nowrap">No wrap text.</p>
```

```js
改变大小写

通过这几个类可以改变文本的大小写。

<p class="text-lowercase">Lowercased text.</p>
<p class="text-uppercase">Uppercased text.</p>
<p class="text-capitalize">Capitalized text.</p>
```

```js
缩略语,自己去看bootstrap。
```
```js
无样式列表
移除了默认的 list-style 样式和左侧外边距的一组元素（只针对直接子元素）。这是针对直接子元素的，也就是说，你需要对所有嵌套的列表都添加这个类才能具有同样的样式。
<ul class="list-unstyled">
  <li>...</li>
</ul>
```
```js
内联列表

通过设置 display: inline-block; 并添加少量的内补（padding），将所有元素放置于同一行。

    Lorem ipsum Phasellus iaculis Nulla volutpat 

<ul class="list-inline">
  <li>...</li>
</ul>
```

```js
描述

带有描述的短语列表。

Description lists
    A description list is perfect for defining terms.
Euismod
    Vestibulum id ligula porta felis euismod semper eget lacinia odio sem nec elit.
    Donec id elit non mi porta gravida at eget metus.
Malesuada porta
    Etiam porta sem malesuada magna mollis euismod.

复制

<dl>
  <dt>...</dt>
  <dd>...</dd>
</dl>
(
可以有多个dt和dd，但dl才设置了下面的margin。)

水平排列的描述

.dl-horizontal 可以让 <dl> 内的短语及其描述排在一行。开始是像 <dl> 的默认样式堆叠在一起，随着导航条逐渐展开而排列在一行。

Description lists
    A description list is perfect for defining terms.
Euismod
    Vestibulum id ligula porta felis euismod semper eget lacinia odio sem nec elit.
    Donec id elit non mi porta gravida at eget metus.
Malesuada porta
    Etiam porta sem malesuada magna mollis euismod.
Felis euismod semper eget lacinia
    Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa justo sit amet risus.

复制

<dl class="dl-horizontal">
  <dt>...</dt>
  <dd>...</dd>
</dl>

自动截断

通过 text-overflow 属性，水平排列的描述列表将会截断左侧太长的短语。在较窄的视口（viewport）内，列表将变为默认堆叠排列的布局方式。
```
