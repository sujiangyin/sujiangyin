1、在循环外面去追加元素
去触碰DOM元素的耗费是很大的.如果你往DOM里追加大量的元素，你可能想要一次追加全部而不是一次追加一个。当在一个循环里面追加元素时这是个问题。

```js
$.each( myArray, function( i, item ) {
 
    var newListItem = "<li>" + item + "</li>";
 
    $( "#ballers" ).append( newListItem );
 
});
```
2、Don’t Act on Absent Elements。
不要在空元素上操作：
坏的：
```js
/ Bad: This runs three functions before it
// realizes there's nothing in the selection
$( "#nosuchthing" ).slideUp();
```
好一点的：
```js
// Better:
var elem = $( "#nosuchthing" );
 
if ( elem.length ) {
 
    elem.slideUp();
 
}
```
最好的：
```js
// Best: Add a doOnce plugin.
jQuery.fn.doOnce = function( func ) {
 
    this.length && func.apply( this );
 
    return this;
 
}
 
$( "li.cartitems" ).doOnce(function() { 
 
    // make it ajax! \o/ 
 
});
```
3、优化选择器
选择器优化没有之前那么重要了，因为很多浏览器完善了document.querySelectorAll()文件，jquery的选择器对于浏览器的反应速度也加快了。但是，还是有些tips需要我们注意：

3.1、选择器以id开头会更好：
```js
// Fast:
$( "#container div.robotarm" );
 
// Super-fast:
$( "#container" ).find( "div.robotarm" );
```
解释！因为第一种会次用sizble的方式搜索（就是先找所有的子节点div.robotarm,再去找匹配的父节点）；而第二种会先找父节点的id，再通过find找子节点。

3.2、具体化问题：
```js
// Unoptimized:
$( "div.data .gonzalez" );
 
// Optimized:
$( ".data td.gonzalez" );
```
最好在右边具体化（写tag.class），左边不具体化（只写类）.

3.3、避免使用普遍选择器
那种明确或者暗含在哪里都可以找的选择器是很慢的：
```js
$( ".buttons > *" ); // Extremely expensive.
$( ".buttons" ).children(); // Much better.
 
$( ".category :radio" ); // Implied universal selection.
$( ".category *:radio" ); // Same thing, explicit now.
$( ".category input:radio" ); // Much better.
```
4、有一种情况：当你想要在很多很多个元素上去改变元素的css样式，用jquery就要去找很多个元素，很慢。应该去改变它的样式表：
```js
// Fine for up to 20 elements, slow after that:
$( "a.swedberg" ).css( "color", "#0769ad" );
 
// Much faster:
$( "<style type=\"text/css\">a.swedberg { color: #0769ad }</style>")
    .appendTo( "head" );
```


