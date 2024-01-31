---
title: "Building a react native app-03"
description: "react native notes"
publishDate: " 07 Dec 2023"
tags: ["react-native"]
---

[项目地址 FreeSoulRN](https://github.com/fsyud/FreeSoulRN)

## 打包发布

Android 要求所有应用都有一个数字签名才会被允许安装在用户手机上，所以在把应用发布到应用市场之前，你需要先生成一个签名的 AAB 或 APK 包（Google Play 现在要求 AAB 格式，而国内的应用市场目前仅支持 APK 格式。但无论哪种格式，下面的签名步骤是一样的）。Android 开发者官网上的如何给你的应用签名文档描述了签名的细节。本指南旨在提供一个简化的签名和打包的操作步骤，不会涉及太多理论。

生成一个签名密钥
你可以用keytool命令生成一个私有密钥。在 Windows 上keytool命令放在 JDK 的 bin 目录中（比如C:\Program Files\Java\jdkx.x.x_x\bin），你可能需要在命令行中先进入那个目录才能执行此命令。

```
keytool -genkeypair -v -storetype PKCS12 -keystore my-upload-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
```

![Snipaste_2023-12-21_14-40-43](https://github.com/fsyud/astro-web/assets/26371465/71aabfb9-4d9d-457c-928b-3245a1bd5741)

**keytool**需配置环境变量，通过`管理员身份`打开Powershell

此命令会提示您输入密钥库和密钥的密码以及密钥的专有名称字段。然后，它将密钥库生成为名为 my-upload-key.keystore. 的文件。

密钥库包含单个密钥，有效期为 10000 天。别名是您稍后在签署应用程序时将使用的名称，因此请记住记下别名。

### 设置 Gradle 变量

- 1.将`my-upload-key.keystore` 文件放置在项目文件夹的android/app 目录下。
  -2.编辑文件`~/.gradle/gradle.properties或android/gradle.properties`，并添加以下内容（将**\***替换为正确的密钥库密码、别名和密钥密码）

```
MYAPP_UPLOAD_STORE_FILE=my-upload-key.keystore
MYAPP_UPLOAD_KEY_ALIAS=my-key-alias
MYAPP_UPLOAD_STORE_PASSWORD=*****
MYAPP_UPLOAD_KEY_PASSWORD=*****
```

这些将是全局 Gradle 变量，我们稍后可以在 Gradle 配置中使用它们来签署我们的应用程序。

### 把签名配置加入到项目的 gradle 配置中

编辑你项目目录下的android/app/build.gradle，添加如下的签名配置：

```
...
android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            if (project.hasProperty('MYAPP_RELEASE_STORE_FILE')) {
                storeFile file(MYAPP_RELEASE_STORE_FILE)
                storePassword MYAPP_RELEASE_STORE_PASSWORD
                keyAlias MYAPP_RELEASE_KEY_ALIAS
                keyPassword MYAPP_RELEASE_KEY_PASSWORD
            }
        }
    }
    buildTypes {
        release {
            ...
            signingConfig signingConfigs.release
        }
    }
}
...
```

### 生成发行 APK 包

只需在终端中运行以下命令：

```
cd android
./gradlew assembleRelease
```

> 译注：cd android表示进入 android 目录（如果你已经在 android 目录中了那就不用输入了）。./gradlew assembleRelease在 macOS、Linux 或是 windows 的 PowerShell 环境中表示执行当前目录下的名为 gradlew 的脚本文件，且其运行参数为 assembleRelease，注意这个./不可省略；而在 windows 的传统 CMD 命令行下则需要去掉./。

> Gradle 的assembleRelease参数会把所有用到的 JavaScript 代码都打包到一起，然后内置到 APK 包中。如果你想调整下这个行为（比如 js 代码以及静态资源打包的默认文件名或是目录结构等），可以看看android/app/build.gradle文件，然后琢磨下应该怎么修改以满足你的需求。

> 注意：请确保 gradle.properties 中没有包含*org.gradle.configureondemand=true*，否则会跳过 js 打包的步骤，导致最终生成的是一个无法运行的空壳。

生成的 APK 文件位于`android/app/build/outputs/apk/release/app-release.apk`，它已经可以用来发布了。

### 运行apk文件

下载android模拟机 [MUMU](https://mumu.163.com/)

完成
![Snipaste_2023-12-21_14-47-16](https://github.com/fsyud/astro-web/assets/26371465/9eded7d7-d46a-4eb6-bc5d-e58c2e658dd8)

参考 [https://reactnative.dev/docs/signed-apk-android](https://reactnative.dev/docs/signed-apk-android)
