# 适配器模式

它将一个接口转换成客户希望的另一个接口。

它可以在一个接口使用多个类似的类，来解决不兼容性问题。其别名为包装器

注：它符合` 开闭原则 `

## 优缺点

优点：

* 将` 目标 `与` 适配者 `解耦。在不修改源代码的情况下，通过引入一个` 适配器 `来重用适配者。
* 增加了类的透明性和复用性，将具体的实现封装在` 适配者 `中，这对使用者来说是透明的，但它提高了适配者的复用性。
* 通过` 适配器 `可以体现出它的灵活性和扩展性。

缺点：

* 修改适配者类的方法比较麻烦

## 实例介绍

在开发中，通常会遇到下面相似的情况：

```
var googleMap = {
  show: function(){
    console.log( '开始渲染谷歌地图' );
  }
}; 
 
var baiduMap = {
  display: function(){
    console.log( '开始渲染百度地图' );
  }
};
 
var baiduMapAdapter = {
  show: function(){
    return baiduMap.display();
  }
}; 
 
renderMap( googleMap );        // 输出：开始渲染谷歌地图
renderMap( baiduMapAdapter );  // 输出：开始渲染百度地图 
```

通过建立一个` baiduMapAdapter `来解决解决` baiduMap `和` googleMap `接口不一致的问题。

注：它通过创建一个对象，来改装原有的对象，从而满足` 使用者 `(上面实例中的使用者是` renderMap() `)的需求。
