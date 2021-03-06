# 项目的通用规范

## 图片

在项目开发过程中，图片的处理往往是比较容易被忽略的环节，也会在一定程度影响开发的效率和页面的性能

### 基本使用原则

前端对图片的使用遵循以下几个原则：

**1. 少用图片元素，尽量用css3代替**

对于一些简单的效果，可以使用css3实现，比如圆角，提示框，不会二次渲染的元素的阴影等

**2. 大图、背景图首选 `JPG`**

处理不同业务场景时，图片格式首选JPG，适用于呈现色彩丰富的图片。
JPG 图片经常作为大的背景图、轮播图或 Banner 图出现。

**3. 透明图使用 `PNG`**

其中PNG32格式比PNG8格式会大很多，使用的时候根据下面的情况进行选择。

**4. 简单的小图片使用字体图标或雪碧图**

a) 如果页面中有较多的小icon，iconfont可以满足的，首先考虑使用字体图标iconfont
b) Logo、颜色简单且对比强烈的图片或背景、需要透明度且变动频率很低的小图片，使用css sprite将图片合并

### 图片压缩

图片压缩问题，除非特别要求图片必须高质量的显示，否则都应该进行对应的压缩。

一般情况下，ui设计直出的设计图都很大，我们在业务功能开发完毕之后，将图片压缩当做收尾优化工作的一部分。

图片压缩规则如下：

1. 图片压缩以 60~80 的品质为宜，这样在保证了图片质量的情况下使文件体积变小。
2. 对于不透明的，可以转换为jpg格式的png图片，先转换为jpg格式，再进行压缩。

**使用在线压缩网站进行压缩**

使用在线压缩网站进行压缩，已经基本上满足开发需求了，简单又方便。

推荐网站：

[https://tinypng.com/](https://tinypng.com/)

[https://www.picdiet.com/zh-cn](https://www.picdiet.com/zh-cn)
