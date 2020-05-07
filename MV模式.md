# MV*模式

##### MVC模式

module：主要是处理数据，负责业务逻辑。

view：管理用户界面，负责展示逻辑。

controller：负责module和view之间的协作，负责应用逻辑，不能操作view。



1. view更新后，把处理权交给controller，controller对数据进行预处理，决定调用module的哪个接口。

2. 然后由module处理相应的业务逻辑。

3. module变更后，通过观察者模式通知view，View通过观察者模式收到Model变更的消息以后，会向Model请求最新的数据，然后重新更新界面。

   **注意：通过观察者模式的好处就是：不同的MVC三角关系可能会有共同的Model，一个MVC三角中的Controller操作了Model以后，两个MVC三角的View都会接受到通知，然后更新自己。保持了依赖同一块Model的不同View显示数据的实时性和准确性。**



##### MVP（Passive view）模式

与mvc的区别是：

1. view与module没有依赖关系了。

2. module同步视图的功能交给presenter（module通过观察者模式把消息传给presenter，presenter通过view提供的接口更新视图）。

   module通过观察者模式把消息传给presenter，presenter通过view提供的接口更新视图。



module：主要是处理数据，负责业务逻辑。

view：管理用户界面，负责展示逻辑。

Presenter：负责module和view之间的协作，负责应用逻辑，可以操作view。

1. view更新后，把处理权交给Presenter，Presenter对数据进行预处理，决定调用module的哪个接口。
2. 然后由module处理相应的业务逻辑。
3. module变更后，通过观察者模式通知Presenter，Presenter通过观察者模式收到Model变更的消息以后，会向Model请求最新的数据，然后调用view提供的接口（view需要提供接口）更新界面。



##### MVVM（Model-View-ViewModel）模式 ---- 对mvp的改良

module：主要是处理数据，负责业务逻辑（Domain Model）。

view：管理用户界面，负责展示逻辑。

ViewModel：它是连接view和model的桥梁。它有两个方向：一是将【模型】转化成【视图】，即将后端传递的数据转化成所看到的页面，实现的方式是：数据绑定。二是将【视图】转化成【模型】，即将所看到的页面转化成后端的数据，实现的方式是：DOM 事件监听。这两个方向都实现的，我们称之为数据的双向绑定。

1. view与viewModel没有直接联系
2. viewModel与module之间的交互是双向的
3. viewModel通过双向数据绑定把view和module联系起来
4. view改变自动更新module，module改变自动更新view，开发者只需关注业务逻辑，其他的操作由MVVM来实现



参考：

https://github.com/livoras/blog/issues/11

https://www.cnblogs.com/wzfwaf/p/10553160.html