## 如何学正则（告别复制粘贴）

知识学到自己手里的才是自己的，如果复制，粘贴别人的对自己帮助并不大，它只能帮自己解决一时的问题（有时还要花费自己大量的时间来查找），而不能从根本上解决问题。

就好像前段时间我的的大学同学问了我一个正则问题，如何验证用户输入的密码必须包含字符、数字、特殊符号，他说在百度上找了大量的正则示例都不能解决问题，我就问然后呢，他给我说我问你，我当时就无语了，我当时因为正在做项目，一时没想出来，我跟他说你没试着换个思路，暂时没有找到用一个正则解决这个问题的你就不会试着分别对字符，数字，特殊字符单独判断，然后进行与运算不就行了，再者说了从用户体验上用一个正则判断后给出一个结果，用户体验也不好，应该针对用户输入不同的情况给出不同的提示信息，如果密码的组成没有数字，就提示没有数字，如果没有字符就提示没有字符...... , 如果以此类推觉得判断过多，你可以再简化处理，如只有密码组成包含两种，就提示缺少的一种，如果密码组成只包含一种，就提示密码应该有字符、数字、特殊符号组成。

这无形中给我上了生动的一课，正则不仅其他人忽视了，我也忽视了，有所欠缺。所以尽管最近在努力拿下设计模式这个高地，还是决定抽出一部分时间梳理一下自己的正则知识的掌握。

### 具有特殊含义的字符

下面只列出常用的字符，以及我个人对它们的分类。

#### 分组和集合

* `()` : 括号内的表达式表示一个分组
* `[]` : 方括号内的表达式表示一个集合

#### 运算符

* `^` : 如果出现在集合（`[]`）中表示取反，否则是是定位符，从字符串的前边界开始匹配
* `|` ：它表示或的意思，就是起到或运算的作用
* `?:` : 它的作用是放在第一个选项前来消除相关匹配会被缓存这种副作用

#### 定位符

* `^` : 上面已经说了它定义正则运算的前边界
* `$` : 它定义了正则运算的后边界
* `\b` : 匹配一个字符的边界（也即是字符和空白字符的分界）

#### 字符类（代表一类字符）

* `\d` : 代表数字，而 `\D` ，非数字
* `\w` : 代表单词，而 `\W` ，非单词
* `\s` : 代表空白符，而 `\D` ，非空白字符
* `.` : 任意字符

#### 限定符 

它是用来指定匹配结果的长度或次数。

匹配该符号前面的表达式

* `+` : 一次或更多次
* `*` ：零次或多次
* `？` : 零次或一次

* `{}` : 匹配次数与花括号内的值有关。

	如果 `{n}` ，就是匹配n次；
	如果 `{n,}` ，就是匹配至少n次；
	如果 `{n,m}` ，就是匹配n到m之间的任意次数。

### 如何玩转正则

正则用在字符串的处理上，可以减少我们的js代码的书写量，优化我们的代码，同时对于我们学习别人源码中复杂的正则已有帮助。

下面是一张来自知乎关于[你是如何学会正则表达式的？](https://www.zhihu.com/question/48219401/answer/126612931)问题的一张图，掌握这张图的正则，大概你就能解决你所面临的大部分问题。

[regular](https://pic2.zhimg.com/50/v2-f87997d3fede150d3edc3b173fef1493_hd.jpg)

	/^\s*[A-Za-z_$][\w$]*(?:\.[A-Za-z_$][\w$]*|\['*?'\]|\[".*?"\]|\[\d+\]|\[[A-Za-z_$][\w$]*\])*\s*$/

下面推荐几款可视化的正则编辑器。

[regexper](https://link.zhihu.com/?target=https%3A//regexper.com/) (这是我最早接触到的一款)

[Regulex](https://jex.im/regulex/) (这一款是我现在经常使用的)

[RegExr](https://regexr.com/) (这一款功能很强大，对于学习正则很有帮助，如果学习正则的话强烈推荐)

### js如何使用

正则是一个很强大的字符串查询和替换的方法。

以前我们有时侯总是在想将字符串转换为数字数组，利用数组的方法来处理字符，但是要知道字符串就是我们在生活和工作中常见的形式，数字、数组、Boolean类型的相对较少，尤其是最近在做微信开发时发现正则很重要，我同学的例子，只是给了我一个深入学习和研究的动力，这只是我的初步总结，以后有必要的话还会加强。

在JavaScript中我们使用 `RegExp` 来创建一个对象来实现正则表达式。

#### 基本定义

一个正则有两部分组成：正则主体和修饰符。

形式如下：

	regExp = new RegExp('pattern', 'flag');

	// 或者
	regExp = /pattern/gmi

正则的修饰符一共有5种，分别为:

* `g` : 所有匹配的情况，如果没有它，只一种匹配情况
* `i` : 忽略字符的大小写
* `m` : 支持多行
* `u` : 支持 Unicode
* `y` : 严格模式（返回指定位置后的匹配结果）

#### 正则对象的一些方法

##### regexp.test(str)

`test` 方法返回值为true/false

	let str = "Hello world!";
	let regexp = /hello/i;
	console.log(regexp.test(str));

##### regexp.exec(str)

由于这个方法不好用，所以很少有人使用。

	let str = "Hello world!";
	let regexp = /l(o)/ig; // 如果用exec返回所有的的匹配结果需要加上 ‘g’ 修饰符
	let matchOne = regexp.exec(str);
	console.log(matchOne[0]); // lo
	console.log(matchOne[1]); // o
	console.log(matchOne.index); // 3
	console.log(matchOne.input); // Hello world!
	console.log(matchOne.lastIndex); // 5

如果没有匹配返回null

### js中String可以使用正则的方法

在String的方法中使用正则，可以轻松的解决我们日常开发中的问题。

#### str.search()

	如果有匹配结果，返回第一个匹配结果的首字符位置；否则，返回 `-1`。

	let str = "Hello world!";
	regexp = /o/i;
	str.search(regexp); // 4

注；`search` 只能返回第一次匹配的结果，而不能返回其他匹配结果

#### str.match(str|reg) 

	let str = "Hello world!";
	regexp = /o/i;
	let result = str.match(regexp);

	console.log(result[0]); // o
	console.log(result.index); // 4
	console.log(result.input); // Hello world!

我们发现 `str.match()` 的用法和 `regexp.exec()` 返回的结果很一样，其实match的底层实现就是 `regexp.exec()`，使用也一样，注意修饰符 `g` 。

#### str.split(reg|substr, limit)

将给定的字符串按单词为单位进行分割，返回一个由单词组成的数组。

	let str = 'Hello world, my   name  is lzb.'
	let regexp = /\s+/i;
	str.split(regexp); // ["Hello", "world,", "my", "name", "is", "lzb."]
	str.split(regexp, 3) //  ["Hello", "world,", "my"]

在这个字符串的方法中第二参数限制返回结果数组的长度。

在返回的结果中，我们发现有的单词带有特殊符号，下面一个字符串方法将实现清除特殊符号。

#### str.replace(str|reg, str|func)

如果要实现上面示例的清除字符中特殊符号的目标，我们可以使用 `str.replace()` ，效果如下：

	let str = 'Hello world, my   name  is lzb.'
	let regexp = /[.,\/#!$%\^&\*;:{}=\-_`~()]/g;
	str.replace(regexp, ''); // "Hello world my   name  is lzb"

	或者

	let str = 'Hello world, my   name  is lzb.'
	let regexp = /[^\w\s|-]/g;
	str.replace(regexp, ''); // "Hello world my   name  is lzb"

然后，接着使用上面的 `str.split()` 方法即可，或者有同学可能想到如下方法：

	let str = 'Hello world, my   name  is u-lzb.'
	let regexp = /[^\w]+/g;
	str.split(regexp); // ["Hello", "world", "my", "name", "is", "u", "lzb", ""]

这种方法不建议使用，问题很明显，这里就不多说了。

我们发现上面实现清除字符串中特殊符号的方法有两种，这两种方法谈不上孰优孰劣，它们各有优势。如果在我们把字符串中 `work_up`, `call&apply`，`::arg`，`a=b` … 都当作特殊的单词，我们就需要第一种方法；如果我们就是要中规中矩的单词我们可以使用第二种方法。

如果第二个参数是func，介绍一个例子，字符串中单词的首字母大写：

	let str = 'hello world';
	str.replace(/\b\w+\b/g, (word) => word.substring(0,1).toUpperCase() + word.substring(1) );

字符串还有 `length`，`indexOf`，`concat`，`toLowerCase`，`toUpperCase` 等方法，这里就不一一介绍了。

### 推荐

如果喜欢码题的同学可到[https://www.hackerrank.com/domains/regex/re-introduction](https://www.hackerrank.com/domains/regex/re-introduction)这个网站去。

[github.com/lvzhenbang/article]()