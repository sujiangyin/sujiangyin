1、默认提示等表单验证带有的
```js
<form role="form" data-toggle="validator">
  <div class="form-group">
    <label for="inputEmail">Email</label>
    <input type="email" id="inputEmail">
    <div class="help-block with-errors"></div>
  </div>
</form>
```
2、命名要有语义化

3、后台中文，前台英文

4、阻止事件的默认行为：
```js
$("a").click(function(event){ event.preventDefault(); }); 
```
