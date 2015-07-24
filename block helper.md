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

在这个例子里，我们在传递的参数里迭代items，对于每个items的项都调用这个块，
当我们迭代的时候，我们建立了一个字符串结果然后返回它。
这个模式可以被用来使得更多先进的迭代器生效。例如，让我们们创建一个迭代器，可以产生<ul>的封装，然后把每一个结果元素封装在li标签里面。

```js
{{#list nav}}
  <a href="{{url}}">{{title}}</a>
{{/list}}
```
你可以评估这个模块使用一些类似上下文的东西：
```js
{
  nav: [
    { url: "http://www.yehudakatz.com", title: "Katz Got Your Tongue" },
    { url: "http://www.sproutcore.com/block", title: "SproutCore Blog" },
  ]
}

````
这个helper和原先的each helper很相似。
```js
Handlebars.registerHelper('list', function(context, options) {
  var ret = "<ul>";

  for(var i=0, j=context.length; i<j; i++) {
    ret = ret + "<li>" + options.fn(context[i]) + "</li>";
  }

  return ret + "</ul>";
});
```
使用一个库就像underscore.js或者SproutCore的运行时间库让这个变得更漂亮，例如，下面这个是当使用SproutCore的运行时间库时候的样子：
```js
Handlebars.registerHelper('list', function(context, options) {
  return "<ul>" + context.map(function(item) {
    return "<li>" + options.fn(item) + "</li>";
  }).join("\n") + "</ul>";
});
```
条件语句
块helper的另外一个普遍用例就是评估条件的状态。与迭代器一样，handlebars的内置if和unless控制结构按照handlebarshelper的规律被生效。
```js
{{#if isActive}}
  <img src="star.gif" alt="Active">
{{/if}}
```
控制结构通常没有改变现有的上下文，取而代之，它们基于某些变量决定是否调用这个块。
```js
Handlebars.registerHelper('if', function(conditional, options) {
  if(conditional) {
    return options.fn(this);
  }
});
```

当写一个条件语句时，你就会经常想要模块提供一个html的块，那么你的helper 可以插入如果条件语句设置为false。
handlebars处理这个问题通过提供else功能给块helpers。
```js
{{#if isActive}}
  <img src="star.gif" alt="Active">
{{else}}
  <img src="cry.gif" alt="Inactive">
{{/if}}
```
hanldebars为else片段提供块如option.inverse。你不需要核对else片段是否存在：handlebars会自动侦查它以及注册一个noop的功能。
```js
Handlebars.registerHelper('if', function(conditional, options) {
  if(conditional) {
    return options.fn(this);
  } else {
    return options.inverse(this);
  }
});

```
handlebars通过把他们作为options hash的属性栓住的方法为块helper提供额外的元数据。继续阅读看更多的例子。

条件语句也可以被包括在随后的其他helper，它在else的区域里被调用
```js
{{#if isActive}}
  <img src="star.gif" alt="Active">
{{else if isInactive}}
  <img src="cry.gif" alt="Inactive">
{{/if}}
```
在随后的调用中使用同样名字的helper是不需要的，unless这个helper可能在和其他任何helper配用的else部分被使用。
当helper的值不同，关闭大括号就会匹配开着的helper的名字。

Hash 参数
像普通的helpers一样，块helper可以接受一个可选择的hash作为它的最终参数。让我们回顾list helper，它可以让我们添加任意可选属性的数字到我们创建的ul元素里。
```js
{{#list nav id="nav-bar" class="top"}}
  <a href="{{url}}">{{title}}</a>
{{/list}}
```
handlebars 提供最终的hash作为options.hash。这让接受一个可变数量的参数，而且还可以接受option hash。
如果模版没有提供hash参数，handlebars将会自动通过一个空的对象，所以你不需要核对hash参数的存在性。
```js
Handlebars.registerHelper('list', function(context, options) {
  var attrs = Em.keys(options.hash).map(function(key) {
    return key + '="' + options.hash[key] + '"';
  }).join(" ");

  return "<ul " + attrs + ">" + context.map(function(item) {
    return "<li>" + options.fn(item) + "</li>";
  }).join("\n") + "</ul>";
});
```
hash参数提供一个强大的方法来提供可选参数的数字到一个块helper，没有位置参数引起的复杂性。
