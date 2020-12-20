# CSS 函数那些事（五）计数函数

![image.png](https://i.loli.net/2020/12/20/dQc4XgFWS71tsej.png)

## counter()

counter 返回一个代表计数器当前值的字符串。接收两个参数，一个名称，一个计数样式。**counter(name,styleName)**,name 区分大小写，作为代表当前计数器的名称标识。styleName 参数是可选的，代表递增数字或者字母的种类，可接受的参数为 **list-style-type** 所支持的种类。常用的有以下这些:

- disc （实心圆点）
- circle （空心圆点）
- square （实心方块）
- decimal （阿拉伯数字 12345...）
- lower-roman（罗马数字 i, ii, iii...）
- upper-roman (罗马数字 I, II, III, IV...)
- simp-chinese-informal (中文计数 一、二、三、....九十九、)
- simp-chinese-formal （中文繁体 壹贰叁肆伍...）
- lower-latin (小写字母 abcd...)
- upper-latin (大写字母 ABCD....)
- ...

更多信息以及兼容性可看 [MDN list-style-type](https://developer.mozilla.org/zh-CN/docs/Web/CSS/list-style-type "MDN list-style-type")

---

与计数器利益相关的还有两个属性值：

- **counter-reset**
- **counter-increment**

---

#### counter-reset,counter-increment

**counter-reset** 用于重置 CSS 计数器，重置内容包括名称，初始数字。例子：

    	<div class="demo1"></div>

          .demo1 {
            counter-reset: counter1 123;
          }
          .demo1:before {
            content: counter(counter1,simp-chinese-formal);
          }

效果

![image.png](https://i.loli.net/2020/12/20/ehidJGm4Ojc9fUq.png)

---

counter-increment 用于代表计数器的递增间隔，看代码

        <div class="demo2">
          <section></section>
          <section></section>
          <section></section>
          <section></section>
        </div>
          .demo2{
            counter-reset: counter2 1;
            /* counter-increment: counter2 -2; */
          }
          section:before {
            content: counter(counter2,decimal);
            counter-increment: counter2 2;
          }

效果

![image.png](https://i.loli.net/2020/12/20/9QxF6KbYUwLdsOc.png)

#### 兼容性

![image.png](https://i.loli.net/2020/12/20/rtmzQVyMJFY8Efn.png)

基本都支持

## counters()

counters()是一个嵌套计数器，用于定义嵌套计数器的连接字符.**counters(counterName,string,styleName)**,接收 3 个参数 counterName,string,styleName.其中第三个参数是可选的。看栗子

        <div class="father">
          <div class="son">
            内容一
            <div class="father">
              <div class="son">子内容一</div>
              <div class="son">子内容二</div>
              <div class="son">子内容三</div>
            </div>
          </div>
          <div class="son">
            内容二
            <div class="father">
              <div class="son">
                子内容一
                <div class="father">
                  <div class="son">子子内容一</div>
                  <div class="son">子子内容二</div>
                </div>
              </div>
              <div class="son"></div>
              <div class="son"></div>
              <div class="son"></div>
            </div>
          </div>
          <div class="son">
            内容三
          </div>
        </div>

          .father {
            counter-reset: counter3;
            padding-left: 30px;
          }
          .son:before {
            content: counters(counter3, "-")'.';
            counter-increment: counter3;
          }

效果

![image.png](https://i.loli.net/2020/12/20/Owu2ZiWvpkcbr6h.png)

列表元素用 counters 定义相互之间的计数连接规则，可以很方便模拟有序列表。

#### 兼容性

![image.png](https://i.loli.net/2020/12/20/TVZ4hwnJzvCWt9X.png)

兼容性跟 counter 一样

## 总结

counter 类比 ol,ul，在样式的把握上，会更加灵活，设置样式也更加随心所欲。对于有列表相关样式优化的项目，可以考虑使用 counter(),counters()来优化。兼容性也不错。

## 最后

我最近在总结 css 函数相关的东西，这篇是系列文章的第四篇，目前已产出

- [CSS 函数那些事（一）比较函数](https://juejin.cn/post/6898141267771801613 "CSS函数那些事（一）比较函数")
- [CSS 函数那些事（二）你不知道的 attr()](https://juejin.cn/post/6898945436988473358 "CSS 函数那些事（二）你不知道的 attr()")
- [CSS 函数那些事（三）背景图片函数](https://juejin.cn/post/6901848317823549454 "CSS函数那些事（三）背景图片函数")
- [CSS 函数那些事（四）网格函数](https://juejin.cn/post/6907061355291869197 "CSS函数那些事（四）网格函数")

项目中会包含文章中的测试代码，都做好了相应的分类，欢迎各位持续关注，有帮助话可以点个赞哦！[项目地址戳这里](https://github.com/Kerinlin/CSS-Function "项目地址戳这里")
