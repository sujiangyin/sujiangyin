1、简介

实战场景：React开发移动端项目，Vue开发pc端erp。本文综合react和vue的使用，做下对比，整理一下异同点。

2、区别

2.1、基础demo

Vue:
```js
<template>
    <div>
        <button class="my-button" @click="handleClick()">
            {{text}}
        </button>
    </div>
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
