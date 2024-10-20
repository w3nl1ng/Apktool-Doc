# FAQ

## YouTube视频中展示的`-j`选项是什么？

详见 [Issue 199](https://github.com/iBotPeaches/Apktool/issues/199), 简而言之 - 这个选项不存在了

## 在Android设备上运行Apktool可能吗？

很遗憾官方软件包不支持，在Android上运行`aapt`存在一些兼容性问题. 而且, 老的Android版本软件包在使用java.nio时也存在一些问题. 只有通过交叉编译的非官方软件包才能成功在Android上运行

## Apktool的源代码在哪里下载

从 [GitHub](https://github.com/iBotPeaches/Apktool) 或者 [Bitbucket](https://bitbucket.org/iBotPeaches/apktool/overview) 仓库

## 重新打包的APK比原始版本小很多. 是什么地方出问题了吗？

导致这种情况发生的原因有几种：
- Apktool 从新打包的APK不会签名. 这意味着会缺失类似 `META-INF` 这样的文件或目录
- 构建过程更现代了. Apktool使用现代版本的 `aapt/aapt2`，这可能导致文件体积缩小

## 重新打包的APK中没有 `META-INF` 目录，这OK吗？

是的，`META-INF` 包含APK的签名. 修改APK后，不会对其进行签名。你可以使用 `-c / --copy-original`保留这些签名。但是`-c`选项会使用原始的`AndroidManifest.xml`文件，它只能和原始的APK签名v1方案一起使用才行

## 什么是"Magic APKs"?

有一些APK是使用针对特定OEM修改后的构建工具构建的。这些APK不是使用常规的AOSP工具构建的，并且不兼容Apktool。Apktool无法解包或者重新打包这些APK。

## 我可以将Apktool整合到我自己的项目中吗？我可以修改Apktool吗？我必须在Credit中添加原作者吗？

- 是的，你可以不经许可分发或者修改Apktool
- 但是，如果你可以将原作者(brut.all, iBotPeaches, JesusFreke)添加到你项目的credit中，那真是太棒了

## Apktool将framework文件保存在哪里

- Windows：`$HOME/AppData/Local/apktool`
- Mac: `$HOME/Library/apktool`
- Linux: `$HOME/.local/share/apktool`