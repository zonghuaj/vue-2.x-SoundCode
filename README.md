# vue-2.x  SourceCode
vue 2.x 源码解析

`关于vue，以及为何要来写这份源码解析的原因`

笔者从最开始接触到 vue 应该还是在 15年 10月份左右，当时听说 前端圈中发生很多的变化，从那个时候，决定正式成为一名 前端er，且专业的前端，索性就离开当时所在的公司，做了为期大约 一个月的时间来准备找这么一份 前端的工作， 在这一个月了，每天基本上就宅在家里 写写demo ，看看视频。 那个时候看的 还是慕课，因为 在大学里面 根本就不是 cs系的。所以那段时间里面看了大量的视频，因为基础知识太匮乏了，如果连最基本的 知识都不能很好的掌握的话，可能也不会有 后面能面试到一份还不错的前端工作。（当时是在武汉的最大的一家房地产 上市企业）
（tips： 现在再回过头来，仔细看看 打好基础最好的方法方式就是去看书学习，什么看视频，看别人写的文章。都很不靠谱，因为别人写的是别人理解过一遍的东西，和你自己理解的完全是不一样的。）

嗯，进入到这家企业之后，见到了真正的 什么是互联网公司，各个部门分工明确，前端组，java组，运维组，UI产品部门 等等，这里我就不过多赘述了，当然这里要非常感谢，在 博客园中给我 留言的那位前辈，当时我还在一家 小企业做 程序员，tips，是程序员 不是什么 前端工程师，也不是什么 php工程师，也不是什么 运维工程师，就是程序员，从切图，到代码实现，到交互，到后台数据库，到 运维，基本上都是一个人在做，并且 现在的我看来，都做的很差。 希望现在有和我当年一样经历的同学，赶紧辞职，去找一份 专业互联网企业的工作（当然，扫地、倒茶之类的就算了），去看看外面的世界。 会对你有很大的帮助的。

好了，废话也说了这么多了。let's do i(ai)t(ti)

### Vue 源码第一步

#### 当前 Vue 的版本 V2.2.2

`
在最新版本中，Vue 给出了 各种 规格大小的 种类。有专门做服务端渲染的vue， 也有专门用于 组件化前端开发的 版本，这里先介绍到这些。在后面的源码分析中，我们会 详细的解释到，vue 为什么这么做。
`


`
在了解 vue 的源码之前。我们需要 先了解一下 vuejs 的生命周期图。然后我们才方便 根据 这个 生命周期图，来一步步的揭开 vue 的神秘面纱。
`

![](http://images2015.cnblogs.com/blog/675289/201703/675289-20170310000248672-1522982858.png)

(如果有同学觉得这张图片太大不方便查看，可以打开 vuejs 的官网中的 生命周期图，进行对照 本系列文章 进行阅读。)

（tips：另外笔者是一个 发散性思维的人，遇到看到的问题，不了解可能会 开一个分支去 做详细的理解和 介绍。 本文中 关于 js 基础的知识也会提及到很多）

1、在 vue 的生命周期的第一步 就是一个 大大的 new 顶在了上面。

```javascript
   new Vue()
```

JS 知识扩展

`
看上去是不是就很熟悉了，对。这里就说一个 构造函数的 实例化 的过程。
那么，我们来详细的介绍下， new 这么一个东西，在 js 中到底是 做了哪些事情。简单来讲 js 在new 的过程中只做了三件事
`

1. 创建一个新对象
2. 将这个新对象的 __proto__ 指向 构造函数的 prototype 成员对象
3. 将 构造函数的 this 指针 指向 这个新创建的对象

具体的文档

[关于 new 的相关介绍](https://github.com/vueSpa/vue-2.x-SoundCode/blob/master/prototype-proto.md)


然后，我们需要思考的问题来了。

去哪里找到 vue 实例化的 构造函数呢？

嗯，最后还是找到了。

[vue 实例化构造函数](https://github.com/vuejs/vue/blob/dev/src/core/instance/index.js)

就是我们当前 该项目的 vue 文件夹下 core ／ instance ／ index.js

`
好，我们打开这个文件 看看里面具体是 什么
`

```javascript
import { initMixin } from './init'
import { stateMixin } from './state'
import { renderMixin } from './render'
import { eventsMixin } from './events'
import { lifecycleMixin } from './lifecycle'
import { warn } from '../util/index'

function Vue (options) {
  if (process.env.NODE_ENV !== 'production' &&
    !(this instanceof Vue)) {
    warn('Vue is a constructor and should be called with the `new` keyword')
  }
  this._init(options)
}

initMixin(Vue)
stateMixin(Vue)
eventsMixin(Vue)
lifecycleMixin(Vue)
renderMixin(Vue)

export default Vue
```

看着这个代码觉得还ok，但是，在你查找 init 方法的时候，你会发现

```javascript
Vue.prototype._init = function (options?: Object) {
    const vm: Component = this
    // a uid
    vm._uid = uid++
    // ...
```

这是什么 js 代码啊？

原来一看， 这里写的 flow 的代码， 所以这里又得 插播一条 关于flow 的相关 知识的介绍了。

首先我们看看 vue 的作者 对于 选择 flow 而不是 typescript 的原因

[vue 源码为什么选择 flow 来完成](https://www.zhihu.com/question/46397274)










