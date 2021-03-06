# JCT
  ==> javascript : css3 transform/transition

一款方便js操作css3 transform、transition的工具.
正在开发。
框架整体结构类似jquery，能够快速上手。全局就一个对象JCT，也可以用$代替。

***

## API

$等同于JCT。当页面中引用jquery时，使用JCT。

### $(selector)

##### 描述：
通过设置css选择器作为参数，可以得到css选择器选中的标签集合。
##### 参数：
selector => string , 必选
##### 返回：
Object : JCT实例对象
传入一个css选择器字符串，得到一个JCT对象，通过JCT对象能够对dom进行各种操作。

##### 示例1-1
    var $dom = $("body");      
    alert($dom.length); // 1       
    alert($dom[0].tagName); // "BODY"

### .rotateX( deg )

##### 描述：
获取第一个标签的rotateX值，或设置所有标签的rotateX值
##### 参数：
deg => string || number , 可选
  要旋转的角度值。可以为数字、字符串，如：`.rotateX(30)` , `.rotateX("30")` , `.rotateX("30deg")`
  大部分关于transform的方法都可以如此使用参数，包含：`translate` , `translateX` , `translateY` , `translateZ` , `translate3d` , `scale` , `scaleX` , `scaleY` , `skew` , `skewX` , `skewY` , `rotateY` , `rotateZ` , `rotate` 。不再重复

##### 返回：
1. 没有传入参数：

    获取第一个标签的rotateX值，类型为字符串，示例："30deg"。

    __注1：__所有得到`transform`值的方法，包括`scale系列`,`rotate系列`,`translate系列`,`skew系列`，都是从标签的内联样式（即div.style.cssText）中分析而得，有可能不是标签最终呈现的样子。因为通过getComputedStyle获取到的是最终的矩阵，无法保证准确的倒推出设置的rotate等属性。采用style.cssText分析属于折中，通过JCT统一的接口调用，基本可以避免字符串分析的缺点。
    
    __注2：__这里还存在一个悖论问题，css是允许开发者在同一条transform中设置多条rotate等同类属性的，并且是类似叠加的关系，譬如可以写：`transform:rotate(30deg) translate(0,0) rotate(50deg)`，最后显示的实际上是30+50=80deg，但是这种写法存在诸多问题，譬如开发者意图不可测，前端开发师这么写到底是效果必须要求这么写，还是手误写错了？接下来开发者假设要得到rotate值，他期望的是30？50？80？不得而知。因此这里建立一条规则，负责大部分开发环境下的transform设置和获取需求，详见注3
    
    __注3：_a.不要为相同的属性前后设置多个（如注2,）；b.当获取某个属性的值时，JCT会选择获取第一个出现的值，譬如`transform:rotate(30deg) translate(0,0) rotate(50deg)`使用JCT获取，会得到`"30deg"`；c.当设置某个属性的值时，JCT会先检查当前是否有相同属性设置，如果有则替换为新的值，如果没有则会查找相应的相关属性或合写属性，进行替换，如果前两者都没有，JCT会在最后字符串最后插入设置。譬如style.cssText中设置为`transform:translateX(30px)`，使用JCT设置translateX=40，JCT首先找到了已经设置有translateX，则会将其替换为：`transform:translateX(40px)`，假设使用JCT设置的是translate=50，在字符串中没有显示的设置translate，但我们都知道css中translate有两个参数，只写一个意味着设置translateX，JCT就会将字符串修改为：`transform:translateX(50px)`，最后一种情况：假设设置rotate=40deg，JCT发现相同属性和相关属性都没有显示的出现，就会在最后插入，新的字符串为：`transform:translateX(30px) rotate(40deg)`。
    
2. 传入正确的参数：
    返回this对象，可以链式操作，示例：
    $('body').rotateX("30deg").rotateX("40deg");
