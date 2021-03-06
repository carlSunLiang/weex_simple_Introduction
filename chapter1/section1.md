# 第一节
什么是动态性？
 移动应用本身的灵活性、迭代更新的周期和成本优化到极致

为什么要解决动态化的问题？
动态化的程度和产品迭代演进的速度有着强烈的正相关
多个端投入重复的精力，导致频繁的触发新版本

其实本质是历史问题？

动态性问题不只是移动端特有的，桌面端也存在。
目前为止桌面端的结论似乎已经形成，那就是W3C长期倡导的WebPlatform (或被称为 Web App 或 HTML5 或浏览器)，然而这也经历了去平台差异化、native插件支持 (flash player)、设备性能提升、渲染引擎优化等过程。

Hybrid方案：以WebView为容器，以HTML5为基石，通过定义native特性的扩展来支持的动态化产品研发，比如手机淘宝内部的名为WindVane的容器，这类方案通常具有非常高的动态性，但存在的问题和动态性本身一样明显，那就是性能和展现效果上的不足，而且想把其优势在工程中充分发挥出来，对开发者在前端知识和经验上的积累也有较高的要求。

结构化native view方案：以native view为容器进行 native 级别的渲染，并定义一套描述视图结构的数据格式 (如 XML 或 JSON 等) ，然后通过动态改变或请求新的这样的数据信息达到动态化的界面效果，比如阿里巴巴集团内出现 (过) 的 WeApp、鸟巢、Dynative、PageKit 等，这类方案依赖一个结构化的界面描述，并重点保障纯展现输出维度的动态性，各有千秋，但有一些共性的不足之处，比如对其它维度的动态性处理，比如逻辑的动态性，加载策略的动态性等。

React Native方案：RN，以native为渲染引擎，通过脚本引擎支持界面Virtual DOM的转换和逻辑控制，来实现界面的动态性。RN 前半年在阿里很多团队都得到了实践，包括我所在的无线事业部，但效果并不令人满意，首先是RN量级非常重，在请求、加载、渲染、交互、稳定性等层面都不够理想，而整个技术方案在社区的迭代和演进过程也一直充满着不确定性，这给团队产品级别的运用和后期跟进带来了很大的困惑。


