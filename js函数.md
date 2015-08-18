'''js
text.replace(/\W+/g, " ").split(' ').indexOf(n)
用空格取代非单词符号，按照空格分隔单词，找出字符串n的下标。
'''

'''js
单词和空格间的位置。
//b
'''

'''js
!!text.match('\\b' + n + '\\b')
把右边的值转为Boolean值。
'''

'''js
match() 方法可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。
该方法类似 indexOf() 和 lastIndexOf()，但是它返回指定的值，而不是字符串的位置。
语法：
stringObject.match(searchvalue)
stringObject.match(regexp)
'''

'''js
search() 方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串。
返回字符串的起始位置。
'''

'''js
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
  
也就是通俗一点讲就是:用student去执行Person这个类里面的内容,在Person这个类里面存在this.name等之类的语句,这样就将属性创建到了student对象里面  
  
'''
'''js
保留（两）几位小数
return String(Math.floor([n] * 100) / 100).replace(/\d(?=(\d{3})+\.)/g, '$&,');
'''
