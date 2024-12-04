# 第一周作业笔记

## 1.编译源码版UE5.4

注册EPIC账号并连携github账号后将UE5库clone到本地

运行Setup.bat下载依赖文件

在VS2022中编译源码版UE5（生成解决方案）

![image-20241204193421408](C:\Users\34016\AppData\Roaming\Typora\typora-user-images\image-20241204193421408.png)

首次生成解决方案用时6h+（综合电脑配置考虑，此处应有提升解决速度的方法），遇到两类报错，以下为解决方法总结。

类型一：

```
错误形式的警告: 包 "System.Drawing.Common" 4.7.0 具有已知的 严重 严重性漏洞，https://github.com/advisories/GHSA-rxg9-xrhp-64gj
错误形式的警告: 包 "System.Drawing.Common" 4.7.0 具有已知的 严重 严重性漏洞，https://github.com/advisories/GHSA-rxg9-xrhp-64gj
错误形式的警告: 包 "System.Text.RegularExpressions" 4.3.0 具有已知的 高 严重性漏洞，https://github.com/advisories/GHSA-cmhx-cq75-c4mj
(以下略)
```

![image-20241204194451058](C:\Users\34016\AppData\Roaming\Typora\typora-user-images\image-20241204194451058.png)

通过管理NuGet程序包并更新对应的包可以解决一部分问题，但有一部分程序包无法更新，所以选择在报错的代码中添加`<WarningsNotAsErrors>NU1901,NU1902,NU1903,NU1904</WarningsNotAsErrors>`忽略高风险问题。

类型二：

![4b5c03177982ad9d06699a839502210](C:\Users\34016\Documents\WeChat Files\wxid_tbwttgt4adi812\FileStorage\Temp\4b5c03177982ad9d06699a839502210.png)

通过求助同学和在UE官方论坛上查询资料，发现是 Windows SDK 版本过高导致INF常量不兼容从而导致溢出，根据官方论坛所述，可以通过修改源码中引用无限大常量的代码或修改 Windows SDK 版本来解决该报错，此处选择后者。

在下载 10.0.18362.0 版本的 Windows SDK 并使用后顺利解决了该报错。

解决以上两个报错后编译成功，能顺利运行UE5

![image-20241204200222296](C:\Users\34016\AppData\Roaming\Typora\typora-user-images\image-20241204200222296.png)

![image-20241204200449552](C:\Users\34016\AppData\Roaming\Typora\typora-user-images\image-20241204200449552.png)

![image-20241204200251854](C:\Users\34016\AppData\Roaming\Typora\typora-user-images\image-20241204200251854.png)

## 2.编译并构建安装包，并在 Android 系统手机上运行

参考UE官方文档安装 Android Studio 及其对应的 SDK、NDK 并尝试将 First Person Template 打包为 APK 传输至 Android 系统手机。

打包过程中遇到以一下报错为首的一系列错误，

```
UATHelper: Packaging (Android (ASTC)): java.lang.NoClassDefFoundError: Could not initialize class org.codehaus.groovy.vmplugin.v7.Java7
```

通过查阅官方文档和咨询 ChatGPT，怀疑是由于JAVA版本不兼容所致。

在“编辑——项目设置——平台——Android SDK”菜单中可以调整使用的版本。

首先尝试了现有的所有版本 jdk-21、jdk-23 ，均不奏效。

下载 jdk-11 并做尝试，成功打包APK。

![image-20241204201823451](C:\Users\34016\AppData\Roaming\Typora\typora-user-images\image-20241204201823451.png)

（也可以通过修改系统环境变量来设置版本，考虑到可能的其余 JAVA 使用场景故没有选择修改环境变量）

![image-20241204202034989](C:\Users\34016\AppData\Roaming\Typora\typora-user-images\image-20241204202034989.png)

（此处有一个失误是没有单独创建一个 AndroidBuilds 文件夹，必可活用于下次）

将手机设为开发者模式，顺利安装游戏程序并能在手机上运行。

![e9d3a003613089910d10cbfddf28d8c](C:\Users\34016\Documents\WeChat Files\wxid_tbwttgt4adi812\FileStorage\Temp\e9d3a003613089910d10cbfddf28d8c.jpg)

![9d1fd11c13a807155c7ada515bc6b5d](C:\Users\34016\Documents\WeChat Files\wxid_tbwttgt4adi812\FileStorage\Temp\9d1fd11c13a807155c7ada515bc6b5d.jpg)