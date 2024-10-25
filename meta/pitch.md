# Pitch & anti-pitch

有一个问题经常被问起 - Apktool和xxx工具有什么不同？这是一个合理的问题，也是一个应该被提出的问题。但是，这也是一个经常被恶意提出的问题。这里将同时解决这两个问题。

因此这篇文章将给你一个“pitch”(为什么你应该使用Apktool)以及一个“anti-pitch”(为什么你不应该使用Apktool)

## Pitch
- Apktool已经有十三年以上的历史了，从下载量来看，它以被数十万人使用 - 因此你几乎可以在任何地方得到支持
- Apktool是开源的，这意味着你可以知道它做了什么以及怎么做的，你甚至可以为它出一份力
- Apktool是免费的，它使用Apache 2.0许可，这给予了使用Apktool时极大的灵活性
  - 在Apktool的基础上开发了大量工具，你也可以这么做
  - Apktool甚至是从他人的研究和工作中起步的
- Apktool是跨平台的，它可以在Windows，Linux和MacOS上运行
- Apktool是一个巨大的jar，你不需要安装其他任何东西来是他工作（除了java）
- Apktool使用一个简单的命令解包apk，以及一个简单的命令重新打包apk

## Anti-pitch
- Apktool在Android设备上运行的并不好，因为它使用了Android Studio编译应用时使用的本地工具
  - 这意味着为了使用Apktool在Android设备上打包应用程序 - 你需要交叉编译对应架构的aapt/aapt2
  - 这并不是不可能的，但这并不容易，Apktool官方也不想在上面花时间
  - 像[ARSCLib](https://github.com/REAndroid/ARSCLib)这样的库被引入，来替换aapt/aapt2在Android设备上的使用
- Apktool在修复问题和添加特性方面工作缓慢
  - Apktool是当前唯一维护者的副业。我已经得到了一个全职工作并且我还有其它责任
  - 像PNF Software的[JEB](https://www.pnfsoftware.com/jeb/android)这样的付费替代品价格昂贵，它们可能会超过Apktool
  - 像[apkanalyzer]这样的免费替代品可能更新地更快，但是也会被最简单的第三方混淆所困扰
- Apktool返回smali格式的代码而不是java格式，这对入门者来说有一定阻碍
  - [JADX](https://github.com/skylot/jadx)是一个伟大的替代品，他直接返回java格式的代码而且有一个健壮的资源解析器