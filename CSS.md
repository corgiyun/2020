### CSS3 新特性
box-shadow, animate, transform, transition, rotate, 渐变等


### 普通盒模型和怪异盒模型的区别
普通盒模型：实际大小就等于定义的尺寸大小，包含元素内容 + padding + border （border-box）
怪异盒模型：定义的尺寸是元素内容尺寸，整体大小需加上额外的 padding + border （content-box）


### BFC (块级格式化上下文)
元素按照其在HTML中的先后位置至上而下布局，在这个过程中，行内元素排列，知道当行被占满然后换行，块级元素则会被渲染为完整的一个新行，除非另外指定，否则所有元素默认都是普通流定位，也可以说，普通流中元素的位置由该元素在HTML文档中的位置决定。


### css 实现一个三角形
```css
/* border */
{
  margin: 200px auto;
  width: 0;
  height: 0;
  border: 10px solid transparent;
  border-top-color: red;
}

/* clip */
{
  width: 10px;
  height: 10px;
  background: red;
  clip-path: polygon(0px 0px, 5px 10px, 10px 0);
  transform: rotate(180deg);
}
```


### 圣杯布局的实现
两边定宽，中间宽度百分百，自适应。用flex，一把梭


### 0.5px 的线怎么实现
scaleY(.5)，或者box-shadow, linear-gradient


### 元素实现水平垂直居中
```css
/* flex */
{
  display: flex;
  justify-content: center;
  align-items: center;
}

/* position + 负边距 */
{
  position: absolute;
  top: 50%;
  left: 50%;
  /* 尺寸已知 */
  margin-top: -(height / 2);
  margin-left: - (width / 2);
  /* 尺寸未知 */
  transform: translate(-50%, -50%)
}

/* position + margin */
{
  display: table-cell;
  text-align: center;
  vertical-align: center;
}
```


### flex 是如何实现的


