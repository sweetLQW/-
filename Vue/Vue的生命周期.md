# Vue的生命周期

![Vue生命周期钩子](%E2%9D%A4%EF%B8%8FVue%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.assets/Vue%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90.jpg)



#### Vue生命周期特点：

1. 判断`vm`（vm指Vue实例）中是否有`el`选项，如果有会继续执行到`mounted`完毕后结束。如果没有，到`created`执行完毕后结束，直到遇到`vm.$mount(el)`函数时再继续执行`beforeMount`及`mounted`钩子函数。

2. 如果有template模板，则将template编译到render函数中。反之将html作为template进行编译。

3. 如果template模板与render函数同时存在时，使用render函数。

4. 有template模板情况下，beforeMount钩子中，vm.$el为html默认模板。mounted钩子中，vm.$el变成template模板。   

   ```html
   <body>
       <div id="app">
           <p>我是默认html模板</p>
       </div>
   
       <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
       <script>
           var vm = new Vue({
               el: '#app',
               template: '<p>我是template编译的render</p>',
               data: {
                   num: 0
               },
               beforeCreate: function() {
                   console.log('------------------------------------');
                   console.log('beforeCreate钩子');
                   console.log(this.num, this.$el);
               },
               created: function() {
                   console.log('------------------------------------');
                   console.log('created钩子：实例被创建之后执行');
                   console.log(this.num, this.$el);
               },
               beforeMount: function() {
                   console.log('------------------------------------');
                   console.log('beforeMount钩子');
                   console.log(this.num, this.$el);
               },
               mounted: function() {
                   console.log('------------------------------------');
                   console.log('mounted钩子');
                   console.log(this.num, this.$el);
               }
           }) 
       </script>
   </body>
   ```
   

   Result：

   ![image-20200503121048758](%E2%9D%A4%EF%B8%8FVue%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.assets/image-20200503121048758.png)

5. beforeCreate和created钩子中执行this.$nextTick(fun)，相当于把内容延迟到mounted钩子中执行。

   ```html
   <body>
       <div id="app">
           <p>我是默认html模板</p>
       </div>
   
       <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
       <script>
           var vm = new Vue({
               el: '#app',
               data: {
                   num: 0
               },
               beforeCreate: function() {
                   console.log('------------------------------------');
                   console.log('beforeCreate钩子：');
                   console.log(this.num, this.$el);
               },
               created: function() {
                   console.log('------------------------------------');
                   console.log('created钩子：实例被创建之后执行');
                   console.log(this.num, this.$el);
                   this.$nextTick(function() {
                       console.log('nextTick开始执行');
                       console.log(this.num, this.$el);
                   })
               },
               beforeMount: function() {
                   console.log('------------------------------------');
                   console.log('beforeMount钩子：');
                   console.log(this.num, this.$el);
               },
               mounted: function() {
                   console.log('------------------------------------');
                   console.log('mounted钩子：');
                   console.log(this.num, this.$el);
               }
           }) 
       </script>
   </body>
   ```

   Result:

   ![image-20200503124621754](%E2%9D%A4%EF%B8%8FVue%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.assets/image-20200503124621754.png)