# 解包

可以像下面这样使用`d`或者`decode`解包apk文件

```
$ apktool d foo.jar
# decodes foo.jar to foo.jar.out folder

$ apktool decode foo.jar
# decodes foo.jar to foo.jar.out folder

$ apktool d bar.apk
# decodes bar.apk to bar folder

$ apktool decode bar.apk
# decodes bar.apk to bar folder

$ apktool d bar.apk -o baz
# decodes bar.apk to baz folder
```