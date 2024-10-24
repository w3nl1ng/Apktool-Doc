# Building

可以向下面这样使用`b`或者`build`来重打包apk

```
$ apktool b foo.jar.out
# builds foo.jar.out folder into foo.jar.out/dist/foo.jar

$ apktool build foo.jar.out
# builds foo.jar.out folder into foo.jar.out/dist/foo.jar

$ apktool b bar
# builds bar folder into bar/dist/bar.apk

$ apktool b .
# builds current directory into ./dist

$ apktool b bar -o new_bar.apk
# builds bar folder into new_bar.apk

$ apktool b bar.apk
# WRONG: brut.androlib.AndrolibException: brut.directory.PathNotExist: apktool.yml
# Must use folder, not apk/jar file
```

{% hint style="info" %}
为了运行重打包之后的apk，你必须重新签名它。Android[文档](https://developer.android.com/tools/publishing/app-signing.html#signing-manually)可以帮到你
{% endhint %}