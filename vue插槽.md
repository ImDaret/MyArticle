---
title: Vue插槽
image: /image/1.jpg  #设置本地图片
---

vue中`<slot></slot>`标签代表插槽
### 一、内容插槽
顾名思义，插槽的作用就是存放内容

html代码如下：
```html
<test-man>
      <v-card dark>
        is handsome
      </v-card>
</test-man>
```
javascript代码如下：
```javascript
Vue.component('test-man', {
    template:`
    <v-card>
      <div>
      pingchangxin
      </div>
      <slot>
      </slot>
    </v-card>
    `
  })
```
效果如下：![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9f8bd71c3d0c4521a89fcaef1f8c6fd7~tplv-k3u1fbpfcp-zoom-1.image)

### 二、后备插槽
在组件模板的`<slot></slot>`中添加内容，如果html利用的这个组件中没有内容的话，就是默认显示插槽里面的内容，如果有内容则覆盖插槽中的内容
html代码如下：
```html
<test-man></test-man>
<test-man>
  <v-btn>保存</v-btn>
</test-man>
```
javascript代码如下：
```javascript
Vue.component('test-man', {
    template:`
    <v-card>
      <div>
      pingchangxin
      </div>
      <slot>
      <v-btn>提交</v-btn>
      </slot>
    </v-card>
    `
  })
```
效果如下：
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a34a55742c2e4fa6b9b62447c9d435df~tplv-k3u1fbpfcp-zoom-1.image)

### 三、作用域插槽
这类插槽可以让父组件使用子组件的数据，只需子组件在插槽中用`v-bind`绑定自身的数据，然后父组件在使用组件时就可以用`v-slot="slotProps"`接收，注意：这里的`slotProps`可以自定义。

html代码如下：
```html
<test-man v-slot="slotProps">
      <v-btn >{{slotProps.obj2.name}}</v-btn>
    </test-man>
```
javascript代码如下：
```javascript
Vue.component('test-man', {
    data(){
      return {
        obj2:{
        name:'zhubajie',
        age:3
        }
      }
    },
    template:`
    <v-card>
      <div>
      pingchangxin
      </div>
      <slot v-bind:obj2="obj2">
      </slot>
    </v-card>
    `
  })
```
效果如下：![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3454aa7cb62a43029ddbaa65a2d2e01e~tplv-k3u1fbpfcp-zoom-1.image)

### 四、具名插槽
这类插槽可以有多个，每个都用`name`属性绑定一个不同的名字，在接收内容时避免发生混乱，在组件使用时用`v-slot：名字`的方式进行绑定。

html代码如下:
```html
<test-man>
      <template v-slot:second>
        <span>这是第二个插槽的内容</span>
      </template>
      <template v-slot:first>
        <span>这是第一个插槽的内容</span>
      </template>
    </test-man>
```
javascript代码如下
```javascript
Vue.component('test-man', {
    data(){
      return {
        obj2:{
        name:'zhubajie',
        age:3
        }
      }
    },
    template:`
    <v-card>
      <div>
      pingchangxin
      </div>
      <div>
      <slot name="first">
      </slot>
      </div>
      <span>-----这是一条分界线-----</span>
      <div>
      <slot name="second">
      </slot>
      </div>
    </v-card>
    `
  })
```
效果如下：
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c7ab071b2d8842bcb03ecd7e0f4f8610~tplv-k3u1fbpfcp-zoom-1.image)