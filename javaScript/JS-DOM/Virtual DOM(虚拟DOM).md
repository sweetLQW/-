# 虚拟DOM（Virtual DOM）

##### 虚拟DOM的出现主要是由于JavaScript频繁操作DOM（会引起reflow）带来的性能消耗太大，导致浏览器性能降低、卡顿，影响用户体验。



虚拟DOM的工作原理：

1. 利用js对象来模拟DOM树结构，通过js对象去渲染DOM树。
2. 当有DOM改变时，直接更改js对象，通过比对新老js对象（虚拟dom），记录差异对象。
3. 将差异对象渲染到真实DOM上。

