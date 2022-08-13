## 项目开发中的图片问题

某些前端项目里含有大量图片，对这些图片进行系列操作似乎是必须的，
因为你不可能指望设计师给到你的图片就是能最终上到生产的。
你可能需要对图片进行一系列的处理，才能满足最终要求。


这里面最常见的图片处理可能就是图片压缩和图片格式转换。

### 图片压缩

因为dpr的关系，如果是做移动端项目，切图往往都是按照设备物理像素给定的，
经常是3倍图，这些图片是很大的。所以进行图片压缩是必不可少的一环。

如果只是少量图片压缩，我们可以使用诸如https://tinypng.com/   这样的
在线图片处理工具，但是他们都有一个问题，即能处理的图片量或收费问题。
如果有上百张图片要处理，使用他们是比较麻烦的。


以tinypng这个网站为例，虽然他说在免费版本里，一次最多可以帮忙转换20张图片，
但是如果你压缩完20张，马上再去压缩另外20张，
有一定的概率它不会帮你压缩完，会提示你：too many files uploaded。
那么这个时候，如果你可能需要稍等几分钟，再次操作，
它才可能帮你多压缩几张。
如此下来，压缩上百张图片，可能需要你几分钟甚至十几分钟时间。

并不是说十几分钟很长，只是说 你明知道，本可以几秒完成的事情，
却要花这么久，长此以往，你会觉得虽然免费但很不划算。

### 图片格式转换

第二个问题，也是基于3倍图很大的问题。如果我们使用常规的png或jpg图片，
在保证图片质量的情况下，那么再怎么压缩，都是有个下限的。

此时换一个空间占用更小的图片格式就可以解决这个下限问题。
我们可以将图片转换成webp和avif这两种格式，他们的大小非常小。

然后在需要引入图片的代码里，这样写：
```html
    <picture>
        <source srcset="@/assets/a.avif" type="image/avif">
        <source srcset="@/assets/a.webp" type="image/webp">
        <img src="@/assets/a.jpg" @error="onErr">
    </picture>
```

那么，浏览器将基于从上到下的顺序，依次查找图片，如果第一种格式的图片
浏览器可以渲染，就停止向下加载图片。

于是当你项目有上百张图片的时候，你就需要对这上百张图片进行格式转换。


## bat-sharp

正是由于项目开发中的这两个图片处理问题，我开发了一个工具bat-sharp。
他可以几秒钟就处理几百张图片的压缩和格式转换。

个人觉得很好用，所以发了个npm包，推荐给大家。

关于bat-sharp你可能有一些问题。

#### 为什么叫bat-sharp?

为什么叫bat-sharp呢？因为它底层使用的是sharp这个图片处理包，
我这里主要是封装了一些批处理的能力，所以叫bat-sharp。

#### 为什么它这么快？

因为sharp包底层是libvips，一个c++写的图片处理库。

## 使用bat-sharp

#### install

```
npm i bat-sharp -D
```

#### usage

```javascript
const { batSharp } = require('bat-sharp');

batSharp({
  inputArr: ['./images/*.png'],
  format: 'webp', // png jpeg webp avif等
  outputPath: './images2/',
  outputConfig: { // 参考 https://sharp.pixelplumbing.com/api-output#png
    quality: 60,
  },
})
```

请注意outputConfig, 根据你要输出的格式不同，内部字段不同。
不过也有一些相同的字段，比如控制图片质量的quality字段。


## 最后

bat-sharp的GitHub地址：https://github.com/cunzaizhuyi/bat-sharp

关于bat-sharp的讲解视频：https://www.bilibili.com/video/BV1ea411U7Vu/
关于如果在引用图片的时候使用多种格式：https://www.bilibili.com/video/BV17t4y157Ep/

我是飞叶，欢迎关注。 