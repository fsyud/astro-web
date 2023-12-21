---
title: "Building a react native app-02"
description: "react native notes"
publishDate: " 06 Dec 2023"
tags: ["react-native"]
---

[项目地址 FreeSoulRN](https://github.com/ligdy7/FreeSoulRN)

## 搭建开发环境

### 完整原生环境

根据你所使用的操作系统、针对的目标平台不同，具体步骤有所不同。如果想同时开发 iOS 和 Android 也没问题，你只需要先选一个平台开始，另一个平台的环境搭建只是稍有不同。

> 本文搭建环境 windows10，目标平台android

#### 安装依赖

必须安装的依赖有：Node、JDK 和 Android Studio。

虽然你可以使用任何编辑器来开发应用（编写 js 代码），但你仍然必须安装 Android Studio 来获得编译 Android 应用所需的工具和环境。

Node, JDK
我们建议直接使用搜索引擎搜索下载 Node 和[Java SE Development Kit (JDK)](https://www.oracle.com/java/technologies/downloads/#java17)

**注意 Node 的版本应大于等于 16**，安装完 Node 后建议设置 npm 镜像（淘宝源）以加速后面的过程（或使用科学上网工具）。

> 注意：强烈建议始终选择 Node 当前的 LTS （长期维护）版本，一般是偶数版本，不要选择偏实验性质的奇数版本。
> 注意：不要使用 cnpm！cnpm 安装的模块路径比较奇怪，packager 不能正常识别！

React Native 需要 **Java Development Kit [JDK] 17**。你可以在命令行中输入 javac -version（请注意是 javac，不是 java）来查看你当前安装的 JDK 版本。如果版本不合要求，则可以去 [Temurin](https://adoptium.net/temurin/releases/?variant=openjdk17&jvmVariant=hotspot) 或[Oracle JDK](https://www.oracle.com/java/technologies/downloads/#java17)上下载(后者下载需注册登录)。

> 低于 0.73 版本的 React Native 需要 JDK 11 版本，而低于 0.67 的需要 JDK 8 版本。

**本文作者使用版本 "react-native": "0.73.1" "react": "18.2.0"**

```
# 使用nrm工具切换淘宝源
npx nrm use taobao

# 如果之后需要切换回官方源可使用
npx nrm use npm
```

## Android 开发环境

> 译注：请注意！！！国内用户必须必须必须有稳定的代理软件，否则在下载、安装、配置过程中会不断遭遇链接超时或断开，无法进行开发工作。某些代理软件可能只提供浏览器的代理功能，或只针对特定网站代理等等，请自行研究配置或更换其他软件。总之如果报错中出现有网址，那就是因为链接源仓库的网络链接被阻断了，这一阻断现象可能因时间、地区、运营商而不同。

### 1. 安装 Android Studio

首先下载和安装[Android Studio](https://developer.android.google.cn/studio?hl=zh-cn)，国内用户可能无法打开官方链接，请自行使用搜索引擎搜索可用的下载链接。安装界面中选择"`Custom`"选项，确保选中了以下几项：

- Android SDK
- Android SDK Platform
- Android Virtual Device

然后点击"Next"来安装选中的组件。

> 如果选择框是灰的，你也可以先跳过，稍后再来安装这些组件。

安装完成后，看到欢迎界面时，就可以进行下面的操作了。

### 2. 安装 Android SDK

Android Studio 默认会安装最新版本的 Android SDK。目前编译 React Native 应用需要的是Android 13 (Tiramisu)版本的 SDK（注意 SDK 版本不等于终端系统版本，RN 目前支持 android 5 以上设备）。你可以在 Android Studio 的 SDK Manager 中选择安装各版本的 SDK。

你可以在 Android Studio 的欢迎界面中找到`SDK Manager`。点击"Configure"，然后就能看到"SDK Manager"。

SDK Manager 还可以在 Android Studio 的"Preferences"菜单中找到。具体路径是Appearance & Behavior → System Settings → Android SDK。

在 SDK Manager 中选择"SDK Platforms"选项卡，然后在右下角勾选"Show Package Details"。展开Android 13 (Tiramisu)选项，确保勾选了下面这些组件（如果你看不到这个界面，则需要使用稳定的代理软件）：

- Android SDK Platform 33
- Intel x86 Atom_64 System Image（官方模拟器镜像文件，使用非官方模拟器不需要安装此组件）
  然后点击"SDK Tools"选项卡，同样勾中右下角的"Show Package Details"。展开"Android SDK Build-Tools"选项，确保选中了 React Native 所必须的33.0.0版本。你可以同时安装多个其他版本。

### 3. 配置 ANDROID_HOME 环境变量

React Native 需要通过环境变量来了解你的 Android SDK 装在什么路径，从而正常进行编译。

打开控制面板 -> 系统和安全 -> 系统 -> 高级系统设置 -> 高级 -> 环境变量 -> 新建，创建一个名为ANDROID_HOME的环境变量（系统或用户变量均可），指向你的 Android SDK 所在的目录（具体的路径可能和下图不一致，请自行确认）：

SDK 默认是安装在下面的目录：

```
C:\Users\你的用户名\AppData\Local\Android\Sdk
```

你可以在 Android Studio 的"Preferences"菜单中查看 SDK 的真实路径，具体是Appearance & Behavior → System Settings → Android SDK。

你需要关闭现有的命令符提示窗口然后重新打开，这样新的环境变量才能生效。

### 4. 把一些工具目录添加到环境变量 Path

打开控制面板 -> 系统和安全 -> 系统 -> 高级系统设置 -> 高级 -> 环境变量，选中Path变量，然后点击编辑。点击新建然后把这些工具目录路径添加进去：platform-tools、emulator、tools、tools/bin

```
%ANDROID_HOME%\platform-tools
%ANDROID_HOME%\emulator
%ANDROID_HOME%\tools
%ANDROID_HOME%\tools\bin
```

**JAVA_HOME**也需要配置到系统变量

## 创建新项目

如果你之前全局安装过旧的react-native-cli命令行工具，请使用npm uninstall -g react-native-cli卸载掉它以避免一些冲突：

```
npm uninstall -g react-native-cli @react-native-community/cli
```

使用 React Native 内建的命令行工具来创建一个名为"AwesomeProject"的新项目。这个命令行工具不需要安装，可以直接用 node 自带的npx命令来使用：

> 必须要看的注意事项一：请不要在目录、文件名中使用中文、空格等特殊符号。请不要单独使用常见的关键字作为项目名（如 class, native, new, package 等等）。请不要使用与核心模块同名的项目名（如 react, react-native 等）。
> 必须要看的注意事项二：请不要在某些权限敏感的目录例如 System32 目录中 init 项目！会有各种权限限制导致不能运行！
> 必须要看的注意事项三：请不要使用一些移植的终端环境，例如git bash或mingw等等，这些在 windows 下可能导致找不到环境变量。请使用系统自带的命令行（CMD 或 powershell）运行。

```
npx react-native@latest init AwesomeProject
```

如果你是想把 React Native 集成到现有的原生项目中，则步骤完全不同，请参考集成到现有原生应用。

### [可选参数] 指定版本或项目模板

你可以使用--version参数（注意是两个杠）创建指定版本的项目。注意版本号必须精确到两个小数点。

```
npx react-native@X.XX.X init AwesomeProject --version X.XX.X
```

还可以使用--template来使用一些社区提供的模板。

### 准备 Android 设备

你需要准备一台 Android 设备来运行 React Native Android 应用。这里所指的设备既可以是真机，也可以是模拟器。后面我们所有的文档除非特别说明，并不区分真机或者模拟器。Android 官方提供了名为 Android Virtual Device（简称 AVD）的模拟器。此外还有很多第三方提供的模拟器如Genymotion、BlueStack 等。一般来说官方模拟器免费、功能完整，但性能较差。第三方模拟器性能较好，但可能需要付费，或带有广告。

### 使用 Android 真机

你也可以使用 Android 真机来代替模拟器进行开发，只需用 usb 数据线连接到电脑，然后遵照在设备上运行这篇文档的说明操作即可。

### 使用 Android 模拟器

你可以使用 Android Studio 打开项目下的"android"目录，然后可以使用"AVD Manager"来查看可用的虚拟设备，它的图标看起来像下面这样：

Android Studio AVD Manager

如果你刚刚才安装 Android Studio，那么可能需要先创建一个虚拟设备。点击"Create Virtual Device..."，然后选择所需的设备类型并点击"Next"，然后选择Tiramisu API Level 33 image.

> 译注：请不要轻易点击 Android Studio 中可能弹出的建议更新项目中某依赖项的建议，否则可能导致无法运行。

## 编译并运行 React Native 应用

确保你先运行了模拟器或者连接了真机，然后在你的项目目录中运行yarn android或者yarn react-native

此命令会对项目的原生部分进行编译，同时在另外一个命令行中启动Metro服务对 js 代码进行实时打包处理（类似 webpack）。Metro服务也可以使用yarn start命令单独启动。

```
<!-- 开启Metro服务 -->
yarn start
yarn android

# 或者
yarn start
yarn react-native run-android
```

如果配置没有问题，你应该可以看到应用自动安装到设备上并开始运行。注意第一次运行时需要下载大量编译依赖，耗时可能数十分钟。此过程严重依赖稳定的代理软件，否则将频繁遭遇链接超时和断开，导致无法运行。

`npx react-native run-android`只是运行应用的方式之一。你也可以在 Android Studio 中直接运行应用。

译注：建议在run-android成功后再尝试使用 Android Studio 启动。请不要轻易点击 Android Studio 中可能弹出的建议更新项目中某依赖项的建议，否则可能导致无法运行。

如果你无法正常运行，遇到奇奇怪怪的红屏错误，先回头仔细对照文档检查，然后可以看看问题讨论区。不同时期不同版本可能会碰到不同的问题，我们会在论坛中及时解答更新。但请注意千万不要执行 bundle 命令，那样会导致代码完全无法刷新。

### 修改项目

现在你已经成功运行了项目，我们可以开始尝试动手改一改了：

使用你喜欢的文本编辑器打开App.js并随便改上几行
按两下 R 键，或是在开发者菜单中选择 Reload，就可以看到你的最新修改。
完成了！
恭喜！你已经成功运行并修改了你的第一个 React Native 应用

参考[<https://reactnative.cn/docs/environment-setup](https://reactnative.cn/docs/environment-setup>)
