---
title: Vue组件之间传值
image: /image/8.jpg  #设置本地图片
---
# 目录
### 一、父组件向子组件传值
### 二、子组件向父组件传值
### 三、兄弟组件之间的传值
![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/07545cbece4246eeb432bd82e47f7578~tplv-k3u1fbpfcp-zoom-1.image)


如上图所示，2是1的子组件，1是3的父组件，2和3是兄弟组件

### 一、父组件向子组件传值：
html代码:
```html
<div id="app">
  <v-app>
    <!-- 用:xxxx="xxxx"绑定父组件的数据 -->
    <list-group :listgroup="listgroup"></list-group>
  </v-app>
</div>
```
子组件注册代码:
```javascript
Vue.component('list-group', {
    // 接收父组件的数据
    props:['listgroup'],
    template:`
      <v-card
        max-width="300"
        tile
      >
        <v-list rounded>
          <v-list-item-group v-model="listgroup.item" color="primary">
            <v-list-item
              v-for="(item, index) in listgroup.items"
              :key="index" v-text="item.text""
            >
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-card>
    `
  })
```
父组件代码：
```javascript
new Vue({
    el: '#app',
    vuetify: new Vuetify(),
    data: {
      // 父组件中的数据
      listgroup:{
        item: 1,
        items: [
          { id:1, text: 'Real-Time', icon: 'mdi-clock' },
          { id:2, text: 'Audience', icon: 'mdi-account' },
          { id:3, text: 'Conversions', icon: 'mdi-flag' },
        ],
      },
    },
  })
```
综上所述，父组件向子组件传值需要三步：

1、首先在父组件中创建数据

2、然后再html代码中用`:`绑定数据

3、最后在子组件注册的代码中用`props`接收数据

### 二、子组件向父组件传值:
我们在上面代码的基础上略作调整,加一个index传值

html代码：
```html
<div id="app">
  <v-app>
    <!-- @xxxx(xxxx要与子组件中的方法名对应) =后面跟父组件中的函数名($event接收子组件的值) -->
    <list-group :listgroup="listgroup" @handleindex="handle($event)"></list-group>
  </v-app>
</div>
```
子组件代码：
```javascript
Vue.component('list-group', {
    props:['listgroup'],
    template:`
      <v-card
        max-width="300"
        tile
      >
        <v-list rounded>
          <v-list-item-group v-model="listgroup.item" color="primary">
            <v-list-item
              v-for="(item, index) in listgroup.items"
              :key="index" v-text="item.text" @click="$emit('handleindex',index)" //这里$emit就是子组件传值给父组件的方法，handleindex是方法名，index是数据
            >
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-card>
    `
  })
```
父组件代码:
```javascript
new Vue({
    el: '#app',
    vuetify: new Vuetify(),
    data: {
      listgroup:{
        item: 1,
        items: [
          { id:1, text: 'Real-Time', icon: 'mdi-clock' },
          { id:2, text: 'Audience', icon: 'mdi-account' },
          { id:3, text: 'Conversions', icon: 'mdi-flag' },
        ],
      },
      // 父组件中的值
      index:-1,
    },
    methods:{
      // 父组件中的函数，val接收子组件传过来的值，然后相应的修改自身的index变量
      handle: function(val){
        this.index = val
        console.log(this.index) //打印index
      }
    }
  })
```
综上，子组件向父组件传值也是三步：

1、在子组件中准备想传的那个值，然后用`$emit`方法携带一个方法名

2、在html代码中用`@`绑定子组件中的方法名，然后用`$event`接收子组件中的值

3、父组件中随便找个参数接收即可

### 三、兄弟组件之间的传值:
之前我学习到的是创建事件中心的方法来实现，但是我想了一想发现，兄弟组件共同的父组件不就是事件中心吗，所以我们在上面的代码再略作修改看看能否实现效果(其实就是在上面的基础上再注册一个组件接收index的值就可测试)

html代码：
```html
<div id="app">
  <v-app>
    <!-- 列表组件绑定父组件中的函数 -->
    <list-group :listgroup="listgroup" @handleindex="handle($event)"></list-group>
    <!-- 测试组件绑定父组件中的index值 -->
    <test-box :index="index"></test-box>
  </v-app>
</div>
```
兄弟组件代码:
```javascript
  // 测试组件
  Vue.component('test-box',{
    props:['index'],
    template:`
      <div>{{index}}</div>
    `
  })
  // 列表组件
  Vue.component('list-group', {
    props:['listgroup'],
    template:`
      <v-card
        max-width="300"
        tile
      >
        <v-list rounded>
          <v-list-item-group v-model="listgroup.item" color="primary">
            <v-list-item
              v-for="(item, index) in listgroup.items"
              :key="index" v-text="item.text" @click="$emit('handleindex',index)"
            >
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-card>
    `
  })
```

父组件代码:
```javascript
new Vue({
    el: '#app',
    vuetify: new Vuetify(),
    data: {
      listgroup:{
        item: 1,
        items: [
          { id:1, text: 'Real-Time', icon: 'mdi-clock' },
          { id:2, text: 'Audience', icon: 'mdi-account' },
          { id:3, text: 'Conversions', icon: 'mdi-flag' },
        ],
      },
      // 父组件中的值
      index:-1,
    },
    methods:{
      // 父组件中的函数，val接收子组件传过来的值，然后相应的修改自身的index变量
      handle: function(val){
        this.index = val
        console.log(this.index) //打印index
      }
    }
  })
```
这是最后的完整版代码，包括了父组件向子组件传值，子组件向父组件传值，兄弟组件之间的传值

综上，兄弟组件之间的传值包括以下三步(这里的测试组件和列表组件是兄弟组件关系):

1、列表组件给父组件传值

2、父组件接收列表组件的值，并且利用函数的方式修改了自身的`index`的值

3、父组件向测试组件传`index`的值

### 最后附上效果:
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cc0716f9081a48d7809b4b6046843ff9~tplv-k3u1fbpfcp-zoom-1.image)