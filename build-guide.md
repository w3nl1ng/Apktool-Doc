# Build Apktool

Apktool包含以下几个子项目

- brut.apktool.lib \- Main Library
- brut.apktool.cli \- CLI Interface
- brut.j.dir \- Utility
- brut.j.util \- Utility
- brut.j.common \- Utility

## Requirements

- [JDK](https://www.oracle.com/java/technologies/downloads/) > 11
- [git](https://git-scm.com/downloads)
- [Gradle](https://gradle.org/)

## 构建步骤

首先使用git克隆仓库 https://github.com/iBotPeaches/Apktool.git

1. 导航到Apktool文件夹，使用`./gradlew`(Unix-like系统)或者`gradlew.bat`(windows系统)

2. 使用一下两种方式构建

**不使用R8 (JDK > 8)**

```
[./gradlew][gradlew.bat] build shadowJar
```

这会生成一个jar文件：`brut.apktool/apktool-cli/build/libs/apktool-cli-all.jar`

**使用R8 (JDK > 11)**

```
[./gradlew][gradlew.bat] build shadowJar proguard
```

这会生成一个jar文件：`brut.apktool/apktool-cli/build/libs/apktool-vXXX.jar`

**windows Requirements**

windows有文件路径长度的限制，Apktool项目中有一个路径的长度来到了218个字符，由于windows最大256个字符的限制，你需要满足如下要求：

克隆Apktool到一个目录时，这个目录的路径的长度不超过37个字符，举个例子，下面这个路径是合法的

```
C:/Users/Connor/Desktop/Apktool
```

它的路径长度是31个字符，在windows下，不要将Apktool克隆到一个路径长度超过37个字符的目录