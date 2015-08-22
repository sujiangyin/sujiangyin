```js
1、通过Ajax生成页面内容时是一个常见的问题。对此，一种常见的解决方案是：
在页面内容更新时重新绑定处理程序，但这样相当繁琐。
另外一种值得推荐的方法是事件委托：
本质是把事件处理程序绑定到一个祖先元素，而这个元素始终不能变。可以使用.on()方法绑定。
这样即使解释内容是通过Ajax后来添加到文档中的也没问题。
```
绑定ajax时事件：
```js
// Setting up a loading indicator using Ajax Events
$( "#loading_indicator" )
    .ajaxStart(function() {
        $( this ).show();
    })
    .ajaxStop(function() {
        $( this ).hide();
    });
```
