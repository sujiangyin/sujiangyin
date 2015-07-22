html:
```js
<div class="selected-address ship-block" id="selected-addr">
									<!-- handlebars load here -->
</div>
```
js:(把json数据data利用模版shipAddress进行渲染，得到html代码，再将代码插入到html代码的某个id中。)
```js
/*AJAX*/
	$.getJSON('./js/json/ship_address.json', function(data) {
		var html = Handlebars.templates.shipAddress(data);
		$('#selected-addr').html(html);
	});
```
json文件：（保存在js的json文件里，路径如上所示）
```js
{
	"addrDetail": "12312",
	"city": "France",
	"country": "France",
	"deliveraddrId": 22,
	"isDefault": false,
	"name": "1231211",
	"phone": "12312",
	"region": "France",
	"state": "France",
	"userId": 1,
	"zipcode": "12312"
}
```
模版：
```js
<div class="selected-address">
	<dl class="dl-horizontal">
		<dt>Receiver:</dt>
		<dd>{{name}}</dd>
		<dt>Address:</dt>
		<dd>{{country}} {{state}} {{city}} {{region}} {{addrDetail}}</dd>
		<dt>Zip Code:</dt>
		<dd>{{zipcode}}</dd>
		<dt>Phone Number:</dt>
		<dd>{{phone}}</dd>
	</dl>
	<button class="btn btn-default edit-select">Edit Address</button>
</div>
```
需要导入的handlebars的js文件：
```js
	<script src="./js/libs/handlebars.runtime-v3.0.3.min.js"></script>
	<script src="./js/hbs/ship-template.js"></script>
```

