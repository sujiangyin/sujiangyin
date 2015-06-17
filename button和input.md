一、定义和用法

<button> 标签定义的是一个按钮。

在 button 元素内部，可以放置文本或图像。这是<button>与使用 input 元素创建的按钮的不同之处。

二者相比较， <button> 控件提供了更为强大的功能和更丰富的内容。<button> 与 </button> 标签之间的所有内容都是按钮的内容，其中包括任何可接受的正文内容，比如文本或多媒体内容。例如，我们可以在按钮中包括一个图像和相关的文本，用它们在按钮中创建一个吸引人的标记图像。

唯一禁止使用的元素是图像映射，因为它对鼠标和键盘敏感的动作会干扰表单按钮的行为。

请始终为按钮规定 type 属性。Internet Explorer 的默认类型是 "button"，而其他浏览器中（包括 W3C 规范）的默认值是 "submit"。

二、浏览器支持

所有主流浏览器都支持 <button> 标签。

重要事项：如果在 HTML 表单中使用 button 元素，不同的浏览器会提交不同的值。Internet Explorer 将提交 <button> 与 <button/> 之间的文本，而其他浏览器将提交 value 属性的内容。请在 HTML 表单中使用 input 元素来创建按钮。

三、注意事项

在使用<button>标签时很容易想当然的当成 <input type="button">使用，这很容易产生以下几点错误用法：

 1、通过$('#customBtn').val()获取<button id="customBtn" value="test">按钮</button> value的值

     在IE(IE内核)下这样用到得的是值是“按钮”，而不是“test”，非IE下得到的是“test”。 参加上面标红的第一句话。

　　这一点要和<input type="button">区分开。                         

     通过这两种方式$('#customBtn').val()，$('#customBtn').attr('value')在不同浏览器的获得值，如下： 

Browser/Value
	

$('#customBtn').val()
	

$('#customBtn').attr('value')

Firefox13.0
	

test
	

test

Chrome15.0
	

test
	

test

Opera11.61
	

test
	

test

Safari5.1.4
	

test
	

test

IE9.0
	

按钮
	

按钮

 验证这一点可以在测试下面的代码 

 
Java代码 

    <html>  
     <head>  
      <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />  
      <script type="text/javascript" src="jquery-1.4.4.min.js"></script>  
      <script type="text/javascript">  
        $(function() {  
            $('#test1').click(function() {  
                alert($('#customBtn').attr('value'));      
            });  
            $('#test2').click(function() {  
                alert($('#customBtn').val());      
            });  
        });  
      </script>  
     </head>  
     <body>  
        <button id="customBtn" value="test">&#x6309;&#x94AE;</button>   
        <input type="button" id="test1" value="get attr"/>  
        <input type="button" id="test2" value="get val"/>  
     </body>  
    </html>  

 2、无意中把<button>标签放到了<form>标签中，你会发现点击这个button变成了提交，相当于<input type="submit"/>

 

    这一点参见上面第二句标红的话就明白什么意思了。

    不要把<button>标签当成<form>中的input元素。

    验证这一点可以在测试下面的代码 

 
Java代码 

    <html>  
    <body>  
        <form action="">  
            <button> button </button>  
            <input type="submit" value="input submit"/>  
            <input type="button" value="input button"/>  
        </form>  
    </body>  

```js
两种input组件：button和submit的区别
button默认是啥都不做的，必须要在button上写js才能触发各种动作。

submit默认即使啥js都不写，也会提交form（准确的说是触发onsubmit事件）。当然你也可以在上面写js做点其他什么事儿。

button上写入onclick事件，只能通过鼠标点击触发。要用键盘触发的话，需要再写个onkeypress事件。

submit则除了鼠标之外，还可以通过键盘上的Enter触发。
```
