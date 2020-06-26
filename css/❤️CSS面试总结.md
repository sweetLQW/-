# CSS面试总结

## 设置ul下除了第一个li其他li的样式

```css
ul li:not(:first-child) {}
```



## 设置ul下奇偶行样式

```css
/* 奇数行 */
ul li:nth-child(odd) {}
/* 偶数行 */
ul li:nth-child(even) {}
```



## animation与transition的区别

- transition需要事件触发，触发一次执行一次；animation可以通过@keyframes自执行，可以设置循环次数。

- transition比animation的性能要更好，transition可以配合transform使用。

- transition只能定义开始和结束状态；animation可以定义很多状态。

- transition: property duration timing-function delay;

  | 值                                                           | 描述                                |
  | :----------------------------------------------------------- | :---------------------------------- |
  | [transition-property](https://www.w3school.com.cn/cssref/pr_transition-property.asp) | 规定设置过渡效果的 CSS 属性的名称。 |
  | [transition-duration](https://www.w3school.com.cn/cssref/pr_transition-duration.asp) | 规定完成过渡效果需要多少秒或毫秒。  |
  | [transition-timing-function](https://www.w3school.com.cn/cssref/pr_transition-timing-function.asp) | 规定速度效果的速度曲线。            |
  | [transition-delay](https://www.w3school.com.cn/cssref/pr_transition-delay.asp) | 定义过渡效果何时开始。              |

- animation: name duration timing-function delay iteration-count direction;

  | 值                                                           | 描述                                     |
  | :----------------------------------------------------------- | :--------------------------------------- |
  | *[animation-name](https://www.w3school.com.cn/cssref/pr_animation-name.asp)* | 规定需要绑定到选择器的 keyframe 名称。。 |
  | *[animation-duration](https://www.w3school.com.cn/cssref/pr_animation-duration.asp)* | 规定完成动画所花费的时间，以秒或毫秒计。 |
  | *[animation-timing-function](https://www.w3school.com.cn/cssref/pr_animation-timing-function.asp)* | 规定动画的速度曲线。                     |
  | *[animation-delay](https://www.w3school.com.cn/cssref/pr_animation-delay.asp)* | 规定在动画开始之前的延迟。               |
  | *[animation-iteration-count](https://www.w3school.com.cn/cssref/pr_animation-iteration-count.asp)* | 规定动画应该播放的次数。                 |
  | *[animation-direction](https://www.w3school.com.cn/cssref/pr_animation-direction.asp)* | 规定是否应该轮流反向播放动画。           |

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>transition与animation区别</title>
    <style>
        .box {
            width: 100px;
            height: 100px;
            background-color: red;
        }

        .transition-box {
            transition: width 3s linear 0.5s;
        }
        .transition-box:hover {
            width: 300px;
        }

        .animation-box {
            animation: changewidth 3s linear 0.5s 3 alternate;
        }
        @keyframes changewidth {
            to { width:300px; }
        }
    </style>
</head>
<body>
    <div class="transition-box box"></div>
    <div class="animation-box box"></div>
</body>
</html>
```



## 两栏布局（一边固定宽度，另一边自适应，以左边固定200px为例）

- display: flex（父） +  flex-grow: 1（右子） 

- position: relative（父） + position: absolute; top: 0; left: 0（左子） 

- display: inline-block;（两子元素） + width: calc(100% - 200px);（右子）

- display: table; width: 100%;（父） + display: tabel-cell;（两子元素）

- float: left;（左子） + margin-left: 200px;（右子） 

- display: grid; grid-template-columns: 200px auto;（父）

  - IE10以上才支持grid，手机端兼容性不好

  

## flex布局

### 1、容器属性（父元素）

- `flex-direction`（项目的排列方向）

  ```css
  flex-direction: row(默认值) | row-reverse | column | column-reverse;
  ```

- `flex-wrap`（如果一条轴线排不下，如何换行）

  ```css
  flex-wrap: nowrap(默认值) | wrap(第一行在上) | wrap-reverse(第一行在下);
  ```

- `flex-flow`（默认值为`row nowrap`）

  ```css
  flex-flow: <flex-direction> || <flex-wrap>;
  ```

- `justify-content`（主轴的对齐方式）

  ```css
  justify-content: flex-start(默认值) | flex-end | center | space-between | space-around;
  ```

- `align-items`（交叉轴的对齐方式）

  ```css
  align-items: flex-start | flex-end | center | baseline | 
  						 stretch(默认值,如果项目未设置高度或设为auto，将占满整个容器的高度);
  ```

- `align-content`（定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。）

  ```css
  align-content: flex-start | flex-end | center | space-between | space-around | 								 space-evenly | stretch(默认值,轴线占满整个交叉轴);
  ```

  

### 2、项目属性（子元素）

- `order`	(定义项目的排列顺序。数值越小，排列越靠前，默认为0。)

- `flex-grow`

  定义项目的放大比例，默认为`0`，即如果存在剩余空间也不放大。

  如果所有项目的`flex-grow`属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的`flex-grow`属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。负值无效。

  

- `flex-shrink`

  属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

  如果所有项目的`flex-shrink`属性都为1，当空间不足时，都将等比例缩小。如果一个项目的`flex-shrink`属性为0，其他项目都为1，则空间不足时，前者不缩小。负值无效。

  

- `flex-basis`

  `flex-basis`属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为`auto`，即项目的本来大小。

  它可以设为跟`width`或`height`属性一样的值（比如100px），则项目将占据固定空间。

  

- `flex`（默认值为`0 1 auto`，该属性有两个快捷值：`auto` (`1 1 auto`) 和 none (`0 0 auto`)。）

  ```css
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
  ```

  flex取值为一个或两个`非负数字`时：

  ```css
  .item {flex: 1;}
  /* 对应的是: */
  .item {
      flex-grow: 1;
      flex-shrink: 1;
      flex-basis: 0%;
  }
  
  .item {flex: 1 2;}
  /* 对应的是: */
  .item {
      flex-grow: 1;
      flex-shrink: 2;
      flex-basis: 0%;
  }
  ```

  flex取值为`百分比`或`长度`时（此值为flex-basis的值）：

  ```css
  .item-1 {flex: 0%;}
  /* 对应的是: */
  .item-1 {
      flex-grow: 1;
      flex-shrink: 1;
      flex-basis: 0%;
  }
  
  .item-2 {flex: 24px;}
  /* 对应的是: */
  .item-1 {
      flex-grow: 1;
      flex-shrink: 1;
      flex-basis: 24px;
  }
  ```

  当 `flex` 取值为一个非负数字和一个长度或百分比时：

  ```css
  .item-1 {flex: 2 20px;}
  /* 对应的是: */
  .item-1 {
      flex-grow: 2;
      flex-shrink: 1;
      flex-basis: 20px;
  }
  ```

  

- `align-self`

  `align-self`属性允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`。

  ```css
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
  ```

  

## 响应式布局

### 1、CSS长度单位

**dpr**（设备像素比）：缩放为100%的情况下，设备像素和CSS像素的比值。

**设备像素**（物理像素）：每个设备的物理像素都是相同的。

**css像素**（逻辑像素）：web开发中的抽象逻辑像素。

桌面应用中，一般 1 个设备像素对应 1 个CSS像素，移动端由于屏幕的限制，经常会有缩放的操作，所以移动端dpr值会稍大。比如iphone6的dpr为2，也就是在iphone6中，缩放为100%的情况下，2 个设备像素对应 1 个CSS像素，我们还有另外一种方法可以修改dpr，就是通过设置viewport的width=device-width。

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

这样设置以后dpr的值就为1了，此时1 个设备像素对应 1 个CSS像素。



- **px** ：绝对单位，相对显示器分辨率而定的

- **100%**：相对单位

  - 相对于父元素宽度的属性
    - `width`、`left`、`right`、`max-width`、`min-width`、`text-indent`、`padding`、`margin`、`grid-template-columns`、`grid-auto-columns`、`column-gap` 等；
  - 相对于父元素高度的属性
    - `height`、`top`、`bottom`、`max-height`、`min-height`、`grid-template-rows`、`grid-auto-rows`、`row-gap` 等；
  - 相对于主轴长度
    - `flex-basis` 等；
  - 相对于最近祖先的font-size
    - `font-size` 等；
  - 相对于自身font-size
    - `line-height` 等；
  - 相对于自身宽高
    - `border-radius`、`background-size`、`transform: translate()`、`transform-origin`、`border-image-width`、`zoom`、`clip-path` 等；

- **vw与vh**：相对单位，取决于浏览器视口的宽高，1w为浏览器视口宽度的1%，1h为浏览器视口高度的1%

- **rem**：相对单位，根据html的font-size大小决定

  ```html
  html {
  	font-size: 18px;
  }
  .father {
  	font-size: 1rem; /* 1 * 18 = 18px */
  	width: 5rem; /* 5 * 18 = 90px */
  	background-color: red;
  }
  .children {
  	width: 4rem; /* 4 * 18 = 72px */
    background-color: blue;
  }
  
  <div class="father">
    father
    <div class="children">
      children
    </div>
  </div>
  ```

- **em**：相对单位

  - 子元素的font-size: 1em;相对于父元素的font-size

  - 元素的width / height / padding / margin用em的话是相对于自身的font-size

    ```html
    .father {
    	font-size: 20px;
    	background-color: red;
    }
    .children {
    	font-size: 1.5em; /* 20 * 1.5 = 30px */
    	width: 15em; /* 10 * 30 = 300px */
    	display: inline-block;
    	background-color: blue;
    }
    .son {
    	font-size: 0.3em;
    	width: 10em;
    	display: inline-block;
    	background-color: green;
    }
    
    <div class="father">
      <div class="children">
        <div class="son"></div>
      </div>
    </div>
    ```

    注意⚠️：

    正常情况下，son元素的font-size应该为9px，但是在chrome中最小字体像素为12px，所以son元素的字体大小会是12px。

    此处width的值应该根据font-size为9设置，所以width为10*9=90px。

- **vmin & vmax**：相对长度

  - vmin的值是当前vw和vh中较小的值。
  - vmax的值是vw和vh中的较大的值。vmax的兼容性不太好。

- **ex**：相对长度，依赖于英文字母小 x 的高度

- **ch**：相对长度，数字 0 的宽度

- **pt**：绝对长度，一般用于字体尺寸，point，大约1/72英寸； (1pt = 1/72in)。

- **in**：绝对长度， 英寸 (1in = 96px = 2.54cm)

- **cm**：绝对长度， 厘米

- **mm**：绝对长度， 毫米

- **pc**：绝对长度， pica，大约6pt，1/6英寸； (1pc = 12 pt)

### 2、动态设置rem

```js
/**
 * @auther: 刘倩文<18511753482@163.com>
 * @date: 2018-11-20
 * @params：{number} designWidth  设计稿的宽度
 * @params：{number} count 1rem换算成多少px
 */
 
(function (designWidth, count) {
	function setFontSize() {
		var ele = document.getElementsByTagName('html')[0];
  		ele.style.fontSize = window.innerWidth/designWidth*count + 'px';
	}
	setFontSize();
	window.onresize = setFontSize;
})(1080, 100);
```

