# css伪类

a:link {color:#FF0000;} 未访问的链接 
a:visited {color:#00FF00;} 已访问的链接 
a:hover {color:#FF00FF;} 鼠标划过链接
a:active {color:#0000FF;} 已选中的链接
注意： 在CSS定义中，a:hover 必须被置于 a:link 和 a:visited 之后，才是有效的。
注意： 在 CSS 定义中，a:active 必须被置于 a:hover 之后，才是有效的。
注意：伪类的名称不区分大小写。

p:first-child {}
p:nth-child(n) {} 选择所有 p 元素的父元素的第二个子元素
p:before {} 在每个<p>元素之前插入内容
p:after {} 在每个<p>元素之后插入内容
div:not(p) {} 选择div下所有p以外的元素
input:checked {} 选择所有选中的表单元素
input:disabled {} 选择所有禁用的表单元素
……