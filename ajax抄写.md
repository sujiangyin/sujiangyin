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











