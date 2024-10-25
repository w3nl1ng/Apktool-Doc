# 资源模式

Apktool在v2.9.0引入了在反汇编期间决定如何处理未解析的资源的配置

## 背景

你可以使用aapt`aapt d resources app.apk`来查看一个给定应用所使用的资源

```
➜ aapt d resources bugged.apk
Package Groups (1)
Package Group 0 id=0x7f packageCount=1 name=com.ibotpeaches.arsctest
  Package 0 id=0x7f name=com.ibotpeaches.arsctest
    type 0 configCount=1 entryCount=10
      spec resource 0x7f010000 com.ibotpeaches.arsctest:color/black: flags=0x00000000
      spec resource 0x7f010001 com.ibotpeaches.arsctest:color/purple_200: flags=0x00000000
      spec resource 0x7f010002 com.ibotpeaches.arsctest:color/purple_500: flags=0x00000000
      spec resource 0x7f010003 com.ibotpeaches.arsctest:color/purple_700: flags=0x00000000
      spec resource 0x7f010004 com.ibotpeaches.arsctest:color/teal_200: flags=0x00000000
      spec resource 0x7f010005 com.ibotpeaches.arsctest:color/teal_700: flags=0x00000000
      spec resource 0x7f010006 com.ibotpeaches.arsctest:color/white: flags=0x00000000
      INVALID TYPE CONFIG FOR RESOURCE 0x7f010007
      INVALID TYPE CONFIG FOR RESOURCE 0x7f010008
      spec resource 0x7f010009 com.ibotpeaches.arsctest:color/bugged: flags=0x00000000
```

在这个例子中你可以看到下面这两个资源解析时存在问题：

- `0x7f010007`
- `0x7f010008`

但问题是，用户最终看到的报错消息只是真正错误的包装，真正引起错误的可能是以下原因之一：

- 找不到资源包
- 资源没有类型
- 非法的资源名称标识
- 非法的UTF-8字符串下标
- 非法的UTF-16字符串下标
- 找不到资源入口

所以基本上很难了解错误发生的确切原因

## 历史做法

在Apktool能够保证资源ID静态之前，它会为这些无法解析的资源创建虚拟资源

这些虚拟资源看起来像这样：

``` xml
<item type="color" name="APKTOOL_DUMMY_7f010007" />
<item type="color" name="APKTOOL_DUMMY_7f010008" />
```

所以在资源表建立起来后，资源的ID不会前移，因为虚拟资源填补了那些无法正确解析的资源的位置

随着时间的推移，这种做法没有其必要了，因为资源ID保持静态的代价是将资源设置为公开属性。时至今日，Android系统的安全性仍在不断提高，这一点仍在困扰我们。

此外，这也成为了一个问题，因为Apktool会在无意中使用虚拟资源对应用程序进行指纹识别。在2019年，一家大公司使用Apktool重命名它们的包并重新发布到商店时，这个问题暴露出来了。这篇文章可以在[这里](https://connortumbleson.com/2019/06/02/apktool-in-the-wild/)查看

因此，在支持稀疏资源的过程中，虚拟资源被无意间劈坏了，但人们并没有注意到这一点。这是一件好事，但也导致了一些以前处理过的奇怪问题。

下面举个例子：

```xml
<attr name="actionBarSize" format="dimension">
    <enum name="@null" value="0" />
</attr>
```

这展示了v2.8.x的Apktool如何反汇编未知资源。这是不恰当的因为未被解析的资源的名称不能为`@null`

但是，使用虚拟资源替换它会变得更好吗？

```xml
<attr name="actionBarSize" formats="dimension">
    <enum name="APKTOOL_DUMMY_0x7f090459" value="0" />
</attr>
```

你可以说它更好了，但是如果你正在解析一个没有引用此属性的资源，使用一个虚拟资源来填充它看起来就很奇怪

因此，一种看起来更好的做法是移除未解析的资源

```xml
<attr name="actionBarSize" formats="dimension"/>
```

这里面临的挑战是，每种使用情况都可能偏好不同的选项。

## 不同的模式

解析过程中使用 `-resm <mode>`来设置Apktool处理未解析资源的模式

### Remove (default)

- `remove` | `delete`

Apktool将移除任何为解析的资源，这也是默认做法。

```xml
<attr name="actionBarSize" formats="dimension"/>
```

### Dummy

- `dummy` | `dummies`

Apktool将使用虚拟资源填充为解析的资源

```xml
<attr name="actionBarSize" formats="dimension">
    <enum name="APKTOOL_DUMMY_0x7f090459" value="0" />
</attr>
```

### Keep

- `keep` | `preserve`

Apktool将保留资源，但是如果资源名称被剥离了，将适当生成有效的名称

```xml
<attr name="actionBarSize" format="dimension">
    <enum name="resUnk0x7f090459" value="0" />
</attr>
```