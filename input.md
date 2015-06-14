对于checkbox：只有当它处于选中状态的时候才会把它的name和对应的value传过去，如果不选中的话，则在接收端收

不到任何关于此checkbox的信息，相当于在页面的表单里没有放进checkbox类型的<input>

对于radio也是一样；

 

而<label>则可以用它的for属性定位到具有该值的ID上，如：

<label for="male">Male</label><input type="checkbox" name="sex" id="male" />

如果在Male上单击一下，则就相当于在<input type="checkbox" name="sex" id="male" />上单击一样

当然，你可以把for的值设置成任何具有相同ID值的控件上，以取得跟在该控件上单击时达到同样的效果


```js

定义和用法

value 属性为 input 元素设定值。

对于不同的输入类型，value 属性的用法也不同：

    type="button", "reset", "submit" - 定义按钮上的显示的文本
    type="text", "password", "hidden" - 定义输入字段的初始值
    type="checkbox", "radio", "image" - 定义与输入相关联的值

注释：<input type="checkbox"> 和 <input type="radio"> 中必须设置 value 属性。

注释：value 属性无法与 <input type="file"> 一同使用。

```
