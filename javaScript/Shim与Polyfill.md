# Shim与Polyfill

##### Shim是将有差异化的API封装成一个接口

例如Jquery的$.ajax，会先判断是否支持window.XMLHttpRequest，支持则使用XMLHttpRequest来创建xhr，不支持则使用ActiveXObject来创建。



##### Polyfill将差异化抹平，将不支持变成支持

例如IE中不支持XMLHttpRequest，Polyfill会创建一个window.XMLHttpRequest，内部实现使用ActiveXObject。







