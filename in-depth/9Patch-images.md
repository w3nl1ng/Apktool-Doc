# 9Patch图片

到处都有关于神秘的9Patch图片的文档
- https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch
- https://developer.android.com/tools/help/draw9patch.html

不过，这些文档是为开发人员准备的，对于那些使用编译好的第三方应用的人来说，缺少关键的信息。你可以在这些文档中学习如何创建9Patch图片，但是无法学习他具体是如何工作的

官方文档忽略了一点：9Patch图片有两种形式：source&compiled

- source - 你应该熟悉这种形式。你可以在一个应用的源代码中找到他，或者在网上免费获取。它们就是有黑色边框的图片

- compiled - apk文件中存在的形式。9Patch图片的数据被存储到了名叫`npTc`的二进制块中。你不能轻易查看或者修改它，但是Android系统可以。

关于上面两点有一些相关问题。

- 如果不进行转换，你无法在两种类型之间移动9Patch图片。如果你尝试从一个apk中解压9Patch图片并在另一个项目的源代码中使用，你会在构建期间得到报错。反之亦然，你无法在apk中直接使用源码形式的9Patch图片。

- 9Patch二进制块无法被现代的图片处理工具识别。所以修改编译后的图片可能会损坏`npTc`块，进而损坏设备中显示的图片。

这个问题的唯一解决方法是在这两种格式之间做转换。编码器被集成到了aapt工具中，在构建期间自动调用。这意味着我们只需要构建一个解码器，从`v1.3.0`起，apktool会自动调用解码器解码9Patch图片。

所以如果你想修改9Patch图片，不要直接在二进制层面做改动。先使用apktool解码应用，然后再在source层面做修改。当你重打包apk时，source类型的9Patch图片将被自动编译成二进制类型。