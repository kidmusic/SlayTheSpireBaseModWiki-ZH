# API
想要添加/移除遗物需要将你的遗物相关代码放到 `EditRelicsSubscriber`  的回调函数 `receiveEditRelics` 中。

* `addRelic(AbstractRelic relic, RelicType type)` - 可以被用于添加战士或者静默术士的以及公共池遗物，**不能**用于添加自定义角色的遗物
* `removeRelic(AbstractRelic relic)` - 移除遗物，同上
* `addRelicToCustomPool(AbstractRelic relic, String color)` - 用于添加自定义角色遗物，**不能**用于添加公共池或者战士和静默术士的遗物 

# 遗物字串
你必须已经设置好了遗物字串（ [参考](./Custom-Strings.md) ），否则游戏将在启动期间崩溃。

# 自定义遗物类
继承 `CustomRelic` 类可以使添加遗物更简单。它会为你处理图像资源的加载（如果你继承了AbstractRelic则需要加载图像资源）。构造函数如下：
* `CustomRelic(String id, Texture texture, RelicTier tier, LandingSound sfx)`

遗物图像资源的大小应为 `128x128px` 且遗物的实际图片仅是其中间的一块大约48到64px的方形。其他部分应该为完全透明。

这里是一个开发者提供的[模板](https://cdn.discordapp.com/attachments/484898639391621143/529958811570798592/unknown.png)，可以对比制作你自己的遗物。

# 举个栗子
这是一个实例遗物，它会在玩家获得它时获得最大生命值：

```Java
public class Blueberries extends CustomRelic {
	public static final String ID = "Blueberries";
	private static final int HP_PER_CARD = 1;
	
	public Blueberries() {
		super(ID, MyMod.getBlueberriesTexture(), // 你也可以在此创建图像资源
				RelicTier.UNCOMMON, LandingSound.MAGICAL); // 这个遗物是稀有的且获得它时会有音效
	}
	
	@Override
	public String getUpdatedDescription() {
		return DESCRIPTIONS[0] + HP_PER_CARD + DESCRIPTIONS[1]; // DESCRIPTIONS从你的本地文件中提取
	}
	
	@Override
	public void onEquip() {
		int count = 0;
		for (AbstractCard c : AbstractDungeon.player.masterDeck.group) {
			if (c.isEthereal) { // 当你装备（获得）这个遗物时计算玩家卡组里有多少虚无卡
				count++;
			}
		}
		AbstractDungeon.player.increaseMaxHp(count * HP_PER_CARD, true);
	}
	
	@Override
	public AbstractRelic makeCopy() { // 总是要复写这个方法，它会返回你的遗物的实例
		return new Blueberries();
	}
	
}
```