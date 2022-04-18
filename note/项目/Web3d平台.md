## 项目说明

这是中国大学生服务外包大赛的项目，是一个Web3d短视频在线教育平台。

- 首先门户是一个动态门户页面，使用了gif动图。
- 而后是登录页面，用户可以进行登录注册。
- 而后是一个地图页，地图上标注虚拟学院位置。学院可以查看，被选中的学院的描述以及提供进入学院的链接。进入学院链接，可以看到多个3d的工作室，每个工作室都有简要介绍以及进入内部的链接。
- 进入工作室后可以提供短视频的观看以及任务的发布

（不过上述功能都没能实现）

## 技术栈

Vue + ThreeJs + axios + echart + elementUI + videoPlayer

- threeJS 用于构建3d页面以及3d可视化
- axios用于向后台请求数据
- echart用于制作地图
- videoPlayer是vue里的一个插件，用于播放短视频，提供更好的操作控制。

## 技术点

### 1. 怎么实现3d界面的？

首先关于WebGL方面，这里使用的是threejs，threejs是一个利用基于

WebGL创建的GPU增强的3D图形和动画，支持光照阴影反射，使用广泛的Web图形方法，而无需去关注动画的每一个细节。

**在threejs中，要渲染物体到网页中，需要4个组件：场景、相机、渲染器和几何体**。

- 使用 `var scene = new THREE.Scene()`  来创建一个场景
- 相机决定了场景中哪个角度的景色会显示出来，我们最终能够在浏览器中看到的景象，就是相机拍摄出来的。这里我使用的是透视相机。
- 渲染器决定了画家怎么把眼前的场景画到屏幕上，即渲染器决定了渲染的结果应该画在页面的什么元素上，并且以怎样的方式来绘制。

```javascript
var renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth,window.innerHeight);
document.body.appendChild(renderer.domElement);
```

- 几何体就是要渲染的元素



