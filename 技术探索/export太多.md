## 文件太多忘记写export怎么办？

我们在写一个组件库或者npm包的时候，会遇到这种情况：需要导出多个组件或多个文件
里的方法，然后会有一个入口文件index.js，里面写满了export * from './xx.js'这样的语句。

可以看一下element-plus里面的代码，
[element-plus导出N个组件](https://github.com/element-plus/element-plus/blob/dev/packages/components/index.ts)

```javascript
export * from './select'
export * from './select-v2'
export * from './skeleton'
export * from './slider'
export * from './space'
export * from './steps'
export * from './switch'
export * from './table'
export * from './table-v2'
export * from './tabs'
export * from './tag'
export * from './time-picker'
export * from './time-select'
export * from './timeline'
export * from './tooltip'
export * from './transfer'
export * from './tree'
```

其实这样的代码很整洁了，但是有的时候会出现这样的情况：
新增了一个组件或者文件，但是忘记在入口文件里写导出语句。

而且本身这些代码如此规整，看起来可以自动生成啊！


## 自动生成export语句 小工具

为了应对忘记写export语句的情况发生，我们可以写一个小工具，来自动生成
这个入口文件。

```javascript
import fg from 'fast-glob';
import fs from 'fs';


const exec = async () => {
    const entries = await fg(['src/*.js']);

    // 如果入口文件已经存在，则删除它，
    const arr = await fg(['./main.js']);
    if (arr.length) {
        fs.unlinkSync('./main.js');
    }
    
    // 入口文件里的代码用文件追加方式一句句生成
    for (let key in entries) {
        fs.writeFile('./main.js', `export * from './${entries[key]}';\n`, {'flag':'a'}, function (err) {
            if (err) throw err;
        });
    }
};

exec();

```
请注意，这只是示例代码，对我的项目生效，如果你要用，请修改里面的路径部分！

## 视频讲解

录了一个视频，讲解这个问题， [总是忘记写export怎么办？](https://www.bilibili.com/video/BV1DW4y117sC/)


同时欢迎大家关注B站：飞叶-前端，我会持续分享前端技术。


有疑问也可以关注公众号：飞叶前端，加入我们的微信群讨论。