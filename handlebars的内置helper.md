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
