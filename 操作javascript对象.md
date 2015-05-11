1、json数据的好处是可以节约代码，减少数据传输量。
   使用的限制：
   所有对象键和字符串值必须保存在双引号中，而且，函数也不是有效的json值。
 2、将json保存在一个json文件中：
如下：
```js
[
{
"terms":"dfmk",
”bfj" :"ffmfgf",
"dffk":"fkemf",
"melfm":[
"bhef",
"mfkef",
"vfved",
"nfje"
],
"sjfbjebrf":"jfnfjrebffdfu"
},
"terems":"dfefgmk",
”bfgj" :"ffmfffegf",
"dwqeffk":"fkegqmf",
"meldffm":[
"bhewef",
"mfkf2qef",
"vfvfwwed",
"nfjqr2e"
],
"sjfbjefeb":"jfnfjreefwbffu"
}
]
```
要取得这些数据，要用$getJSON()方法，这个方法会在取得这些数据后，对数据进行处理。在数据从服务器返回后，它只是一个简单的json格式的文本字符串。$.getJSON()方法会解析这个字符串，并将处理得到的javascript对象提供给调用代码。

使用全局函数：
用别名$.的方法是一个jquery库定义的方法，是全局函数。

使用：
```js
$(document).ready(function(){
$('#letter-b a').click(function(event){
event.preventDefault();
$getJSON('b.json');
});
});
```
但是返回的数据要怎么处理呢？使用回调函数,第二个参数，用返回数据作为函数的参数：
```js
$(document).ready(function(){
$('#letter-b a').click(function(event){
event.preventDefault();
$getJSON('b.json'，function(data){
处理程序。。。。
});
});
});
```

3、在处理程序里面可能会使用$.each()这个实用全局函数：
该函数不操作jquery对象，他以数组或者对象作为第一个参数，以回调函数作为第二个参数。此外，还需要将每次迭代的数组或对象的当前索引和当前项作为回调函数的两个参数。见下：
```js
$(document).ready(function(){
$('#letter-b a').click(function(event){
event.preventDefault();
$getJSON('b.json'，function(data){
$.each(data,function(entryIndex,entry){
});
});
});
});
```

4、执行脚本

在页面中注入脚本与加载html片段一样。在这里，可以使用全局函数$.getScript(),使用方法：
```js
$(document).ready(function(){
$('#letter-b a').click(function(event){
event.preventDefault();
$.getScript('c.js');
});
});
```
以这种方式取得的脚本会在当前页面的全局环境下执行。即是，脚本有权访问在全局环境中定义的函数和变量，当然也包括jquery自身。
