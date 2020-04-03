# 配置IntelliJ

下载地址： [here](https://www.jetbrains.com/idea/download/)

## 创建新的项目

如果你第一次使用IntelliJ，你将会面临最重要的选择——选择你的IDE主题：

![](https://cdn.discordapp.com/attachments/478705112773034004/482040748037111818/unknown.png)

建议使用黑色，因为白色非常伤眼。

接下来点击 `Create New Project`

![](https://i.imgur.com/NxYFb6D.png)

左侧选择Maven并设置JDK为Java 1.8.

![](https://i.imgur.com/mZDhWhS.png)

设置你想要的设置的ID

![](https://i.imgur.com/dWS14vM.png)

保存到 `my_mods` 文件夹

![](https://i.imgur.com/vdMDwW8.png)

他将会打开新生成的 `pom.xml` 文件，将示例文件拷贝过来，然后在右下角点击Import Changes。

示例pom.xml: [Link](https://gist.github.com/alexdriedger/fb74397086ee80073417f19d6305bb05)

![](https://cdn.discordapp.com/attachments/398373038732738570/538281373019013130/unknown.png)

# 依赖

复制 `ModTheSpire.jar` 和 `BaseMod.jar` 到 `lib` 文件夹。把Slay The Spire文件夹的 `desktop-1.0.jar` 也拷过来。

点击Shift+S打开Project Structure并点击Modules。确保你的所有依赖都在并点击 `Apply` 。

![](https://i.imgur.com/zcsFzzJ.png)

你所有的代码会被放入 `your_mod\src\main\java` 。你可以右键点击目录，选择 `New > Class` 。也可以创建文件夹，使用： `New > Directory` 。

# 构建

点击 `View > Tool Windows > Maven Projects` ：

![](https://i.imgur.com/7aaKFdc.png)

选择 `Lifecycle` 并双击 `package`

![](https://cdn.discordapp.com/attachments/398373038732738570/485632315901345802/idea64_2018-09-01_19-08-21.png)

接下来会生成jar包并放入你在 `pom.xml` 配置好的位置

```xml
<target>
    <copy file="target/ExampleMod.jar" tofile="../lib/ExampleMod.jar"/> // tofile location is where you can find your compiled .jar file
</target>
```
## 开始制作mod吧！

下面我们开始制作mod吧:

* go！: [Link](./Starting-Your-Mod)

