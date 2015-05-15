在本地窗口和新窗口打开页面的方法：
```js
超链接<a href="http://www.jb51.net" title="脚本之家">Welcome</a>
等效于js代码
window.location.href="http://www.jb51.net";     //在同当前窗口中打开窗口
超链接<a href="http://www.jb51.net" title="脚本之家" target="_blank">Welcome</a>
等效于js代码
window.open("http://www.jb51.net");                 //在另外新建窗口中打开窗口
```
