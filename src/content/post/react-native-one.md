---
title: "Building a react native app-01"
description: "react native notes"
publishDate: " 05 Dec 2023"
tags: ["react-native"]
---

[项目地址 FreeSoulRN](https://github.com/ligdy7/FreeSoulRN)

## 初识

### 核心组件与原生组件

React Native 是一个使用React和应用平台的原生功能来构建 Android 和 iOS 应用的开源框架。通过 React Native，您可以使用 JavaScript 来访问移动平台的 API，以及使用 React 组件来描述 UI 的外观和行为：一系列可重用、可嵌套的代码。你可以在下一节了解更多关于 React 的信息。但首先，让我们介绍一下组件在 React Native 中是如何工作的。

### 视图（Views）与移动开发

在 Android 和 iOS 开发中，一个视图是 UI 的基本组成部分：屏幕上的一个小矩形元素、可用于显示文本、图像或响应用户输入。甚至应用程序最小的视觉元素（例如一行文本或一个按钮）也都是各种视图。某些类型的视图可以包含其他视图。全部都是视图。

### 原生组件

在 Android 开发中是使用 Kotlin 或 Java 来编写视图；在 iOS 开发中是使用 Swift 或 Objective-C 来编写视图。在 React Native 中，则使用 React 组件通过 JavaScript 来调用这些视图。在运行时，React Native 为这些组件创建相应的 Android 和 iOS 视图。由于 React Native 组件就是对原生视图的封装，因此使用 React Native 编写的应用外观、感觉和性能与其他任何原生应用一样。我们将这些平台支持的组件称为原生组件。

React Native 允许您为 Android 和 iOS 构建自己的 Native Components（原生组件），以满足您开发应用程序的独特需求。我们还有一个由社区贡献的繁荣生态系统，您可以到Native Directory来查找社区已创建的内容。

### 核心组件

React Native 还包括一组基本的，随时可用的原生组件，您可以使用它们来构建您的应用程序。这些是 React Native 的核心组件。

示例请参考
[https://reactnative.cn/docs/using-a-listview](https://reactnative.cn/docs/using-a-listview)

## 特定平台代码

在编写跨平台的应用时，我们肯定希望尽可能多地复用代码。但是总有些时候我们会碰到针对不同平台编写不同代码的需求。

React Native 提供了两种方法来区分平台：

- 使用Platform模块.
- 使用特定平台后缀.

另外有些内置组件的某些属性可能只在特定平台上有效。请在阅读文档时留意。

### Platform 模块

React Native 提供了一个检测当前运行平台的模块。如果组件只有一小部分代码需要依据平台定制，那么这个模块就可以派上用场。

```react
import {Platform, StyleSheet} from 'react-native';

const styles = StyleSheet.create({
  height: Platform.OS === 'ios' ? 200 : 100,
});
```

Platform.OS在 iOS 上会返回ios，而在 Android 设备或模拟器上则会返回android。

还有个实用的方法是 Platform.select()，它可以以 Platform.OS 为 key，从传入的对象中返回对应平台的值，见下面的示例：

```react
import {Platform, StyleSheet} from 'react-native';

const styles = StyleSheet.create({
  container: {
    flex: 1,
    ...Platform.select({
      ios: {
        backgroundColor: 'red',
      },
      android: {
        backgroundColor: 'blue',
      },
    }),
  },
});
```

上面的代码会根据平台的不同返回不同的 container 样式 —— iOS 上背景色为红色，而 android 为蓝色。

这一方法可以接受任何合法类型的参数，因此你也可以直接用它针对不同平台返回不同的组件，像下面这样：

```react
const Component = Platform.select({
  ios: () => require('ComponentIOS'),
  android: () => require('ComponentAndroid'),
})();

<Component />;
```

### 检测 Android 版本

在 Android 上，Version属性是一个数字，表示 Android 的 api level：

```react
import {Platform} from 'react-native';

if (Platform.Version === 25) {
  console.log('Running on Nougat!');
}
```

### 检测 iOS 版本

在 iOS 上，Version属性是-[UIDevice systemVersion]的返回值，具体形式为一个表示当前系统版本的字符串。比如可能是"10.3"。

```react
import {Platform} from 'react-native';

const majorVersionIOS = parseInt(Platform.Version, 10);
if (majorVersionIOS <= 9) {
  console.log('Work around a change in behavior');
}
```

### 特定平台后缀

当不同平台的代码逻辑较为复杂时，最好是放到不同的文件里，这时候我们可以使用特定平台后缀。React Native 会检测某个文件是否具有.ios.或是.android.的后缀，然后根据当前运行的平台自动加载正确对应的文件。

比如你可以在项目中创建下面这样的组件：

```
BigButton.ios.js
BigButton.android.js
```

然后去掉平台后缀直接引用：

```
import BigButton from './BigButton';
```

React Native 会根据运行平台的不同自动引入正确对应的组件。

如果你还希望在 web 端复用 React Native 的代码，那么还可以使用.native.js的后缀。此时 iOS 和 Android 会使用BigButton.native.js文件，而 web 端会使用BigButton.js。（注意目前官方并没有直接提供 web 端的支持，请在社区搜索第三方方案）。

比如在你的项目中存在如下的两个文件:

```
Container.js # 由 Webpack, Rollup 或者其他打包工具打包的文件
Container.native.js # 由 React Native 自带打包工具(Metro) 为ios和android 打包的文件
```

在引用时并不需要添加.native.后缀:

```
import Container from './Container';
```

提示: 为避免在构建后的 Web 生产环境的代码中出现多余代码，要记得在你的 Web 打包工具中配置忽略.native.js结尾的文件, 这样可以减少打包后文件的大小。

## 其他参考资源

[https://reactnative.cn/docs/more-resources](https://reactnative.cn/docs/more-resources)

如果你耐心的读完并理解了本网站上的所有文档，那么你应该已经可以编写一个像样的 React Native 应用了。但是 React Native 并不全是某一家公司的作品——它汇聚了成千上万开源社区开发者的智慧结晶。如果你想深入研究 React Native，那么建议不要错过下面这些参考资源。

### 常用的第三方库

如果你正在使用 React Native，那你应该已经对React有一定的了解了。React 是基础中的基础所以我其实不太好意思提这个——但是，如果不幸你属于“但是”，那么请一定先了解下 React，它也非常适合编写现代化的网站。

开发实践中的一个常见问题就是如何管理应用的“状态（state）”。这方面目前最流行的库非Redux莫属了。不要被 Redux 中经常出现的类似"reducer"这样的概念术语给吓住了——它其实是个很简单的库，网上也有很多优秀的视频教程（英文） 。

如果你在寻找具有某个特定功能的第三方库，那么可以看看别人精心整理的资源列表。这里还有个类似的中文资源列表。

更重要的技能是学会在 github 上搜索。比如你需要搜索视频相关的库，那么可以在 github 中搜索react native video。

### 开发工具

VS Code是目前非常受 JS 开发者欢迎的 IDE 工具。

Ignite是一套整合了 Redux 以及一些常见 UI 组件的脚手架。它带有一个命令行可以生成 app、组件或是容器。如果你喜欢它的选择搭配，那么不妨一试。

App Center是由微软提供的热更新服务。热更新可以使你绕过 AppStore 的审核机制，直接修改已经上架的应用。对于国内用户，我们也推荐由本网站提供的Pushy热更新服务，相比 CodePush 来说，提供了全中文的文档和技术支持，服务器部署在国内速度更快，还提供了全自动的差量更新方式，大幅节约更新流量，欢迎朋友们试用和反馈意见！

Expo是一套沙盒开发环境，还带有一个已上架的空应用容器。这样你可以在没有原生开发平台（Xcode 或是 Android Studio）的情况下直接编写 React Native 应用（当然这样你只能写 js 部分代码而没法写原生代码）。

Yoga是一个独立的布局引擎。它并不局限于 React Native，高度优化的开源布局引擎能让产品工程师为多个平台快速构建布局。该引擎在设计时考虑了速度、大小和易用性。

Bugsnag, Microsoft App Center, 以及Sentry都为 React 和 React Native 应用程序提供了出色的崩溃和错误监控服务。通过这些服务，你可以实时主动监控应用程序上发生的崩溃和问题，以便快速修复它们并改善用户体验。
