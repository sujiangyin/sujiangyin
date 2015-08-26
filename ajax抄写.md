$ajax（)的转换器支持的数据类型映射到其他数据类型。但是，如果你想把自定义的数据类型映射到已知的数据类型比如json，您必须contents选项在响应的content-type和实际的数据类型之间添加一个相关的转换函数。
```js
$ajaxSetup({
contents: {
   mycustomtype:/mycustomtype/
},
converters: {
   "mycustomtype json":function(result){
   //do stuff
   return newresult;
}
}
});
```
这额外的对象是必要的，因为响应内容类型和数据类型从来没有一个严格的一对一的关系。


转换一个支持的类型成自定义的数据类型，然后再返回，使用另一个直通转换器：
```js
$ajaxSetup({
   contents： ｛
   mycustomtype: /mycustomtype/
｝，
converters: {
   "text mycustomtype": true,
  "mycustomtype json":function(result){
  //do stuff
   return newresult;
}
}
});
```
现在上面的代码允许text类型转换成自定义的类型，进而，自定义类型转换成json数据类型。



保存数据到服务器，成功时返回信息：
$ajax({
    type:"post",
    url: "some.php",
   data: {name:"john",location:"Boston"}
}).done(
alert("done")
)；



post方法：
ajax函数简写：
```js
$.ajax({
  type: "POST",(post请求)
  url: url,（请求的url地址）
  data: data,(发送给服务器的数据)
  success: success,（请求成功后执行的回调函数）
  dataType: dataType（预期返回的数据类型）
});
```

success回调函数在1.5的jq开始将指定一个成功的处理函数:
```js
   $post("test.html",function(data）｛
       $('.result').html(data);
｝)；
```
将请求的所有html代码插入到页面中。
另外post方法请求得到的页面是不会缓存的。



w3cschool ajax
实例解释：
```js
<html>
<body>

<div id="myDiv"><h3>Let AJAX change this text</h3></div>
<button type="button" onclick="loadXMLDoc()">Change Content</button>

</body>
</html>
```
上面这个实例包括一个div和一个按钮，div用来显示来自服务器的信息。当按钮被点击时，它负责调用名为loadXMLdoc（）的函数：
如下：
```js
<head>
<script type="text/javascript">
function loadXMLDoc()
{
.... AJAX script goes here ...
}
</script>
</head>
```
向服务器发送请求
如需将请求发送到服务器，我们使用xmlhttprequest对象的open（）和send（）方法：
```js
 xmlhttp.open("GET","test1.txt",true);
 xmlhttp.send();
```
open方法:
open(method,url,async)
规定请求的类型、url以及是否异步处理请求。
请求的类型get和post方法；url是文件在服务器上的位置；true为异步，false为同步。
send(string)将请求发送到服务器。string仅用于post请求。

与post相比，get更简单更快，在大部分情况下都能用。
然而，在以下情况要使用post：
1、无法使用缓存文件（更新服务器上的文件或数据库）
2、向服务器上发送大量数据（post没有数据量限制）
3、发送包含未知字符的用户输入时，post比get更稳定可靠。

get请求，一个简单的get请求：
```js
xmlhttp.open("GET","demo_get.asp",true);
xmlhttp.send();
```
上面的结果你可能得到的是缓存结果。为了避免这种情况，请向url添加一个唯一的id：
```js
xmlhttp.open("GET","demo_get.asp?t=" + Math.random(),true);
xmlhttp.send();
```
如果你希望通过get方法发送信息，请向url添加信息：
```js
xmlhttp.open("GET","demo_get2.asp?fname=Bill&lname=Gates",true);
xmlhttp.send();
```
一个简单post请求：
```js
xmlhttp.open("POST","demo_post.asp",true);
xmlhttp.send();
```

如果需要像html表单那样post数据，请使用setRequestHeader（）来添加http头。然后在send（）方法中规定您希望发送的数据：
```js
xmlhttp.open("post","ajax_test.asp",true);
xmlhttp.setRequestHeader("content-type","application/x-www-form-urlencoded");
xmlhttp.send("fname=Bill&lnmae=Gates");

setRequestHeader(header,value)
header规定头的名称，value规定头的值。
```

url-服务器上的文件

open（）方法的url参数是服务器上文件的地址：
xmlhttp.open("GET",ajax_test.asp",true);
该文件可以是任何类型的文件。

异步-True或false;
ajax指的是异步javascript和xml。
XMLHttpRequest对象如果要用于ajax的话，其open（）方法的asnc参数必须设置为true：
```js
xmlhttp.open("GET","ajax_test.asp",true);
```
对于web开发人员来说，发送异步请求是一个巨大的进步。很多在服务器执行的任务都相当的费时。ajax出现之前，这可能会引起应用程序挂起或停止。
通过ajax，javascript无需等待服务器的响应，而是：
在等待服务器响应时执行其他脚本
在响应就绪后对响应进行处理

async=true
当使用async=true时，请规定在响应处于onreadystatechange事件中的就绪状态时执行的函数：
```js
xmlhttp.onreadystatechange=function()
  {
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
    document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    }
  }
xmlhttp.open("GET","test1.txt",true);
xmlhttp.send();
```

Async = false

如需使用 async=false，请将 open() 方法中的第三个参数改为 false：

xmlhttp.open("GET","test1.txt",false);

我们不推荐使用 async=false，但是对于一些小型的请求，也是可以的。

请记住，JavaScript 会等到服务器响应就绪才继续执行。如果服务器繁忙或缓慢，应用程序会挂起或停止。

注释：当您使用 async=false 时，请不要编写 onreadystatechange 函数 - 把代码放到 send() 语句后面即可：
```js
xmlhttp.open("GET","test1.txt",false);
xmlhttp.send();
document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
```







