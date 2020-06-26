# BFC(块级格式化上下文)

**块级格式化上下文（Block Formatting Context，BFC） 是Web页面的可视化CSS渲染的一部分，是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域。**

**BFC主要解决的问题：防止外边距塌陷，清除浮动，两栏布局等。**

下列方式会创建**块格式化上下文**：

- 根元素(<html>)
- 浮动元素（ `float` 不是 `none`的元素）
- 定位元素（ `position` 为 `absolute` 或 `fixed`的元素）
- `overflow`为`hidden`、`auto`、`scroll`的元素
- display为`inline-block`、`flow-root`、 `flex` 、 `inline-flex`、 `grid` 、 `inline-grid` 、`table系列`（ `table`、`table-row`、 `table-row-group`、`table-header-group`、`table-footer-group`（分别是HTML table、row、tbody、thead、tfoot的默认属性）及 `inline-table`、`table-cell`、`table-caption`）的元素
- [`contain`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/contain) 值为 `layout`、`content`或 paint 的元素
- 多列容器（元素的 [`column-count`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/column-count) 或 [`column-width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/column-width) 不为 `auto`，包括 `column-count` 为`1`）
- `column-span` 为 `all` 的元素始终会创建一个新的BFC，即使该元素没有包裹在一个多列容器中（[标准变更](https://github.com/w3c/csswg-drafts/commit/a8634b96900279916bd6c505fda88dda71d8ec51)，[Chrome bug](https://bugs.chromium.org/p/chromium/issues/detail?id=709362)）。



1. 当body设置overflow; hidden;时，body下元素的margin-top依然会塌陷

   