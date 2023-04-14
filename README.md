# 适配方案

在现代化的互联网时代，大屏幕的应用越来越广泛，如何实现大屏幕的适配成为了一个重要的问题。本文将介绍一种基于缩放的大屏适配方案。

## 方案介绍

该方案主要是通过缩放页面来实现大屏幕的适配。具体实现方式是通过计算当前屏幕与设计稿的比例，然后将页面进行缩放，使得页面元素在大屏幕上的大小与在设计稿上的大小相同。

## 实现细节

### 屏幕缩放实现

在代码中，通过以下函数实现屏幕缩放：

```javascript
function refreshScale() {
  bodyStyle.innerHTML = `body{width:${designWidth}px; height:${designHeight}px!important;}`
  document.documentElement.firstElementChild.appendChild(bodyStyle)
  document.getElementById('main').style = 'display:flex'
  document.getElementsByClassName('mobile')[0].style = 'display:none'

  var widthRatio = docWidth / designWidth,
    heightRatio = docHeight / designHeight
  document.body.style = `transform:scale(${widthRatio},${heightRatio});transform-origin:left top;`
  // 应对浏览器全屏切换前后窗口因短暂滚动条问题出现未占满情况
  setTimeout(function () {
    var lateWidth = document.documentElement.clientWidth,
      lateHeight = document.documentElement.clientHeight
    if (lateWidth === docWidth) return

    widthRatio = lateWidth / designWidth
    heightRatio = lateHeight / designHeight
    document.body.style =
      'transform:scale(' +
      widthRatio +
      ',' +
      heightRatio +
      ');transform-origin:left top;'
  }, 0)
}
```

该函数主要实现以下功能：

1. 设置页面宽高为设计稿的宽高。
2. 将页面元素进行缩放，使得页面元素在大屏幕上的大小与在设计稿上的大小相同。

### 清除缩放

在代码中，通过以下函数实现清除缩放：

```javascript
function clearScale() {
  // 清除pc样式
  bodyStyle.innerHTML = ``
  document.documentElement.firstElementChild.appendChild(bodyStyle)
  document.body.style = 'transform:none;transform-origin:none'
}
```


该函数主要实现以下功能：

1. 清除页面的缩放样式。

### 初始化

在代码中，通过以下函数实现初始化：

```javascript
function init() {
  // 获取当前屏幕可视区域大小
  docWidth = document.documentElement.clientWidth
  docHeight = document.documentElement.clientHeight
  // 判断是否是移动设备
  if (
    /Android|webOS|iPhone|iPad|iPod|BlackBerry|Windows Phone/i.test(
      navigator.userAgent
    )
  ) {
    mobilePage()
  } else {
    let mainClass = document.getElementById('main').classList
    if (docWidth == 1680 || docWidth == 3584) {
      // 模拟大屏
      designWidth = docWidth
      designHeight = (docWidth / 28) * 9
      mainClass.add('large')
      mainClass.remove('pc')
      largePage()
    } else {
      // pc
      designWidth = 1920
      designHeight = 1080
      mainClass.add('pc')
      mainClass.remove('large')
      refreshScale()
    }
  }
}
```


该函数主要实现以下功能：

1. 获取当前屏幕可视区域大小。
2. 判断当前设备是否为移动设备。
3. 如果是移动设备，则展示移动设备页面。
4. 如果是大屏幕，则展示大屏幕页面。
5. 如果是普通PC，则进行缩放。

### 大屏设置 rem 函数

在代码中，通过以下函数实现大屏设置 rem 函数：

```javascript
function setRem(designSize) {
  // 基准大小
  baseSize = 100
  let basePc = baseSize / designSize // 表示1680的设计图,使用100PX的默认值
  let vW = window.innerWidth // 当前窗口的宽度

  let rem = vW * basePc // 以默认比例值乘以当前窗口宽度,得到该宽度下的相应font-size值
  document.documentElement.style.fontSize = rem + 'px'
}
```

该函数主要实现以下功能：

1. 根据设计稿的大小计算出基准大小。
2. 根据当前窗口的宽度计算出相应的 font-size 值。

### 大屏页面

在代码中，通过以下函数实现大屏页面：

```javascript
function largePage() {
  clearScale()
  document.getElementById('main').style = 'display:flex'
  document.getElementsByClassName('mobile')[0].style = 'display:none'
  // 大屏 设置 rem 函数
  let designSize = 1680
  setRem(designSize)
}
```

该函数主要实现以下功能：

1. 清除页面的缩放样式。
2. 展示大屏幕页面。
3. 设置大屏幕页面的 font-size 值。

### 移动端页面

在代码中，通过以下函数实现移动端页面：

```javascript
function mobilePage() {
  clearScale()
  // 是移动设备 展示移动设备页
  document.getElementById('main').style = 'display:none'
  document.getElementsByClassName('mobile')[0].style = 'display:flex'
  // mobile 设置 rem 函数
  let designSize = 750
  setRem(designSize)
}
```


该函数主要实现以下功能：

1. 清除页面的缩放样式。
2. 展示移动端页面。
3. 设置移动端页面的 font-size 值。

## 总结

通过以上方案，我们可以实现大屏幕的适配。该方案主要是通过缩放页面来实现大屏幕的适配，具有简单易懂、易于实现的特点。同时，该方案也可以很好地适配移动端设备。
