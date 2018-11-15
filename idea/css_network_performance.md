# CSS和网络性能

尽管十多年来这个网站被称为CSS魔法。但在一段时间内，这个网站上并没有太多CSS相关的内容。下面就让我结合我最喜欢的两个主题来谈论这个话题：CSS和性能。

CSS对于页面的渲染至关重要。浏览器在找到、下载和解析所有的CSS之前都不会开始渲染，所以我们必须尽快将它放到用户的设备上。关键路径上的任何延迟都会影响我们的 `初始渲染`，并让用户看到一个空白屏幕。

## 这说明了什么？

总的来说，这就是CSS对于性能如此关键的原因:

* 浏览器在构建了渲染树之后才能渲染页面；
* 渲染树是DOM和CSSOM的组合结果；
* DOM是在HTML加上任何需要对其进行操作的JavaScript（它会阻塞渲染）；
* CSSOM是所有应用于DOM的CSS规则；
* 使用async和defer属性使JavaScript变为非阻塞很容易；
* 使CSS异步要困难得多；
* 因此，需要记住的一条经验规则：页面渲染的速度只能与最慢的样式表渲染的速度一样快。

考虑到这一点，我们需要尽快构造DOM和CSSOM。在很大程度上，构建DOM相对比较快：你的第一个HTML响应是DOM。然而，由于CSS几乎总是HTML的子资源，因此构建CSSOM通常要花费较长的时间。

在这篇文章中，我想说一说CSS是如何在网络上成为一个实质性的瓶颈(它本身和其他资源)，以及我们如何减轻它，从而缩短关键路径和减少我们开始渲染的时间。

## 找到内联CSS的边界

如果你有能力，一个最有效来减少开始渲染时间的方法是使用关键的CSS模式：识别所需的所有样式，然后开始渲染所需的样式，内联在文档的 `<style>` 标记中，然后通过关键路径异步加载剩余的样式。

虽然这种策略很有效，但并不简单。高度动态的站点很难从中提取样式；这个过程需要自动化；我们必须对需要内联的部分做出假设；事实上很难捕捉到边缘情况；工具仍处于相对起步阶段。如果你使用的是大型或旧版本的代码库，那么事情就会变得更加困难……

## 依据媒体类型分割你的CSS

要找到内联CSS的边界被证明是相当棘手的，它也确实是这样。另一个选择是将我们的主CSS文件依据媒体查询分割成单独的文件。这样就将问题交给了浏览器：

* 下载当前上下文(medium、屏幕大小、分辨率、方向等)所需的CSS，优先级非常高，阻塞关键路径；
* 下载当前上下文不需要的CSS，优先级非常低，完全脱离关键路径。

基本上，任何不需要呈现当前视图的CSS都会被浏览器使用 `lazyload`。

```
<link rel="stylesheet" href="all.css" />
```

如果我们将所有的CSS都打包到一个文件中，那么网络如何对待它：

![注意，单个CSS文件的优先级最高。](https://res.cloudinary.com/csswizardry/image/fetch/f_auto,q_auto/https://csswizardry.com//wp-content/uploads/2018/11/screenshot-css-media-all.png)

如果我们按照媒体查询把那个单一的，包含所有渲染阻塞的文件进行分割:

```
<link rel="stylesheet" href="all.css" media="all" />
<link rel="stylesheet" href="small.css" media="(min-width: 20em)" />
<link rel="stylesheet" href="medium.css" media="(min-width: 64em)" />
<link rel="stylesheet" href="large.css" media="(min-width: 90em)" />
<link rel="stylesheet" href="extra-large.css" media="(min-width: 120em)" />
<link rel="stylesheet" href="print.css" media="print" />
```

然后我们看到网络以不同的方式对待这些文件:

![不需要在当前上下文渲染的CSS文件的优先级最低。](https://res.cloudinary.com/csswizardry/image/fetch/f_auto,q_auto/https://csswizardry.com//wp-content/uploads/2018/11/screenshot-css-media-split.png)

浏览器仍然会下载所有CSS文件，但它只会阻塞对满足当前上下文所需的文件的渲染。

## 不要在css文件中使用 `@import` 语法

接下来，我们可以做的对开始渲染有帮助的事情是非常非常简单的。不要再css文件中使用 `@import`。

`@import`由于它的工作原理，速度很慢。这对开始渲染性能非常不好。这是因为我们会在关键路径上创造更多的循环调用：

1. 下载HTML；
2. HTML 需要 CSS；
  * (这是我们想要构建渲染树的地方，但是；)
3. CSS 需要更多的 CSS；
4. 生成渲染Tree。

参考下面的HTML：

```
<link rel="stylesheet" href="all.css" />
```

… `all.css` 的内容如下：

```
@import url(imported.css);
```

… 这样会形成如下这样一个瀑布流：

![在关键路径上明显缺乏并行化。](https://res.cloudinary.com/csswizardry/image/fetch/f_auto,q_auto/https://csswizardry.com//wp-content/uploads/2018/11/screenshot-import-before.png)

通过简单地将其分化为两个 `<link rel="stylesheet" />` 和不使用 `@imports`：

```
<link rel="stylesheet" href="all.css" />
<link rel="stylesheet" href="imported.css" />
```

…我们得到了一个更健壮的瀑布流：

![关键路径的CSS开始并行化。](https://res.cloudinary.com/csswizardry/image/fetch/f_auto,q_auto/https://csswizardry.com//wp-content/uploads/2018/11/screenshot-import-after.png)

注意：我想简要地讨论一个不寻常的边界案例。在一些的情况下，你没有访问包含 `@import` 的CSS文件(也就是说你没有删除它)，你可以安全地将其保留在CSS中。你也可以在HTML中使用 `<link rel="stylesheet" />` 对其进行引用，浏览器将启动从HTML中导入的CSS下载，并跳过 `@import`，你不会得到任何双重下载。

## 小心在HTML中的 `@import` 

这部分是奇怪的，非常奇怪。WebKit因为一个bug坏了；火狐和IE/Edge似乎都坏了。可参考[相关bug](https://bugs.chromium.org/p/chromium/issues/detail?id=903785)。

要完全理解这一部分。首先，我们需要了解浏览器的预加载扫描程序：所有浏览器都实现一个二级的、惰性的解析器，通常称为预加载扫描程序。浏览器的主解析器负责构造DOM、CSSSOM、运行JavaScript等，并为文档的不同部分不断地停止和启动它。预加载扫描器可以安全地跳转到主解析器前面，并扫描HTML的其余部分，以发现对其他子资源(如CSS文件、JS、图像)的引用。一旦它们被发现，预加载扫描器就开始下载它们，以便主要解析器在以后提取它们并执行或应用它们。预加载扫描器的引入，提高了19%的网页性能，开发人员都不需要动一下手指。这对用户来说是个好消息!

作为开发人员，我们需要注意的一件事，是在不经意间对预加载扫描器隐藏了一些可能发生的事情。稍后将详细介绍。

本节讨论WebKit和Blink的预加载扫描器中的bug，以及Firefox和IE/Edge的预加载扫描器的低效。

## Firefox 和 IE/Edge：需要放 @import 在 HTML中的 JS 和 CSS 之前

在 Firefox 和 IE/Edge 中，预加载扫描程序似乎没有获取在 `<script src="">` 或 `<link rel="stylesheet"/>` 之后定义的任何 `@import`。

这意味着这个HTML：

```
<script src="app.js"></script>

<style>
  @import url(app.css);
</style>

```

…将产生下面这样的瀑布流：

![由于无效的预加载扫描程序导致Firefox失去并行化（注意：在IE/Edge中出现相同的瀑布流。）](https://res.cloudinary.com/csswizardry/image/fetch/f_auto,q_auto/https://csswizardry.com//wp-content/uploads/2018/11/screenshot-ff-import-blocked-by-js.png)

在这里我们可以看到 `@import` 的样式表在JavaScript文件完成下载之前不会开始下载。

这个问题也不是JavaScript独有的。以下HTML有相同的现象:

```
<link rel="stylesheet" href="style.css" />

<style>
  @import url(app.css);
</style>
```

由于没有有效的预加载扫描器，Firefox失去了并行性(注意，IE/Edge中也出现了相同的瀑布)。

这个问题的直接解决方案是交换 `<script>` 和 `<link rel="stylesheet" />` 的位置。然而，当我们改变依赖顺序时，这可能会破坏一些东西(如：级联)。

最好的解决方案是不要使用 `@import`，而是使用 `<link rel="stylesheet" />` ：

<link rel="stylesheet" href="style.css" />
<link rel="stylesheet" href="app.css" />

那么，就会出现如下的瀑布流：

![两个<link rel="stylesheet"/> 为我们提供了并行化。（注意：IE/Edge中出现相同的瀑布流。）](https://res.cloudinary.com/csswizardry/image/fetch/f_auto,q_auto/https://csswizardry.com//wp-content/uploads/2018/11/screenshot-ff-import-unblocked-by-css.png)

## Blink 和 WebKit： 给HTML中的 @import 的 URLs添加引号

如果 `@import` 的 URLs 没有 `"` 标记，那么WebKit 和 Blink 的表现行为将和Firefox 和 IE/Edge一致。这意味着 WebKit 和 Blink 的预加载扫描器有一个bug。

简单地将 `@import` 的URL用引号括起来就可以解决问题，你不需要重新排序任何东西。不过，与前面一样，我的建议是完全不要使用 `@import`，而是选择 `<link rel="stylesheet" />`。

之前：

```
<link rel="stylesheet" href="style.css" />

<style>
  @import url(app.css);
</style>
```

![我们的 `@import` 的URL中缺少引号会破坏Chrome的预加载扫描器（注意：在Opera和Safari中会出现相同的瀑布流。）](https://res.cloudinary.com/csswizardry/image/fetch/f_auto,q_auto/https://csswizardry.com//wp-content/uploads/2018/11/screenshot-chrome-import-blocked-by-css.png)


之后：

```
<link rel="stylesheet" href="style.css" />

<style>
  @import url("app.css");
</style>
```

![在我们的 `@import` 的URL中添加引号，可以修复Chrome的预加载扫描器。（注意：在Opera和Safari中也会出现相同的瀑布。）](https://res.cloudinary.com/csswizardry/image/fetch/f_auto,q_auto/https://csswizardry.com//wp-content/uploads/2018/11/screenshot-chrome-import-unblocked-by-css.png)


这肯定是WebKit/Blink中的一个错误。缺少引号不应该隐藏来自预加载扫描器的@imported样式表。

详情可参考[Chromium的修复](https://chromium-review.googlesource.com/c/chromium/src/+/1331042)。

## 不要再异步代码片段前放置 `<link rel="stylesheet" />`

上一部分讨论了CSS如何被其他资源减慢速度(必须承认，这是由于一些怪异模式引起的)，这一部分将讨论CSS如何无意中延迟后续资源的下载，主要是JavaScript插入了一个异步加载片段，如下所示:

```
<script>
  var script = document.createElement('script');
  script.src = "analytics.js";
  document.getElementsByTagName('head')[0].appendChild(script);
</script>
```

在所有浏览器中都存在一种目眩神迷的行为，这种行为是有意而为之的，但开发人员很少知道它。有的对性能的巨大影响：

如果当前有正在运行的CSS，浏览器将不会执行 `<script>` ：

```
<link rel="stylesheet" href="slow-loading-stylesheet.css" />
<script>
  console.log("I will not run until slow-loading-stylesheet.css is downloaded.");
</script>
```

这是设计是故意为之的。当前正在下载任何CSS时，HTML中的任何同步的 `<script>` 都不会执行。 这是一个简单的防御策略，用于解决脚本询问页面样式的情况。如果脚本在解析CSS之前询问页面的颜色，那么javascript给我们的答案可能更好或更糟。为了缓解这种情况，浏览器在构造CSSOM之前不会执行 `<script>` 。

这样做的结果是CSS的任何延迟下载，都会对你的异步片段产生连锁反应。用一个例子可以很好地说明这一点。

如果我们删除异步片段之前的 `<link rel="stylesheet" />`，它将不运行直到我们的css文件下载并解析。

```
<link rel="stylesheet" href="app.css" />

<script>
  var script = document.createElement('script');
  script.src = "analytics.js";
  document.getElementsByTagName('head')[0].appendChild(script);
</script>
```

我们可以清楚地看到，在构建CSSOM之前，JavaScript文件甚至没有开始下载。我们完全失去了任何并行化：

![在异步片段之前使用样式表可以消除我们并行化的机会。](https://res.cloudinary.com/csswizardry/image/fetch/f_auto,q_auto/https://csswizardry.com//wp-content/uploads/2018/11/screenshot-async-js-blocked-b)

有趣的是，预加载扫描器（Preload Scanner）希望提前获得对 `analytics.js` 的引用，但是我们无意中隐藏了它：`analytics.js` 是一个字符串，并且在DOM中存在`<script>`元素之前不会成为可标记的src属性。稍后会详细说明。

第三方供应商提供这样的异步代码片段，从而更安全地加载脚本。开发人员对这些第三方供应商持怀疑态度，并尝试在页面后面放置异步片段，这种现象比较常见。虽然这是出于目中好的目的，我们不想在自己的静态资源之前放置第三方 `<script>` ！通常这可能是净损失。事实上，`谷歌分析` 甚至告诉我们该做什么，他们是对的：

> 将此代码作为首要选择，并复制粘贴到要跟踪的每个网页的中。

所以，我的建议是：

如果你的 `<script>…</script>` 代码块不依赖css，把它们放在你的样式文钱前面。

我们可以这样做：

```
<script>
  var script = document.createElement('script');
  script.src = "analytics.js";
  document.getElementsByTagName('head')[0].appendChild(script);
</script>

<link rel="stylesheet" href="app.css" />
```

![交换样式表和异步代码片段可以重新获得并行性。](https://res.cloudinary.com/csswizardry/image/fetch/f_auto,q_auto/https://csswizardry.com//wp-content/uploads/2018/11/screenshot-async-js-blocked-by-css-fixed.png)

现在你可以看到，我们已经几乎完全恢复了并行化加载，并且页面加载2×更快。

## 把任何非CSSOM查询的JavaScript放在CSS之前；把任何CSSOM查询的JavaScript放在CSS之后

更进一步来讲，除了异步加载片段之外，我们该如何更普遍地加载CSS和JavaScript？为了解决这个问题，我提出了以下问题，并解释从那里开始工作：

如果：

* 在CSSOM构造CSS后定义的同步JS；
* 同步JS块DOM结构…

然后，假设没有相互依赖，那么哪个更快、更好?

* 先脚本后样式；
* 先样式后脚本？

答案：

如果文件之间不依赖于另一个文件，那么你应该将阻塞脚本置于阻塞样式之上，使用CSS延迟JavaScript执行是没有意义的，因为JavaScript实际上并不依赖CSS。

> 预加载扫描器可以保证，即使脚本上的DOM构造被阻塞，CSS仍然可以并行下载。

如果你的一些JavaScript依赖，但有些不依赖于CSS，那么加载同步JavaScript和CSS的最佳方案就是将JavaScript分成两部分，并将其加载到CSS的任何一侧：

```
<!-- This JavaScript executes as soon as it has arrived. -->
<script src="i-need-to-block-dom-but-DONT-need-to-query-cssom.js"></script>

<link rel="stylesheet" href="app.css" />

<!-- This JavaScript executes as soon as the CSSOM is built. -->
<script src="i-need-to-block-dom-but-DO-need-to-query-cssom.js"></script>
```

使用这种加载模式，我们可以按最佳顺序进行下载和执行。我为下面的截图中的微小细节道歉，但希望你能看到代表JavaScript执行的小粉红色标记。 入口（1）是计划在其他文件到达和/或执行时执行某些JavaScript的HTML; 入口（2）执行它到达的那一刻; 入口（3）是CSS，所以不执行任何JavaScript; 入口（4）实际上不会执行，直到CSS完成。

![CSS如何影响JavaScript执行的点。](https://res.cloudinary.com/csswizardry/image/fetch/f_auto,q_auto/https://csswizardry.com//wp-content/uploads/2018/11/waterfall-js-execution.png)

> 你必须根据自己的特定用例测试此模式：根据你之前的CSS、JavaScript文件与CSS本身之间的文件大小和执行成本是否存在巨大差异，可能会有不同的结果。一定要注意：测试，测试，测试。

## 将 `<link rel="stylesheet" />` 放在 `<body>` 中

这个最终策略是一个相对较新的策略，对感知性能和渐进式渲染有很大好处。它也非常友好。

在HTTP/1.1中，我们将所有样式放到一个文件中是很典型的，如下所示：

```
<html>
<head>
  <link rel="stylesheet" href="app.css" />
</head>

<body>

  <header class="site-header">
    <nav class="site-nav">...</nav>
  </header>

  <main class="content">
    <section class="content-primary">
      <h1>...</h1>
      <div class="date-picker">...</div>
    </section>

    <aside class="content-secondary">
      <div class="ads">...</div>
    </aside>
  </main>

  <footer class="site-footer">
  </footer>

</body>
```

这样做会从三个方面表现出低效率：

1. 任何给定的页面只会使用app.css中的一小部分样式：我们几乎肯定会下载比我们需要的更多的CSS。
2. 我们受限于一种效率低下的缓存策略：例如，对仅在一个页面上使用的日期选择器上所选日期的背景颜色进行更改，那么将需要缓存整个 `app.css` 。
3. 整个app.css阻止渲染：如果当前页面只需要17％的 `app.css`，我们仍然需要等待其他83％才能开始渲染。

在 HTTP/2 中，我们可以开始解决问题（1）和（2）：

```
<html>
<head>

  <link rel="stylesheet" href="core.css" />
  <link rel="stylesheet" href="site-header.css" />
  <link rel="stylesheet" href="site-nav.css" />
  <link rel="stylesheet" href="content.css" />
  <link rel="stylesheet" href="content-primary.css" />
  <link rel="stylesheet" href="date-picker.css" />
  <link rel="stylesheet" href="content-secondary.css" />
  <link rel="stylesheet" href="ads.css" />
  <link rel="stylesheet" href="site-footer.css" />

</head>
<body>
  <header class="site-header">
    <nav class="site-nav">...</nav>
  </header>

  <main class="content">
    <section class="content-primary">
      <h1>...</h1>
      <div class="date-picker">...</div>
    </section>

    <aside class="content-secondary">
      <div class="ads">...</div>
    </aside>
  </main>

  <footer class="site-footer">
  </footer>
</body>
```

现在我们正在解决冗余问题，因为我们能够加载更适合页面的CSS，而不是不加选择地下载所有内容。这减少了关键路径上阻塞CSS的大小。

我们还能够采用经过深思熟虑的缓存策略，只缓存触发需要它的文件，并保持其余部分不受影响。

我们还没有解决的问题是它仍然阻止渲染，我们仍然只有最慢的样式表。这意味着，如果由于某种原因，`page-footer.css` 需要很长时间才能下载，浏览器无法开始渲染 `.page-header`。

但是，由于Chrome最近发生了变化（如：版本69），以及Firefox和IE/Edge中已经存在的行为，`<link rel="stylesheet"/>` 只会阻止后续内容的渲染，而不是整个页面。这意味着我们现在能够像这样构建我们的页面：

```
<html>
<head>
  <link rel="stylesheet" href="core.css" />
</head>
<body>

  <link rel="stylesheet" href="site-header.css" />
  <header class="site-header">
    <link rel="stylesheet" href="site-nav.css" />
    <nav class="site-nav">...</nav>
  </header>

  <link rel="stylesheet" href="content.css" />
  <main class="content">
    <link rel="stylesheet" href="content-primary.css" />
    <section class="content-primary">
      <h1>...</h1>
      <link rel="stylesheet" href="date-picker.css" />
      <div class="date-picker">...</div>
    </section>

    <link rel="stylesheet" href="content-secondary.css" />
    <aside class="content-secondary">
      <link rel="stylesheet" href="ads.css" />
      <div class="ads">...</div>
    </aside>
  </main>

  <link rel="stylesheet" href="site-footer.css" />
  <footer class="site-footer">
  </footer>
</body>
```

这样做的实际意义是，我们现在能够逐步渲染我们的页面，在页面可用时有效地将样式添加到页面中。

在目前不支持这种新行为的浏览器中，我们不会遇到性能下降：我们会回到原来的行为，我们只有最慢的CSS文件。

## 总结

本文中有很多要消化的内容。它最终超出了我的预想。尝试总结加载CSS的最佳网络性能实践：

* Lazyload任何不需要`开始渲染`的CSS：
  * 这可能是绝对路径的CSS；
  * 将你的CSS根据媒体查询来拆分。
* 不用 `@import` ：
  * 在HTML中；
  * 特别是在CSS中；
  * 提防预加载扫描器（Preload Scanner）的奇怪之处。
* 注意同步CSS和JavaScript的顺序：
  * 在CSSOM完成之后，CSS之后定义的JavaScript将无法运行；
  * 如果你的JavaScript不依赖于你的CSS，在CSS之前加载它;
  * 如果它依赖于你的CSS，在CSS之后加载它。
* 在DOM需要时加载CSS：
  * 这将取消阻塞 `开始渲染` ，并允许渐进式渲染。

警告：

我上面列出的所有东西都遵循规范或已知/预期的行为，但是像往常一样，自己测试所有东西。虽然这在理论上是正确的，但在实践中，事情的运作总是不同的。可参考[The Three Types of Performance Testing](https://csswizardry.com/2018/10/three-types-of-performance-testing/)

## 参考资料

* [CSS and Network Performance](https://csswizardry.com/2018/11/css-and-network-performance/)
* [The future of loading CSS](https://jakearchibald.com/2016/link-in-body/)