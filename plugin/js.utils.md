# JavaScript插件库

* [` lodash `](https://github.com/lodash/lodash)
* [` underscore `](https://github.com/jashkenas/underscore)
* [` You-Dont-Need-Lodash-Underscore `](https://github.com/you-dont-need/You-Dont-Need-Lodash-Underscore)

* [` underscore源码解读 `](https://github.com/lessfish/underscore-analysis)

## camelize

hyphen-delimited -> camelize

```
function camelize(val) {
  return val.replace(/-(\w)/g, (_, c) => c ? c.toUpperCase() : '')
}
```

## hyphenize

camelize -> hyphen-delimited

```
function hyphen(val) {
  return val.replace(/\B([A-Z])/g, '-$1').toLowerCase()
}
```

## deepMerge

[` deepmerge `](https://github.com/TehShrike/deepmerge)
