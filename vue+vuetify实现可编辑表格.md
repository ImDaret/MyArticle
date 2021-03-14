---
title: 可编辑表格
image: /image/9.jpg  #设置本地图片
---
![](https://user-gold-cdn.xitu.io/2020/7/26/1738b463bfb83bc3?w=1903&h=563&f=gif&s=355009)
我查遍整个vuetify官网，里面只有弹出对话框的可编辑表格，并没有这种在表格内就可编辑的功能，所以写了这篇文章，以便大家备一手，学会这招你可以秒天秒地秒空气，防老师防领导防项目经理。

HTML代码如下:
```html
<div id="app">
    <v-app>
      <template>
        <v-card style="width:70%; margin: 200px auto;">
          <v-card-title>
            可编辑表格
          </v-card-title>
        <v-simple-table id="tab">
          <template v-slot:default>
            <thead>
              <!--  表头 -->
              <tr>
                <th class="text-left">Name</th>
                <th class="text-left">Calories</th>
                <th class="text-left">fat</th>
                <th class="text-left">carbs</th>
                <th class="text-left">protein</th>
                <th class="text-left">iron</th>
                <th class="text-left">actions</th>
              </tr>
            </thead>
            <tbody>
              <!-- 表格内容 -->
              <tr v-for="item in desserts" :key="item.id">
                <td><v-text-field
                  v-model="item.name"
                  :readonly="item.readonly"
                  autofocus
                ></v-text-field></td>
                <td>
                  <v-text-field
                  v-model="item.calories"
                  :readonly="item.readonly"
                  autofocus
                ></v-text-field></td>
                <td>
                  <v-text-field
                  v-model="item.fat"
                  :readonly="item.readonly"
                  autofocus
                ></v-text-field></td>
                <td><v-text-field
                  v-model="item.carbs"
                  :readonly="item.readonly"
                  autofocus
                ></v-text-field></td>
                <td><v-text-field
                  v-model="item.protein"
                  :readonly="item.readonly"
                  autofocus
                ></v-text-field></td>
                <td><v-text-field
                  v-model="item.iron"
                  :readonly="item.readonly"
                  autofocus
                ></v-text-field></td>
                <template>
                  <!-- 按钮区域 -->
                <td>
                  <!-- 非修改界面显示修改，修改界面显示保存 -->
                  <v-btn rounded color="primary" dark @click="editItem(item)" small style="margin-right: 15px;">{{ item.readonly? "修改":"保存" }}</v-btn>
                  <!-- 非修改界面显示删除 -->
                  <v-btn rounded color="error" dark small v-if="item.readonly" @click="delItem(item)">删除</v-btn>
                  <!-- 修改界面显示取消 -->
                  <v-btn rounded color="success" dark small v-if="!item.readonly" @click="cancelItem(item)">取消</v-btn>
                </td>
              </template>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
        </v-card>
      </template>
    </v-app>
  </div>
```
JS代码如下：
```
<!-- 引入vue -->
  <script src="https://cdn.jsdelivr.net/npm/vue@2.x/dist/vue.js"></script>
  <!-- 引入vuetify -->
  <script src="https://cdn.jsdelivr.net/npm/vuetify@2.x/dist/vuetify.js"></script>
  <script>
    new Vue({
      el: '#app',
      vuetify: new Vuetify(),
      data () {
      return {
        // 表格数据
        desserts: [
          {
            id: 1,
            name: 'Frozen Yogurt',
            calories: 159,
            fat: 6.0,
            carbs: 24,
            protein: 4.0,
            iron: '1%',
            readonly: true
          },
          {
            id: 2,
            name: 'Ice cream sandwich',
            calories: 237,
            fat: 9.0,
            carbs: 37,
            protein: 4.3,
            iron: '1%',
            readonly: true
          },
          {
            id: 3,
            name: 'Eclair',
            calories: 262,
            fat: 16.0,
            carbs: 23,
            protein: 6.0,
            iron: '7%',
            readonly: true
          },
        ],
        editedItem: {
          id: 0,
          name: '',
          calories: 0,
          fat: 0,
          carbs: 0,
          protein: 0,
          iron: '',
          readonly: true
        },
        editedIndex: -1,
      }
    },
    methods: {
      // 修改数据（保存数据)
      editItem(item) {
        this.editedIndex = this.desserts.indexOf(item);//先找到desserts数组里面对应的索引，通俗点说就是确定修改哪一行数据
        this.editedItem = Object.assign({}, item);//把未修改之前的值存到editedItem对象里面，方便用户取消修改
        //以上两行主要是为取消修改服务，要实现修改其实只需下面一行就够了，因为html中本身的标签为<v-text-field>,也就是说只需控制它的只读和非只读来回切换即可做到修改保存
        item.readonly = !item.readonly;
      },
      // 删除数据
      delItem(item) {
        const index = this.desserts.indexOf(item);//找到desserts数组里面对应的索引，通俗点说就是确定删除哪一行数据
        confirm('Are you sure you want to delete this item?') && this.desserts.splice(index, 1);//系统弹出确认框，点击确定就是删除这一行数据
      },
      // 取消
      cancelItem(item) {
        item.readonly = !item.readonly;//切换文本框的读写状态
        this.$nextTick(() =>{
        Object.assign(this.desserts[this.editedIndex], this.editedItem);//点击取消之后，把未修改之前的数据还原到desserts数组
        this.editedIndex = -1;//把索引标志置空
        })
      },
      }
    })
  </script>
```
总结：这是我能想到的最最最简单的可编辑表格实现了，萌新小白仔细看我的注释，细细品估计也能看懂，在实际开发中，为了总体更加美观 ，大家大可不必像我一样控制文本框的编辑功能，而是可以用一个`span`标签和`v-text-field`标签来回切换他们的显示状态即可，原理跟这个一样，我就懒得改了（狗头保命），有问题或者想要了解更多前端知识欢迎下方留言讨论。
​