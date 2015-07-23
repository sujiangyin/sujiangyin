Block helper 可以自定义迭代器和其他功能，即使得通过的块解释成一个新的上下文。
基本块：
出于示范目的，让我们定义一个helper来解释block尽管没有helper存在。
```js
<div class="entry">
  <h1>{{title}}</h1>
  <div class="body">
    {{#noop}}{{body}}{{/noop}}
  </div>
</div>
```
noop helper，简称“no operation”，会受到一个选择的hash。这个选择的hash具有一个功能（options.fn），它表现是正常编译handlebars 模版。
具体而言，它的功能会得到一个上下文，然后返回一个字符串。
```js
Handlebars.registerHelper('noop', function(options) {
  return options.fn(this);
});
```
handlebars 总是用现在的上下文解释helpers就如this，所以你可以解释这个块用this来在当前上下文评估块。


基本块的变化：
为了更好地表明语法，让我们定义另外一个block helper来添加一些标记到被包住的文本。
```js
<div class="entry">
  <h1>{{title}}</h1>
  <div class="body">
    {{#bold}}{{body}}{{/bold}}
  </div>
</div>
```
bold helper 会添加标记来让它的文本突出。正如前面所说，它的功能会得到一个上下文然后返回一个字符串。
```js
Handlebars.registerHelper('bold', function(options) {
  return new Handlebars.SafeString(
      '<div class="mybold">'
      + options.fn(this)
      + '</div>');
});
```

with helper

