# resources.arsc文件

Android应用中有一个`resources.arsc`，这是一个包含应用中资源的二进制文件。`.arsc`后缀表示`Android Resource Storage Container`，这是一个庞大的块集合。

如果我们dump这个文件的第一行，我们会看到：

```
00000000: 0200 0c00 f00b 0000 0100 0000 0100 1c00  ................
```

`.arsc`文件使用被称为`ResChunk_header`的结构来描述文件中的块。这个结构是文件的开头，它的具体结构如下：

**ResChunk_header**

| 名称 | 大小 | 藐视 |
| :-:  |:---:| :--: |
|Type  |2字节 |块的类型|
|Header Size|2字节|头部的大小|
|Size  |4字节  |块的大小|

已知字节序是小端序，我们可以解析这个header得到：

- 类型 - 0x0002
- 头部大小 - 0x000c
- 块大小 - 0x00000bf0

我们可以使用[ResourceTypes.h](https://github.com/iBotPeaches/platform_frameworks_base/blob/master/libs/androidfw/include/androidfw/ResourceTypes.h)文件中的定义来理解这个块的类型

```c
enum {
    RES_NULL_TYPE                     = 0x0000,
    RES_STRING_POOL_TYPE              = 0x0001,
    RES_TABLE_TYPE                    = 0x0002,
    RES_XML_TYPE                      = 0x0003,
};
```

文件以一个`RES_TABLE_TYPE`块开始，这个块的结构如下：

**ResTable_header**

|名称|大小|描述|
|:-|:-:|:-|
|Header|8字节|块的类型|
|Package Count|4字节|包的数量|

一个非常简单的结构，作用只是帮助理解文件中有多少个`ResTable_package`块。我们知道每个块都以一个`ResChunk_header`开头后，就可以安全地读取下一个块来查看它的内容了。

现在在原本的行上标记我们已经读取过的内容

```
00000000: [0200 0c00 f00b 0000] 0100 0000 0100 1c00  ................
```

读取`ResChunk_header`的类型后可以知道下一个块是一个`RES_STRING_POOL_TYPE`块。它的详细结构如下：

|名称|大小|描述|
|:-|:-|:-|
|String Count|4字节|池中字符串的数量|
|Style Count|4字节|池中style span数组的数量|
|Flags|4字节|字符串编码是UTF-8还是UTF-16,是否排序|
|String Offset|4字节|头部到字符串数据的字节偏移|
|Style Offset|4字节|头部到样式数据的字节偏移|

这个块比之前的块复杂一点，但是我们仍然可以解析它。我们可以知道文件中包含多少字符串和样式以及它们的起始位置。因此，在这种情况下，Apktool可以读取数据池，并将它们存储到内存中。这允许我们在需要时通过索引来引用它们。

字符串池存在很多怪癖和奇特之处（LEB128&&MUTF），本文中将不加以介绍。如果想你想要深入了解字符串池如何工作，这篇字符串文档（TODO）会帮到你。现在只需要假设你可以通过索引引用到字符串池中的字符串。

首先，读取字符串池十分重要，因为字符串池中包含了整个资源表中的字符串。不过，除了条目名称和类型标识符之外，其他字符串不包含在该池中。

现在假设我们从`ResChunk_header `包含的信息中正确读取了字符串池或者跳过了这一步，我们来到了另一个新的块。以下是字符串池之后的相关部分。

```
00000340: 0000 0000] 0002 2001 ac08 0000 7f00 0000  ...... .........
00000350: 6300 6f00 6d00 2e00 6900 6200 6f00 7400  c.o.m...i.b.o.t.
00000360: 7000 6500 6100 6300 6800 6500 7300 2e00  p.e.a.c.h.e.s...
00000370: 6100 7200 7300 6300 7400 6500 7300 7400  a.r.s.c.t.e.s.t.
```

- `]` - 字符串池的结尾

我们读取这个块的`ResChunk_header`并得到了一个新类型的块，这个块的类型是`RES_TABLE_TYPE`，它的详细结构如下：

|名称|大小|描述|
|:-|:-|:-|
|Package Id|4字节|包的ID|
|Package Name|可变长度|包的名称，以空字符结尾|
|Type String Offset|4字节|类型符号表的偏移，如果继承自其他包则为0|
|Last Public Type String Offset|4字节|上一个表中最后一个公共类型的索引|
|Key Symbol Offset|4字节|关键符号表的偏移，如果继承自其他包则为0|
|Last Public Key Symbol Offset|4字节|上一个表中最有一个关键符号的索引|
|Type Id Offset|4字节|分块应用程序特定类型 ID 的偏移量|

从属性和解释中可以看出，这个块将启动子块`ResTable_type`和`ResTable_typeSpec`