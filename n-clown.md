# 前端的多列布局解决方案

前端页面的布局经历了固定布局，表格布局，浮动、定位布局, 弹性布局，网格布局的历史演进，各种布局方式在web发展的不同阶段扮演你了不同的角色。

多列布局的解决方案我个人总结至少有5种之多，有的兼容性不错但代码结构复杂，有的结构简单但兼容性不佳，但根据具体的使用环境和要求，总有一种解决方案是适合你的。

再介绍这5种解决方案之前，我想有必要废话几句介绍一下布局的演进史。

> 固定布局

这是一种很早的布局方案,就是固定容器的宽度和容器内各个模块的宽度.这种布局方式在现在的开发中仍然占据一席之地,尤其是在pc端,特别是企业站的开发中.
这个就不举例了,任意打开一个企业站你都会发现它的影子.所以,不再赘述.

> 表格布局

这是一种较早期的web布局方案，但现在已经废弃，不再使用。它其实就是使用我们常用的表格代码如下：
    
    <table>
        <tr>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
    </table>
所以，这中布局方案不在赘述

> 浮动、定位布局

浮动：是基于css现代web设计的重要功能之一，是实现多列布局的基础
属性值： none（不浮动）， left（向左）， right（向右）
说到浮动，当我们使用浮动，就要清除浮动，如何清除浮动不是本文所要讲述的内容，百度输入关键词‘清除浮动’,说解决方案的文章有很多。

定位：这个css属性是建立元素布局所用的定位机制
属性值：static， absolute， fixed， relative， inherit

使用并且分析过bootstra等p框架的同学都会有深刻的印象

总结： 以上三种方案的局限最突出点就是自适应性差，为了达到某种布局效果免不了要使用hack，这个使用不恰当会导致某种无法言语的bug。

> 弹性布局

弹性布局css3引入的强大的布局方案，因为是css3引入的所以存在兼容性问题，这个兼容问题主要存在pc端，由于移动端移动设备更新换代快，所以在开发中可以不用考虑这方面的问题，至少在我所做的这种移动项目中没有问题。各位看官根据自己项目的情况来定方案。

使用并分析过mui框架和aui等移动框架的同学会有更直观的感受

> 网格布局

网格布局 已经被w3c纳入到了css3的一个布局模块中, 被称为css grid module，这个布局解决方案是未来的web页面布局的一个趋势，但是他的各浏览器兼容不佳，我目前实验的支持的只有chrome，firefox等有限的几款浏览器,像IE浏览器和微软现在新推出的斯巴达浏览器根本就不支持，国产的浏览器360，qq也不支持，这种布局解决方案，现在还停留在实验阶段。不过看好它前景的不在少数哦！

这个我现在没有找到好的例子，不过我写了一个demo，各位看官可以用chrome浏览器看一下。

### 多列布局解决方案
   
   这几种解决方案是有列间距的，没有列间距的相对简单一点。
   ok，我们下边开始我们的方案解决之旅~
   
   最终这几种解决方案要实现的效果皆如下图所示：
   
![col-1.png](http://lvzhenbang.github.io/article/img/col-1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


   1.固定布局 和 float布局（float：left，兼容性最优）
   
       html代码
       
      <h3> float布局 外包一个box l-list-container overflow:hidden, l-list加宽一个l-list-item margin-right</h3>
      <div class="l-list-container">
          <div class="l-list">
              <div class="l-list-item"></div>
              <div class="l-list-item"></div>
              <div class="l-list-item"></div>
              <div class="l-list-item"></div>
          </div>
      </div>
      
   css代码
   
       .l-list-container {
           width: 1000px; height: auto; margin: 0 auto; overflow: hidden;  border: 1px solid #ff0000;
       }
       
       .l-list {
           width: 1020px; height: auto;  /* margin: 0 auto; */  border: 1px solid #cecece; overflow: hidden;
       }
       
       .l-list-item{
           width: 235px; height: 220px;  margin-right: 20px; float: left; background-color: #cecece;
        }
   
demo地址:[demo中的第一个案例](http://lvzhenbang.github.io/article/colown.html )
   2.固定布局 和 float布局(float：left；css3的辅助的优化方案)（不支持css3的免谈）
    
   就是去掉容器l-list-container, 在样式中加入如下代码：
     
     .l-list-item:last-child {
        margin-right: 0;
     }
     
demo地址:[demo中的第二个案例](http://lvzhenbang.github.io/article/colown.html )
   3.转换为内联元素模块（display：inline-block，兼容性最优）
   
   html代码：
   
    <h3>转换为内联模块 diaplay: inline-block </h3>
    <div class="l-list3">
        <div class="l-list-item3"></div>
        <div class="l-list-item3"></div>
        <div class="l-list-item3"></div>
        <div class="l-list-item3"></div>
    </div>
    
   css代码：
   
    .l-list3 {
        width: 1000px;  height: auto;  margin: 0 auto;  font-size: 0; border: 1px solid #cecece;overflow: hidden;
    }
    
    .l-list-item3 {
        display: inline-block; width: 235px;  height: 220px; margin-right: 20px;  background-color: #cecece;
    }
    
demo地址:[demo中的第三个案例](http://lvzhenbang.github.io/article/colown.html )
   4.table布局（display：table，兼容性优）
   
   补充说明：这个是我想的没有在开发中大面积使用，这个方案我感觉是最佳的，各个浏览器测试下来没有发现问题，各位使用中发现问题，记得@我
   
   就是在父容器中使用用display：table，在子容器中使用display：table-cell

![col-1.png](http://lvzhenbang.github.io/article/img/col-2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  
demo地址:[demo中的第四个案例](http://lvzhenbang.github.io/article/colown.html )
   
   5.弹性布局（display：flex，兼容性分为两部分，pc端不佳，手机端优秀）
   
   它是在父容器中使用 display: flex; justify-content: space-between;
    
demo地址:[demo中的第五个案例](http://lvzhenbang.github.io/article/colown.html )
   
   6.网格布局（display：grid，兼容性不佳）
    
    .l-list5 {
        width: 1000px; height: auto; display: grid; grid-column-gap: 20px; grid-template-columns: 235px 235px 235px 235px; grid-template-rows: 220px; margin: 0 auto; border: 1px solid #cecece;
    }
    
demo地址:[demo中的第六个案例](http://lvzhenbang.github.io/article/colown.html )