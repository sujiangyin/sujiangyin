each和if

```js
{{#each student}}
        {{#if name}}
          <tr>
            <td>{{name}}</td>
            <td>{{sex}}</td>
            <td>{{age}}</td>
          </tr>
         {{/if}}
     {{/each}}
```

```js
巢状嵌套的 handlebars 路径也可以使用 ../， 这样会把路径指向父级（上层）上下文。
../ 标识符表示对模板的父级作用域的引用，并不表示在上下文数据中的上一层。这是因为块级 helpers 可以以任何上下文来调用一个块级表达式
，所以这个【上一层】的概念用来指模板作用域的父级更有意义些。
```

```js
Handlebars 同样也支持嵌套的路径，这样的话就可以在当前的上下文中查找内部嵌套的属性了。

<div class="entry">
  <h1>{{title}}</h1>
  <h2>By {{author.name}}</h2>

  <div class="body">
    {{body}}
  </div>
</div>

上面的模板使用下面这段上下文：

var context = {
  title: "My First Blog Post!",
  author: {
    id: 47,
    name: "Yehuda Katz"
  },
  body: "My first post. Wheeeee!"
};

```

```js
Helpers

Handlebars 的 helpers 在模板中可以访问任何的上下文。可以通过 Handlebars.registerHelper 方法注册一个 helper。

<div class="post">
  <h1>By {{fullName author}}</h1>
  <div class="body">{{body}}</div>

  <h1>Comments</h1>

  {{#each comments}}
  <h2>By {{fullName author}}</h2>
  <div class="body">{{body}}</div>
  {{/each}}
</div>

当时用下面的上下文数据和 helpers：

var context = {
  author: {firstName: "Alan", lastName: "Johnson"},
  body: "I Love Handlebars",
  comments: [{
    author: {firstName: "Yehuda", lastName: "Katz"},
    body: "Me too!"
  }]
};

Handlebars.registerHelper('fullName', function(person) {
  return person.firstName + " " + person.lastName;
});

会得到如下结果：

<div class="post">
  <h1>By Alan Johnson</h1>
  <div class="body">I Love Handlebars</div>

  <h1>Comments</h1>

  <h2>By Yehuda Katz</h2>
  <div class="body">Me Too!</div>
</div>

Helpers 会把当前的上下文作为函数中的 this 上下文。

<ul>
  {{#each items}}
  <li>{{agree_button}}</li>
  {{/each}}
</ul>

当使用下面的 this上下文 和 helpers：

var context = {
  items: [
    {name: "Handlebars", emotion: "love"},
    {name: "Mustache", emotion: "enjoy"},
    {name: "Ember", emotion: "want to learn"}
  ]
};

Handlebars.registerHelper('agree_button', function() {
  return new Handlebars.SafeString(
    "<button>I agree. I " + this.emotion + " " + this.name + "</button>"
  );
});

会得到如下结果：

<ul>
  <li><button>I agree. I love Handlebars</button></li>
  <li><button>I agree. I enjoy Mustache</button></li>
  <li><button>I agree. I want to learn Ember</button></li>
</ul>

如果你不希望你的 helper 返回的 HTML 值被编码，就请务必返回一个 new Handlebars.SafeString
```

```js
内置的 Helpers
with helper

一般情况下，Handlebars 模板在计算值时，会把传递给模板的参数作为上下文。

var source   = "<p>{{lastName}}, {{firstName}}</p>";
var template = Handlebars.compile(source);
template({firstName: "Alan", lastName: "Johnson"});

结果如下：

<p>Johnson, Alan</p>

不过也可以在模板的某个区域切换上下文，使用内置的 with helper即可。

<div class="entry">
  <h1>{{title}}</h1>

  {{#with author}}
  <h2>By {{firstName}} {{lastName}}</h2>
  {{/with}}
</div>

在使用下面数据作为上下文时：

{
  title: "My first post!",
  author: {
    firstName: "Charles",
    lastName: "Jolley"
  }
}

会得到如下结果：

<div class="entry">
  <h1>My first post!</h1>

  <h2>By Charles Jolley</h2>
</div>

each helper

你可以使用内置的 each helper 来循环一个列表，循环中可以使用 this 来代表当前被循环的列表项。

<ul class="people_list">
  {{#each people}}
  <li>{{this}}</li>
  {{/each}}
</ul>

使用这个上下文：

{
  people: [
    "Yehuda Katz",
    "Alan Johnson",
    "Charles Jolley"
  ]
}

会得到：

<ul class="people_list">
  <li>Yehuda Katz</li>
  <li>Alan Johnson</li>
  <li>Charles Jolley</li>
</ul>

事实上，可以使用 this 表达式在任何上下文中表示对当前的上下文的引用。

还可以选择性的使用 else ，当被循环的是一个空列表的时候会显示其中的内容。

{{#each paragraphs}}
  <p>{{this}}</p>
{{else}}
  <p class="empty">No content</p>
{{/each}}

在使用 each 来循环列表的时候，可以使用 {{@index}} 来表示当前循环的索引值。

{{#each array}}
  {{@index}}: {{this}}
{{/each}}

对于 object 类型的循环，可以使用 {{@key}} 来表示：

{{#each object}}
  {{@key}}: {{this}}
{{/each}}

if helper

if 表达式可以选择性的渲染一些区块。如果它的参数返回 false, undefined, null, "" 或 []（译注：还有 0）（都是JS中的“假”值），Handlebars 就不会渲染这一块内容：

<div class="entry">
  {{#if author}}
  <h1>{{firstName}} {{lastName}}</h1>
  {{/if}}
</div>

当时用一个空对象（{}）作为上下文时，会得到：

<div class="entry">
</div>

在使用 if 表达式的时候，可以配合 {{else}} 来使用，这样当参数返回 假 值时，可以渲染 else 区块：

<div class="entry">
  {{#if author}}
    <h1>{{firstName}} {{lastName}}</h1>
  {{else}}
    <h1>Unknown Author</h1>
  {{/if}}
</div>

unless helper

unless helper 和 if helper　是正好相反的，当表达式返回假值时就会渲染其内容：

<div class="entry">
  {{#unless license}}
  <h3 class="warning">WARNING: This entry does not have a license!</h3>
  {{/unless}}
</div>

如果在当前上下文中查找 license 返回假值，Handlebars 就会渲染这段警告信息。反之，就什么也不输出。
log helper

log helper 可以在执行模板的时候输出当前上下文的状态。

{{log "Look at me!"}}

这样会把委托信息发送给 Handlebars.logger.log，而且这个函数可以重写来实现自定义的log。
```
