# [创建前端平移动画为何 translate() 优于 top/right/bottom/left](https://segmentfault.com/a/1190000010364647)

## 前言

研究这个问题的起因，源于我在阅读[作曲家与听众](http://efe.baidu.com/blog/composers-and-audiences/)这篇文章时，看到的这样一句话。

> 例如，我对很多开发者（不管新手还是老手）仍然使用 CSS 的 top 和 left 而不是 transform 创建平移动画感到震惊，尽管只要你在除了 8 核 MacBook Pro 之外的设备上进行过测试，就会发现帧率的差别极其明显。

看到这句话我也同样震惊，震惊之处正如作者所言，我就是从未在意过两者差别而使用 top 和 left 创建平移动画的人之一。那么为什么两者会有区别，让我们来一探究竟吧。

## 测试用例

其实很惭愧，早在2012年就有许多人在博文中阐述这一事实了，也因此，我能很轻松的找到非常棒的demo（来源见参考文献），如下：

- [top、left实现基于关键帧的平移动画](https://codepen.io/paulirish/full/nkwKs)
- [translate实现基于关键帧的平移动画](https://codepen.io/paulirish/full/LsxyF)

相信读者在把Macbook Pro增加到一定数量的时候，都能发现两者之间的差异了吧。

## 深入背后

现在让我们来探究其中的原因。打开Chrome dev tools，切到Performance选项，记录一段时间后，选择某一帧查看各个过程所占时间。

首先是top、left的时间线：

![top、left的时间线](https://segmentfault.com/img/bVRElB?w=1076&h=658)

在这一帧中，我们看到多个步骤占用了CPU时间。

换到translate则变成了截然不同的样子：

![图片描述](https://segmentfault.com/img/bVRElU?w=1075&h=603)

根本没有CPU时间。我看了很多帧，除了少部分帧有0.1ms左右的脚本执行时间之外，大部分帧的CPU时间都是0。

答案已经很显然了，使用translate来做平移运动，大部分时间都不需要CPU参与。这个结果和五年前的文章不太相同，但时间已经过了五年，浏览器的优化至此地步也不算出乎意料吧。

接下来我们打开Render，开启Paint Flashing和Layer Borders。

![Rendering](https://segmentfault.com/img/bVREn1?w=411&h=437)

![开启选项](https://segmentfault.com/img/bVREn7?w=470&h=316)

对于top、left我们会看到如下情况：

![图片描述](https://segmentfault.com/img/bVREpm?w=420&h=313)

其中绿框是浏览器重绘的区域，对于使用position的情况，浏览器要在动画的执行中不停地绘制绿框中的区域。

而对于transform则是另一幅场景：

![QQ20170726-200457.gif](https://segmentfault.com/img/bVRF91?w=383&h=419)

我们可以看到transform的版本我们的MacBook图片周围会多一个框框，这是因为transform会生成一个新的层，而对这个图层进行变换，对于浏览器来说，是不需要重绘的。包括在我点击“add 10 more macbook”之后，也只有插入元素的时候会引起重绘。同样，新插入的“MacBook”也各自有独立的图层，不会引起重绘。

那么，Chrome创建层的标准是什么呢？在[Accelerated Rendering in Chrome](https://www.html5rocks.com/en/tutorials/speed/layers/)中有如下表述：

> What else gets its own layer? Chrome’s heuristics here have evolved over time and continue to, but currently any of the following trigger layer creation:
>
> 1. 3D or perspective transform CSS properties
> 2. <video> elements using accelerated video decoding
> 3. <canvas> elements with a 3D (WebGL) context or accelerated 2D context
> 4. Composited plugins (i.e. Flash)
> 5. Elements with CSS animation for their opacity or using an animated transform
> 6. Elements with accelerated CSS filters
> 7. Element has a descendant that has a compositing layer (in other words if the element has a child element that’s in its own layer)
> 8. Element has a sibling with a lower z-index which has a compositing layer (in other words the it’s rendered on top of a composited layer)

所以，对于我们的transform动画，其符合上述第五条规则中的“using an animated transform”，因此会创建自己独立的层；同样，我们也可以根据上述规则来给top、left方案创建独立的层来减少重绘。

接下来，根据第一条规则，让我们在top、left示例中的`.macbook`类上面加上这样一行css：

```
transform: translateZ(0);
```

我们就能看到和translate方案一样的场景了。

![加上3D CSS属性的position方案](https://segmentfault.com/img/bVRGyV?w=1478&h=878)

通过查阅资料，我简单说一下我的理解。Chrome会预先将层的内容绘制成位图发送给GPU，如果层仅仅是位置与透明度等特定的一些属性发生变化，而不是内容发生变化，则无需重绘这一位图。因此无论是translate动画方案，还是加上3D变换属性的top、left方案，由于其都在独立的层上，且只是发生位置变化，因此无需重绘。

然而，层并不是越多越好。参考[Accelerated Rendering in Chrome](https://www.html5rocks.com/en/tutorials/speed/layers/)中的表述。

> But beware just blindly creating them, as they’re not free: they take up memory in system RAM and on the GPU (particularly limited on mobile devices) and having lots of them can introduce other overhead in the logic keeps track of which are visible. Many layers can also actually increase time spent rasterizing if they layers are large and overlap a lot where they didn’t previously, leading to what’s sometimes referred to as “overdraw”. So use your knowledge wisely!

简单翻译一下就是：层会占用系统 RAM 与 GPU(在移动设备上尤其有限)的内存，并且拥有大量的层会因为记录哪些是可见的而引入额外的开销。许多层还会因为过大与许多内容重叠而导致“过度绘制(overdraw)”的情况发生，从而增加栅格化的时间。关于这一点，在[Jank Busting Apple's Home Page](http://wesleyhales.com/blog/2013/10/26/Jank-Busting-Apples-Home-Page/)中也有所体现。

> In this case, too many elements have translateZ(0) applied when only one or two applications are really needed. This is forcing a longer composite time and ultimately giving the animations some jank.

“So use your knowledge wisely!”

## 参考文献

- [A Tale of Animation Performance](https://css-tricks.com/tale-of-animation-performance/)
- [Why Moving Elements With Translate() Is Better Than Pos:abs Top/left](https://www.paulirish.com/2012/why-moving-elements-with-translate-is-better-than-posabs-topleft/)
- [Accelerated Rendering in Chrome](https://www.html5rocks.com/en/tutorials/speed/layers/)
- [Jank Busting Apple's Home Page](http://wesleyhales.com/blog/2013/10/26/Jank-Busting-Apples-Home-Page/)