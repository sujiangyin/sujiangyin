链接：https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce。
语法
```js
arr.reduce(callback[, initialValue])
```
参数
```js
callback
    对数组的每个数执行的方法，有四个参数：

    前面的值
        The value previously returned in the last invocation of the callback, 
        or initialValue, if supplied. (See below.)
    现在的值
        The current element being processed in the array.
    现在计算的数组数值的下标
        The index of the current element being processed in the array.
    数组
        The array reduce was called upon.

初始值
    Optional. Object to use as the first argument to the first call of the callback. 
```
原则：
如果初始值给出，那么第一次调用回调函数的前面的值是初始值，当前值是数组的第一个值；如果初始值没有给出，那么第一次调用回调函数的前面的值是数组的第一个值，当前值是第二个值。

如果初始值为空并且数组为空，则抛出一个异常TypeError 。如果数组为空但初始值提供了或者数组只有一个值但初始值为空。那么单独的值就会被返回而没有调用回调函数。

实例：
```js
[0, 1, 2, 3, 4].reduce(function(previousValue, currentValue, index, array) {
  return previousValue + currentValue;
});
```
结果：
```js
 	previousValue 	currentValue 	index 	array 	return value
first call 	             0 	1 	         1 	        [0, 1, 2, 3, 4] 	1
second call 	     1 	2 	           2 	        [0, 1, 2, 3, 4] 	3
third call 	             3 	3 	          3 	         [0, 1, 2, 3, 4] 	6
fourth call 	      6 	4 	          4 	        [0, 1, 2, 3, 4] 	10
```
另外一个例子：
```js
[0, 1, 2, 3, 4].reduce(function(previousValue, currentValue, index, array) {
  return previousValue + currentValue;
}, 10);
```
结果：
```js
  	previousValue 	currentValue 	index 	array 	return value
first call 	10 	0 	0 	[0, 1, 2, 3, 4] 	10
second call 	10 	1 	1 	[0, 1, 2, 3, 4] 	11
third call 	11 	2 	2 	[0, 1, 2, 3, 4] 	13
fourth call 	13 	3 	3 	[0, 1, 2, 3, 4] 	16
fifth call 	16 	4 	4 	[0, 1, 2, 3, 4] 	20
```

例子
```js
var total = [0, 1, 2, 3].reduce(function(a, b) {
  return a + b;
});
// total == 6
```
例子：
```js
var flattened = [[0, 1], [2, 3], [4, 5]].reduce(function(a, b) {
  return a.concat(b);
});
// flattened is [0, 1, 2, 3, 4, 5]
```
reduce()是第五版才增加的，不能满足所以版本的标准。使用前，在脚本前面插入以下代码，防止不支持标准的问题：
```js
// Production steps of ECMA-262, Edition 5, 15.4.4.21
// Reference: http://es5.github.io/#x15.4.4.21
if (!Array.prototype.reduce) {
  Array.prototype.reduce = function(callback /*, initialValue*/) {
    'use strict';
    if (this == null) {
      throw new TypeError('Array.prototype.reduce called on null or undefined');
    }
    if (typeof callback !== 'function') {
      throw new TypeError(callback + ' is not a function');
    }
    var t = Object(this), len = t.length >>> 0, k = 0, value;
    if (arguments.length == 2) {
      value = arguments[1];
    } else {
      while (k < len && !(k in t)) {
        k++; 
      }
      if (k >= len) {
        throw new TypeError('Reduce of empty array with no initial value');
      }
      value = t[k++];
    }
    for (; k < len; k++) {
      if (k in t) {
        value = callback(value, t[k], k, t);
      }
    }
    return value;
  };
}
```
