# js其他判断

## string 相关

### isNonBlankString

```
function isNonBlankString(val) {
  return typeof val === 'string' && (/\S/).test(val);
}
```

### isBlankString

```
function isBlankString(val) {
  return typeof val === 'string' && val.trim().length === 0;
}
```