让列往左右对齐或者居中：
```js
<html>
<body>

<table width="100%" border="1">
  <colgroup span="2" align="left"></colgroup>
  <colgroup align="right" style="color:#0000FF;"></colgroup>
  <tr>
    <th>ISBN</th>
    <th>Title</th>
    <th>Price</th>
  </tr>
  <tr>
    <td>3476896</td>
    <td>My first HTML</td>
    <td>$53</td>
  </tr>
  <tr>
    <td>2489604</td>
    <td>My first CSS</td>
    <td>$47</td>
  </tr>
</table>

</body>
</html>

```
效果：
http://www.w3school.com.cn/tiy/t.asp?f=html_colgroup_test
所有主流浏览器都支持 <colgroup> 标签。
Firefox、Chrome 以及 Safari 仅支持 colgroup 元素的 span 和 width 属性。
