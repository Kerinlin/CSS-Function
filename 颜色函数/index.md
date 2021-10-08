# CSS 函数那些事（六）多姿多彩颜色函数

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1923f4ec70cd47daa6954ffc8a68f5e2~tplv-k3u1fbpfcp-zoom-1.image)

## rgb(),rgba()
#### rgb(red,green,blue)
rgb(red,green,blue),分别代表着red,green,blue,是色彩三原色，原色以不同比例混合时，会产生其他颜色。为什么会是这三种颜色？这里引用下维基百科的解释：
> 人的眼睛内有几种辨别颜色的锥状细胞，分别对黄绿色、绿色和蓝紫色（564、534和420纳米波长）的光最敏感。人类以及其他具有这三种感光受体的生物称为“三色感光体生物”。虽然眼球中的椎状细胞并非对红、绿、蓝三色的感受度最强，但是由肉眼的椎状细胞所能感受的光的带宽很大，红、绿、蓝也能够独立刺激这三种颜色的受光体，因此这三色被视为原色。"原色"的指定并没有唯一的选法，因为就理论上而言，凡是彼此之间无法替代的颜色都可以被选为“原色”，只是目前普遍认定“光的三原色”为红绿蓝。

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/83b340ed943d424d906e02546b44b7b7~tplv-k3u1fbpfcp-zoom-1.image)

rgb函数接收三个参数：

**R：** 红色值。正整数[0-255] | 百分数[0%-100%]

**G：** 绿色值。正整数[0-255] | 百分数[0%-100%]

**B：** 蓝色值。正整数[0-255] | 百分数[0%-100%]

#### rgba(red,green,blue,alpha)
rgba在rgb的基础上添加了不透明度(alpha):

**A：** 不透明度。取值[0-1]，0代表完全透明，也就是完全看不见，1代表完全不透明，也就是颜色本身

#### 为什么颜色数值最大为255？
rgb在计算机中存储为3 byte,每个byte是8个bits，所以rgb对应的最大2进制为11111111,换算成十进制为256，这就意味着有256个不同值，从0开始算，所以最大值为255.颜色的种数那就是256^3 = 16,777,216.

## hsl(),hsla()
#### hsl(hue,saturation,lightness)
hsl,使用**色调(hue)**,**饱和度(saturation)**,**亮度(lightness)** 表示各种颜色。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e0718f53a49e4d05a763731612e20fe7~tplv-k3u1fbpfcp-zoom-1.image)

**色调(hue)**,取值[0,360],代表的是人眼所能感知的颜色范围，这些颜色分布在一个平面的色调环上，取值范围是0°到360°的圆心角，每个角度可以代表一种颜色。色调的意义在于，我们可以在不改变光感的情况下，通过旋转色相环来改变颜色。

![色调环](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTmm1iAtnCpBzZfmYm-rPomRU6cEyGtFAv8Jg&usqp=CAU)

**饱和度(saturation)**，取值[0%,100%],用0%至100%的值描述了相同色调、亮度下色彩纯度的变化。数值越大，颜色中的灰色越少，颜色越鲜艳，呈现一种从灰度到纯色的变化。

**亮度(lightness)**,取值[0%,100%],亮度的作用是控制色彩的明暗变化。它同样使用了0%至100%的取值范围。数值越小，色彩越暗，越接近于黑色；数值越大，色彩越亮，越接近于白色。

hsl更符合人类对颜色表达的习惯，也就是"**什么颜色？颜色多深？颜色多亮？**"。

#### hsla()
与rgba一样，hsla在hsl基础上添加了不透明度(alpha),取值[0-1];

#### hsl与rgb
RGB的几何模型是一个立方体，HSL 则是一个圆柱体，它们之间存在着非线性转换的关系。
```javascript
function hslToRgb(hue, sat, light) {
  if( light <= .5 ) {
    var t2 = light * (sat + 1);
  } else {
    var t2 = light + sat - (light * sat);
  }
  var t1 = light * 2 - t2;
  var r = hueToRgb(t1, t2, hue + 2);
  var g = hueToRgb(t1, t2, hue);
  var b = hueToRgb(t1, t2, hue - 2);
  return [r,g,b];
}

function hueToRgb(t1, t2, hue) {
  if(hue < 0) hue += 6;
  if(hue >= 6) hue -= 6;

  if(hue < 1) return (t2 - t1) * hue + t1;
  else if(hue < 3) return t2;
  else if(hue < 4) return (t2 - t1) * (4 - hue) + t1;
  else return t1;
}
```

## hwb()
**hwb(hue,whiteness,blackness)**,hwb类似于hsl，也是一种将RGB色彩模型中的点在圆柱坐标系中的表示法。
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/200cdedb83114781b4ac0f725b32b03d~tplv-k3u1fbpfcp-zoom-1.image)

**H(hue)**：表示色调,与hsl中的色调一样，取值范围[0-360];

**W(whiteness)** :  表示白色浓度，取值[0%,100%];

**B(blackness)**:  表示黑色浓度，取值[0%,100%];

由于这个属性暂时还是实验属性，这里只了解一下，不做过多的说明，有兴趣的同学可以去[这里](https://web.archive.org/web/20150709065126/http://dev.w3.org/csswg/css-color/#funcdef-hwb)了解一下

## lab()
lab函数用来表示**CIELAB色彩空间**,表示为L*a*b*, 是CIE(国际照明委员会)在1976年定义的色彩空间.
![](https://sensing.konicaminolta.asia/wp-content/uploads/2018/09/3D-LAB.jpg)


**L** :  表示亮度,取值[0%,100%],取值0%是黑色，取值100%是白色；
**A** :  表示在a轴上的坐标值
**B**：表示在b轴上的坐标值
a,b的值理论上是无限的，但是实际上是不会超过±160。

这个函数处于也是实验阶段。

## lch()

lch(lightness ,chroma ,hue),采用了和CIE L*a*b*相同的颜色空间, 不过表达的方式有不同。
![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e4af543af1c64066b526475f1b907053~tplv-k3u1fbpfcp-zoom-1.image)

**L：** 代表亮度,取值[0%，100%];

**C:** 色度，即色彩饱和程度，最小值为0，理论上没有最大值,

**h:** 色调，即色彩的总体倾向，取值[0,360]。

这个函数处于实验阶段。

## device-cmyk()
device-cmyk()
device-cmyk函数用来表示**印刷四分色模式（CMYK）**,相对于RGB的加色混色模型，CMYK是减色混色模型，颜色混在一起，亮度会降低。之所以加入黑色是因为打印时由品红、黄、青构成的黑色不够纯粹。这个函数包含4个参数：

**C:** Cyan,青色，取值[0-1],或者[0%,100%];

**M:**  Magenta ,品红色，取值[0-1],或者[0%,100%];

**Y:** Yellow ,黄色，取值[0-1],或者[0%,100%];

**K:** Black,黑色，取值[0-1],或者[0%,100%];

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/205edcf89a5b41eea9fd83bd4037669b~tplv-k3u1fbpfcp-zoom-1.image)

这个函数处于实验阶段。

## 总结

目前浏览器已经能支持hsl,以及hsla这两个新的函数，其他的用来表示色彩的函数也已经在 CSS Color Module Level 4 的草案中了，写这篇文章提前先了解下这些色彩模型。


## 最后

我最近在总结 css 函数相关的东西，这篇是系列文章的第六篇，目前已产出

- [CSS 函数那些事（一）比较函数](https://juejin.cn/post/6898141267771801613 "CSS函数那些事（一）比较函数")
- [CSS 函数那些事（二）你不知道的 attr()](https://juejin.cn/post/6898945436988473358 "CSS 函数那些事（二）你不知道的 attr()")
- [CSS 函数那些事（三）背景图片函数](https://juejin.cn/post/6901848317823549454 "CSS函数那些事（三）背景图片函数")
- [CSS 函数那些事（四）网格函数](https://juejin.cn/post/6907061355291869197 "CSS函数那些事（四）网格函数")

- [CSS 函数那些事（五）计数函数](https://juejin.cn/post/6909709349183029256 "CSS函数那些事（四）计数函数")

项目中会包含文章中的测试代码，都做好了相应的分类，欢迎各位持续关注，有帮助话可以点个赞哦！[项目地址戳这里](https://github.com/Kerinlin/CSS-Function "项目地址戳这里")



## 参考资料
1. [CSS color](http://dev.w3.org/csswg/css-color/)
2. [HLS原理](https://www.zhihu.com/question/22077462)
3. [CSS Color Module Level 4 (CSS Color 4)](https://drafts.csswg.org/css-color)





