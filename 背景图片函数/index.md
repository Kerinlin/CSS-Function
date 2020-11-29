# CSS函数那些事（三）背景图片函数

[![DcV9V1.png](https://s3.ax1x.com/2020/11/29/DcV9V1.png)](https://imgchr.com/i/DcV9V1)

## url()
url函数表示对某个资源的引用，可传入链接以及相对地址，如

    background-image: url('./背景图片函数.png');
	background-image: url('https://s3.ax1x.com/2020/11/29/DcV9V1.png');

## image()
image功能类似于url，但是与url不同的是，image提供了一种**优雅降级**的能力。比如

    background-image: image('a.webP','a.png','a.jpg');

这段代码意思就是，假如浏览器支持webP格式图片就应用a.webP,如果不支持，就再测试a.png，直到适配到当前浏览器。遗憾的是，这个函数目前还处于草案阶段。

![DcujxK.png](https://s3.ax1x.com/2020/11/29/DcujxK.png)

所以这个函数其他的功能暂时先不关注，有兴趣的同学，可自行去 MDN 了解更多相关的信息,也可了解 [最新进展](https://www.w3.org/TR/css-images-3/ "最新进展")

## image-set()
image-set可以保证图片在不同分辨率设备上的适配，能根据不同的设备类型展示不同的图片，看下面的例子

    background-image: -webkit-image-set(url(./bg1x.png) 1x , url(./bg2x.png) 2x);

这段例子意思就是在1倍屏上显示bg1x.png，在2倍屏上显示 bg2x.png 。兼容性上，目前最新主流的浏览器都已支持，对于不支持的设备可以在使用这个函数前使用background：url()来进行兼容。

[![Dc6Kjf.png](https://s3.ax1x.com/2020/11/29/Dc6Kjf.png)](https://imgchr.com/i/Dc6Kjf)

## cross-fade()
cross-fade用于在两张叠加的背景图片上施加透明度。用法如下

    background-image: -webkit-cross-fade(url('./bg1x.png'),url('./bg2x.png'),70%);

![DcIRFf.png](https://s3.ax1x.com/2020/11/29/DcIRFf.png)

前面两个参数为图片的资源位置，后面一个需要传入百分比，表示透明度，这个透明度是相对于最后那张图片的，比如，当百分比为0%时,此时应该只显示第一张图片

![DcHhEn.png](https://s3.ax1x.com/2020/11/29/DcHhEn.png)

当百分比为100%时，只显示第二张图片。

![DcHoCV.png](https://s3.ax1x.com/2020/11/29/DcHoCV.png)

这个属性,在firefox中完全不兼容，在chrome和safari中兼容性要好太多

[![Dcq5lT.png](https://s3.ax1x.com/2020/11/29/Dcq5lT.png)](https://imgchr.com/i/Dcq5lT)

## element()
element函数可以将网站上的某部分元素当做图片使用，适用于图片的属性同时也适用于应用element的对象，使用方法

> element(id)

必须传入的是id，看下面的例子，用element实现了一种类似双向绑定的功能效果

          <div class="element-wrapper">
            <span id="ele" contenteditable>hello world</span>
          </div>

          <div id="element-test"></div>

    //style
          .element-wrapper{
            width: 200px;
            height: 200px;
          }
          #element-test {
            width: 200px;
            height: 200px;
            background: -moz-element(#ele);
            background-size: contain;
            background-repeat: no-repeat;
          }

效果图

![DcvzeU.gif](https://s3.ax1x.com/2020/11/29/DcvzeU.gif)

这个属性还能做到更多有趣的效果，更多有趣的效果可去[这里查看](https://www.w3cplus.com/css4/css-element-function.html "这里查看")，在兼容性上，遗憾的是目前只有firefox支持这个属性

[![DcOLz6.png](https://s3.ax1x.com/2020/11/29/DcOLz6.png)](https://imgchr.com/i/DcOLz6)

## 最后
我最近在总结css函数相关的东西，系列的大纲

![DQs4IO.png](https://s3.ax1x.com/2020/11/20/DQs4IO.png)

这篇是系列文章的第三篇，目前已产出
- [CSS函数那些事（一）比较函数](https://juejin.cn/post/6898141267771801613 "CSS函数那些事（一）比较函数")
- [CSS 函数那些事（二）你不知道的 attr()](https://juejin.cn/post/6898945436988473358 "CSS 函数那些事（二）你不知道的 attr()")

项目中会包含文章中的测试代码，都做好了相应的分类，欢迎各位持续关注，有帮助话可以点个赞哦！[项目地址戳这里](https://github.com/Kerinlin/CSS-Function "项目地址戳这里")