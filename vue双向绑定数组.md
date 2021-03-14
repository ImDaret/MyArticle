---
title: Vue双向绑定数组
image: /image/3.jpg  #设置本地图片
---
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1650e0743a644657bb90624778d79d90~tplv-k3u1fbpfcp-zoom-1.image)
众所周知,vue中`v-model` 会忽略所有表单元素的 `value、checked、selected attribute` 的初始值而总是将 Vue 实例的数据作为数据来源。

大部分情况,`v-model`是绑定一个对象的属性，但是如果数据库中的数据是一个数组，这种情况下，如果把数组转换成对象再绑定，然后再转换回去提交到数据库显然是一件工程量很大的事情，本着程序员偷懒的原则，我发现了一个便捷的方法，废话不多说，直接上干货。

###HTML代码如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <link href="https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900" rel="stylesheet">
  <link href="https://cdn.jsdelivr.net/npm/@mdi/font@5.x/css/materialdesignicons.min.css" rel="stylesheet">
  <link href="https://cdn.jsdelivr.net/npm/vuetify@2.x/dist/vuetify.min.css" rel="stylesheet">
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>vue双向绑定数组</title>
</head>
<body>
<div id="app">
  <v-app>
    <!-- 表格 -->
    <table>
      <tr>
        <td v-for="(item,index) in arr">
          <!-- 文本框 -->
          <v-text-field v-model="arr[index]"></v-text-field>
        </td>
      </tr>
      <tr>
        <td v-for="(item,index) in arr">
          <span>{{item}}</span>
        </td>
      </tr>
    </table>
  </v-app>
</div>
```  
###js代码如下：

```javascript
<!-- 引入vue -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.x/dist/vue.js"></script>
<!-- 引入vuetify -->
<script src="https://cdn.jsdelivr.net/npm/vuetify@2.x/dist/vuetify.js"></script>
<script>
  new Vue({
    el: '#app',
    vuetify: new Vuetify(),
    data () {
      return{
        arr:[12123,134123,12]
      }
    },
  })
</script>
```
这里我用最简单的方法，给大家演示了一下，利用`item`遍历数组，然后利用`index`索引找到数组的下标，`v-model`绑定数组的下标即可实现上图的效果。