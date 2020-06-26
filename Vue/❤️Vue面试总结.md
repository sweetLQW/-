# Vue重点

## Vue的优缺点

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

  

## 组件之间如何传值

通信一般分为父子组件通信、兄弟组件通信、隔代（跨级）组件通信。

#### props / $emit + events （适用于父子）

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



#### $emit + $on（适用于父子、兄弟、隔代）

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



#### $attrs + $listeners（适用于父子、隔代）

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



#### provide / inject（适用于父子）

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



#### $parent / $children / $refs（适用于父子）

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



#### [Vuex](/Users/lqw/Desktop/前端知识点总结/Vue/❤️Vuex.md)（适用于父子、兄弟、隔代）



#### 总结

   - 父传子
     - props
     - $attrs
     - provide / inject
     - $parent（非响应式）
     - vuex
   - 父传孙（跨级向下传递）
     - $attrs
     - provide / inject
     - vuex
   - 子传父
     - $emit + events
     - $emit + $on
     - $listeners + events
     - $refs / $children（非响应式）
     - vuex
   - 孙传父（跨级向上传递）
     - $listeners  / $emit + events
     - vuex
   - 兄弟之间传值
     - $emit + $on
     - vuex

   

## Vue的路由模式有哪几种

- `hash`: 使用 URL hash 值来作路由。支持所有浏览器，包括不支持 HTML5 History Api 的浏览器。
- `history`: 依赖 HTML5 History API 和服务器配置。查看 [HTML5 History 模式](https://router.vuejs.org/zh/guide/essentials/history-mode.html)。
- `abstract`: 支持所有 JavaScript 运行环境，如 Node.js 服务器端。**如果发现没有浏览器的 API，路由会自动强制进入这个模式。**



## 路由按需加载有几种方式

使用vue-cli构建项目后，router/index.js路由文件体积会非常大，如果一开始就全部加载的话，会导致加载非常缓慢，用户体验差，所以按需加载就显得非常重要。

#### 1. Webpack - require ensure()

webpack 在编译时，会静态地解析代码中的 `require.ensure()`，同时将模块添加到一个分开的 chunk 当中。这个新的 chunk 会被 webpack 通过 `jsonp` 来按需加载。指定相同的chunkName的路由，会被合并打包成同一个js文件。

```js
require.ensure(dependencies: String[], callback: function(require), chunkName: String)
```

**依赖 dependencies**

这是一个字符串数组，通过这个参数，在所有的回调函数的代码被执行前，我们可以将所有需要用到的模块进行声明。

**回调 callback**

当所有的依赖都加载完成后，webpack会执行这个回调函数。`require` 对象的一个实现会作为一个参数传递给这个回调函数。因此，我们可以进一步 `require()` 依赖和其它模块提供下一步的执行。

**chunk名称 chunkName**

chunkName 是提供给这个特定的 `require.ensure()` 的 chunk 的名称。通过提供 `require.ensure()` 不同执行点相同的名称，我们可以保证所有的依赖都会一起放进相同的 文件束(bundle)。

```js
{
  path: '/home',
  name: 'home',
  component: r => require.ensure([], () => r(require('@/home')), 'chunkName')
}
```



#### 2. vue异步组件技术

每个组件会被单独打包成一个js文件

```js
{
  path: '/home',
  name: 'home',
  component: resolve => require(['@/home'], resolve)
}
```



#### 3. 使用动态的import()语法

- 结合 Vue 的异步组件和 Webpack 的代码分割功能，轻松实现路由组件的懒加载。

- 定义相同webpackChunkName的会被打包成一个js文件

- 未定义webpackChunkName每个组件会被单独打包成一个js文件。

```js
const Home =  () => import(/* webpackChunkName: 'group1' */ '@/components/home');
const Foo =  () => import(/* webpackChunkName: 'group1' */ '@/components/foo');
const Bar =  () => import('@/components/bar');

{
  path: '/home',
  name: 'home',
  component: Home
},
{
  path: '/home',
  name: 'home',
  component: Foo
},
  {
  path: '/home',
  name: 'home',
  component: Bar
}
```





## vue-router的动态路由以及如何获取动态参数

- 通过/:name的形式来定义动态路由
- 使用$route.params.name来获取参数

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



## `v-show`与`v-if`区别

- `v-if`在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建
- `v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
- `v-show` 不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS（display） 进行切换。
- 一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。
- 尽量避免v-if与v-for用在同一个元素上。



## `<keep-alive></keep-alive>`的作用

保存组件的状态，避免反复渲染



## 为什么避免`v-for`与`v-if`一起使用

当它们处于同一节点的时候，v-for比v-if优先级高，不管需要展示多少数据，他都会遍历整个数组，并且每次都会计算v-if，影响性能，当需要过滤一些数据的时候，可以使用computed属性。



## SPA的优缺点

优点

- 良好的交互体验，减少了不必要的跳转和重复的渲染。
- 经典MVVM开发模式，前后端职责分离，前端负责view，后端负责module。
- 减轻服务端压力

缺点

- 初始化加载慢

- 不利于SEO

  

## 如何优化SPA首屏加载速度慢的问题

- 减小入口文件体积
  - 路由懒加载
  - 代码分块，按需加载
  - js库采用CDN引入，减少app.bundel大小
- 开启本地缓存
  - 缓存HTTP请求：分为协商缓存（Last-Modified/If-Modified-Since或Etag/If-None-Match）和强缓存（Cache-Control & Expires）。
  - Service Worker离线缓存，只适用于HTTPS。
- 文件压缩
  - 服务端开启gzip压缩
- SSR

- 图片懒加载（vue-lazyload）
- 展示loading图片，优化用户体验



## 父组件如何监听子组件的生命周期

- 方法一：

  ```html
  <body>
      <div id="app">
          <com-child @mounted="childMounted"></com-child>
      </div>
  
      <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
      <script>
          var comChild = {
              template: `<div class="child">子组件</div>`,
              mounted: function() {
                  console.log('子组件的mounted已触发');
                  this.$emit('mounted');
              }
          }
  
          var vm = new Vue({
              el: '#app',
              components: {comChild},
              methods: {
                  childMounted: function() {
                      console.log('父组件监听到子组件的mounted了');
                  }
              },
              mounted: function() {
                  console.log('父组件的mounted已触发');
              }
          })
      </script>
  </body>
  ```

  

- 方法二：使用@hook

  ```html
  <body>
      <div id="app">
          <com-child @hook:mounted="childMounted"></com-child>
      </div>
  
      <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
      <script>
          var comChild = {
              template: `<div class="child">子组件</div>`,
              mounted: function() {
                  console.log('子组件的mounted已触发');
              }
          }
  
          var vm = new Vue({
              el: '#app',
              components: {comChild},
              methods: {
                  childMounted: function() {
                      console.log('父组件监听到子组件的mounted了');
                  }
              },
              mounted: function() {
                  console.log('父组件的mounted已触发');
              }
          })
      </script>
  </body>
  ```

  

## 谈谈对SSR & 预渲染的理解

**Vue.js 是构建客户端应用程序的框架。默认情况下，可以在浏览器中输出 Vue 组件，进行生成 DOM 和操作 DOM。然而，也可以将同一个组件渲染为服务器端的 HTML 字符串，将它们直接发送到浏览器，最后将这些静态标记"激活"为客户端上完全可交互的应用程序。**

服务端渲染的优点：

- 更好的SEO
- 更快的内容到达时间

需要衡量的地方：

- 开发条件所限，一些扩展库在服务端可能需要特殊处理。
- 涉及构建设置和部署的更多要求。与可以部署在任何静态文件服务器上的完全静态单页面应用程序 (SPA) 不同，服务器渲染应用程序，需要处于 Node.js server 运行环境。
- 更多的服务器端负载。



**预渲染：**

在构建时 (build time) 简单地生成针对特定路由的静态 HTML 文件。优点是设置预渲染更简单，并可以将你的前端作为一个完全静态的站点。如果需要与渲染则可以使用webpack的 [prerender-spa-plugin](https://github.com/chrisvfritz/prerender-spa-plugin)。



## Vue如何监听某个属性值的变化

监听整个对象变化：

```html
<body>
    <div id="app">
        <p>{{ obj }}</p>
        <button @click="changeData()">点击更改obj</button>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        const vm = new Vue({
            el: '#app',
            data: {
                obj: {
                    name: 'lily',
                    age: 25,
                    list: [1, 2, 3]
                }
            },
            methods: {
                changeData: function() {
                    vm.obj.name = 'cindy';
                    vm.$set(vm.obj.list, 0, 3);
                }
            },
            watch: {
                obj: {
                    handler: function(newValue, oldValue) {
                        console.log(newValue);
                    },
                    deep: true
                }
            }
        })
    </script>
</body>
```

监听单个属性：

```html
<body>
    <div id="app">
        <p>{{ obj }}</p>
        <button @click="changeData()">点击更改obj</button>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        const vm = new Vue({
            el: '#app',
            data: {
                obj: {
                    name: 'lily',
                    age: 25,
                    list: [1, 2, 3]
                }
            },
            methods: {
                changeData: function() {
                    vm.obj.name = 'cindy';
                    vm.$set(vm.obj.list, 0, 3);
                }
            },
            watch: {
                'obj.name': function() {
                    console.log(vm.obj);
                },
                'obj.list': function() {
                    console.log(vm.obj);
                }
            }
        })
    </script>
</body>
```



## Vue如何自定义过滤器

```html
<body>
    <div id="app">
        <p>{{ list | filterList }}</p>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        const vm = new Vue({
            el: '#app',
            data: {
                list: [
                    { name: 'lily', age: 25},
                    { name: 'cindy', age: 18},
                    { name: 'sunny', age: 21}
                ]
            },
            filters: {
                filterList: function(data) {
                    return data.filter((ele) => ele.age > 20);
                }
            }
        })
    </script>
</body>
```

高级用法：传参数

```html
<body>
    <div id="app">
        <p>{{ num | filterNum(1, 2) }}</p>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        const vm = new Vue({
            el: '#app',
            data: {
                num: 0
            },
            filters: {
                filterNum: function(data, arg1, arg2) {
                    console.log(arg1, arg2)
                    return data + arg1 + arg2;
                }
            }
        })
    </script>
</body>
```



## Vue3.0特性

- 压缩包体积减小一半（10kb左右）
- 支持typeScript
- 响应式改用Proxy
  - 因为defineProperty是在初始化的时候遍历对象，给每个对象设置defineProperty方法，新添加的属性不拥有响应式变化。
  - Proxy则是代理整个对象，省去了遍历的操作，对性能有很大提升，新添加的属性也可以进行响应式操作。
  - Proxy提供多种拦截方法，如apply、ownKeys、deleteProperty、has等



## title

