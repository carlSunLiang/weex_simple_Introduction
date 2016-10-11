# 第一节

![](/assets/三位一体.jpeg)

云端同学实现实时发布和动态部署、模版预解析处理，前端同学在 JS Framework 实现动态内容解析并处理成 Virtual DOM，客户端同学提供渲染实现和 native 特性的支持，接下来业务同学根据 DSL 实现动态内容的开发或配置即可。

1、JS Framework 通过对数据的依赖收集，实现响应式的视图层，再加上一层 diff 算法的优化，可以有效的过滤冗余的操作和复杂的计算。

2、Native 端对通信，Virtual DOM 解析以及布局实现等进行异步线程的处理，防止 UI 线程的阻塞。

3、对 UI 组件节点实现了复用处理，并对图片资源进行监控和回收，有效的减少内存的占用。

4、对于实时性要求较高的处理，Weex 允许第三方实现 native 的定制需求来保证体验的流畅性。

性能展示图片   在加载时间、帧率、内存消耗、CPU占用（包括静默和峰值）等多个方面，Weex都表现得非常出色

https:\/\/segmentfault.com\/img\/bVsxT2

https:\/\/segmentfault.com\/img\/bVsxUN

https:\/\/segmentfault.com\/img\/bVsxUO

https:\/\/segmentfault.com\/img\/bVsxUT

https:\/\/segmentfault.com\/img\/bVsxU0

http:\/\/s3.51cto.com\/wyfs02\/M02\/7F\/96\/wKiom1cjM03i1TsrAAB5UGxTLnM104.jpg-s\_472785496.jpg

原理图

http:\/\/s3.51cto.com\/wyfs02\/M01\/7F\/96\/wKiom1cjMx2wbShGAAB09XtNG78519.jpg-s\_2897733784.jpg

http:\/\/s2.51cto.com\/wyfs02\/M00\/7F\/96\/wKiom1cjMzGjl3jEAABdOiQ3Kf0180.jpg-s\_1413676178.jpg

输入是Virtual DOM输出是native或者H5 view，还原成内存中的树型数据结构，再创建view，把事件绑定在view上，把view基本属性设上去。Weex Render会分三个线程，不同的线程负责不同的事情，让JS线程优先保障流畅性。忽略了一个事实，那就是Native UI开发中通常没有JS资源在服务器端加载。Weex会把JS内置到客户端里，以免除下载的问题，从而更为有效地提升性能。

Weex的三种工作模式

1. 全页模式

目前支持单页使用或整个App使用Weex开发（还不完善，需要开发Router和生命周期管理），这是主推的模式，可以类比RN。

1. Native Component模式

把Weex当作一个iOS\/Android组件来使用，类比ImageView。这类需求遍布手淘主链路，如首页、主搜结果、交易组件化等，这类Native页面主体已经很稳定，但是局部动态化需求旺盛导致频繁发版，解决这类问题也是Weex的重点。

1. H5 Component模式

在H5种使用Weex，类比WVC。一些较复杂或特殊的H5页面短期内无法完全转为Weex全页模式（或RN），比如互动类页面、一些复杂频道页等。这个痛点的解决办法是：在现有的H5页面上做微调，引入Native解决长列表内存暴增、滚动不流畅、动画\/手势体验差等问题。

另外，WVC将会融入到Weex中，成为Weex的H5 Components模式。

render渲染原理

weex h5 渲染经历3次文档加载： 加载index.html 加载weex框架 加载我们写的程序

首先在自执行函数内部：

```
function getUrlParam (key) {
      var reg = new RegExp('[?|&]' + key + '=([^&]+)')
      var match = location.search.match(reg)
      return match && match[1]
    }
```

这个函数主要用对url的参数进行过滤

\/\[?\|&\]KEY=\(\[^&\]+\)\/是用来匹配&KEY=testWorld值用的，testWorld是不能包含&字符的。

接下来:

```
var loader = getUrlParam('loader') || 'xhr'
    var page = getUrlParam('page') || 'demo/build/index.js'
```

用于初始化loader和page参数，然后利用weex.init\(\)实例化weex页面

```
window.weex.init({
      appId: location.href,
      loader: loader,
      source: page,
      rootId: 'weex'
    })
```

在js中我们知道定时器有两种，超时执行定时器和间隔执行定时器。但是在原生app中，只有超时执行定时器。可能weex为了兼容原生开发，只提供了一种定时器--超时执行定时器（setTimeout）。

```
'use strict'

const timer = {
  setTimeout: function (timeoutCallbackId, delay) {
/*sender = Sender {
                instanceId: "http://127.0.0.1:8081/weex_tmp/h5_render/?hot-reload_controller&page=tech_list.js&loader=xhr",
                 weexInstance: Weex
          }*/
    const sender = this.sender
    const timerId = setTimeout(function () {
      sender.performCallback(timeoutCallbackId)
    }, delay)
    return timerId
  },
  clearTimeout: function (timerId) {
    clearTimeout(timerId)
  }
}
timer._meta = {
  timer: [{
    name: 'setTimeout',
    args: ['function', 'number']
  }, {
    name: 'clearTimeout',
    args: ['number']
  }]
}
module.exports = timer
/** WEBPACK FOOTER **
 ** ./html5/browser/api/timer.js
 **/
```

我们可以看到这里面定义了两个方法，setTimeout和claerTimeout。

```
timer._meta = {
  timer: [{
    name: 'setTimeout',
    args: ['function', 'number']
  }, {
    name: 'clearTimeout',
    args: ['number']
  }]
}
```

这句代码中.\_meta是为weex注册apimodule使用的。这在browser\/api\/index.js中可以得到验证：

http:\/\/www.weexstore.com\/image\/show\/attachments-2016-08-eAh6qPgq57b5274951fde.png

如果想使用setInterval，那么可以使用setTimeout模拟，代码如下：

```
var intervalId=function(){
                require('@weex-module/timer').setTimeout(function(){
                    _this.user.timeoutNum=_this.user.timeoutNum-1;
                    intervalId();
                },1000);        
          };
      intervalId();
```

html5\/shared\/console

weex对console类进行了一个封装，我们在程序里调用的console.log\(\)不再直接是直接调用系统的了，而是调用Weex的封装了

```
const { console, nativeLog } = global
const LEVELS = ['error', 'warn', 'info', 'log', 'debug']
const levelMap = {}

generateLevelMap()

/* istanbul ignore if */
if (
  typeof console === 'undefined' || // Android
  (global.WXEnvironment && global.WXEnvironment.platform === 'iOS') // iOS
) {
  global.console = {
    debug: (...args) => {
      if (checkLevel('debug')) { nativeLog(...format(args), '__DEBUG') }
    },
    log: (...args) => {
      if (checkLevel('log')) { nativeLog(...format(args), '__LOG') }
    },
    info: (...args) => {
      if (checkLevel('info')) { nativeLog(...format(args), '__INFO') }
    },
    warn: (...args) => {
      if (checkLevel('warn')) { nativeLog(...format(args), '__WARN') }
    },
    error: (...args) => {
      if (checkLevel('error')) { nativeLog(...format(args), '__ERROR') }
    }
  }
}
else { // HTML5
  const { debug, log, info, warn, error } = console
  console.__ori__ = { debug, log, info, warn, error }
  console.debug = (...args) => {
    if (checkLevel('debug')) { console.__ori__.debug.apply(console, args) }
  }
  console.log = (...args) => {
    if (checkLevel('log')) { console.__ori__.log.apply(console, args) }
  }
  console.info = (...args) => {
    if (checkLevel('info')) { console.__ori__.info.apply(console, args) }
  }
  console.warn = (...args) => {
    if (checkLevel('warn')) { console.__ori__.warn.apply(console, args) }
  }
  console.error = (...args) => {
    if (checkLevel('error')) { console.__ori__.error.apply(console, args) }
  }
}

function generateLevelMap () {
  LEVELS.forEach(level => {
    const levelIndex = LEVELS.indexOf(level)
    levelMap[level] = {}
    LEVELS.forEach(type => {
      const typeIndex = LEVELS.indexOf(type)
      if (typeIndex <= levelIndex) {
        levelMap[level][type] = true
      }
    })
  })
}

function normalize (v) {
  const type = Object.prototype.toString.call(v)
  if (type.toLowerCase() === '[object object]') {
    v = JSON.stringify(v)
  }
  else {
    v = String(v)
  }
  return v
}

function checkLevel (type) {
  const logLevel = (global.WXEnvironment && global.WXEnvironment.logLevel) || 'log'
  return levelMap[logLevel] && levelMap[logLevel][type]
}

function format (args) {
  return args.map(v => normalize(v))
}



/** WEBPACK FOOTER **
 ** ./html5/shared/console.js
 **/
```

generateLevelMap \(\)是对日志事件做等级区分，即建立一个二维数组，比较任意两个的优先级。

normalize\(\)是对参数v做json序列化工作。

if\(\){}else{}则是对console中的每个函数进行调用。比如当调用console.warn\(\)是先检查这个日志等级是否高于当前的等级，然后调用相应方法。

promise封装

stream 两种方法sendhttp和fetch   \_jsonp.apply\(this, \_callArgs\)和\_xhr.apply\(this, \_callArgs\)这两个方法

sender提供了一个在原生调用js上提供了一个桥。

. 找到需要执行函数的实例

. 找到这一个方法

. 注入调用参数

. 启动调用

.调用结果的记录

实战可能出现的问题

http:\/\/www.weexstore.com\/image\/show\/attachments-2016-08-dLe1pjrG57b571f20b196.png

调试weex

```
1、console.error(this); 
```

http:\/\/www.weexstore.com\/image\/show\/attachments-2016-08-ABsBfaDU57b528da47ce0.png

http:\/\/www.weexstore.com\/image\/show\/attachments-2016-08-72WGrWPC57b529155dca6.png

缺点： 重新刷新页面

```
 2、debugger就可以自动进入到那断点
```

```
 3、webpack 配置
   devtool: 'source-map' 'display-error-details': true
```

# **推荐官方 Devtools**

