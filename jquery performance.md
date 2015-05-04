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
