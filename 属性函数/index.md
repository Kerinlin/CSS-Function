# CSS 函数那些事（二）你不知道的 attr()

属性函数 attr() 用于获取HTML元素里面的属性值,并用于样式中，但目前暂时只能应用于CSS元素中的伪元素。

### 例子

**实现一个Tooltip**

![DU9XAf.png](https://s3.ax1x.com/2020/11/24/DU9XAf.png)


    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>css attr函数</title>
        <style>
          .tooltip {
            width: 100px;
            position: relative;
            margin: 0 auto;
          }
          .tooltip:hover::after {
            padding: 5px;
            position: absolute;

            /* 在伪元素中作为字符串中使用 */
            content: attr(data-tooltip);
            color: #fff;
            background-color: #000;
            border-radius: 10px;
            top: 25px;
            left: 0;
          }

          /* 箭头 */
          .tooltip:hover .arrow::after {
            content: "";
            position: absolute;
            bottom: -5px;
            left: 20%;
            margin-left: -5px;
            border-width: 5px;
            border-style: solid;
            border-color: transparent transparent black transparent;
          }

        </style>
      </head>
      <body>
        <div class="tooltip" data-tooltip="一段提示">
          Hover me
          <span class="arrow"></span>
        </div>
      </body>
    </html>


### 语法中的实验属性（目前所有浏览器都不支持）

在新的语法中支持各种类型的CSS属性，具体支持的可查看[MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/attr()#Specifications "MDN文档")，举个例子,假如需要设置一个margin-top,正常是需要去找到类名然后设置，稍微图省事一点可能会集中书写css类名，然后全局引入再调用.这种写法一定程度上能方便一点，但是不够个性化，假如我要设置成上边距15px,又得重新加一个类名，还是很麻烦。

    <div class="mt10"></div>

    //style
    .mt10{
    	margin-top: 10px;
    }


但是如果实验属性支持的话，可以写成这样。

    <div mt="10px"></div>

    //style

    [mt] {
    	margin-top: attr(mt,0);
    }

这种写法就很类似组件开发，不需要指定特定大小的px值，在HTML元素上直接能指定任意大小的PX值，而且基于CSS，没有JS的参与，会更加轻巧。但是，很遗憾的是目前所有浏览器都不支持，估计很长一段时间内也是不支持的，这里做一下了解，提供一种组件开发的思路。幸运的是，在找资料的过程发现[张鑫旭大佬](https://www.zhangxinxu.com/ "张鑫旭大佬")已经探索过这种可能性，然后对这种特性做了 Polyfill，[查看Polyfill](https://www.zhangxinxu.com/wordpress/2020/10/css-attr-polyfill/?shrink=1 "查看Polyfill")。

##### Polyfill attr()实验属性原理

利用CSS自定义属性传递attr的属性值

          .test-attr {
            --mbNum: attr(mb px);
            margin-bottom: var(--mbNum);
            --mlNum: attr(ml px);
            margin-left: var(--mlNum);
          }

然后获取所有包含attr()函数的自定义的属性名

        // 获取页面中所有的CSS自定义属性
        var isSameDomain = function (styleSheet) {
            if (!styleSheet.href) {
                return true;
            }

            return styleSheet.href.indexOf(window.location.origin) === 0;
        };

        var isStyleRule = function (rule) {
            return rule.type === 1;
        };

        var arrCSSCustomProps = (function () {
            return [].slice.call(document.styleSheets).filter(isSameDomain).reduce(function (finalArr, sheet) {
                return finalArr.concat([].slice.call(sheet.cssRules).filter(isStyleRule).reduce(function (propValArr, rule) {
                    var props = [].slice.call(rule.style).map(function (propName) {
                        return [
                            propName.trim(),
                            rule.style.getPropertyValue(propName).trim()
                        ];
                    }).filter(function ([propName]) {
                        return propName.indexOf('--') === 0;
                    });

                    return [].concat(propValArr, props);
                }, []));
            }, []);
        })();
打印下 arrCSSCustomProps ，得到

![DUnnnU.png](https://s3.ax1x.com/2020/11/25/DUnnnU.png)

最后一步是遍历Dom,如果设置了对应的自定义属性，就将通过attr定义属性值，转换成css能够解析的自定义属性值 var


        // attr()语法转换成目前CSS变量可识别的语法
        var funAttrVar2NormalVar = function (objParseAttr, valueAttr) {
            // attr()语法 attr( <attr-name> <type-or-unit>? [, <attr-fallback> ]? )
            // valueVar示意：attr(bgcolor color, deeppink)
            // valueAttr示意： 'deepskyblue'或者null

            var attrName = objParseAttr.attrName;
            var typeOrUnit = objParseAttr.typeOrUnit;

            // typeOrUnit值包括：
            // string | color | url | integer | number | length | angle | time | frequency | cap | ch | em | ex | ic | lh | rlh | rem | vb | vi | vw | vh | vmin | vmax | mm | Q | cm | in | pt | pc | px | deg | grad | rad | turn | ms | s | Hz | kHz | %

            var arrUnits = ['ch', 'em', 'ex', 'ic', 'lh', 'rlh', 'rem', 'vb', 'vi', 'vw', 'vh', 'vmin', 'vmax', 'mm', 'cm', 'in', 'pt', 'pc', 'px', 'deg', 'grad', 'rad', 'turn', 'ms', 's', 'Hz', 'kHz', '%'];

            var valueVarNormal = valueAttr;
            // 如果是string类型
            switch (typeOrUnit) {
                case 'string': {
                    valueVarNormal = '"' + valueAttr + '"';
                    break;
                }
                case 'url': {
                    if (/^url\(/i.test(valueAttr) == false) {
                        valueVarNormal = 'url(' + valueAttr + ')';
                    }
                    break;
                }
            }

            // 数值变单位的处理
            if (arrUnits.includes(typeOrUnit) && valueAttr.indexOf(typeOrUnit) == -1 && parseFloat(valueAttr) == valueAttr) {
                valueVarNormal = parseFloat(valueAttr) + typeOrUnit;
            }

            return valueVarNormal;
        };

			var valueVarNormal = funAttrVar2NormalVar(objParseAttr, strHtmlAttr);

            console.log(valueVarNormal); //100px
            // 设置
            node.style.setProperty(cssProp, valueVarNormal);  // margin-bottom : 100px

objParseAttr就是 attr(mb px)解析后的对象，valueAttr就是 自定义属性的值，也就是例子中的 100

![DUucGR.png](https://s3.ax1x.com/2020/11/25/DUucGR.png)

详细的Polyfill代码请 [戳这](https://www.zhangxinxu.com/study/202008/css-attr.js "戳这")

### 最后

attr()加上做兼容后的实验功能很强大，非常的灵活，后面我打算整合一些常用的需要这种写法的属性，封装成npm包，方便日常应用的开发
