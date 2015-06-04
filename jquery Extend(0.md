
jQuery.extend()
example1:
```js
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>jQuery.extend demo</title>
  <script src="https://code.jquery.com/jquery-1.10.2.js"></script>
</head>
<body>
 
<div id="log"></div>
 
<script>
var object1 = {
  apple: 0,
  banana: { weight: 52, price: 100 },
  cherry: 97
};
var object2 = {
  banana: { price: 200 },
  durian: 100
};
 
// Merge object2 into object1
$.extend( object1, object2 );
 
// Assuming JSON.stringify - not available in IE<8
$( "#log" ).append( JSON.stringify( object1 ) );
</script>
 
</body>
</html>
```
结果：
```js
{"apple":0,"banana":{"price":200},"cherry":97,"durian":100}
```
example2:
```js
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>jQuery.extend demo</title>
  <script src="https://code.jquery.com/jquery-1.10.2.js"></script>
</head>
<body>
 
<div id="log"></div>
 
<script>
var object1 = {
  apple: 0,
  banana: { weight: 52, price: 100 },
  cherry: 97
};
var object2 = {
  banana: { price: 200 },
  durian: 100
};
 
// Merge object2 into object1, recursively
$.extend( true, object1, object2 );
 
// Assuming JSON.stringify - not available in IE<8
$( "#log" ).append( JSON.stringify( object1 ) );
</script>
 
</body>
</html>
```
结果：
```js
{"apple":0,"banana":{"weight":52,"price":200},"cherry":97,"durian":100}
```
example3:
```js
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>jQuery.extend demo</title>
  <script src="https://code.jquery.com/jquery-1.10.2.js"></script>
</head>
<body>
 
<div id="log"></div>
 
<script>
var defaults = { validate: false, limit: 5, name: "foo" };
var options = { validate: true, name: "bar" };
 
// Merge defaults and options, without modifying defaults
var settings = $.extend( {}, defaults, options );
 
// Assuming JSON.stringify - not available in IE<8
$( "#log" ).append( "<div><b>defaults -- </b>" + JSON.stringify( defaults ) + "</div>" );
$( "#log" ).append( "<div><b>options -- </b>" + JSON.stringify( options ) + "</div>" );
$( "#log" ).append( "<div><b>settings -- </b>" + JSON.stringify( settings ) + "</div>" );
</script>
 
</body>
</html>
```
结果：
```js
defaults -- {"validate":false,"limit":5,"name":"foo"}
options -- {"validate":true,"name":"bar"}
settings -- {"validate":true,"limit":5,"name":"bar"}
```
