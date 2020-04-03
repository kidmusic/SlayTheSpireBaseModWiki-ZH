# 介绍

如果你想要为游戏添加新角色，那么你需要为他添加新的颜色。就像战士的红色和静默术士的绿色，你的新角色也需要一种颜色，比如灰色，橙色或者黄色。

# 枚举

基础游戏使用了 `AbstractCard.CardColor` 枚举来定义颜色，所以为了添加新的颜色，你需要向 `ModTheSpire`枚举中添加新的颜色。

**你必须定义CardColor和LibraryType且他们的名字必须相等。**

 像这样：
```java
import com.evacipated.cardcrawl.modthespire.lib.SpireEnum;
import com.megacrit.cardcrawl.cards.AbstractCard;

public class AbstractCardEnum {

	@SpireEnum
	public static AbstractCard.CardColor MOD_NAME_COLOR;
	
}
```
还有这样：
```java
import com.evacipated.cardcrawl.modthespire.lib.SpireEnum;
import com.megacrit.cardcrawl.helpers.CardLibrary;

public class LibraryTypeEnum {

	@SpireEnum
	public static CardLibrary.LibraryType MOD_NAME_COLOR;
	
}


```

# 如何使用
为了添加新的枚举到 `CardColor` 你需要使用BaseMod注册自定义颜色以便于后续的使用。比如，你需要为能量球和卡牌背景指定一个路径。为此，你需要调用 `addColor` 方法。需要**注意**的是 `addColor` 方法在 `@SpireInitializer` 方法中被调用。不要在 `postInitialize` 或其他地方调用它。通常来说你应该在颜色名前加上你的模块名，以避免和别人使用了相同的名字而引发不必要的错误。

# API
```Java
addColor(AbstractCard.CardColor color, Color bgColor, Color backColor, Color frameColor, Color frameOutlineColor, Color descBoxColor, Color trailVfxColor, Color glowColor, String attackBg, String skillBg, String powerBg, String energyOrb, String attackBgPortrait, String skillBgPortrait, String powerBgPortrait, String energyOrbPortrait, String cardEnergyOrb)
```

* `color` - 应该为 `MY_CUSTOM_COLOR`
* `bgColor` - 背景色
* `backColor` - 背面颜色
* `frameColor` - 边框色
* `frameOutlineColor` - 边框外线色
* `descBoxColor` - 描述部分的颜色
* `glowColor` - 发光色
* `attackBg` - 攻击牌背景路径
* `skillBg` - 技能牌背景路径
* `powerBg` - 能量牌背景路径
* `energyOrb` - 能量球背景路径
* `attackBgPortrait` - 卡牌查阅页面的攻击牌背景路径
* `skillBgPortrait` - 卡牌查阅页面的技能牌背景路径
* `powerBgPortrait` - 卡牌查阅页面的能量背景路径
* `energyOrbPortrait` - 卡牌查阅页面的能量球路径
* `cardEnergyOrb` - 卡牌能量球路径

```Java
addColor(AbstractCard.CardColor color, Color everythingColor, String attackBg, String skillBg, String powerBg, String energyOrb, String attackBgPortrait, String skillBgPortrait, String powerBgPortrait, String energyOrbPortrait, String cardEnergyOrb)
```
* `everythingColor` - 是当前版本的颜色属性使用同一种颜色