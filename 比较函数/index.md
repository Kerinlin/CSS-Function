# CSS 比较函数

![D8vrLQ.png](https://s3.ax1x.com/2020/11/22/D8vrLQ.png)

CSS比较函数有三个：
- max()
- min()
- clamp()

## min与max
CSS min,max函数作用类似于js函数中的min,max，用于取多个属性中的最小值或者最大值，属性之间用逗号分隔。例子如下

          width: min(100px,200px,300px); //取值100px
          height: max(100px,200px,300px); //取值300px


![D8mqOA.png](https://s3.ax1x.com/2020/11/22/D8mqOA.png)

如图，宽度取了最小值100px,高度取了最大值300px.


## clamp
clamp函数需要传入3个参数，一个最小值，一个默认值，一个最大值，用于处理边界值，当**默认值**大于最大值时，取最大值，小于最小值时，取最小值，介于最小与最大之间时，取默认值。


### 使用方法

    clamp(MIN,DEFAULT,MAX)

clamp就相当于** max(MIN,min(DEFAULT,MAX))**

**案例**

    font-size: clamp(20px,10vw,40px);

分析下，当10vw小于20px，也就是页面宽度小于等于200px时，字体最小为20px,当10vw大于40px,也就是页面宽度大于等于400px时，字体最大为40px.处于200px-400px之间的，则按照 width/10的计算公式进行计算，下面验证一下

#### 小于200px 

![D83xzV.png](https://s3.ax1x.com/2020/11/22/D83xzV.png)


#### 大于400px

![D882OU.png](https://s3.ax1x.com/2020/11/22/D882OU.png)


#### 200px到400px之间 

![D8GSfI.png](https://s3.ax1x.com/2020/11/22/D8GSfI.png)


## 兼容性
![D8JVKK.png](https://s3.ax1x.com/2020/11/22/D8JVKK.png)

可以看出这3个函数都是最近不久才出来的，所以兼容性不太好，国产浏览器全挂，主流浏览器最新的版本基本能够支持，这是个好事，因为这三个数学在响应式开发中的作用还是很明显的，未来或许这3个函数在响应式开发中的比重会慢慢的得到提升。

## 常用的使用场景
下面会列举几个常用的[使用场景](https://ishadeed.com/article/css-min-max-clamp/ "使用场景")

#### 侧边栏响应
对于侧边栏布局，需要侧边栏固定宽度，做响应式时可以考虑超过最大宽度时通过vw来固定侧边栏的占比

          aside {
            background: #ccc;
            flex-basis: max(30vw, 150px);
          }
          main {
            background: #09acdd;
            flex-grow: 1;
          }

![D8om1s.gif](https://s3.ax1x.com/2020/11/22/D8om1s.gif)

#### 字体响应
通过clamp限制最大最小值，然后中间的默认值根据视窗改变

`font-size: clamp(20px, 10vw, 40px);`

#### 渐变平滑过渡
渐变指定渐变的梯度线，按照一般操作会出现过渡不够平滑的情况，在移动端会有一条明显的过渡线

    background: linear-gradient(135deg, #2c3e50, #2c3e50, #3498db);

![D8XXA1.png](https://s3.ax1x.com/2020/11/22/D8XXA1.png)

利用min修改一下，过渡会更加平滑一点

`background: linear-gradient(135deg, #2c3e50, #2c3e50 min(20vw, 60%), #3498db);`

[![D8jiBd.png](https://s3.ax1x.com/2020/11/22/D8jiBd.png)](https://imgchr.com/i/D8jiBd)

#### 动态容器宽度
在实际运用中，比如如果我们想在桌面端限定宽度，在移动端显示100%，需要这样写

        .container{
          width: 1440px;
          max-width: 100%;
        }

现在只需要

        .container{
          width: min(1440px,100%);
        }

非常简洁明了。

## 总结
这3个函数适用于响应式布局的开发，在不需要考虑兼容性问题的情况下可以酌情使用，但如果要考虑兼容性，还是最好不要使用。我最近在总结css函数相关的东西，欢迎各位持续关注，源码在这，[戳这里戳这里](https://github.com/Kerinlin/CSS-Function/tree/main/%E6%AF%94%E8%BE%83%E5%87%BD%E6%95%B0 "戳这里戳这里")
