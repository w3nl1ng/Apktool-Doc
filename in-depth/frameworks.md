# Frameworks

你大概知道，Android应用使用Android系统自身的代码和资源。这些代码和资源就是Android系统的framework，Apktool依赖framework来正确地解包和打包apk。

每个Apktool发行版都会内置发行时最新的AOSP framework。这使得你正确地解包和打包大部分apk。但是除了AOSP framework，制造商还会添加他们自己的framework。为了是Apktool正确处理这些apk，你需要首先安装制造商的framework文件。

举例：假设你想解包一个HTC设备上的`HtcContacts.apk`，使用Apktool你会得到一个错误信息。

```
$ apktool d HtcContacts.apk
I: Loading resource table...
I: Decoding resources...
I: Loading resource table from file: 1.apk
W: Could not decode attr value, using undecoded value instead: ns=android, name=drawable
W: Could not decode attr value, using undecoded value instead: ns=android, name=icon
Can't find framework resources for package of id: 2. You must install proper framework files, see project website for more info.
```

解包这个apk之前，我们必须先安装HTC的framework文件。我们从HTC设备中提取`com.htc.resources.apk`并安装它

```
$ apktool if com.htc.resources.apk
I: Framework installed to: 2.apk
```

接着再次尝试解包

```
$ apktool d HtcContacts.apk
I: Loading resource table...
I: Decoding resources...
I: Loading resource table from file: /home/brutall/apktool/framework/1.apk
I: Loading resource table from file: /home/brutall/apktool/framework/2.apk
I: Copying assets and libs...
```

如你所见，Apktool同时使用`1.apk`和`2.apk`framework文件来正确解包这个应用

**寻找Frameworks**

大部分情况下，设备上`/system/framework`目录下的任何apk都是一个framework文件。在一些设备上，framework文件可能在`/data/system-framework`目录甚至隐藏在`/system/app`或者`/system/priv-app`目录下。它们通常被命名为"resources", "res"或者"framework"。

{% hint style="info" %}
HTC的framework文件叫`com.htc.resources.apk`，LG的framework文件则叫`lge-res.apk`
{% endhint %}

找到framework文件之后，使用adb或者文件管理器将framework文件拉取到本地。当framework文件存在本地后，你需要注意Apktool如何安装它。安装过程中framework文件的命名与对应apk的pkgId相关，这个值的范围应该是1~30，pkgId为127的apk是内部framework文件

## 内部framework

Apktool附带一个上述的内部framework。这个文件是执行期间从本地复制过来的。

{% hint style="warning" %}
Apktool无法识别内部framework的版本，它默认已有的内部framework是最新的，所以更新Apktool之后记得删除之前复制过来的内部framework
{% endhint %}

## 管理framework文件

根据操作系统的不同，framework文件存储在不同的位置

- Linux - `$HOME/.local/share/apktool`
- Windows - `%UserProfile%\AppData\Local\apktool`
- Mac - `$HOME/Library/apktool`

如果这些目录不可访问，Apktool默认使用`java.io.tmpdir`，通常是`/tmp`目录。这是一个不稳定的目录，所以使用`--frame-path`指定一个目录保存framework文件很有必要。

管理framework文件有一定挑战，因为这些目录有时是隐藏目录。`v2.2.1`添加的一个辅助功能允许你运行`apktool empty-framework-dir`来清空framework文件目录。

{% hint style="info" %}
一旦framework被安装，Apktool就无法控制它们的使用，但是你可以自由管理framework对应的apk文件
{% endhint %}

## 标记framework文件

framework文件使用`<id>-<tag>.apk`的命名约定。使用pkgId和自定义的tag来区分它们。通常不需要标记framework文件，但是如果你处理的apk来自不同的设备，它们有互不兼容的framework，你就需要有一种灵活切换framework的方法。

你可以像这样标记framework：

```
$ apktool if com.htc.resources.apk -t hero
I: Framework installed to: /home/brutall/apktool/framework/2-hero.apk
$ apktool if com.htc.resources.apk -t desire
I: Framework installed to: /home/brutall/apktool/framework/2-desire.apk
```

然后这样使用不同的framework：

```
$ apktool d HtcContacts.apk -t hero
I: Loading resource table...
I: Decoding resources...
I: Loading resource table from file: /home/brutall/apktool/framework/1.apk
I: Loading resource table from file: /home/brutall/apktool/framework/2-hero.apk
I: Copying assets and libs...
$ apktool d HtcContacts.apk -t desire
I: Loading resource table...
I: Decoding resources...
I: Loading resource table from file: /home/brutall/apktool/framework/1.apk
I: Loading resource table from file: /home/brutall/apktool/framework/2-desire.apk
I: Copying assets and libs...
```

你不需要在重打包时指定tag，apktool会自动使用解包时使用的tag。