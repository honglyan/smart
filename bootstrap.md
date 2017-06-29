# Bootstrap 网格系统
## 媒体查询
媒体查询是非常别致的"有条件的 CSS 规则"。它只适用于一些基于某些规定条件的 CSS。如果满足那些条件，则应用相应的样式。
Bootstrap 中的媒体查询允许您基于视口大小移动、显示并隐藏内容。下面的媒体查询在 LESS 文件中使用，用来创建 Bootstrap 网格系统中的关键的分界点阈值。 
```html
/* 超小设备（手机，小于 768px） */
/* Bootstrap 中默认情况下没有媒体查询 */
/* 小型设备（平板电脑，768px 起） */
@media (min-width: @screen-sm-min) { ... }
/* 中型设备（台式电脑，992px 起） */
@media (min-width: @screen-md-min) { ... }
/* 大型设备（大台式电脑，1200px 起） */
@media (min-width: @screen-lg-min) { ... }
```
## 基本的网格结构

下面是 Bootstrap 网格的基本结构：

```html
<div class="container">
   <div class="row">
      <div class="col-*-*"></div>
      <div class="col-*-*"></div>      
   </div>
   <div class="row">...</div>
</div>
<div class="container">....
```

