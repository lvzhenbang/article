# 检测浏览器类型

```
var userAgent = window.navigator && window.navigator.userAgent || '';
```

## ios

```
...
var is_ios = (/iPad|iPhone|iPod/i).test(userAgent);
console.log(is_ios);
```

## android

### type

```
var is_android = (/Android/i).test(userAgent);
```

### version

```
var android_version = (function() {
  var match = userAgent.match(/Android (\d+)(?:\.(\d+))?(?:\.(\d+))*/i);

  if (!match) {
    return null;
  }

  var major = match[1] && parseFloat(match[1]);
  var minor = match[2] && parseFloat(match[2]);

  if (major && minor) {
    return parseFloat(match[1] + '.' + match[2]);
  } else if (major) {
    return major;
  }
  return null;
}());
console.log(android_version)
```

### native

```
var webkitVersionMap = (/AppleWebKit\/([\d.]+)/i).exec(userAgent);
var appleWebkitVersion = webkitVersionMap ? parseFloat(webkitVersionMap.pop()) : null;

var is_native_android = is_android && android_version < 5 && appleWebkitVersion < 537;
```

## browser type

```
var is_firefox = (/Firefox/i).test(userAgent);
var is_Edge = (/Edge/i).test(userAgent);
var is_Chrome = !is_edge && (/Chrome/i).test(userAgent) || (/CriOS/i).test(userAgent);
var is_safari = (/Safari/i).test(userAgent)) && !is_Chrome && !is_android && !is_edge;
var is_weixin = (/MicroMessenger/i).test(userAgent);
var weixin_version = userAgent.match(/MicroMessenger\/(\d+)(?:\.(\d+))(?:\.(\d+))(?:\.(\d+))*/i)[0].split('/')[0];
```
