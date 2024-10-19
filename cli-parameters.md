# 命令行参数

## Utility Options

utility 命令使用的可选项 (`apktool {options}`)

```
-advance, --advanced
```
- 输出高级用法

```
-version, --version
```
- 输出当前软件版本. (Ex: 1.5.2)

## Decoding Options

decoding 命令使用的可选项 (`apktool d file.apk {options}`)

```
-api, --api-level <API>
```
- 设置生成smali文件时使用的API-level
  - 默认值为 `targetSdkVersion`

```
-b, --no-debug--info
```
- 阻止 baksmali 输出调试信息 (`.local`, `.param`, `.line`, etc.).

```
-f, --force
```
- 强制删除目标目录

```
--force-manifest
```
- 强制Apktool解码 `AndroidManifest.xml` 无论你如何设置解码命令可选项

```
--keep-broken-res
```
- 抛出错误或者一些资源被删除时仍然执行解码
  - E.g. `Invalid config flags detected. Dropping resources`.
  - 你需要在构建之前手动修复它们

```
-m, --match-original
```
- 是生成的文件尽可能匹配源文件
  - 以可能阻止重建为代价

```
--no-assets
```
- 阻止 解码/复制 未知的资产文件

```
--only-main-classes
```
- 仅反汇编位于应用根目录中的dex classes

```
-p, --frame-path <DIR>
```
- 设置framework文件保存或读取的位置

```
-r, --no-res
```
- 阻止解码资源文件
  - 保持 `resources.arsc` 完好

```
-resm, --resource-mode <mode>
```
- 设置解析资源时使用的模式
  - `remove` (default) - 移除无法被解析的资源
  - `dummy` - 为无法解析的资源添加虚拟资源
  - `keep` - 使用 `APKTOOL_MISSING_{resId}` 命名法保留为解析的资源

```
-s, --no-src
```
- 阻止反汇编dex文件
  - 保持dex文件不变并在构建是移动他们

```
-t, --frame-tag <TAG>
```
- 使用被`TAG`标记的framework文件

## Building Options

构建命令使用的可选项 (`apktool b folder {options}`)

```
-a, --aapt <FILE>
```
- 从特定的位置加载aapt/aapt2二进制文件
  - 使用它们代替内部版本

```
-api, --api-level <API>
```
- 设置构建smali文件时使用的API-level
  - 默认值为 `targetSdkVersion`

```
-c, --copy-original
```
- 复制原本的 `AndroidManifest.xml` 和 `META-INF`.

```
-d, --debug
```
- 添加 `debuggable="true"` 到 `AndroidManifest.xml` 中

```
-f, --force-all
```
- 在构建期间覆盖现有文件
 - 包括资源文件和源代码文件

```
-n, --net-sec-conf
```
- 在apk中添加通用网络安全配置文件

```
-na, --no-apk
```
- 禁止将构建的文件重新打包到新的apk中

```
-nc, --no-crunch
```
- 构建期间禁止处理资源文件
  - 这将阻止aapt/aapt2进行自动bitmap优化

```
-o, --output <FILE>
```
- 设置输出的apk的名称
  - 默认为 `dist/{apkname}.apk`.

```
-p, --frame-path <DIR>
```
- 设置framework文件保存或读取的位置

```
--use-aapt1
```
- 使用`aapt`替代`aapt2`
  - Apktool v2.9.0 及以上的版本默认使用 `aapt2`

```
--use-aapt2
```
- 使用`aapt2`替代`aapt`
  - Apktool v2.8.2及以下的版本默认使用 `aapt`

## Empty Framework Directory Options
empty-framework-dir 命令使用的可选项 (`apktool empty-framework-dir {options}`)

```
-f, --force
```
- 强制删除目标目录

```
-p, --frame-path <DIR>
```
- 设置framework文件保存或读取的位置

## List Framework Directory Options

list-frameworks命令的可选项 (`apktool list-frameworks {options}`)

```
-p, --frame-path <DIR>
```
- 设置framework文件保存或读取的位置

## Common Options

任何命令都可以使用的可选项

```
-v, --verbose
```
- 详细的输出
  - 包含任何被标记为 `FINE` 的日志消息

```
-q, --quiet
```
- 不输出消息到终端