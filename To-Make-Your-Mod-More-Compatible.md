很多人都安装了多个mod，因此最好让你的mod与他人的兼容。但由于大多情况下你仅在启动你的mod的情况下测试，因此很难考虑这些问题。

本指南旨在编写有关兼容性的大多数简易。一般遵循本指南就足够了，不必担心兼容性问题。


## TL;DR

如果你是用 [StS-Default Mod Base](https://github.com/Gremious/StS-DefaultModBase) 作为你的基础代码，那么你可以把 `theDefault` 的每个实例替换成你mod的名字。如果你不使用它，参考 [Mystic](https://github.com/JohnnyDevo/The-Mystic-Project)的代码。


## 防止重复的ID/路径

### 为你的ID打上前缀

每个卡牌，能力或者遗物都有唯一的ID，它在代码中这样定义：
`public static final String ID = "Strike_Default"`
Slay the Spire使用这个ID将他们区分开来，虽然你不会使用相同的ID，但是不可避免的是和他人的mod发生冲突。

为了解决它，用 `你的mod名` 为这些ID打上前缀，像这样： `yourmodname:strike_default`

### 把你的资源文件放在以mod名命名的子目录中

理由同上，所以你可以做如下改动： 

 `resources/localization/relics.json` =>`resources/yourmodname/localization/relics.json` 。

这些问题在过去的mod中很常见([Example 1](https://github.com/daviscook477/BaseMod/issues/151), [Example 2](https://github.com/kiooeht/ModTheSpire/issues/121))如果你想查看更好的代码实例，可以参考 [StS-Default Mod Base](https://github.com/Gremious/StS-DefaultModBase), [Hubris](https://github.com/kiooeht/Hubris) 或者 [Mystic](https://github.com/JohnnyDevo/The-Mystic-Project).


## 补丁

### 除非特殊情况，否则<u>你绝不应</u>该替换补丁文件

如果你替换了补丁文件，那么其他试图替换补丁的mod将无法运作。


## 与其他流行的mod兼容

### 使用 `BaseMod.MAX_HAND_SIZE` 扩展手牌上限

在Slay the Spire中，最大手牌上限为10，但是很多mod都修改了它，所以你要对他进行确认。

### 确保你的遗物不会再其他角色上崩溃

基础游戏不允许获得其他角色的遗物，所以不必担心他会不适配，但是来自 [InfiniteSpire](https://github.com/GraysonnG/InfiniteSpire) 的”发光棱镜“事件以及来自[Hubris](https://github.com/kiooeht/Hubris) 的Backtick遗物都让它成为了可能。所以如果一个角色的起始遗物崩溃或者软锁定，那会变得很糟。

注意：这并不意味着你的其实遗物会对其他角色造成影响。这取决于你，但是通常很少启用遗物交换，无论你做了什么迭代，那都是浪费时间的，且很麻烦。

