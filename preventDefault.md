可以阻止事件的默认行为：
1.a标签默认跳转
```js
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>event.preventDefault demo</title>
  <script src="https://code.jquery.com/jquery-1.10.2.js"></script>
</head>
<body>
 
<a href="http://jquery.com">default click action is prevented</a>
<div id="log"></div>
 
<script>
$( "a" ).click(function( event ) {
  event.preventDefault();
  $( "<div>" )
    .append( "default " + event.type + " prevented" )
    .appendTo( "#log" );
});
</script>
 
</body>
</html>
```
结果显示
```js
default click action is prevented
default click prevented
```
2、submit按钮默认提交表单
```js
该方法将通知 Web 浏览器不要执行与事件关联的默认动作（如果存在这样的动作）。
例如，如果 type 属性是 "submit"，在事件传播的任意阶段可以调用任意的事件句柄
，通过调用该方法，可以阻止提交表单。注意，如果 Event 对象的 cancelable 属性是 fasle，
那么就没有默认动作，或者不能阻止默认动作。无论哪种情况，调用该方法都没有作用。
```
