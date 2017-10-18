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

<style
    lang="css"
    scoped
>
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
