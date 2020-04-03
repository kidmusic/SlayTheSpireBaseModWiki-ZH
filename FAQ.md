# 我应该如何让我的mod使用BaseMod？

在这里，我假设你是用的是Eclipse，但此方法同样适用于其他的IDE。在你安装了BaseMod后，请将 `BaseMod.jar` 和 `ModTheSpire.jar` 都复制到你的项目的Java Build Path中。这将使项目找到并使用这些代码。此外，你还可以把 **SlayTheSpire** 的jar包放到Java Build Path。你可以在 **SlayTheSpire** 文件找到名为 `desktop-1.0.jar` （目前）的jar包。

# BaseMod 和 ModTheSpire 有何不同？应该如何选择？

你应该同时使用！ **BaseMod** 是基于 **ModTheSpire** 提供的功能构建的。**ModTheSpire**允许我们修改游戏并注入游戏补丁。 **BaseMod** 是为了简化该过程而提供的API接口，这样就不用每个mod都去制作一个新的补丁，它可以节约成本并实现更好的代码重用。