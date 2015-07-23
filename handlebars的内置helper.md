1、if 块helper
你可以使用if这个helper而有条件地使用一个块。如果其参数返回false，undefined, null, "", 0, 或[]，handlebars将不会执行这个块。‘’
```js
<div class="entry">
  {{#if author}}
    <h1>{{firstName}} {{lastName}}</h1>
  {{/if}}
</div>
```
当和一个空的上下文({}),author将是undefined，结果为：
```js
<div class="entry">
</div>
```
当正在使用一个block表达式时，如果这个表达式返回假的值，你可以具体化一个模块的部分来运行。
被{{else}}标记的section叫做“else section”。
```js
<div class="entry">
  {{#if author}}
    <h1>{{firstName}} {{lastName}}</h1>
  {{else}}
    <h1>Unknown Author</h1>
  {{/if}}
</div>
```
2、unless块helper
你可以使用unless helper作为if helper的相反方向。如果表达式返回假，则它的块将被启用。
```js
<div class="entry">
  {{#unless license}}
  <h3 class="warning">WARNING: This entry does not have a license!</h3>
  {{/unless}}
</div>
```
如果在当前上下文中查到的license是一个flase的值，handlebars就会执行warming，
否则，不会执行任何东西。
3、each 块helper
你可以使用each helper迭代一个列表。在块里面，你可以使用this来指向正在迭代的元素。
```js
<ul class="people_list">
  {{#each people}}
    <li>{{this}}</li>
  {{/each}}
</ul>
```
上下文如下：
```js
{
  people: [
    "Yehuda Katz",
    "Alan Johnson",
    "Charles Jolley"
  ]
}
```
结果：
```js
<ul class="people_list">
  <li>Yehuda Katz</li>
  <li>Alan Johnson</li>
  <li>Charles Jolley</li>
</ul>
```
你可以在任何上下文使用this表达式来指向当前的上下文。
```js
你可以有选择地提供一个{{else}}部分来展示内容，当list是空的时候。
{{#each paragraphs}}
  <p>{{this}}</p>
{{else}}
  <p class="empty">No content</p>
{{/each}}
```
当遍历到each里面items你可以有选择地指向当前循环的下标使用{{@index}}。
```js
{{#each array}}
  {{@index}}: {{this}}
{{/each}}
```
另外对于迭代的对象,{{@key}}指向当前的key名：
```js
{{#each object}}
  {{@key}}: {{this}}
{{/each}}
```
迭代的第一步和最后一步分别通过变量@first和@last来标注。当迭代一个对象，只有@first可以使用。

嵌套each块可以允许迭代变量基于路径走过深度。可以允许父节点的index，比如{{@../index}}可以使用。

4、with 块 helper
正常来讲，handlebars的模版是依靠上下文通过编译方法来评估的。
```js
var source   = "<p>{{lastName}}, {{firstName}}</p>";
var template = Handlebars.compile(source);
template({firstName: "Alan", lastName: "Johnson"});
```
结果：
```js
<p>Johnson, Alan</p>
```
```js
您可以使用内置的with模块来将上下文转换为模板的一部分。
<div class="entry">
  <h1>{{title}}</h1>

  {{#with author}}
  <h2>By {{firstName}} {{lastName}}</h2>
  {{/with}}
</div>
配合这个上下文：
{
  title: "My first post!",
  author: {
    firstName: "Charles",
    lastName: "Jolley"
  }
}
结果：
<div class="entry">
  <h1>My first post!</h1>

  <h2>By Charles Jolley</h2>
</div>
你还可以使用else部分来显示当通过的值为空：
{{#with author}}
  <p>{{name}}</p>
{{else}}
  <p class="empty">No content</p>
{{/with}}
```
5、lookup块helper（顾名思义：查找）
它允许使用handlebars变量解释动态字段，这对数组索引的值的解释是很有用的。(??????)
```js
{{#each bar}}
  {{lookup ../foo @index}}
{{/each}}
```
6、log块helper
可以在执行模板时进行上下文状态的记录。
```js
{{log "Look at me!"}}
```
代表Handlebars.logger.log（被自定义的logging重写）。
7、blockHelperMissing 块helper
隐式调用当一个helper不能在环境的helpers hash里直接被解释。
```js
{{#foo}}{{/foo}}
```
将会在当前上下文调用这个helper和foo被解释的值,并且options.name域设置“foo”.
这个可能被那些希望改变块的行为的用户重写。比如：
```js
Handlebars.registerHelper('blockHelperMissing', function(context, options) {
  throw new Handlebars.Exception('Only if or each is allowed');
});
```
可能被用来阻止 mustache-style块（对if和each块更青睐的块）的使用。


8、helperMissing 块helper
当一个潜在的helper在环境helper和当前上下文都找不到的·时候，内部helper就会被调用。在两者都运行的情况下，这个会在blockHelperMissing之前运行。
```js
{{foo}}
{{foo bar}}
{{#foo}}{{/foo}}
```
将会一个个调用这个helper，传递任何参数，这些参数已经被传递给别的同名helper。当使用knownHelpersOnly的时候这个helper将不会被调用。
这可能会被应用重写。为了强迫领域的存在，下面可能要被使用：
```js
Handlebars.registerHelper('helperMissing', function(/* [args, ] options */) {
  var options = arguments[arguments.length - 1];
  throw new Handlebars.Exception('Unknown field: ' + options.name);
});
```
