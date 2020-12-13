# CSS函数那些事（四）网格函数

![reshBn.png](https://s3.ax1x.com/2020/12/13/reshBn.png)

这3个函数**都只能在网格布局中使用**
## fit-content()
fit-content函数，接收一个参数，长度值，可以按照字面意思来解释他的作用，"适应内容"。

    <div class="fit-content-wrapper">
      <div class="fit-item item1">test1dsssss3333333 sssssssssssssss sssssssssssssssssss sssssssssssssssssss ssssssssssssssssssss 这是用了fit-content(400px)</div>
      <div class="fit-item item2">test2 这是固定宽度width：400px</div>
      <div class="fit-item item3">test3 这是fit-content(400px)</div>
    </div>

      .fit-content-wrapper{
        width: 100%;
        height: 200px;
        display: grid;
        grid-template-columns: fit-content(400px) 400px fit-content(400px);
        grid-gap: 10px;
      }
      .fit-item{
        background-color: rgb(20, 106, 177);
      }

效果

![re7g3j.png](https://s3.ax1x.com/2020/12/13/re7g3j.png)

可以看到,**当内容长度大于给定长度时，文字会自动换行，不会超过给定长度，当内容长度小于给定长度时，会按照给定的内容长度设置长度。**


#### 兼容性

![rehXuT.png](https://s3.ax1x.com/2020/12/13/rehXuT.png)

兼容性对于现代浏览器没有什么问题，新版本主流浏览器基本都能支持，对于需要支持ie的项目则不能使用。

## minmax()
minmax函数表示一个闭区间范围[min,max]，当值小于等于min时，值等于min,当大于等于max时，值等于max.

#### min-content，max-content
minmax函数接收min-content,max-content参数，这两个参数表示内容最短和最长的内容长度。看下面案例。

        <div class="minmax-wrapper">
          <div class="minmax-item">
            test1dsssss3333333 sssssssssssssss sssssssssssssssssss
            sssssssssssssssssss ssssssssssssssssssss
          </div>
          <div class="minmax-item">
            <p>test2222222222</p>
            <p>test 232232323233</p>
            <p>min-content采用最短的内容长度</p>
          </div>
          <div class="minmax-item">
            <p>test</p>
            <p>test 232232323233222222</p>
            <p>max-content采用最长的内容长度</p>
          </div>
        </div>

          .minmax-wrapper {
            margin-top: 100px;
            width: 100vw;
            display: grid;
            grid-gap: 10px;
            grid-template-columns:
              minmax(300px, 500px) minmax(50px, min-content)
              minmax(100px, max-content);
          }

效果

![rmCx4s.png](https://s3.ax1x.com/2020/12/13/rmCx4s.png)

可以看到，第二个项目的最小的内容宽度为第二个项目中的第一个p标签

![rmPVUJ.png](https://s3.ax1x.com/2020/12/13/rmPVUJ.png)

当设置成minmax(50px,min-content)时，表示列宽最大的宽度也不能超过第一个p标签的内容宽度。

第三个项目的最大的内容宽度为第三个p标签的内容宽度

![rmiz60.png](https://s3.ax1x.com/2020/12/13/rmiz60.png)

当设置成minmax(100px,max-content)时，最大的内容宽度不会超过第三个p标签的宽度

#### 兼容性

![rmFs9s.png](https://s3.ax1x.com/2020/12/13/rmFs9s.png)

跟fit-content函数一样，不支持ie,但对主流的现代浏览器支持还不错。

## repeat()
repeat函数用来批量处理网格，接收2个参数，第一个参数表示执行次数，第二个参数表示长度。看下面例子

        <div class="repeat-wrapper">
          <div class="repeat-item">test1 3</div>
          <div class="repeat-item">test2 23</div>
          <div class="repeat-item">test3 444</div>
        </div>

          .repeat-wrapper {
            margin-top: 100px;
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-gap: 10px;
          }

效果

![rmElAU.png](https://s3.ax1x.com/2020/12/13/rmElAU.png)

grid-template-columns: repeat(3, 100px) 等价于 grid-template-columns: 100px 100px 100px;

#### auto-fill,auto-fit
第一个参数除了指明具体次数外，repeat还接收这几个参数 auto-fill,auto-fit，下面讲一讲这两个参数的概念。

#### auto-fill
auto-fill表示由浏览器自动根据项目填充次数。当容器很宽的时候，会自动留出剩余格子的宽度。如果网格容器在相关轴上具有确定的大小或最大大小，则重复次数是最大可能的正整数，不会导致网格溢出其网格容器。

        <div class="repeat-wrapper">
          <div class="repeat-item">test1 3</div>
          <div class="repeat-item">test2 23</div>
          <div class="repeat-item">test3 444</div>
          <div class="repeat-item">test3 4444</div>
          <div class="repeat-item">test3 444</div>
          <div class="repeat-item">test3 444</div>
        </div>

    grid-template-columns: repeat(auto-fill, minmax(100px,1fr));

效果
![rm1VOg.gif](https://s3.ax1x.com/2020/12/13/rm1VOg.gif)

#### auto-fit
auto-fit也会自动计算，但是与auto-fill不同的是，auto-fit不会保留剩余的空格子，会将auto-fill剩余的空格子重新分配到每个格子中。看下面示例

        <div class="repeat-wrapper">
          <div class="repeat-item">test1 3</div>
          <div class="repeat-item">test2 23</div>
          <div class="repeat-item">test3 444</div>
          <div class="repeat-item">test3 4444</div>
          <div class="repeat-item">test3 444</div>
          <div class="repeat-item">test3 444</div>
        </div>

    grid-template-columns: repeat(auto-fit, minmax(100px,1fr));

效果
![rm3GCt.gif](https://s3.ax1x.com/2020/12/13/rm3GCt.gif)

#### 兼容性

[![rm8SPI.png](https://s3.ax1x.com/2020/12/13/rm8SPI.png)](https://imgchr.com/i/rm8SPI)

最新版本的主流浏览器基本都能支持，依旧不支持ie。

## 总结
这3个网格函数极大的丰富的网格布局，之前用网格布局用的不多，但是今天学习这3个函数以及相关的一些参数后，发现网格布局对比其他布局也是很方便的，后面在一些自己的小项目中可以试着用一下.

## 最后
我最近在总结css函数相关的东西，这篇是系列文章的第四篇，目前已产出
- [CSS函数那些事（一）比较函数](https://juejin.cn/post/6898141267771801613 "CSS函数那些事（一）比较函数")
- [CSS 函数那些事（二）你不知道的 attr()](https://juejin.cn/post/6898945436988473358 "CSS 函数那些事（二）你不知道的 attr()")
- [CSS函数那些事（三）背景图片函数](https://juejin.cn/post/6901848317823549454 "CSS函数那些事（三）背景图片函数")

项目中会包含文章中的测试代码，都做好了相应的分类，欢迎各位持续关注，有帮助话可以点个赞哦！[项目地址戳这里](https://github.com/Kerinlin/CSS-Function "项目地址戳这里")