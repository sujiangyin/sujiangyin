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

4、$().html与$().append()
     前者一次性放入父元素中并且会清除原来的子元素，后者一个个追加并且不会清除原来的内容。

5、为了提高代码的重用性，最好封装成函数。

6、var声明按照java格式，多个变量一起声明可以用一个var和逗号隔开。

7、$的变量名往往表示一个jquery对象，可以直接调用jquery函数；有些jquery函数返回一个jquery对象，比如html();而有些则返回非jquery对象。再次用其来使用jquery函数时候要用$()方法转换成jquery对象。

8、

