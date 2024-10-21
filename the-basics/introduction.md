## 导言

首先让我们认识以下apk文件，Apk文件就是一个包含资源和编译好的java代码的压缩包文件，如果你像下面这样解压缩apk文件，你会得到一些类似`class.dex`和`resources.arsc`的文件

```
$ unzip testapp.apk
Archive:  testapp.apk
inflating: AndroidManifest.xml
inflating: classes.dex
extracting: res/drawable-hdpi/ic_launcher.png
inflating: res/xml/literals.xml
inflating: res/xml/references.xml
extracting: resources.arsc
```

但是现在，你得到的是编译好的文件。如果你尝试查看`AndroidManifest.xml`。你会看到这样的内容。

```
�����������P��������������4���F���������������������0��\��f��n���v�e�r�s�i�o�n�C�o�d�e����v�e�r�s�i�o�n�N�a�m�e����a�n�d�r�o�i�d���*�h�t�t�p�:�/�/�s�c�h�e�m�a�s�.�a�n�d�r�o�i�d�.�c�o�m�/�a�p�k�/�r�e�s�/�a�n�d�r�o�i�d��������p�a�c�k�a�g�e����p�l�a�t�f�o�r�m�B�u�i�l�d�V�e�r�s�i�o�n�C�o�d�e����p�l�a�t�f�o�r�m�B�u�i�l�d�V�e�r�s�i�o�n�N�a�m�e����m�a�n�i�f�e�s�t����b�r�u�t�.�a�p�k�t�o�o�l�.�t�e�s�t�a�p�p����1�.�0����2�1����A�P�K�T�O�O�L����������������������������������������������������������������������������
```

显然，编辑或者查看编译好的文件几乎不可能，现在就是Apktool发挥作用的时候了。

```
$ apktool d testapp.apk
I: Using Apktool 2.0.0 on testapp.apk
I: Loading resource table...
I: Decoding AndroidManifest.xml with resources...
I: Loading resource table from file: 1.apk
I: Regular manifest package...
I: Decoding file-resources...
I: Decoding values */* XMLs...
I: Baksmaling classes.dex...
I: Copying assets and libs...
```

重新查看`AndroidManifest.xml`文件，你会得到一些人类可读的内容

```
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<manifest xmlns:android="https://schemas.android.com/apk/res/android" package="brut.apktool.testapp"
          platformBuildVersionCode="21" platformBuildVersionName="APKTOOL" />
```

除了xml以外，Apktool还可以正确解码9patch图片，布局，字符串等资源