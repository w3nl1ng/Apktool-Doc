# 安装指南

{% hint style="info" %}
最低使用 [Java 8](https://www.java.com/en/download/) 运行Apktool
{% endhint %}

## Windows

{% hint style="info" %}
windows 中大小写不敏感，请调整大小写确保操作正确
{% endhint %}

1. 下载 windows [封装脚本](https://raw.githubusercontent.com/iBotPeaches/Apktool/master/scripts/windows/apktool.bat). (鼠标右键，保存为 `apktool.bat`)

2. 下载 [最新版本](https://bitbucket.org/iBotPeaches/apktool/downloads/) 的Apktool

3. 将下载的apktool_xxxx.jar重命名为 `apktool.jar`

4. 将 `apktool.jar` 和 `apktool.bat` 移动到Windows目录 (通常是 `C://Windows`)

5. 如果你没有权限访问 `C://Windows` ，你可以将上面的两个文件放置到任意可以操作的目录，并将这个目录添加到环境变量

6. 尝试在cmd中运行 `apktool`

## Linux

1. 下载Linux的 [封装脚本](https://raw.githubusercontent.com/iBotPeaches/Apktool/master/scripts/linux/apktool). (鼠标右键，保存为apktool)

2. 下载 [最新版本](https://bitbucket.org/iBotPeaches/apktool/downloads) 的Apktool

3. 将下载的apktool_xxxx.jar重命名为 `apktool.jar`

4. 将 `apktool.jar` 和 `apktool` 移动到 `/usr/local/bin` (需要root)

5. 确保两个文件都有可执行权限. (`chmod +x`)

6. 尝试使用命令行运行 `apktool`

## Mac

1. 下载Mac的 [封装脚本](https://raw.githubusercontent.com/iBotPeaches/Apktool/master/scripts/osx/apktool). (鼠标右键，保存为apktool)

2. 下载 [最新版本](https://bitbucket.org/iBotPeaches/apktool/downloads) 的Apktool

3. 将下载的apktool_xxxx.jar重命名为 `apktool.jar`

4. 将 `apktool.jar` 和 `apktool` 移动到 `/usr/local/bin` (需要root)

5. 确保两个文件都有可执行权限. (`chmod +x`)

6. 尝试使用命令行运行 `apktool`

### Brew

1. 按照 [这个页面](https://brew.sh/) 的描述安装 Homebrew

2. 在终端中执行命令 `brew install apktool`

3. 尝试使用命令行运行 `apktool`

{% hint style="success" %}
封装脚本虽然不是必须的，但是它很有帮助，你不需要再重复地键入 `java -jar apktool.jar` 命令了
{% endhint %}