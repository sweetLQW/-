# Vue Router

1. #### 路由的简单实现

   - 使用 router-link 组件来导航，通过传入 `to` 属性指定链接，router-link 默认会被渲染成一个 a 标签
   - 路由匹配到的组件将渲染在router-view中
   - Vue组件中可以通过this.$router访问全局路由器
   - Vue组件中可以通过this.$route访问当前路由

   ```html
   <body>
       <div id="app">
           <router-link to="/a">go to A.page</router-link>
           <br>
           <router-link to="/b">go to B.page</router-link>
           <br>
         
           <h4>下面是路由页面</h4>
           <button @click="goBack()">返回上一级</button>
           <!-- 路由匹配到的组件将渲染在这里 -->
           <router-view></router-view>
       </div>
   
       <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
       <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
       <script>
           const A = { template: "<h4>A page</h4>"};
           const B = { template: "<h4>B page</h4>"};
   
           const router = new VueRouter({
               routes: [
                   { path: '/a', component: A },
                   { path: '/b', component: B }
               ]
           })
   
           const vm = new Vue({
               el: '#app',
               router,
               methods: {
                   goBack: function() {
                       this.$router.go(-1);
                   }
               },
               created: function() {
                   // 路由器
                   console.log(this.$router);
                   // 当前路由
                   console.log(this.$route);
               }
           })
   
       </script>
   </body>
   ```

   

2. 动态路由的实现

   - 当使用路由参数时，例如从 `/user/foo` 导航到 `/user/bar`，**原来的组件实例会被复用**。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。**不过，这也意味着组件的生命周期钩子不会再被调用**。
   - 如果想对路由参数的变化作出响应的话，你可以简单地 watch (监测变化) `$route` 对象。

   ```html
   <body>
       <div id="app">
           <router-link to="/b/1">go to B-1.page</router-link>
           <br>
           <router-link to="/b/2">go to B-2.page</router-link>
           <br>
           <h4>下面是路由页面</h4>
           <!-- url中如果有路径参数 -->
           <p>{{ $route.params }}</p>
           <!-- url中如果有查询参数 -->
           <p>{{ $route.query }}</p>
   
           <!-- 路由匹配到的组件将渲染在这里 -->
           <router-view></router-view>
       </div>
   
       <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
       <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
       <script>
           const B = { template: "<h4>B page</h4>"};
   
           const router = new VueRouter({
               routes: [
                   { path: '/b/:num', component: B }
               ]
           })
   
           const vm = new Vue({
               el: '#app',
               router
           })
   
       </script>
   </body>
   ```

   还可以使用**通配符及正则表达式**匹配路由。

   ```
   {
     // 会匹配所有路径
     path: '*'
   }
   {
     // 会匹配以 `/user-` 开头的任意路径
     path: '/user-*'
   }
   ```

   当使用一个*通配符*时，`$route.params` 内会自动添加一个名为 `pathMatch` 参数。它包含了 URL 通过*通配符*被匹配的部分：

   ```js
   // 给出一个路由 { path: '/user-*' }
   this.$router.push('/user-admin')
   this.$route.params.pathMatch // 'admin'
   // 给出一个路由 { path: '*' }
   this.$router.push('/non-existing')
   this.$route.params.pathMatch // '/non-existing'
   ```



3. 嵌套路由

   ```html
   <body>
       <div id="app">
           <router-link to="/a">go to A.page</router-link>
           <br>
           <router-link to="/a/children1">go to A-children1.page</router-link>
           <br>
           <router-link to="/a/children2">go to A-children2.page</router-link>
           <br>
   
           <h4>下面是路由页面</h4>
           <router-view></router-view>
       </div>
   
       <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
       <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
       <script>
           const A = { template: "<div><h4>A page</h4><router-view></router-view></div>"};
           const AChildren1 = { template: "<h4>A Children-1 page</h4>" };
           const AChildren2 = { template: "<h4>A Children-2 page</h4>" };
   
           const router = new VueRouter({
               routes: [
                   // children用于嵌套路由
                   { 
                       path: '/a', 
                       component: A, 
                       children: [
                           {
                               path: 'children1',
                               component: AChildren1,
                           },
                           {
                               path: 'children2',
                               component: AChildren2,
                           }
                       ] 
                   }
               ]
           })
   
           const vm = new Vue({
               el: '#app',
               router
           })
   
       </script>
   </body>
   ```



4. 编程式路由

   之前我们都是使用router-link（声明式）进行路由跳转，还有一种方式就是编程式路由，下面举个栗子：

   ```javascript
   // 字符串
   router.push('home')
   
   // 对象
   router.push({ path: 'home' })
   
   // 命名的路由
   router.push({ name: 'user', params: { userId: '123' }})
   
   // 带查询参数，变成 /register?plan=private
   router.push({ path: 'register', query: { plan: 'private' }})
   ```

   Vue Router 的导航方法 (`push`、 `replace`、 `go`)，详情参考文档https://router.vuejs.org/zh/guide/essentials/navigation.html

   

5. 