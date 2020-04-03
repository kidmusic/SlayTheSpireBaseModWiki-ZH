# 配置Eclipse

## 下载 

1. Eclipse： [link](https://www.eclipse.org/downloads/download.php?file=/oomph/epp/oxygen/R2/eclipse-inst-win64.exe)
2. Maven: [link](https://maven.apache.org/download.cgi)

## 新建项目

如果这是你第一次使用Eclipse，它将会让你选择一个 *Workspace* 目录。不要使用默认的 *Workspace* 目录。我们应该使用 `my_mods` 文件夹作为作为workspace。如果你没有这么做，或者之前你使用过Eclipse，请把你的workspace切换到 `my_mods`。图中黑色部分是一些被遮挡的个人信息。 ![Switch Workspace](https://raw.githubusercontent.com/daviscook477/BaseMod/master/github_resources/wiki/switch_workspace.png) ![New Workspace](https://raw.githubusercontent.com/daviscook477/BaseMod/master/github_resources/wiki/new_workspace.png)

点击 `File` -> `New` -> `Java Project`. ![New Java Project](https://raw.githubusercontent.com/daviscook477/BaseMod/master/github_resources/wiki/new_java_project.png) 在弹出的界面设置项目名字为 `example_mod` 并点击 `Finish`. ![New Java Project Modal](https://raw.githubusercontent.com/daviscook477/BaseMod/master/github_resources/wiki/new_java_project_modal.png)接下来我们设置**依赖**。还记得我们把 `ModTheSpire.jar` 和 `BaseMod.jar` 放到 `lib` 文件夹了吗？接下来我们要告诉Eclipse它们在这里，因为Eclipse并不会自动寻找它们。

## 依赖
将 `desktop-1.0.jar` 也放入 `lib` ，它位于 **Slay The Spire** 的文件夹中。最后结果如下： ![Final Lib Folder](https://raw.githubusercontent.com/daviscook477/BaseMod/master/github_resources/wiki/final_lib_folder.png)

右键 `example_mod` 点击 `Properties` ![Properties](https://raw.githubusercontent.com/daviscook477/BaseMod/master/github_resources/wiki/mod_properties.png).

点击 `Java Build Path` -> `Libraries`. ![Build Path](https://raw.githubusercontent.com/daviscook477/BaseMod/master/github_resources/wiki/build_path.png)

点击 `Add External JARs...` 添加 `ModTheSpire.jar`, `BaseMod.jar`和 `desktop-1.0.jar`。点击OK，像这样： ![All Jars](https://raw.githubusercontent.com/daviscook477/BaseMod/master/github_resources/wiki/all_jars.png).

## Maven
Slay The Spire的mod大多使用 [Maven](https://maven.apache.org/) 构建。如果你没有安装Maven，请先安装再做后续步骤。Maven 需要设置你的mod的文件路径。将你所有的源代码放入 `src/main/java` 文件夹。按下图设置： ![Properties Again](https://raw.githubusercontent.com/daviscook477/BaseMod/master/github_resources/wiki/mod_properties.png)

点击 `Java Build Path` -> `Source` 选中 `example_mod/src` 点击 `Remove` 。点击 `Add Folder` 添加 `src/main/java` 作为资源文件夹。点击ok，像这样： ![New Source Folder](https://raw.githubusercontent.com/daviscook477/BaseMod/master/github_resources/wiki/new_source_folder.png)

你需要 `pom.xml` 作为你的 Maven 的配置文件。你可以在此查看示例 [gist](https://gist.github.com/alexdriedger/fb74397086ee80073417f19d6305bb05) 。

确保你的依赖指向了jar包正确的位置。 `${basedir}` 是你的 `pom.xml` 文件所在的位置。 `..` 是父目录。

```Xml
<dependency>
    <groupId>basemod</groupId>
    <artifactId>basemod</artifactId>
    <version>2.10.0</version>
    <scope>system</scope>
    <systemPath>${basedir}/../lib/BaseMod.jar</systemPath> // systemPath should be the path to where BaseMod.jar is located
</dependency>
```

使用 **`mvn package`** 来构建你的mod。Maven将会生成一个jar包。其路径由你在 `pom.xml` 文件中配置。

```Xml
<target>
    <copy file="target/ExampleMod.jar" tofile="../lib/ExampleMod.jar"/> // tofile location is where you can find your compiled .jar file
</target>
```

## 开始制作mod吧！

下面我们开始制作mod吧:

* go！: [Link](./Starting-Your-Mod)
