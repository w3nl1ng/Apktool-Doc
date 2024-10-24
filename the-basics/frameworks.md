# Frameworks

可以使用`if`或者`install-framework`安装Framework，它有两个参数

- -p, --frame-path <dir> - 保存framework文件到`<dir>`目录

- -t, --tag <tag> - 使用`<tag>`标记framework，`<tag>`还会成为framework文件名的后缀

这样可以更好地控制文件的存储和命名方式

```
$ apktool if framework-res.apk
I: Framework installed to: 1.apk
# pkgId of framework-res.apk determines the number. (which is 0x01)

$ apktool if com.htc.resources.apk
I: Framework installed to: 2.apk
# pkgId of com.htc.resources is 0x02

$ apktool if com.htc.resources.apk -t htc
I: Framework installed to: 2-htc.apk
# pkgId-tag.apk

$ apktool if framework-res.apk -p foo/bar
I: Framework installed to: foo/bar/1.apk

$ apktool if framework-res.apk -t baz -p foo/bar
I: Framework installed to: foo/bar/1-baz.apk
```

