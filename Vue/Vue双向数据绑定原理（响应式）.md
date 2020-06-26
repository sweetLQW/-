# Vue双向数据绑定原理（响应式）

双向数据绑定：view变化，module随之变化；module变化，view也随之变化。

Vue的双向数据绑定由**数据劫持（Object.defineProperty()）**结合**发布订阅模式**实现。



#### 一、 Object.defineProperty(obj, prop, descriptor)

obj: 要定义属性的对象

prop: 要定义或修改的属性的名称或Symbol

descriptor：要定义或修改的属性描述符。

- 数据描述符【**configurable**，**enumerable**，**value**，**writable**】
- 存取描述符【**configurable**，**enumerable**，**get**，**set**】

**configurable**：是否可以被改变及删除。如果旧描述符将其`configurable` 属性设置为`false`，则该属性被认为是“不可配置的”，并且没有属性可以被改变（除了单向改变 writable 为 false）。当属性不可配置时，不能在数据和访问器属性类型之间切换。

**enumerable**：是否可枚举（通过赋值操作添加的普通属性是可枚举的）。

**value**：属性值，可以是任何有效的JavaScript 值。

**writable**：value是否可以被赋值运算符改变。当 `writable` 属性设置为 `false` 时，该属性被称为“不可写的”。它不能被重新赋值。

**get**：属性的 getter 函数，该函数的返回值会被用作属性的值。

**set**：属性的 setter 函数，当属性值被修改时，会调用此函数。



注意：

通过Object.defineProperty赋值，configurable，enumerable，writable默认值都是false。value，get，set默认值都是undefined。

但是普通赋值操作，**configurable**，**enumerable**，**writable**默认为true。



#### 二、 **发布订阅模式**



