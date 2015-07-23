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
with helper演示怎么传递一个参数到你的helper。当一个helper被当成一个参数，它就被调用了，不管模版通过什么上下文。
```js
<div class="entry">
  <h1>{{title}}</h1>
  {{#with story}}
    <div class="intro">{{{intro}}}</div>
    <div class="body">{{{body}}}</div>
  {{/with}}
</div>
```
你可能发现一个helper像这种多么有用，如果你的json对象包含很深的嵌套性能，你想要去避免
重复父亲的名字。上面的模版和json一起使用会很有用，就像：
```js
{
  title: "First Post",
  story: {
    intro: "Before the jump",
    body: "After the jump"
  }
}
```
使一个helper生效像这个使noop helper 生效。helper可以带参数，参数的评估就像直接放在{{}}这个里面的表达式一样。
```js
Handlebars.registerHelper('with', function(context, options) {
  return options.fn(context);
});
```
参数被传递到helpers目的是被传递，然后跟随着option hash。


简单的迭代：
块helper的一个普遍的用例是使用他们来定义自定义的迭代器。事实上，所有的handlebars内置的helper都是被按照handlebars的规律来定义的。让我们看看内置的helper each是怎么工作的？
```js
<div class="entry">
  <h1>{{title}}</h1>
  {{#with story}}
    <div class="intro">{{{intro}}}</div>
    <div class="body">{{{body}}}</div>
  {{/with}}
</div>
<div class="comments">
  {{#each comments}}
    <div class="comment">
      <h2>{{subject}}</h2>
      {{{body}}}
    </div>
  {{/each}}
</div>
```
在这个例子里面，我们想要调用block通过each，为数组中的每个元素。
```js
Handlebars.registerHelper('each', function(context, options) {
  var ret = "";

  for(var i=0, j=context.length; i<j; i++) {
    ret = ret + options.fn(context[i]);
  }

  return ret;
});

```
