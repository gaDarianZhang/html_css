# Question #



<span style="color:red;">**line-height这个属性又是怎么继承的**</span>：

::first-letter ::first-line   ::before   ::after  ——》极客时间



p元素中不能放任何的块元素

body外默认有8px的margin

**连着两层的float，第二层float还是在第一层的float内，没有脱离。也就是通过float开启BFC**

**font awesome字体不能再调小吗**：谷歌浏览器内最小字体是12px，想要调小需要一些很麻烦的方法。

**color怎么用，这个属性是怎么继承的**：正常的继承关系。只是说，对于a里边的内容，浏览器自己设置了颜色，比继承级别高。可以用color:inherit；来表示继承。



[E:\webWorkspace\htmlcssStudy\02css\07.5伪类选择器（重要）.html]()

# 选择器

## 选择器优先级

**important > 内联 > id > 类和伪类 和属性选择器> 元素 > 通配(*) > 继承**

对于元素的同一个属性被多次设置，同等级则采用靠下的，否则采用高优先级的。对于没有被重复设置的属性，则全部都采用

## first-child/first-of-type

:first-child 同一级别内的第一个子元素（不管是什么类型的元素），把所有类型的元素都当做子元素

​      如果:first-child前边跟了一个明确的元素类型的话，那就是同一级别中该类元素中的第一个了,就相当于first-of-type

​      eg:

​        span:first-child 那就是指同一级别内第一个span类型，相当于first-of-type，看下边紧接着的两个示例就能证明。

​        <span style="color:red">**ul :first-child，ul加空格表示所有后代元素，也就是选中了ul中的所有元素，这时就选中ul的第一个直接子元素，以及下一级别内的第一个子元素依次继续（也就是同一级别内的第一个子元素，这时的子元素就不区分类别了）**</span>

​        <span style="color:red">**ul>:first-child 这时就指的是ul的直接子元素中的第一个元素了，不再继续向下寻求更级别的元素**</span>

​		<span style="color:red">**ul span:first-child = ul span:first-of-type ul所有后代级别内的第一个span**</span>

​		<span style="color:red">**ul>span:first-child = ul>span:first-of-type**</span>

​		<span style="color:red">**ul :first-of-type 所有后代层级内的所有类型的第一个**</span>

​		<span style="color:red">**ul>:first-of-type ul直接子元素中的所有类型的第一个**</span>

## +/~

兄弟选择器：选不到前边的兄弟
                选择下一个近邻兄弟：**必须是紧接着的**
                    语法：元素+元素
                选择下边所有的兄弟：可以中间隔着其他类型的元素
                    语法：元素~元素

## nth-child()/nth-of-type()

in+j：容易纠结于这个n是从0开始取呢，还是1开始，其实**n是从让in+j>=1的最小非负数开始取的**

## title

[属性名] 选择含有指定属性的元素
[属性名=属性值] 选择属性=指定属性值的元素
[属性名^=属性值] 选择属性值以指定值开头的元素
[属性名$=属性值] 选择属性值以指定值结尾的元素
[属性名*=属性值] 选择属性值中含有某值的元素的元素

里边的**属性值不用加引号**

## 超链接伪类

- :link	用来表示没访问过的链接（正常的链接）
- :visited    用来表示访问过的链接,由于隐私的原因，所以visited这个伪类只能修改链接的颜色
- :hover    用来表示鼠标移入的状态
- :active    用来表示鼠标点击时

## 伪元素选择器

::first-letter 表示第一个字母
::first-line 表示第一行
::selection 表示选中的内容
::before 元素的开始 
::after 元素的最后

**before 和 after 必须结合content属性来使用**

# block盒模型

```
元素的水平方向的布局：
   元素在其父元素中水平方向的位置由以下几个属性共同决定“
   margin-left
   border-left
   padding-left
   width
   padding-right
   border-right
   margin-right

   一个元素在其父元素中，水平布局必须要满足以下的等式
margin-left+border-left+padding-left+width+padding-right+border-right+margin-right = 其父元素内容区的宽度 （必须满足）                
   100 + 0 + 0 + 200 + 0 + 0 + 400 = 800
   100 + 0 + 0 + 200 + 0 + 0 + 500 = 800
   - 以上等式必须满足，如果相加结果使等式不成立，则称为过度约束，则等式会自动调整
   - 调整的情况：
   - *********如果这七个值中没有为 auto 的情况，则浏览器会自动调整margin-right值以使等式满足：
0 + 0 + 0 + 200 + 0 + 0 + 0 = 800 ===》  0 + 0 + 0 + 200 + 0 + 0 + 600 = 800
   - 这七个值中有三个值可设置为auto          
             width：默认就是为auto
             margin-left
             maring-right
   -如果有两个都设置为auto,width的auto优先级最高，margin-left和margin-right的auto优先级相同
   - 如果某个值为auto，则会自动调整为auto的那个值以使等式成立
     0 + 0 + 0 + auto + 0 + 0 + 0 = 800  auto = 800
     0 + 0 + 0 + auto + 0 + 0 + 200 = 800  auto = 600
     200 + 0 + 0 + auto + 0 + 0 + 200 = 800  auto = 400
     auto + 0 + 0 + 200 + 0 + 0 + 200 = 800  auto = 400
     auto + 0 + 0 + 200 + 0 + 0 + auto = 800  auto = 300

   - 如果将一个宽度和一个外边距设置为auto，则宽度会调整到最大，设置为auto的外边距会自动为0
   - 如果将三个值都设置为auto，则外边距都是0，宽度最大
   - 如果将两个外边距设置为auto，宽度固定值，则会将外边距设置为相同的值
    所以我们经常利用这个特点来使一个元素在其父元素中水平居中
     示例：
                                width:xxxpx;
                                margin:0 auto;
```

[D:\E\webWorkspace\htmlcssStudy\03layout\07盒子的垂直布局.html]()

子元素是在父元素的内容区中排列的,

​        如果子元素的大小超过了父元素，

​        则子元素会从父元素中溢出。 

​        **溢出后是被其他元素覆盖，不是向下挤（被覆盖）**

# inline的盒模型

[D:\E\webWorkspace\htmlcssStudy\03layout\09行内元素的盒模型.html]()

**行内元素的盒模型**

​          \- **行内元素不支持设置宽度和高度**

​		  - 不会独占一行

​          \- 水平方向不会产生折叠

​          \- 行内元素可以设置padding，但是**垂直**方向padding**不会影响**页面的布局（内容区在垂直方向不会动），但**会有覆盖（覆盖别人）**

​          \- 行内元素可以设置border，**垂直**方向的border**不会影响**页面的布局，但会有覆盖

​          \- 行内元素可以设置margin，**垂直**方向的margin**不会影响**布局，但会有覆盖

# inline-block

**还是会独占一行，但是宽和高不设置的话是被内容撑开。block不设置宽度的话是全宽。**

# 水平垂直居中等

**text-align**

​		在*块元素、表格单元框*内设置，块元素/表格单元框内的***行内元素***会左右居中。

**vertical-align**

​		用于inline,inline-block,table-cell，***不能用于块元素***
​		将inline,inline-block,table-cell内的line-height与其左右元素的Line-height相比较：

​				1️⃣若该元素的line-height为最大的，则调整该行内未设置vertical-align的元素，使其满足顶部、底部对齐等。

​				2️⃣若该元素line-height不是该行内最大的，则调整该元素的位置，使其满足对齐。

​				3️⃣可以参考[D:\E\webWorkspace\htmlcssStudy\exercise\15再做w3school导航条.html]() line64-70

**line-height**

​		设置line-height后，字体文字的border还是紧紧围绕自文字，框框没有变高。效果类似于在字体上下加了一个margin，但其实不是margin。



# 不会挤压其他元素的情况：

1. 行内元素垂直方向的padding,border,margin(**覆盖别人**)

  2. 一个元素内子元素的overflow部分，**会被其他元素覆盖**

  3. outline: outline不改变元素位置，**会覆盖别人**

  4. 脱离文档流的：float/position:absolute、fixed/

  5. position设置的位置移动，不会挤压其他元素

     

# **隐藏与占位**

1.overflow:hidden; 不占空间

2.display: none; 不占空间

3.visibility: hidden; **仍然占空间**



# **提升元素层级**

**(提升层级和脱离文档流不是一回事)**

float/position

# 脱离文档流

- float
- position: absolute/fixed

# **开启BFC**

​	1.float

​	2.overflow:hidden

​	3.display:inline-block、flex等

​	4.position:absolute或者fixed

​	5.clearfix



# 高级布局&动画

​	浮动（float）、定位（position）、flex、transform(translateX、roateX、scale)配合transition和animation

## **float**

​	浮动的特点：

​            0、**没有自动的宽度和高度，默认被内容撑开**

​            1、浮动元素会完全脱离文档流，不再占据文档流中的位置

​            2、设置浮动以后元素会向父元素的左侧或右侧移动，

​            3、浮动元素默认不会从父元素中移出（一个非浮动的块元素内的一个浮动元素，它的**默认**相对位置还是在**父元素内**，而不是直接浮动到了页面的最边上）

​            4、浮动元素向左或向右浮动时，不会超过它前边的其他浮动元素

​            **5、如果浮动元素的上边是一个没有浮动的块元素，则浮动元素无法上移**

​            6、浮动元素不会超过它上边的浮动的兄弟元素，最多最多就是和它一样高

- **浮动元素不会盖住文字，文字会环绕在浮动元素周围， 所以可以使用浮动来设置文字环绕图片的效果**
- 开启BFC(overflow:hidden)的元素在没有打开float的情况下也可以紧接在开启float的元素后边,但是反过来就不行了。overflow更不要脸一些，喜欢去和float肩并[E:\webWorkspace\htmlcssStudy\04float\05BFC.html]()



## position

relative/absolute/fixed/sticky：**定位都会提升元素层级**

可选值：

- static 默认值，元素是静止的没有开启定位
- relative 开启元素的相对定位
- absolute 开启元素的绝对定位
- fixed 开启元素的固定定位
- sticky 开启元素的粘滞定位

### relative

- 当元素的position属性值设置为relative时则开启了元素的相对定位
        - 相对定位的特点：
             1.元素开启相对定位以后，如果不设置偏移量元素不会发生任何的变化
             2.相对定位是参照于元素在文档流中的位置进行定位的（也即相对于原始位置）
             3.相对定位会**提升元素的层级**（也就是可以遮盖其他元素）
             4.相对定位**不会使元素脱离文档流**
             5.相对定位不会改变元素的性质**块还是块，行内还是行内**
             6.<span style="color:red;font-size:18px;text-decoration:line-through">如果一个元素开启了相对定位，它的子元素的位置还是相对这个元素原来的位置进行布局的，不会因为父元素设置了偏移而导致后边元素位置移动。</span>

             7.不会影响布局

### absolute

- <span style="color:red">**设置的偏移量是：自己的margin外侧相对于父级的border内侧，也就是padding外侧**</span>

- 在祖先元素没开启position的情况下，**默认相对于视口**。

 - 当元素的position属性值设置为absolute时，则开启了元素的绝对定位
                - 绝对定位的特点：
                    1.开启绝对定位后，如果不设置偏移量元素的位置不会发生变化
                    2.开启绝对定位后，**元素会从文档流中脱离**，也就是下边元素会挤上去。
                    3.**绝对定位会改变元素的性质，行内和块都变成类似于inline-block的性质，但不会独占一行**
                    4.绝对定位会使元素**提升一个层级**
                    5.绝对定位元素是相对于其包含块进行定位的

- left right width默认为auto

- **left right不能同时设置值，若同时设置值，则width要为auto，或margin为auto**

- left和right若同时为auto，那么不是平均，是right霸占完了

- ```
  auto的级别：right/left>width>margin-left/margin-right。貌似width和left/right不能同时为auto
  ```

### fixed

固定定位：

- 将元素的position属性设置为fixed则开启了元素的固定定位

- 固定定位也是**脱离了文档流**
- 固定定位也是一种绝对定位，所以固定定位的大部分特点都和绝对定位一样
    唯一不同的是固定定位永远参照于浏览器的视口进行定位！！！什么是视口，一定要在网页里边看一下
    固定定位的元素不会随网页的滚动条滚动

### sticky

粘滞定位
       - 当元素的position属性设置为sticky时则开启了元素的粘滞定位
            - 粘滞定位和相对定位的特点基本一致，不同的是粘滞定位可以在元素到达某个位置时将其固定

## **flex**

​	<span style="color:red">弹性盒：dw(f) JC AI AC  5</span>

- display:flex/inline-flex;
  - display:flex  设置为**块级**弹性容器，是指这个弹性容器为块级
  - display:inline-flex 设置为行内的弹性容器
- flex-direction：row / row-reverse / column / column-reverse
- flex-wrap：nowrap / wrap / wrap-reverse
- (flex-flow)
- justify-content

```
- 如何分配!!主轴!!上的空白空间（主轴上的元素如何排列）
- 可选值：
    flex-start 元素沿着主轴起边排列，不同于flex-direction,
        如flex-direction:row-reverse,紧贴右边且最右边是1；
        justify-content:flex-end也是紧贴右边，但是是最后一个元素紧贴右边。
    flex-end 元素沿着主轴终边排列
    center 元素居中排列,空白均匀分布到两端
    space-around 空白分布到元素两侧，也就是空白有2n份
    space-between 空白均匀分布到元素间，空白分成n-1份
    space-evenly 空白分布到元素的单侧，空白分成n+1份
```

- align-items

```
- 元素在辅轴上如何对齐(每一行主轴独立，就是每一行主轴上的元素在副轴上的对齐方式）
- 元素间的关系
    - 可选值：
        stretch 默认值，将元素的长度设置为相同的值（主轴上的元素高度相同）
        flex-start 元素不会拉伸，沿着辅轴起边对齐
        flex-end 元素不会拉伸，沿着辅轴的终边对齐
        center 元素不会拉伸，居中对齐
        baseline 元素不会拉伸，基线对齐
```

- align-content：辅轴空白空间的分布,**每一行主轴上的一行元素以最长的那个为准**。**只有在flex-wrap：wrap/wrap-reverse；的情况下align-content才有用**

​	弹性元素：gsb(f)o AS  5 

- flex-grow：默认0不伸展
- flex-shrink：默认等比例收缩，若设置为0则不收缩
- flex-basis：指定的是元素在**主轴上**的基础长度

```
如果主轴是 横向的 则 该值指定的就是元素的宽度
如果主轴是 纵向的 则 该值指定的是就是元素的高度
 - 默认值是 auto，表示参考元素自身的高度或宽度
 - 如果传递了一个具体的数值，则以该值为准
```

- (flex)

```
flex 增长 缩减 基础;
    initial "flex: 0 1 auto". 弹性元素可缩不可伸
    auto  "flex: 1 1 auto" 弹性元素可缩可伸
    none "flex: 0 0 auto" 弹性元素没有弹性
```

- align-self：用来覆盖当前弹性元素上的align-items
- order



## **transform**

   - transalteX/Y/Z
   - rotateX/Y/Z
   - scale
   - skew扭曲

> <span style="color:orange">scale在前的话，那么后边的translate的数值也会跟着scale缩放。也就是坐标轴的刻度标识数字没变化，只不过坐标轴缩放了。并且该元素内部的子元素也发生同样的变化。</span>
>
> rotate会使坐标系随着rotate旋转
>
> skew也会使坐标轴发生扭曲

transform-origin：center(默认)、top/bottom、right/left、数值 数值（相对于元素自身content）

transform-box

transform-function

transform-style【flat/preserve-3d】加给父元素

perspective: xxpx;  加给父元素





## transition/4

**transition** 可以同时设置过渡相关的所有属性，只有一个要求，如果要写延迟，则两个时间中第一个是持续时间，第二个是延迟。**用这种简写，是不是一次只能指定一个属性?不同属性要再写一遍这些过渡效果，中间用逗号隔开。**

```css
transition: background-color 5s 2s ease-in,width 2s 1s cubic-bezier(0.6, -0.28, 0.735, 0.045);
```

​			把transition的这些内容写在元素的css代码区内，在另一个代码块，比如hover内会有这个元素的一些属性值的改变

​			  transition-property: ;

​              transition-duration: ;

​      		transition-timing-function: ;

​      		transition-delay: ;

​      		transition: ;

​	<span style="color:orange;font-size:18px">transition-timing-function: 可以是steps(num[,start|end])；表示把动画分Num次走完，间隔都是duration/num，start是一触发就开始动，不用等，消耗了一次的时间；end是要等一次的时间才切到下一帧</span>

## animation/8

​		动画和过渡类似，都是可以实现一些动态的效果，

​        **不同的是过渡需要在某个属性发生变化时才会触发**

​        ***动画可以自动触发动态效果***

​      设置动画效果，必须先要设置一个关键帧，关键帧设置了动画执行每一个步骤。

> 动画执行结束后，元素的状态是没有这个动画帧的状态，也可以理解为瞬间恢复原位，并不是停留在动画的最后状态。

​		animation-name

​        animation-duration

​        animation-timing-function

​        animation-delay

​        animation-iteration-count：次数/infinite

​        animation-direction

		- normal 默认值  从 from 向 to运行 每次都是这样

- reverse 从 to 向 from 运行 每次都是这样

- alternate(来回) 从 from 向 to运行 重复执行动画时反向执行,次数受animation-iteration-count影响
- alternate-reverse 从 to 向 from运行 重复执行动画时反向执行

​        animation-play-state：running/paused

​        **animation-fill-mode**:

| none      | 不改变默认行为。                                             |
| --------- | ------------------------------------------------------------ |
| forwards  | 当动画完成后，保持最后一个属性值（在最后一个关键帧中定义）。 |
| backwards | 在 animation-delay 所指定的一段时间内，在动画显示之前，应用开始属性值（在第一个关键帧中定义）。 |
| both      | 向前和向后填充模式都被应用。                                 |

## z-index元素层级

- 谁的层级数大先显示谁，层级数一样则优先显示在html结构中靠下的。
- 祖先元素层级再高，也不会盖住后代元素。
- 目前来说只能用于：float、position

# @

@font-face

```html
@font-face {
     /* 指定字体的名字 */
    font-family: myfont;
    /* 服务器中字体的路径 */
    src: url('./font/ZCOOLKuaiLe-Regular.ttf') format('truetype');

}
```

@keyframes 【name】

@media







# background/ccisprao

background-color

background-image:url()

background-repeat

background-position

background-size:具体的值、corver、contain

background-clip: border-box/padding-box/content-box;**默认值是border-box**

background-origin: border-box/padding-box/content-box;**默认值是padding-box**

***background-attachment***：background-attachment设置为fixed后，**背景图片的位置是相对于视口的，默认停靠到origin位置**。可以通过background-position来修改位置，但是这个背景图又只能在它的父类元素内容区才能看到。

```
scroll 默认值 背景图片会跟随元素移动
fixed 背景会固定在页面中，不会随元素移动
```

background-image: radial-gradient(大小 at 位置, 颜色 位置 ,颜色 位置 ,颜色 位置)

```
大小：
    circle 圆形
    ellipse 椭圆
    closest-side 近边    
    closest-corner 近角
    farthest-side 远边
    farthest-corner 远角
```

background-image: linear-gradient(to 方向 or xturn or xdeg,color, color1 y1%,color2 y2%)意思是**以颜色color开始，到y1%这个地方颜色就完全变为color1，到y2%这个位置颜色就完全变为color2.**



**backgound**：背景相关的简写属性，所有背景相关的样式都可以通过该样式来设置

​          并且该样式没有顺序要求，也没有哪个属性是必须写的

​          注意：

​            background-position必须写在background-size的前边，并且使用/隔开

​              background-position/background-size

​            background-origin background-clip 两个样式 ，orgin要在clip的前边

# box

box-sizing: content-box/border-box

box-shadow: offset-x | offset-y | blur-radius | spread-radius | color | inset(放首或尾)

border-radius：



# font

## 图标字体

- 方法一：

  - ```html
    <i class="fas fa-cat"></i>
    ```

- 方法二：

  - ```html
    <span class="fas">&#xf0f3;</span>  <!--&#x图标的编码-->
    ```

- 方法三：

  - 通过伪元素来设置图标字体
       1.找到要设置图标的元素通过before或after选中
       2.在content中设置字体的编码
       3.设置字体的样式

    ```css
    fab
    font-family: 'Font Awesome 5 Brands';
    
    fas
    font-family: 'Font Awesome 5 Free';
    font-weight: 900; 
    ```

    ```css
    li::before{
    			content: '\f5d2';
                /* font-family: 'Font Awesome 5 Brands'; */
                font-family: 'Font Awesome 5 Free';
                font-weight: 900; 
                color: blue;
                margin-right: 10px;
            }
    ```



**font:** 字体weight 字体style *字体大小/行高 字体族*

​              - 后三个必须按照顺序写

​              - 字体weight 字体style 和行高可以省略不写，不写的话就是默认值，

​                而且即使前边已经对这三个进行了修改，在这里也会把他们重置回默认值



# 其他

## 一些标签

### a

- target:用来指定超链接的打开位置
            _self: 默认值，在当前标签页面中打开
            _blank：在新的标签页中打开超链接
        
        ```html
        <a href="No-meta.html" target="_blank">超链接3</a>
        <a href="No-块和行内元素.html" target="_self">超链接4</a>
        ```

- 测试跳转到本页其他位置
              -#：单一个#表示回到顶部
              -#id：#加id表示跳转到指定id的标签位置

```html
<a href="#bottom">我要去底部了</a>
<a href="#" id="bottom">回到顶部</a>
```

```
:link 用来表示没访问过的链接（正常的链接）
:visited 用来表示访问过的链接
:hover 用来表示鼠标移入的状态
:active 用来表示鼠标点击时
```

**a内的img............等不能把a的border撑起来，效果像是溢出，但没有覆盖，溢出部分依然占据位置，且那些溢出部分依然属于链接**

### img

img属于替换元素，介于块元素和行内元素之间，具有两种元素的特点

- src: 指定图片路径
- **alt:** 对图片的描述，在浏览器中不会显示。有些浏览器会在图片无法加载时显示。用户搜索图片时，浏览器会根据alt内容来识别图片
- title: 鼠标滑过图片时显示的文字

**布局时，图片底部会有一条缝隙，一般display: block; 或者vertical-align: bottom/top；**

```
图片的格式：
    jpeg(jpg):支持颜色较为丰富，不支持透明效果，不支持动图；一般用来显示照片
    gif      ：支持颜色较少，支持简单透明，支持动图       ；显示动态图
    png      ：支持颜色丰富，支持复杂透明，不支持动图     ；颜色丰富，复杂透明（网页中多用png）
    webp     ：谷歌推出的专门用来表示网页中的图片的一种格式；它具备其他图片格式所有的优点，而且文件还小；但是！！！兼容性不好
    base64   ：将图片使用base64进行编码；一般都是一些需要和网页一起加载的图片才使用base64
```

### audio

 音频标签：
            音频文件引入时，默认情况下不允许用户自己控制播放停止

- controls:是否允许用户控制播放，这个属性没有值。有这个属性则表示允许，没有则表示不允许
- autoplay:音频文件是否自动播放，有这个属性则表示自动播放。但是现在大部分浏览器都不会自动播放
- loop:是否循环播放

```html
<audio src="../media/audio.mp3" controls></audio>

<audio controls>
        对不起，您的浏览器不支持播放音频，请升级浏览器！<!--这样做的好处是：支持的浏览器不会显示文字，会显示音频播放；不支持的浏览器则会显示文字-->
        <source src="../media/audio.mp3">
        <source src="../media/audio.ogg">
        <embed src="../media/audio.mp3" type="audio/mp3" width="100" height="50"> <!--这样做的话，audio内的文字在所有浏览器中都会显示出来了-->
    </audio>
```

### vedio

```html
<video controls>
    <source src="../media/flower.webm">
    <source src="../media/flower.mp4">
    <embed src="../media/flower.mp4" type="video/mp4" width="300" height="300">
</video>
```

### iframe

内联框架：用于向当前页面中引入一个其他页面,很少使用

	- src：指定要引入的网页路径

```html
<iframe src="https://www.qq.com" width="800" height="600" frameborder="0">看看加点东西有什么反应，好像没反应</iframe>
```

### table

- table是一个特殊的**块元素**，不设置width的时候，**不会自动最宽**，也就是默认不是auto了吧

- 如果表格中没有使用tbody而是直接使用tr， 那么浏览器会自动创建一个tbody，并且将tr全都放到tbody中。所以，tr不是table的子元素，而是tbody的

**border-collapse**设置table边框的合并

​        collapse

​        separate

**border-spacing**设置边框间的距离，左右，上下

### form

```
autocomplete="off" 关闭自动补全
readonly 将表单项设置为只读，不能改动，数据会提交
disabled 将表单项设置为禁用，数据不会提交
autofocus 设置表单项自动获取焦点（也即默认在这个输入框开始输入）
checked 多选选中
selected 下拉列表选中
```

**form表单：**[D:\E\webWorkspace\htmlcssStudy\07html-table\04表单.html]()



## 一些属性

### white-space

**white-space** 设置网页如何处理空白

​          可选值：

​            normal 正常

​            nowrap 不换行

​            pre 保留空白

### text-indent

​	将段落的第一行缩进 x像素：

### text-overflow



### border-collapse

border-collapse设置边框的合并
    collapse
    separate

### title

当鼠标放在元素上的时候，就会出现title内容

### alt

一般是给浏览器看的，或者当元素加载失败的时候显示的内容

### backface-visibility

是否显示元素的背面：visible/hidden



# less

less是一门css的预处理语言

- less是一个css的增强版，通过less可以编写更少的代码实现更强大的样式

- 在less中添加了许多的新特性：像对变量的支持、对mixin的支持... ...
- less的语法大体上和css语法一致，但是less中增添了许多对css的扩展，所以浏览器无法直接执行less代码，要执行必须向将less转换为css，然后再由浏览器执行

**import "url"**：用来将其他的less引入到当前的less中

- @

  ```
  变量的语法： @变量名:变量值;
  @a:100px;
  @b:#bfa;
  @c:box6;
  .box5{
      // 使用变量时，如果是直接使用则以 @变量名 的形式使用即可
      width: @a;
      color: @b;
  }
  .@{c}{
      width: @a;
      background-image: url('@{c}/1.png');
  }
  ```

- $

  ```
  $属性名
  div{
      color: @b;
      background-color: $color;//$属性，可以引用其他属性的值
  }
  ```

- &

  ```
  & 就表示外层的祖先元素，外边写的啥，&就是啥
  	&:hover{
          color: orange;
      }
      div &{
          width: 100px;
      }
      // !!!!!!!去看看效果
      &-button{
          color: blue;
      }
  ```

- extend()：效果是并集选择器

```css
.p1{
    width: 100px;
    height: 100px;
}
//:extend() 对当前选择器扩展指定选择器的样式（选择器分组）
.p2:extend(.p1){
    color: red;
}


转化后
.p1,
.p2 {
  width: 100px;
  height: 100px;
}
.p2 {
  color: red;
}
```

- 内部

  ```
  .p3{
      //直接对指定的样式进行引用，这里就相当于将p1的样式在这里进行了复制
      //mixin 混合
      .p1();//.p1;加不加括号都可以
      color: orange;
  }
  转化后，把.p1
  .p3 {
    width: 100px;
    height: 100px;
    color: orange;
  }
  ```

- .p1(){}：这个就不会单独出现在css中，单纯被用来引用

  ```
  // 使用类选择器时可以在选择器后边添加一个括号，这时我们实际上就创建了一个mixins
  .p4(){
      width: 100px;
      height: 100px;
  }
  
  .p5{
      .p4();
  }
  ```

- ```
  //混合函数 在混合函数中可以直接设置变量
  .test(@bg-color:#bfa,@w:100px,@h:200px){
      width: @h;
      height: @w;
      border: 1px solid @bg-color;
  }
  
  div{
      // .test;
      //调用混合函数，按顺序传递参数
      // .test(200px,300px,#bfa);
      //或者直接指定传递
      .test(@bg-color:red,@w:10px);
  }
  ```





# 像素 px pt dpi dp vw

## px

是最小的独立显示单位，px均为整数，不会出现0.5px的情况。



## pt

Pt，则是point的缩写，一般音译为磅数，一磅等于1/72英寸。在国际上一般会用pt作为字体的单位。



## dpi

在一定尺寸的物理屏幕上显示像素的数量，一般使用dpi(dots per inch，每英寸像素数)作为单位。



## dp

Density Independent Pixel，可以翻译为密度无关像素。dp在不同密度的屏幕中实际显示比例将保持一致。根据规定，一个dp相当于160dpi屏幕中的一个px。在320dpi的屏幕中，一个dp相当于2个px。通过这样的成比例放缩，Android解决了需要多个不同屏幕中的大小显示问题。



## vw

vw 表示的是视口的宽度（viewport width）
   - 100vw = 一个视口的宽度
   - 1vw = 1%视口宽度

**vw这个单位永远相当于视口宽度进行计算**



# 响应式布局

**使用媒体查询** 

- 语法： @media 查询规则{}
- 媒体类型：
  - all 所有设备
  - print 打印设备
  - screen 带屏幕的设备
  - speech 屏幕阅读器

                    - 可以使用“,”连接多个媒体类型，这样它们之间就是一个或的关系

- 可以在媒体类型前添加一个only，表示只有。only的使用主要是为了兼容一些老版本浏览器

```
@media only screen and (min-width: 500px) and (max-width:700px){
    body{
       background-color: #bfa;
    }
}
```