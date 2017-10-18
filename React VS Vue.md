1、简介

实战场景：React开发移动端项目，Vue开发pc端erp。本文综合react和vue的使用，做下对比，整理一下异同点。

Vue文档https://cn.vuejs.org/v2/guide/syntax.html

React文档http://www.runoob.com/react/react-tutorial.html

2、区别

2.1、基础demo

Vue:
```js
<template>
        <button class="my-button" @click="handleClick()">
            {{text}}
        </button>
</template>

<script>
    export default {
        name: 'test-demo',
        data() {
            return {
                'text':'点击前'
            }
        },
        methods: {
            //点击新增按钮
            handleClick() {
                this.text = '点击后'
            }
        }
    }

</script>

<style lang="css" scoped>
.my-button {
    width: 100px;
    height: 30px;
    background-color: #ddd;
    color: #333;
}
</style>

```

React:
```js
import Style from  './style.css';  
var Test = React.createClass({
              getInitialState: function() {
                return {
                  text: "点击前"
                };
              },
              handleClick: function() {
                   this.setState({text: '点击后'});
              },
              render: function() {
                return 
                    <button className="my-button" onClick="this.handleClick">
                        {this.state.name}
                    </button>;
              }
          });
          
ReactDOM.render(
  <Test/>,
  document.getElementById('app')
);
```
从上面简单的demo看出，

###vue对样式，js，模板html的区分更加清晰，做到更独立的抽象；而react的使用jsx语法，将js和模板杂糅在一起，css外部导入。

###vue一个页面抽象为一个组件，react是一个React.createClass创建一个组件。均是组件化思维，且节点在未渲染之前都是虚拟dom的思想

###vue的state状态是可以直接修改的，而react必须通过api setState来设置，因为react依赖这个api告知监听状态变化的监听器状态是否发生改变。

###vue的语法更加简单易懂，易上手。代码量相对来说更加简洁
