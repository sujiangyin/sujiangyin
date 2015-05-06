1、button的disbled属性js：
      禁用：attr（'disabled','disabled')
      恢复：attr('disabled',false)
      
2、获取json对象的长度：
```js
<script type="text/javascript">

var myObject = {'name':'Kasun', 'address':'columbo','age': '29'}

var count = Object.keys(myObject).length
console.log(count);
</script>
```
3、data-*:
查找属性元素、获取属性值、设置data属性值：
var levelOneBlock=this.find('[data-level="1"]');
var number=$(this).data('level');
$( "body" ).data( "bar", "foobar" );
alert( $( "body" ).data( "bar" ) ); // foobar

4、
