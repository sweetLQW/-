# JS面试总结

## 事件冒泡及事件捕获

事件冒泡：从目标元素逐级向上访问

事件捕获：从顶级元素向目标元素逐步访问

addEventListener() 方法用于向指定元素添加事件句柄。

`element.addEventListener(event, function, useCapture)`

`useCapture`: false（默认值），事件在冒泡阶段执行；true，事件在捕获阶段执行。



## 阻止事件冒泡

其他：event.stopPropagation()

IE:：event.cancelBubble = true



## 事件委托（事件代理）

利用事件冒泡的原理，把子级的事件绑定在父级上。

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>事件委托</title>
</head>
<body>
	<input type="text" class="txt">
	<button class="btn">添加</button>
	<p>喜欢的水果：</p>
	<ul class="list">
		<li>香蕉 <span class="del">删除</span></li>
		<li>葡萄 <span class="del">删除</span></li>
		<li>毛桃 <span class="del">删除</span></li>
		<li>油桃 <span class="del">删除</span></li>
		<li>苹果 <span class="del"><span>删除</span></span></li>
	</ul>

	<script>
		var oTxt = document.querySelector(".txt");
		var oBtn = document.querySelector(".btn");
		var oList = document.querySelector(".list");
		oBtn.onclick = function(){
			if(oTxt.value){
				var span = document.createTextNode(" 删除");
				var oSpan = document.createElement("span");
				oSpan.appendChild(span);
				oSpan.className = "del";
				var value = oTxt.value;//文本的内容赋给value
				var oStr = document.createTextNode(value);//创建文本节点,li里面的内容
				var oLi = document.createElement("li");//创建元素节点li
				oLi.appendChild(oStr);//给li添加文本
				oLi.appendChild(oSpan);
				oList.appendChild(oLi);//给ul添加li
				oTxt.value = "";//点击按钮时把TXT内容清空
			}
		}
		oList.onclick = function(event){
			var target = event.target;
			console.log(event, target);
			if(target.className == "del"){
				this.removeChild(target.parentNode);
				return;
			}
			for(let item of event.path) {
				console.log(item);
				if(item.className == "del"){
					this.removeChild(item.parentNode);
				}
			}
		}
	</script>
</body>
</html>
```



## 阻止元素默认行为

其他：event.preventDefault()

IE：event.returnValue = false



## 数组原型添加删除重复数据的方法，返回被删除的数据

```js
var arr = ["a", "b", "a", "c", "d", "e", "f", "c", "a"];
Array.prototype.distinct = function() {
  var removeList = [];
  for (var i = 0; i < this.length; i++) {
    for (var j = i+1; j < this.length;j++) {
      if (this[i] === this[j]) {
        removeList.push(this.splice(j, 1)[0]);
      }
    }
  }
  return removeList;
}

var result = arr.distinct();
console.log(result, arr);
```



## 查找两个节点的最近的一个共同父节点，可以包括节点自身

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>查找两个节点的最近的一个共同父节点，可以包括节点自身</title>
</head>
<body style="margin:0;padding:0">
    <div class="content">
        <div class="box">
            <p>dom1</p>
            <div>
                <p>dom2</p>
            </div>
        </div>
    </div>

    <script>
        commonParentNode(document.getElementsByTagName('p')[0], document.getElementsByTagName('p')[1]);
        
        function commonParentNode(oNode1, oNode2) {
            var on1_parents = getParents(oNode1);
            var on2_parents = getParents(oNode2);
            var i = on1_parents.length;
            var j = on2_parents.length;
            while(i >= 0 && j >= 0 && on1_parents[i] === on2_parents[j]) {
                i--;
                j--;
            }
            return on1_parents[i + 1];
        }

        function getParents(oNode) {
            var parents = [];
            var parent = oNode;
            while(parent) {
                parents.push(parent);
                parent = parent.parentNode;
            }
            return parents;
        }
    </script>
</body>
</html>
```



## bind方法的实现

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>手写js实现bind方法</title>
</head>
<body>
    <script>
        Function.prototype.myBind = function (obj) {
            const args = Array.prototype.slice.call(arguments);
            const fn = this;
            return function() {
                fn.apply(obj, args.slice(1));
            }
        }

        function foo(age = 0) {
            this.age = age;
            console.log(this);
        }

        const result = foo.myBind({name: 'lily'}, 25);
        result();

    </script>
</body>
</html>
```



## Event Loop

计算机系统的一种运行机制，js采用的也是这种机制，来解决单线程运行带来的一些问题，用于等待和发送消息和事件。

主线程：执行同步任务

微任务：MutationObserver及promise.resolve、promise.reject等

宏任务：setTimeout、setInterval



## 类似promise.all方法的实现

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>手写js实现Promise.all方法</title>
</head>
<body>
    <script>
        const pro1 = new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve('我是pro1的参数');
            }, 500);
        })

        const pro2 = function () {
            setTimeout(() => {
                console.log("我是pro2的参数");
            }, 2000);
        }

        const pro3 = new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve('我是pro3的参数');
            }, 1000);
        })

        function promiseAll() {
            let count = 0;
            const proList = Array.prototype.slice.call(arguments)[0],
                  resultList = [];
            return new Promise((resolve, reject) => {
                proList.forEach((ele, index) => {
                    if(Array.prototype.toString.call(ele) === '[object Promise]') {
                        ele.then(res => {
                            console.log(res);
                            resultList[index] = res;
                            count++;
                            count === proList.length && resolve(resultList);
                        }).catch(err => reject(err))
                    } else {
                        resultList[index] = ele;
                        count++;
                        count === proList.length && resolve(resultList);
                    }
                });
            })
        }

        promiseAll([pro1, pro2, pro3]).then(res => {
            console.log('result: ', res);
        })
    </script>
</body>
</html>
```



## instanceof判断原理

```js
[] instanceof Array
// true

[] instanceof Object
// true
```

判断左边的原型链（`[].__proto__`）上是否有右边的显示原型（Object.prototype）



## 实例化过程

- 创建一个空对象{}

- 将此对象的`__proto__`指向构造函数的`prototype`

- 将构造函数的作用域（属性方法this）赋值给新对象

- 返回新对象

  

## let与const

- let

  - 创建块级作用域（const同样）

  - 没有变量声明提升（const同样）

  - 会有暂时性死区（const同样）

    ```js
    var num = 1;
    if (true) {
      console.log(num); // 报错 Cannot access 'num' before initialization
      let num = 2;
    }
    ```

  - 不能重复定义，可以重新赋值

    ```js
    let num = 1;
    let num = 2; // 报错 Identifier 'num' has already been declared
    
    let num = 1;
    num = 2; // 成功
    ```

    

- const

  - 创建块级作用域

  - 没有变量声明提升

  - 会有暂时性死区

  - 不能重新赋值，不能重复定义

    ```js
    const num = 1;
    const num = 2; // 报错 Identifier 'num' has already been declared
    
    const num = 1;
    num = 2; // 报错 Assignment to constant variable.
    
    const obj = {}; 
    obj.num = 1; // 成功 如果是对象的话，可以操作对象属性
    ```

    


## 块级作用域与函数作用域

- 块级作用域没有声明提升，函数作用域有声明提升



## Proxy与Reflect

### Proxy

代理对象

### Reflect

ES6 为了操作对象而提供的新 API

- 将 Object 的属于语言内部的方法放到 Reflect 身上，例如Object.defineProperty()，现在可以使用Reflect.defineProperty()，以前提供的方法会在两者身上都部署，未来新方法只会在Reflect上部署。
- 修改某些Object方法的返回结果，以前Object.defineProperty如果遇到不能定义的属性会报错，Reflect.defineProperty则会返回false。
- 让Object的操作都变成函数化，之前使用'keyName' in obj 和delete obj[keyName]，现在可以使用Reflect.has(obj, name)和Reflect.deleteProperty(obj, keyName)。
- `Reflect`对象的方法与`Proxy`对象的方法一一对应，只要是`Proxy`对象的方法，就能在`Reflect`对象上找到对应的方法。这就让`Proxy`对象可以方便地调用对应的`Reflect`方法，完成默认行为，作为修改行为的基础。也就是说，不管`Proxy`怎么修改默认行为，你总可以在`Reflect`上获取默认行为。

