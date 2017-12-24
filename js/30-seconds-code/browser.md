
## Browser

### arrayToHtmlList

Converts the given array elements into `<li>` tags and appends them to the list of the given id.

Use `Array.map()` and `document.querySelector()` to create a list of html tags.

```js
const arrayToHtmlList = (arr, listID) => arr.map(item => document.querySelector("#"+listID).innerHTML+=`<li>${item}</li>`);
// arrayToHtmlList(['item 1', 'item 2'],'myListID')
```


### bottomVisible

Returns `true` if the bottom of the page is visible, `false` otherwise.

Use `scrollY`, `scrollHeight` and `clientHeight` to determine if the bottom of the page is visible.

```js
const bottomVisible = () =>
  document.documentElement.clientHeight + window.scrollY >= (document.documentElement.scrollHeight || document.documentElement.clientHeight);
// bottomVisible() -> true
```


### currentURL

Returns the current URL.

Use `window.location.href` to get current URL.

```js
const currentURL = () => window.location.href;
// currentUrl() -> 'https://google.com'
```


### elementIsVisibleInViewport

Returns `true` if the element specified is visible in the viewport, `false` otherwise.

Use `Element.getBoundingClientRect()` and the `window.inner(Width|Height)` values
to determine if a given element is visible in the viewport.
Omit the second argument to determine if the element is entirely visible, or specify `true` to determine if
it is partially visible.

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


### getScrollPosition

Returns the scroll position of the current page.

Use `pageXOffset` and `pageYOffset` if they are defined, otherwise `scrollLeft` and `scrollTop`.
You can omit `el` to use a default value of `window`.

```js
const getScrollPosition = (el = window) =>
  ({x: (el.pageXOffset !== undefined) ? el.pageXOffset : el.scrollLeft,
    y: (el.pageYOffset !== undefined) ? el.pageYOffset : el.scrollTop});
// getScrollPosition() -> {x: 0, y: 200}
```


### getURLParameters

Returns an object containing the parameters of the current URL.

Use `match()` with an appropriate regular expression to get all key-value pairs, `Array.reduce()` to map and combine them into a single object.
Pass `location.search` as the argument to apply to the current `url`.

```js
const getURLParameters = url =>
  url.match(/([^?=&]+)(=([^&]*))/g).reduce(
    (a, v) => (a[v.slice(0, v.indexOf('='))] = v.slice(v.indexOf('=') + 1), a), {}
  );
// getURLParameters('http://url.com/page?name=Adam&surname=Smith') -> {name: 'Adam', surname: 'Smith'}
```


### httpsRedirect

Redirects the page to HTTPS if its currently in HTTP. Also, pressing the back button doesn't take it back to the HTTP page as its replaced in the history.

Use `location.protocol` to get the protocol currently being used. If it's not HTTPS, use `location.replace()` to replace the existing page with the HTTPS version of the page. Use `location.href` to get the full address, split it with `String.split()` and remove the protocol part of the URL.  

```js
const httpsRedirect = () => {
  if(location.protocol !== "https:") location.replace("https://" + location.href.split("//")[1]);
}
```


### redirect

Redirects to a specified URL.

Use `window.location.href` or `window.location.replace()` to redirect to `url`.
Pass a second argument to simulate a link click (`true` - default) or an HTTP redirect (`false`).

```js
const redirect = (url, asLink = true) =>
  asLink ? window.location.href = url : window.location.replace(url);
// redirect('https://google.com')
```


### scrollToTop

Smooth-scrolls to the top of the page.

Get distance from top using `document.documentElement.scrollTop` or `document.body.scrollTop`.
Scroll by a fraction of the distance from the top. Use `window.requestAnimationFrame()` to animate the scrolling.

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
