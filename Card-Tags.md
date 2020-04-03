卡牌可以被打上任意标签以方便追踪。

**从Slay the Spire10-04-2018版本起，基础游戏就包含了卡牌标签系统。如果你想要使用BaseMod系统，你需要做出转换。旧的BaseMod卡牌标签系统向后兼容，任然适用。**

使用 `card.tags.add(TAG)` 来为卡牌添加标签。

使用 `card.hasTag(TAG)` 来查看卡牌是否带有标签。

## 默认标签
Slay the Spire有一些默认标签：
* `AbstractCard.CardTags.STRIKE`
  * 一个被打上了该标签的卡牌被认为是完美攻击的"攻击"
  * 你的基础攻击如果是完美攻击则应该被同时打上 `BASIC_STRIKE` 和 `STRIKE` 
* `AbstractCard.CardTags.HEALING`
  * 带有此标签的卡不能在战斗中随机生成，目的是防止玩家在战斗中停留而获得治疗效果

BaseMod默认带有一些标签：
* `BaseModCardTags.BASIC_STRIKE`
  * 带有此标签的卡牌被视为基础攻击像是吸血和返璞归真
  * 你的基础攻击应该被同时打上 `BASIC_STRIKE` 和 `STRIKE` 
* `BaseModCardTags.BASIC_DEFEND`
  * 带有此标签的卡被视为基础防御卡如返璞归真
* `BaseModCardTags.FORM`
  * 带有此标签的卡牌被视为形态卡，例如恶魔形态，幽灵形态，等

```Java
public TestStrike()
{
    super(ID, NAME, IMG, COST, DESCRIPTION, CardType.ATTACK, CardColor.RED, CardRarity.RARE, CardTarget.ENEMY);

    tags.add(CardTags.STRIKE);
    tags.add(BaseModCardTags.BASIC_STRIKE);
    ...
}
```

## 自定义标签
你可以这样自定义标签：
```Java
public class CustomTags
{
	@SpireEnum public static AbstractCard.CardTags MY_TAG;
	@SpireEnum public static AbstractCard.CardTags MY_OTHER_TAG;
	@SpireEnum public static AbstractCard.CardTags LOOK_AT_ME;
}
```