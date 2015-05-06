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

8、$this=$(this)

9、tab和空格

10、tag.class  选择器的细化，右边。更快。

11、$.fn.fun和$.的区别
去掉fn是使方法全局化。

12、id可以更快，但是在js中为了避免重用性，最好用class。

13、加一个return this.each，可以让当有多个元素时，对每个元素起作用。不然，可能如果是jquery对象，只对第一个元素起作用。

14、理解jquery插件的用处。
网址：https://learn.jquery.com/plugins/basic-plugin-creation/
```js
(function ( $ ) {
 
    $.fn.greenify = function() {
        this.css( "color", "green" );
        return this;
    };
 
}( jQuery ));
```
会对所有元素起作用。因为css方法好像。。。不是jquery的。
```js
(function ( $ ) {
 
    $.fn.greenify = function() {
        this.attr( "color", "green" );
        return this;
    };
 
}( jQuery ));
```
不会对所有元素起作用，只对第一个起作用。因为attr方法好像。。。是jquery的。

15、方法里面的回调函数：当某个方法或者动作执行完毕之后再调用的函数。
