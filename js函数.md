```js
text.replace(/\W+/g, " ").split(' ').indexOf(n)
用空格取代非单词符号，按照空格分隔单词，找出字符串n的下标。
```

```js
单词和空格间的位置。
//b
```

```js
!!text.match('\\b' + n + '\\b')
把右边的值转为Boolean值。
```

```js
match() 方法可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。
该方法类似 indexOf() 和 lastIndexOf()，但是它返回指定的值，而不是字符串的位置。
语法：
stringObject.match(searchvalue)
stringObject.match(regexp)
```

```js
search() 方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串。
返回字符串的起始位置。
```

```js
.apply的其他巧妙用法（一般在什么情况下可以使用apply）  
  
我首先从网上查到关于apply和call的定义,然后用示例来解释这两个方法的意思和如何去用.  
         apply:方法能劫持另外一个对象的方法，继承另外一个对象的属性.  
 Function.apply(obj,args)方法能接收两个参数  
obj：这个对象将代替Function类里this对象  
args：这个是数组，它将作为参数传给Function（args-->arguments）  
  
         call:和apply的意思一样,只不过是参数列表不一样.  
  
 Function.call(obj,[param1[,param2[,…[,paramN]]]])  
obj：这个对象将代替Function类里this对象  
params：这个是一个参数列表  
  
1.apply示例:  
  
<script type="text/javascript">   
/*定义一个人类*/   
function Person(name,age) {   
    this.name=name; this.age=age;   
}   
 /*定义一个学生类*/   
functionStudent(name,age,grade) {   
    Person.apply(this,arguments); this.grade=grade;   
}   
//创建一个学生类   
var student=new Student("qian",21,"一年级");   
//测试   
alert("name:"+student.name+"\n"+"age:"+student.age+"\n"+"grade:"+student.grade);   
//大家可以看到测试结果name:qian age:21 grade:一年级   
//学生类里面我没有给name和age属性赋值啊,为什么又存在这两个属性的值呢,这个就是apply的神奇之处.   
</script>   
  
分析: Person.apply(this,arguments);  
  
this:在创建对象在这个时候代表的是student  
  
arguments:是一个数组,也就是[“qian”,”21”,”一年级”];  
  
也就是通俗一点讲就是:用student去执行Person这个类里面的内容,在Person这个类里面存在this.name等之类的语句
,这样就将属性创建到了student对象里面  
```
```js
保留（两）几位小数
return String(Math.floor([n] * 100) / 100).replace(/\d(?=(\d{3})+\.)/g, '$&,');
```
```js
join('')连接
```

```js
var spied = spy(myFunction);
把该函数定义一个新的函数名。
```

```js
2) filter

该filter()方法创建一个新的匹配过滤条件的数组。

不用 filter() 时
var arr = [
  {"name":"apple", "count": 2},
  {"name":"orange", "count": 5},
  {"name":"pear", "count": 3},
  {"name":"orange", "count": 16},
];
   
var newArr = [];
 
for(var i= 0, l = arr.length; i< l; i++){
  if(arr[i].name === "orange" ){
newArr.push(arr[i]);
}
}
 
console.log("Filter results:",newArr);

用了 filter():
var arr = [
  {"name":"apple", "count": 2},
  {"name":"orange", "count": 5},
  {"name":"pear", "count": 3},
  {"name":"orange", "count": 16},
];
var newArr = arr.filter(function(item){
  return item.name === "orange";
});
console.log("Filter results:",newArr);

最近发现了jquery的.filter()方法，这真是一个很强大的方法，最强大之处在于，他可以接受一个函数作为参数，
然后根据函数的返回值判断，如果返回值是true，这个元素将被保留，如果返回值是false，这个元素将被剔除。
这就是jquery选择器的过滤器。 
```
```js
some方法

array1.some(callbackfn[, thisArg])

对数组array1中的每个元素调用回调函数callbackfn，当回调函数返回true或者遍历完所有数组后，some方法终止。
可选参数thisArg可以替换回调函数中的this对象

filter方法

array1.filter(callbackfn[, thisArg])

对数组array1中的每个元素调用回调函数callbackfn方法，该方法会返回一个在回调函数中返回true的元素的新的集合。
可选参数thisArg可以替换回调函数中的this对象

两者的区别

some方法返回的是boolean值，可用于检察数组中是否有某对象

filter方法返回的是一个新数组，可用于过滤数组中的对象
```
```js
typeof 运算符把类型信息当作字符串返回，包括有大家常有变量类型。
```

```js
educe()可以实现一个累加器的功能，将数组的每个值（从左到右）将其降低到一个值。

说实话刚开始理解这句话有点难度，它太抽象了。

场景： 统计一个数组中有多少个不重复的单词
```
