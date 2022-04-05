<h1 align='center'>css高级技巧:mag:</h1>
-----
<details>
<summary  style="font-size:large;">目录</summary>

- [cursor](#cursor) 
- [outline](#outline)
- [resize](#resize)
- [vertical-align](#vertical-align)
- [word-break](#word-break)
- [white-space](#white-space)
- [text-overflow](#text-overflow)
- [sprite](#精灵图sprite)
- [svg](#字体图标svg)
- [滑动门](#滑动门)
- [伪元素本质](#伪元素本质)
- [鼠标经过显示边框](#鼠标经过显示边框)
- [transition](#过渡属性)
- [tranform](#2d变形属性)
- [transform3d](#3d变形)
- [animation](#动画)
- [flex](#伸缩布局)
- [grid](#grid布局)
</details>

### cursor
cursor：`point` `move` `text`分别指定*手*、*移动*和*文本*
### outline
- 可用于input表单清除轮廓
```css{.highlight:10}
outline: outline-color || outline-style || outline-width
```
### resize
- 防止文本域拖拽
```css
textarea {
    resize:0;
    outline:0;
}
```

### vertical-align
- 用于行内块元素垂直对齐居中（$\color{red}图片$、表单和文字），默认是基线对齐, **块级元素无效**
- 在低版本浏览器中行内块元素（<font face='Montserrat' color=red>图片</font>、表单）会有底侧空隙问题，也可指定`vertical-align`来清除

```css
img {
    vertical-align:middle || baseline||  bottom || top:
}
```
### word-break
- 用于英语单词的完整显示,`break-all`指在换行时可以拆开单词显示，`keep-all`为不能拆开，或者可以添加-使得单词拆开
```css
word-break:break-all || keep-all || normal;
```
### white-space
- 用于一行显示文字,`nowrap`强制一行显示，直到遇到`<br>`换行
```css
white-space:normal || nowrap
```
### text-overflow
- 用于省略文字显示格式，需搭配其他代码实现。`clip`剪切，`ellipsis`显示省略号
```css
white-space:nowrap;
overflow:hidden;
text-overflow:ellipsis || clip;
```
### 精灵图(sprite)
- 产生原因：由于网页背景图或插入图片过多，导致客户端访问次数增加，所以制作一张**大**的精灵图包含所有不需要改变的小的背景图片，通过调节背景图位置来显示出不同的效果图,这样就只需要请求一次
<img src='imgs/sprite.jpg' style="width:120px;"/>
```css
background:url(imgs/) 0 -488px no-repeat;
background:url(imgs/) 0 -679px no-repeat;
```
- 可显示不同的图形组合

### 字体图标（svg）
- 移动端使用多，页面显示图标，但是实际上是字体，可通过字体样式（阴影，旋转等）进行更改，并且兼容性好。如果是插入图片，则不好更改，还会失真。
### 滑动门
- 原理是利用css精灵和`padding`来撑开盒子，使得背景图片左右两边显示，代码：
```html
<li>
  <a href="#">
    <span></span>
  </a>
</li>
```
```css
a {
    height:;
    padding-left:;
    background:url() no-repeat;
    display:inline-block;
}
span {
    height:;
    padding-right:;
    display:inline-block;
    background:url() right no-repeat;
}
```
### 伪元素本质
- 伪元素选择器是插入一个元素，相当于`span`的行内元素，加内容是用`content`
### 鼠标经过显示边框
- 使用伪类元素选择器
```css
.fl {
  position: relative;
  height: 400px;
  width: 400px;
}
div.fl:hover::before {
  box-sizing: border-box; /*在这里加!!*/
  content: "";
  display: block;
  border: 10px solid rgba(255, 255, 255, 0.5);
  position: absolute;
  width: 100%;
  height: 100%;
}
```
### 过渡属性
1. 常常与hover联用来达到鼠标效果，但是不要写在hover中，因为过渡效果完成后，也会按照之前的效果恢复，写在hover中则不会有恢复效果。同时可通过`，`和拆开写来达到简洁的代码实现多个过渡效果
1. `transition`有四个属性` transition-property `过渡的属性，` transition-duration`过渡时间，` transition-timing-function`过渡类型，` transition-delay`开始时间
1.要精确到自己的标签，谁做动画，谁过渡
1. 过渡类型（默认是ease）![transition](imgs/transition.png)
### 2D变形属性
- transform常常和`hover`、`active`、`transition`一起搭配使用，多个属性冰鞋中间直接用空格隔开
  1. `translate(x,y)`进行平移效果，%是根据自己的大小进行移动，居中的方法
  ```css
  .center {
    float:left;
    left:50%;
    top:50%;  
    transform:translate(-50%,-50%)/*之前是移动自己盒子大小的一半，需要计算*/
  }
  ```
  2. 缩放：`scle(x,y)`进行缩放
  3. 旋转： `rotate(_deg)`围绕中心点旋转，+顺时针，-值逆时针
  4. 变形中心点设置：`transform-origin`可设置变形中心点，属性值可以是精确坐标**20px**，**30px**或者是**top right**这种位置
  5. 倾斜：`skew(deg,deg)`,参数指定左右和上下进行倾斜
### 3d变形
- css的3d坐标，左手法则
![3d axis](imgs/3daxis.png)
- 3d变形仍然是使用`transform`进行指定
1. `rotatex(deg）` `rotatey(deg)`等分别制定x，y轴的旋转
2. 透视`perspective`模拟人眼到屏幕的距离，利用近大远小的原理，以达到旋转时的3d效果。值越小越明显。在使用的时候，应该给父元素添加此标签
3. 移动：同2d，但是`translatez()`可以实现缩放盒子的效果，`translat3d()`的z值不能是百分比值
### 动画
![animation](imgs/animation.png)
其中次数可定义为**infinite**，方向可定义为**alternative**来正反动画
- 定义动画
```css
@keyframes name {
  form{}
  to{} /*or 0%{} 50%{} 70%{} 100%{}来更详细指定*/
}
```
### 伸缩布局
* 父盒子加`display:flex`,子元素加`flex:`
- 常常会加min-width等属性来限制缩放浏览器的盒子的完整性
- 伸缩主轴默认是水平x轴，可通过`flex-direction：column`来指定父盒子属性
1. justify-content属性用于当子盒子没充满父盒子的排列方式
![justify-content](imgs/justify.png)
2. align-items属性调整侧轴排列，多一个**stretch**拉伸子盒子以适应父盒子
1. flex-wrap控制盒子换行
1. align-content控制多行侧轴的排列，父元素需要设置flex-direction:row,flex-wrap:wrap
1. flex-flow是flex-direction和flex-wrap的简写
1. order属性定义排列先后顺序，order小的排前面，默认0

### grid布局
- 如`grid-template-row：50px auto 200px`（3列）和grid-template-column来指定行列
- row-gap和column-gap来间隔

first | two | ds
-------|------ |---
1|2
1|
5|
5 | 22
s|
ds|s