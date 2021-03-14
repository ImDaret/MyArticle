---
title: Vue双向绑定数组
image: /image/7.jpg  #设置本地图片
---
### 定义两个函数:
```javascript
methods:{
      father(){
        console.log('i am father')
      },
      son(){
        console.log('i am son')
      }
    }
```
### 一、没有事件修饰符
```html
<!-- 没有事件修饰符
        点击father 打印 i am father
        点击son 打印 i am son 和 i am father
  -->
<div @click="father">
    father
    <div @click="son">son</div>
  </div>
```
### 二、事件修饰符stop
```html
<!-- 事件修饰符stop（阻止事件继续传播）
      点击father 打印 i am father
      点击son 打印 i am son （成功阻止father的打印）
  -->
  <div @click="father">
    father
    <div @click.stop="son">son</div>
  </div>
```
### 三、事件修饰符capture
```html
<!-- 事件修饰符capture
      点击father 打印 i am father
      点击son 打印 i am father 和 i am son
      注意：这里先打印了 i am father 即先触发father函数再触发son函数
-->
<div @click.capture="father">
  father
  <div @click="son">son</div>
</div>
```
### 四、事件修饰符self
```html
<!-- 事件修饰符self
    点击father 打印 i am father
    点击son 打印 i am son
    即加上self修饰符后，该元素只能由自己触发
-->
<div @click.self="father">
  father
  <div @click="son">son</div>
</div>
```
### 五、事件修饰符once
```html
<!-- 事件修饰符once
    1.先点击father 打印 i am father 
    再点击son 打印 i am son
    2.先点击son 打印 i am son 和 i am father
    再点击father 没有任何打印结果
    即加上once修饰符后，该事件只会触发一次 
-->
<div @click.once="father">
  father
  <div @click="son">son</div>
</div>
```
### 六、事件修饰符prevent
```html
<!-- 事件修饰符prevent
        当表单元素只有一个时，绑定提交事件会重新刷新页面
        加上prevent后不会重新刷新页面
  -->
  <form>
    <input type="text" value="123" @keydown.enter.prevent="father">
  </form>
```
### 七、事件修饰符passive
```html
<!-- 对移动端的性能提升较大,当滚动行为发生时就立即触发事件，而不是等onScroll完成 -->
```
### 八、事件修饰符可以嵌套
```html
<!-- 修饰符可以嵌套
  这样就表示father只能触发一次，且只能由自己触发(嵌套发生有先后顺序)
-->
<div @click.once.self="father">
  father
  <div @click="son">son</div>
</div>
```