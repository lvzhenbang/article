
## Browser

### currentURL

返回当前页面的 URL。

用 `window.location.href` 获得当前的 URL.

```js
const currentURL = () => window.location.href;
// currentUrl() -> 'https://google.com'
```

### getURLParameters

返回一个包含当前页面 URL 的参数的对象.

用 `match()` 和一个合适的正则来获得所有的键值对， 用`Array.reduce()` 去映射它们到一个对象内。
用 `location.search` 获得当前 `url` 的参数.

```js
const getURLParameters = url =>
  url.match(/([^?=&]+)(=([^&]*))/g).reduce(
    (a, v) => (a[v.slice(0, v.indexOf('='))] = v.slice(v.indexOf('=') + 1), a), {}
  );
// getURLParameters('http://url.com/page?name=Adam&surname=Smith') -> {name: 'Adam', surname: 'Smith'}
```

### redirect

重定向到一个指定的 URL.

用 `window.location.href` 或 `window.location.replace()` 重定向 `url` 。
如果asLink 为 `true`，则模拟一个 `<a href="https://google.com"></a>` 链接单击，否则为重定向。

```js
const redirect = (url, asLink = true) =>
  asLink ? window.location.href = url : window.location.replace(url);
// redirect('https://google.com')
```

### httpsRedirect

如果它当前在HTTP中，将页面重定向到HTTPS。另外，按后退按钮不会将其返回到HTTP页面，因为它在历史中被替换了。

用 `location.protocol` 去获得当前所使用的协议。如果不是 HTTPS，用 `location.replace()` 将当前页转换为使用 HTTPS 协议。用 `location.href` 去获得链接地址，用 `String.split()` 移除当前 URL的协议。  

```js
const httpsRedirect = () => {
  if(location.protocol !== "https:") location.replace("https://" + location.href.split("//")[1]);
}
```

### detectDeviceType

检测网站是在移动设备，还是在桌面/笔记本上打开。

用一个正则表达式检测 `navigator.userAgent` 属性来判断是移动设备还是桌面/笔记本。

```js
const detectDeviceType = () =>
  /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent)
    ? 'Mobile'
    : 'Desktop';
detectDeviceType(); // "Mobile" or "Desktop"
```

### scrollToTop

平滑滚动到页面顶部。

用 `document.documentElement.scrollTop` 或 `document.body.scrollTop` 得到页面滚动后的top值。
用 `window.requestAnimationFrame()` 去实现平滑滚动.

```js
const scrollToTop = () => {
  const c = document.documentElement.scrollTop || document.body.scrollTop;
  if (c > 0) {
    window.requestAnimationFrame(scrollToTop);
    window.scrollTo(0, c - c / 8);
  }
};
// scrollToTop()
```

### elementIsVisibleInViewport

如果指定的元素出现在可视窗口内，返回 `true`；否则，返回 `false` .

用 `Element.getBoundingClientRect()` 和 `window.inner(Width|Height)` 的值来判断给定元素是否在可视窗口内.
如果忽略第二个参数或者指定为 `true` 将判断元素的部分出现在可是窗口内。

```js
const elementIsVisibleInViewport = (el, partiallyVisible = false) => {
  const { top, left, bottom, right } = el.getBoundingClientRect();
  const { innerHeight, innerWidth } = window;
  return partiallyVisible
    ? ((top > 0 && top < innerHeight) || (bottom > 0 && bottom < innerHeight)) &&
      ((left > 0 && left < innerWidth) || (right > 0 && right < innerWidth))
    : top >= 0 && left >= 0 && bottom <= innerHeight && right <= innerWidth;
};
// e.g. 100x100 viewport and a 10x10px element at position {top: -1, left: 0, bottom: 9, right: 10}
// elementIsVisibleInViewport(el) -> false (not fully visible)
// elementIsVisibleInViewport(el, true) -> true (partially visible)
```

### bottomVisible

如果到达页面可显示区域的底部，返回 `true` ，否则返回 `false` .

用 `scrollY`, `scrollHeight` 和 `clientHeight` 去判断是否到达页面可显示区域底部.

```js
const bottomVisible = () =>
  document.documentElement.clientHeight + window.scrollY >= (document.documentElement.scrollHeight || document.documentElement.clientHeight);
// bottomVisible() -> true
```

### getScrollPosition

返回页面滚动后的位置，也即是html文档的卷起位置.

如果浏览器支持 `pageXOffset` 和 `pageYOffset`  就用它们；否则就用 `scrollLeft` 和 `scrollTop`.
参数 `el` 的默认值为 `window`.

```js
const getScrollPosition = (el = window) =>
  ({x: (el.pageXOffset !== undefined) ? el.pageXOffset : el.scrollLeft,
    y: (el.pageYOffset !== undefined) ? el.pageYOffset : el.scrollTop});
// getScrollPosition() -> {x: 0, y: 200}
```

### getStyle

返回指定元素属性的css值。

用 `Window.getComputedStyle()` 去获取指定元素的属性值。

```js
const getStyle = (el, ruleName) => getComputedStyle(el)[ruleName];
getStyle(document.querySelector('p'), 'font-size'); // '16px'
```

### hasClass

如果一个元素有指定的 class，返回 `true` ；否则，返回 `false`.

用 `element.classList.contains()` 去判断一个元素是否有一个指定的class.

```js
const hasClass = (el, className) => el.classList.contains(className);
hasClass(document.querySelector('p.special'), 'special'); // true
```

### toggleClass

切换一个元素的class。

用 `element.classList.toggle()` 去切换指定元素的class。

```js
const toggleClass = (el, className) => el.classList.toggle(className);
toggleClass(document.querySelector('p.special'), 'special'); // 这个段落将不再有 'special' class
```

### setStyle

设置指定元素css样式属性的值.

用 `element.style` 设置指定元素的样式 `value`。

```js
const setStyle = (el, ruleName, value) => (el.style[ruleName] = value);
setStyle(document.querySelector('p'), 'font-size', '20px'); // The first <p> element on the page will have a font-size of 20px
```

### hide

隐藏所有指定的元素。

用 spread (`...`) 操作符和 `Array.forEach()` 去给所有的指定元素添加 `display: none` 样式 。

```js
const hide = (...el) => [...el].forEach(e => (e.style.display = 'none'));
hide(document.querySelectorAll('img')); // Hides all <img> elements on the page
```

### show

显示所有指定的元素

用spread (`...`) 操作符和 `Array.forEach()` 清除所有指定元素的 `display` 属性。

```js
const show = (...el) => [...el].forEach(e => (e.style.display = ''));
show(document.querySelectorAll('img')); // Shows all <img> elements on the page
```

### speechSynthesis

语音合成 (实验特性)

用 `SpeechSynthesisUtterance.voice` 和 `window.speechSynthesis.getVoices()` 将一个消息转换为语音.
用 `window.speechSynthesis.speak()` 播放消息.

更多详情请参考 [SpeechSynthesisUtterance interface of the Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesisUtterance).

```js
const speechSynthesis = message => {
  const msg = new SpeechSynthesisUtterance(message);
  msg.voice = window.speechSynthesis.getVoices()[0];
  window.speechSynthesis.speak(msg);
};
speechSynthesis('Hello, World'); // // plays the message
```

### onUserInputChange

不管什么时候用户的input类型改变 (`mouse` or `touch`) 执行这个函数。用于启用/禁用依赖于输入设备的代码。这个过程是动态的，并于混合设备（例如：触摸屏，笔记本）一起工作。

使用了两个事件监听器。假如 `mouse` 输入初始化，然后绑定一个 `touchstart` 时间将听页面。 
依据 `touchstart` 添加一个 `mousemove` 事件监听器，然后用 `performance.now()` 在20ms触发两次 `mousemove` 事件。
在这两种情况下，执行回调函数以input类型作为参数。

```js
const onUserInputChange = callback => {
  let type = 'mouse',
    lastTime = 0;
  const mousemoveHandler = () => {
    const now = performance.now();
    if (now - lastTime < 20)
      (type = 'mouse'), callback(type), document.removeEventListener('mousemove', mousemoveHandler);
    lastTime = now;
  };
  document.addEventListener('touchstart', () => {
    if (type === 'touch') return;
    (type = 'touch'), callback(type), document.addEventListener('mousemove', mousemoveHandler);
  });
};

onUserInputChange(type => {
  console.log('The user is now using', type, 'as an input method.');
});
```


### UUIDGeneratorBrowser

在浏览器中生成UUID。

用 `crypto` API 生成一个 UUID, 可参考 [RFC4122](https://www.ietf.org/rfc/rfc4122.txt) v.4 。 

```js
const UUIDGeneratorBrowser = () =>
  ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, c =>
    (c ^ (crypto.getRandomValues(new Uint8Array(1))[0] & (15 >> (c / 4)))).toString(16)
  );
UUIDGeneratorBrowser(); // '7982fcfe-5721-4632-bede-6000885be57d'
```

### arrayToHtmlList

将指定的数组元素转换成 `<li>` 元素标签，然后将它们插入指定的id选择器元素内.

用 `Array.map()` 和 `document.querySelector()` 去生成一个html元素标签列表.

```js
const arrayToHtmlList = (arr, listID) => arr.map(item => document.querySelector("#"+listID).innerHTML+=`<li>${item}</li>`);
// arrayToHtmlList(['item 1', 'item 2'],'myListID')
```

### copyToClipboard

复制一个字符串到剪切板。只用在用户触发事件时才会执行 (i.e. 内部用 `click` 事件监听)。

创建一个 `<textarea>` 元素，然后给它填充指定的字符串作为文本，最后将这个元素添加到html文档中。
用 `Selection.getRangeAt()` 存储选中的范围 (如果有的情况下)。
用 `document.execCommand('copy')` 去执行复制到剪切板。
从HTML文档中移除 `<textarea>` 元素.
最后，用 `Selection().addRange()` 去恢复原来的选中范围 (如果有的情况下).

```js
const copyToClipboard = str => {
  const el = document.createElement('textarea');
  el.value = str;
  el.setAttribute('readonly', '');
  el.style.position = 'absolute';
  el.style.left = '-9999px';
  document.body.appendChild(el);
  const selected =
    document.getSelection().rangeCount > 0 ? document.getSelection().getRangeAt(0) : false;
  el.select();
  document.execCommand('copy');
  document.body.removeChild(el);
  if (selected) {
    document.getSelection().removeAllRanges();
    document.getSelection().addRange(selected);
  }
};
```

```js
copyToClipboard('Lorem ipsum'); // 'Lorem ipsum' copied to clipboard.
```
