笔记链接地址：https://learn.jquery.com/plugins/advanced-plugin-concepts/。
定义我们的插件：
```js
// Plugin definition.
$.fn.hilight = function( options ) {
 
    // Extend our default options with those provided.
    // Note that the first argument to extend is an empty
    // object – this is to keep from overriding our "defaults" object.
    var opts = $.extend( {}, $.fn.hilight.defaults, options );
 
    // Our plugin implementation code goes here.
 
};
 
// Plugin defaults – added as a property on our plugin function.
$.fn.hilight.defaults = {
    foreground: "red",
    background: "yellow"
};
```
在script里面修改插件的默认属性：
```js
// This needs only be called once and does not
// have to be called from within a "ready" block
$.fn.hilight.defaults.foreground = "blue";
```
现在的调用就是设置以后的新的默认属性：
```js
$( "#myDiv" ).hilight();
```
可以通过给插件传递参数覆盖新设置的默认属性：
```js
// Override plugin default foreground color.
$.fn.hilight.defaults.foreground = "blue";
 
// ...
 
// Invoke plugin using new defaults.
$( ".hilightDiv" ).hilight();
 
// ...
 
// Override default by passing options to plugin method.
$( "#green" ).hilight({
    foreground: "green"
});
```

