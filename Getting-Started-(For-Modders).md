# 开始为SlayTheSpire制作Mod吧！
注意这篇向导文章是针对 **Windows** 用户的。如果你是用的是OS X或者Linux，大部分的内容是适用的，除了一些文件路径等可能会有出入。

**Slay The Spire** 是用**java**写的。所以需要一些 **Java** 编程经验或者愿意去学习它。

## Mod制作工具
想要为 **Slay The Spire** 制作Mod，你需要下载**ModTheSpire**（请前往Steam创意工坊下载）。你还需要**BaseMod**（请前往Steam创意工坊下载）来为你提供编辑Mod的API入口。

你还需要安装 [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 。请**不要**使用其他版本的Java！

## 文件夹配置
在本教程中，我们将会使用 `my_mods` 这个文件夹，于 `Documents` 文件夹创建它。 `my_mods` 文件夹将会用来存放你所有的mod。在本例中我们将会在 ``my_mods``文件夹中创建`example_mod` 文件夹用来存放本mod的所有内容。![Directory Structure](https://raw.githubusercontent.com/daviscook477/BaseMod/master/github_resources/wiki/folder_setup.png)现在，在 `my_mods` 文件夹中创建 `lib` 文件夹（`lib` 是 `libraries`的缩写），该文件夹是用来存放mod的依赖库的。将 `ModTheSpire.jar` 和 `BaseMod.jar` 放入 `lib` 文件夹中。

![LibFolder](https://raw.githubusercontent.com/daviscook477/BaseMod/master/github_resources/wiki/lib_folder.png)

## 环境配置

这里有两个IDE可以用来制作Mod。请使用**IntelliJ**。你可以使用Eclipse，但是它非常不方便。

* [安装IntelliJ](./IntelliJ-Environment-Setup)
* [安装Eclipse](./Eclipse-Environment-Setup)

如果你已经安装好了IDE，你可以从这里直接开始制作Mod：

* [开始制作Mod](./Starting-Your-Mod)

## 反编译游戏
从这篇[教程](./Decompiling-Your-Game)中学习如何反编译Slay The Spire以便于查看游戏的源码。

## 问题解决
如果你有问题，参考[问题解决](./Troubleshooting)页面。