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

2. Native Component模式

把Weex当作一个iOS\/Android组件来使用，类比ImageView。这类需求遍布手淘主链路，如首页、主搜结果、交易组件化等，这类Native页面主体已经很稳定，但是局部动态化需求旺盛导致频繁发版，解决这类问题也是Weex的重点。

3. H5 Component模式

在H5种使用Weex，类比WVC。一些较复杂或特殊的H5页面短期内无法完全转为Weex全页模式（或RN），比如互动类页面、一些复杂频道页等。这个痛点的解决办法是：在现有的H5页面上做微调，引入Native解决长列表内存暴增、滚动不流畅、动画\/手势体验差等问题。

另外，WVC将会融入到Weex中，成为Weex的H5 Components模式。





