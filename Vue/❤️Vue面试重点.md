# Vue重点

1. ### Vue的优缺点

   - 优点

     - 轻量级框架，学习成本低
     - 组件化，可复用性高
     - 前后端分离，开发效率高
     - 使用MVVM模式，视图、数据、逻辑分离，支持双向数据绑定，开发者只需关注业务逻辑即可
     - SPA，用户体验更好

   - 缺点

     - 不支持低版本浏览器
     - 不利于SEO
     - SPA页面首次加载慢

     

2. ### 组件之间如何传值

   #### 通信一般分为父子组件通信、兄弟组件通信、隔代（跨级）组件通信。

   

   1. ##### props / $emit + events （适用于父子组件通信）

      ```html
      <body>
          <div id="app">
              <p>父组件中的value：{{ value }}</p>
              <com-child :value="value" @value-changed="updateValue"></com-child>
          </div>
      
          <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
          <script>
              var comChild = {
                  props: ['value'],
                  template: `
                      <div class="child">
                          <p>子组件中的value：{{ childValue }}</p>
                          <button @click="changeValue()">点击我</button>
                      </div>
                  `,
                  data: function() {
                      return {
                          childValue: this.value
                      }
                  },
                  methods: {
                      changeValue: function() {
                          this.childValue++;
                          this.$emit('value-changed', this.childValue);
                      }
                  }
              }
      
              var vm = new Vue({
                  el: '#app',
                  data: {
                      value: 0
                  },
                  methods: {
                      updateValue: function(newValue) {
                          this.value = newValue;
                      }
                  },
                  components: {
                      'com-child': comChild
                  }
              })
              
          </script>
      </body>
      ```

      

   2. ##### $emit + $on（适用于父子、兄弟、隔代）

      ```html
      <body>
          <div id="app">
              <p>父组件中的value：{{ value }}</p>
              <com-child :value="value"></com-child>
          </div>
      
          <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
          <script>
            	// 空的vue实例作为事件中心，$emit用来触发事件，$on用来监听事件
              var Event = new Vue();
      
              var comChild = {
                  props: ['value'],
                  template: `
                      <div class="child">
                          <p>子组件中的value：{{ childValue }}</p>
                          <button @click="changeValue()">点击我</button>
                      </div>
                  `,
                  data: function() {
                      return {
                          childValue: this.value
                      }
                  },
                  methods: {
                      changeValue: function() {
                          this.childValue++;
                          Event.$emit('value-changed', this.childValue);
                      }
                  }
              }
      
              var vm = new Vue({
                  el: '#app',
                  data: {
                      value: 0
                  },
                  components: {
                      'com-child': comChild
                  },
                  created: function() {
                      Event.$on('value-changed', data => {
                          this.value = data;
                      })
                  }
              })
              
          </script>
      </body>
      ```

      

   3. ##### $attrs + $listeners（适用于父子、隔代）

      **$listeners**: 包含了父作用域中的 (不含 `.native` 修饰器的) `v-on` 事件监听器。可以通过 `v-on="$listeners"` 传入内部组件。

      **$attrs**: 包含了父作用域中不作为 prop 被识别 (且获取) 的 attribute 绑定 (`class` 和 `style` 除外)。可以通过 `v-bind="$attrs"` 传入内部组件。

      

      ```html
      <body>
          <div id="app">
              <p>name: {{ name }}</p>
              <p>age: {{ age }}</p>
              <com-child :name="name" :age="age" @update="updateData"></com-child>
          </div>
      
          <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
          <script>
              var comChildChild = {
                  template: `<p>com-child-child: {{ $attrs }}</p>`,
                  created: function() {
                      console.log('com-child-child继承的参数:', this.$attrs);
                      console.log('com-child-child继承的事件:', this.$listeners);
                      // 孙子组件中修改name，同步给父组件
                      this.$attrs.name = 'cindy';
                      this.$emit('update', 'name', this.$attrs.name);
                  }
              }
      
              var comChild = {
                  template: `
                      <div class="child">
                          <p>com-child: {{ $attrs }}</p>
                          <com-child-child 
                              v-bind="$attrs" 
                              v-on="$listeners">
                          </com-child-child>
                      </div>
                  `,
                  components: { comChildChild },
                  created: function() {
                      console.log('com-child继承的参数:', this.$attrs);
                      console.log('com-child继承的事件:', this.$listeners);
                  }
              }
      
              var vm = new Vue({
                  el: '#app',
                  data: {
                      name: 'lily',
                      age: 25
                  },
                  methods: {
                      updateData: function(key, value) {
                          this[key] = value;
                      }
                  },
                  components: { comChild }
              })
              
          </script>
      </body>
      ```

      

   4. ##### provide / inject

      `provide` 和 `inject` 主要在开发高阶插件/组件库时使用。并不推荐用于普通应用程序代码中。

      这对选项需要一起使用，以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效。

      提示：`provide` 和 `inject` 绑定并不是可响应的。这是刻意为之的。然而，如果你传入了一个可监听的对象，那么其对象的 property 还是可响应的。

      

      ```html
      <body>
          <div id="app">
              <com-child></com-child>
          </div>
      
          <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
          <script>
              var comChildChild = {
                  template: `<p>com-child-child: {{ value }}</p>`,
                  inject: [ 'value' ]
              }
      
              var comChild = {
                  template: `<div>
                              com-child: {{ value }}
                              <com-child-child></com-child-child>
                            </div>`,
                  components: { comChildChild },
                  inject: [ 'value' ]
              }
      
              var vm = new Vue({
                  el: '#app',
                  provide: {
                      value: 1
                  },
                  components: { comChild }
              })
              
          </script>
      </body>
      ```

      

   5. ##### $parent / $children / $refs

      -  ref 被用来给元素或子组件注册引用信息。
      - 引用信息将会注册在父组件的 $refs 对象上。 
      - 如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素。
      - 如果用在子组件上，引用就指向组件实例。
      - $refs需要在挂载完成后（mounted）访问。
      - $refs 也不是响应式的，因此你不应该试图用它在模板中做数据绑定。
      - 子实例可以用 `this.$parent` 访问父实例，子实例被推入父实例的 `$children` 数组中。
      - 需要注意$children、$parent不是响应式的。并且 $children 并不保证顺序。

      

      ```html
      <body>
          <div id="app">
              <p ref="p"></p>
              <com-child ref="child"></com-child>
          </div>
      
          <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
          <script>
              var comChild = {
                  template: `<p>com-child: {{ childValue }}</p>`,
                  data: function() {
                      return {
                          childValue: 2
                      }
                  },
                  mounted: function() {
                      console.log('子组件中获取父组件的值：', this.$parent.value);
                  }
              }
      
              var vm = new Vue({
                  el: '#app',
                  components: { comChild },
                  data: {
                      value: 1
                  },
                  mounted: function() {
                      var child = this.$children[0];
                      var childRef = this.$refs.child;
                      var p = this.$refs.p;
                      console.log('普通DOM元素元素', p);
                      console.log('父组件中获取子组件的值：', child.childValue, childRef.childValue);
                  }
              })
              
          </script>
      </body>
      ```

      

   6. ##### Vuex

      

   

   #### 总结

   - 父传子
     - props
     - $attrs
     - provide / inject
   - 父传孙（跨级向下传递）
     - $attrs
     - provide / inject
   - 子传父
     - $emit + events
     - $emit + $on
   - 孙传父（跨级向上传递）
     - $listeners  / $emit + events
   - 兄弟之间互相传值

   

1. ### Vue支持的路由方式有几种

   1. hash模式 

   2. history模式

   3. abstract模式

      

2. ### 如何定义vue-router的动态路由以及如何获取传过来的动态参数

   

3. ### `v-show`与`v-if`区别

   

4. ### `<keep-alive></keep-alive>`的作用

   

5. ### 为什么避免`v-for`与`v-if`一起使用

   

6. ### SPA的优缺点

   

7. ### 如何优化SPA首屏加载速度慢的问题

   

8. ### 父组件如何监听子组件的生命周期

   

9. ### 谈谈对SSR的理解

   

10. ### Vue如何监听某个属性值的变化

    

11. ### Vue如何自定义过滤器

    