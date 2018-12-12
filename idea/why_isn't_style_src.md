# 为什么没有 `<style src="">`

最近在Twitter上看到这样一个有趣的话题，为什么现在用的是 `<link rel="stylesheet" href="">` 引入外部共享的样式文件，而不使用 `<style src="">` ？详情请点击[https://twitter.com/csswizardry/status/1068154542192242688](https://twitter.com/csswizardry/status/1068154542192242688)

我们都知道，内联的JavaScript是在页面的 `<script> ... </script>` 标签内添加，内联的样式是在 `<style> ... </style>` 标签内添加；而外部共享的JavaScript文件，则是通过 `<script src="..."></script>` 来引入，共享的样式文件不是通过 `<style src=""></style>` 的形式引入，而是通过 `<link rel="stylesheet" href="...">` 形式引入，这是为什么呢？

既然JavaScript代码片段可以通过 `<script src="..."></script>` 这样的共享方式引入，那么样式文件就不可以通过类似的形式引入？w3c为什么要这样设计，是不是因为历史的局限性，或者是... 

好了，不扯淡了。w3c这样设计是因为JavaScript代码片段的引入和样式文件有本质的不同。

一个重要原因是，他们压根没有考虑过要实现 `<style src=""></style>` 来引入当前文档外部共享的样式文件，他们的想法就是使用 `<link rel="..." href="...">` 来引入当前文档外部的资源，如：父文档，翻译，或者层叠样式表等。`<link rel="" href="" meidia="">` 元素规定了当前文档和外部资源之间的关系，它常用来引入外部的样式表，我们可以通过`rel`属性设置为`stylesheet`来使用。通过`<link>`的设计原理，我们知道它应该是可以引入scripts的，但是没有定义与JavaScript相关的`rel`属性值，奇怪的是在HTML 3.2 中，也只是定义了`<script>`而没有定义属性`src` ，到了后来不知怎么的就添加这个属性，现在我们通过`<script src="..."></script>` 来引入外部共享的JavaScript文件，至于具体原因我想下面的内容可能会回答这个问题。

注：其中`rel` 属性它用来表示引入的外部资源和当前文档的关系，`href` 属性表示引入的外部资源的url，`media` 属性就是媒体查询，我们可以定义它的值为支持媒体查询的css代码片段，也可以是像`print, screen` 这样的名词，当然还有其它一些优秀的特性，如：preload，prefetch等。详情可参考[MDN link](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/link)。

[Bruce Lawson](https://twitter.com/brucel)的解释可能会给你打开一些思路，他认为 `<foo src="">` 形式的元素，如：`<img src="">` 和 `<script src=""></script>` 它引入的外部文件是被`插入`当文档中的，而样式表不是，它是关联当前这个文档，它作用于不止一个页面，而`<style><style>`将样式限制在了一个文档中。

文档到这里基本上来说已经完了，Twitter中广为大家接受的就是上文中的两个观点。但是有的人给我反馈比较疑惑。特解释如下：

个人觉得应该是w3c设计了两个方案。第一种是`<link>`，它可以获取所有的跟当前document文档相关的外部文件，包括javascript脚本，这个上文中已经说了；另一种，就是`<foo src="">`，它是指待了一类元素如img，script,上文中说明了它是指代了一种外部文件与当前文档关系。

如果觉得还有问题，我建议还是[twitter](https://twitter.com/csswizardry/status/1068154542192242688)一下这个话题。