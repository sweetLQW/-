# Vuex基础

## 特点

- Vuex 是一个专为 Vue.js 应用程序开发的`响应式状态管理模式`。

- 它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

- Vuex更适合开发大型单页应用。

- Vuex应用的核心是`store`（仓库），store包含应用中大部分的状态（`state`）。

- 不能直接更改状态，需要显示提交（commit）mutation。



##  State & Mutation

在Vue组件中获取Vuex状态 & 更改Vuex状态

```html
<body>
    <div id="app">
        <p>{{ $store.state.num }}</p>
        <p>{{ value }}</p>
        <button @click="changeNum()">点击num++</button>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vuex"></script>
    <script>
        const store = new Vuex.Store({
            state: {
                num: 0,
                value: 1
            },
            mutations: {
                addNum: function(state) {
                    state.num++;
                }
            }
        })

        const vm = new Vue({
            el: '#app',
            // 以下内容相当于store: store, 
            // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
            // 注入store后，可以使用this.$store访问
            store,
            methods: {
                changeNum: function() {
                    store.commit('addNum');
                    console.log(store.state.num);
                    console.log(this.$store.state.num);
                }
            },
            computed: {
                'value': function() {
                    return store.state.value;
                }
            }
        })
    </script>
</body>
```

注意：上面代码中，可以直接在模板中使用某个状态，也可以使用计算属性返回某个状态。如果想保存多个状态的话，可以使用mapState函数（详情请参考https://vuex.vuejs.org/zh/guide/state.html）。



## Getter

Vuex 允许我们在 store 中定义`getter`（可以认为是 store 的计算属性）。就像计算属性一样，`getter` 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

```html
<body>
    <div id="app">
        <p>{{ $store.getters.filterListLen }}</p>
        <ul>
            <li v-for="item in $store.getters.filterList">name：{{ item.name }}</li>
        </ul>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vuex"></script>
    <script>
        const store = new Vuex.Store({
            state: {
                num: 0,
                value: 1,
                list: [
                    {name: 'lily', age: 25},
                    {name: 'cindy', age: 18},
                    {name: 'sunny', age: 21},
                ]
            },
            getters: {
                filterList: state => {
                    return state.list.filter(ele => ele.age > 20);
                },
                // 第二个参数getters
                filterListLen: (state, getters) => getters.filterList.length
            }
        })

        const vm = new Vue({
            el: '#app',
            store
        })
    </script>
</body>
```



## Action

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。

- Action 可以包含任意异步操作。

- Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 `context.commit` 提交一个 mutation，或者通过 `context.state` 和 `context.getters` 来获取 state 和 getters。

- Action 通过 `store.dispatch` 方法触发。

- mutation 必须同步执行，Action 就不受约束！

  

```html
<body>
    <div id="app">
        <p>{{ $store.state.num }}</p>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vuex"></script>
    <script>
        const store = new Vuex.Store({
            state: {
                num: 0,
                value: 1
            },
            mutations: {
                addNum: function(state) {
                    state.num++;
                }
            },
            actions: {
                increment (context) {
                    setTimeout(function() {
                        context.commit('addNum');
                    }, 2000);
                }
            }
        })

        const vm = new Vue({
            el: '#app',
            store,
            mounted: function() {
                this.$store.dispatch('increment');
            }
        })

    </script>
</body>
```



## Module

当应用复杂时，store对象可能变得非常臃肿，所以Vuex提供了module来分割模块。

```html
<body>
    <div id="app">
        <p>{{ $store.state.a.num }}</p>
        <p>{{ $store.state.b.num }}</p>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vuex"></script>
    <script>
        const moduleA = {
            state: () => ({ num: 0 }),
            mutations:{},
            actions:{},
            getters:{}
        };
        const moduleB = {
            state: () => ({ num: 1 }),
            mutations:{},
            actions:{},
            getters:{}
        };
        const store = new Vuex.Store({
            modules: {
                a: moduleA,
                b: moduleB
            }
        })

        const vm = new Vue({
            el: '#app',
            store
        })
    </script>
</body>
```




