# 第一节



![](/assets/三位一体.jpeg)

云端同学实现实时发布和动态部署、模版预解析处理，前端同学在 JS Framework 实现动态内容解析并处理成 Virtual DOM，客户端同学提供渲染实现和 native 特性的支持，接下来业务同学根据 DSL 实现动态内容的开发或配置即可。







1、JS Framework 通过对数据的依赖收集，实现响应式的视图层，再加上一层 diff 算法的优化，可以有效的过滤冗余的操作和复杂的计算。

2、Native 端对通信，Virtual DOM 解析以及布局实现等进行异步线程的处理，防止 UI 线程的阻塞。

3、对 UI 组件节点实现了复用处理，并对图片资源进行监控和回收，有效的减少内存的占用。

4、对于实时性要求较高的处理，Weex 允许第三方实现 native 的定制需求来保证体验的流畅性。



性能展示图片

https:\/\/segmentfault.com\/img\/bVsxT2

https:\/\/segmentfault.com\/img\/bVsxUN

https:\/\/segmentfault.com\/img\/bVsxUO

https:\/\/segmentfault.com\/img\/bVsxUT

https:\/\/segmentfault.com\/img\/bVsxU0







原理图

http:\/\/s3.51cto.com\/wyfs02\/M01\/7F\/96\/wKiom1cjMx2wbShGAAB09XtNG78519.jpg-s\_2897733784.jpg

