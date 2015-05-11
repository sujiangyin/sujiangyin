1、有一个div：
```js
<div id="dictionary">
</div>
```
现在要通过Ajax加载一个html片段到它的里面，当点击某个链接的时候跳转到这个片段。
假设它们存在a.html里面（只有html的div片段，或者其他标签）
加载方法：
```js
$(document).ready(function(){
$('#letter-a a").click(function(event)){
event.preventDefault();
$('#dictionary').load('a.html');
});
});
```
我们在上面的代码最后加入一个弹出框，是不是弹出就意味着文件加载完成了呢？因为javascript是严格按照顺序执行的啊！
```js
$(document).ready(function(){
$('#letter-a a").click(function(event)){
event.preventDefault();
$('#dictionary').load('a.html');
alert('ddddd');
});
});
```
其实不是，所有Ajax请求在默认情况下都是异步的，异步加载意味着在发出取得html片段的http请求后，会立即恢复脚本执行，无需等待。在之后的某个时刻，当浏览器收到服务器的响应时，再对响应的数据进行处理。
对于如果非要延迟加载完成才能继续的操作，可以使用回调函数。
