反编译将允许你查看游戏的源代码。如果你仅使用BaseMod来制作Mod，这并不是必要的。但这应对下列情况很有效：

* 了解各种类是如何运作的
* 了解hook在哪，并且是何时被调用的
* 做一些基础游戏没有提供的功能

按照如下方式反编译游戏：

1. 下载 [JD-GUI](http://jd.benow.ca/)。下载1.4左右的版本
2. 启动 `jd-gui.exe` ，将jar导入，他在你的 **Slay The Spire** 文件夹 ( `Program Files/Steam/steamapps/common`)  ![Open File Image](https://raw.githubusercontent.com/daviscook477/BaseMod/master/github_resources/wiki/decompile_open.png)
3. 它将会打开一个新窗口以便与你查看源码
4. 点击 *File* -> *Save All Sources* ，存储为 `desktop-1.0.jar.src.zip` 。这将会导出一个 `zip` 文件，你只有解压才能访问它。（注意，这并不是实际的代码，而是反编译出的代码）![Save All Sources Image](https://raw.githubusercontent.com/daviscook477/BaseMod/master/github_resources/wiki/decompile_save_all.png)

