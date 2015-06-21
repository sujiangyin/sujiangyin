1、怎么让某个按妞成为提交按钮，其他的过滤
```js
http://formvalidation.io/examples/skip-validation-specific-submit-button/
button: {
    // The CSS selector indicates the button
    selector: '[type="submit"]:not([formnovalidate])',

    // The CSS class which is added to the button when it is disabled
    disabled: ''
}
最简单的方法
使用表单验证属性
<!-- Validate and submit if the form is valid -->
<button type="submit" class="btn btn-primary">Submit</button>

<!-- Skip validation and just submit the form -->
<button type="submit" class="btn btn-primary" formnovalidate>Just submit</button>
但要看版本而定。
另外一种方法，这个会验证这个button而不会忽略：
$('#employmentForm').formValidation({
    button: {
        selector: '#validateButton',
        disabled: 'disabled'
    }
    ...
});
```
```js
不要让FontAwesome和bootstrao一起使用，如果非要使用，就要
resolve it by placing the FontAwesome CSS before Bootstrap CSS:

<!-- Load FontAwesome CSS before Bootstrap -->
<link rel="stylesheet" href="/vendor/font-awesome/css/font-awesome.min.css" />
<link rel="stylesheet" href="/vendor/bootstrap/css/bootstrap.min.css"/>
```
```js
filed之所以可以在改变的任何时候去验证是因为有live：
enabled default 	The plugin validates fields as soon as they are changed
默认可以现场验证。
```
```js

message

message: String — The default error message for all fields. You can specify the error message for any fields.
是给所有filed的。
```
```js
为什么要加row，因为有多个的field的时候，同个row里面得到验证正确的filed可以有成功的颜色。
http://formvalidation.io/examples/complex-form/
它还可以设置成功和不成功验证的样式。
```
```js
输入的长度不超过某个值的时候不验证threshold
地址http://formvalidation.io/settings/#field-threshold
```
```js
http://formvalidation.io/settings/#field-trigger
trriger默认是对所有filed起作用，当然放在filed里就是对个别起作用而已。
它可以决定什么时候启动验证。
```
```js
http://formvalidation.io/validators/remote/#using-verbose-option
verbose 
它可以决定当有很多个验证在同一个field的时候，一个发生错误时还要不要继续验证下去。
```
