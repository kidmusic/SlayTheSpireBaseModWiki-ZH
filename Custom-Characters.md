[参考](./Migrating-to-5.0.md)

# 枚举

基础游戏的角色存放在 `AbstractPlayer.PlayerClass` 的枚举中，所以为了添加新的角色，你需要在 `ModTheSpire`的枚举中注册：

```java
import com.evacipated.cardcrawl.modthespire.lib.SpireEnum;
import com.megacrit.cardcrawl.characters.AbstractPlayer;

public class MyPlayerClassEnum {

	@SpireEnum
	public static AbstractPlayer.PlayerClass MY_PLAYER_CLASS;
	
	
}

```

# 需求
1. （前提条件）已经为角色[注册颜色](./Custom-Colors.md)
2. 定义了一个角色类并拥有CharSelectInfo getLoadout()静态方法
3. 使用BaseMod注册了角色

# API
注意 `addCharacter` 应该只在 `EditCharactersSubscriber`  的回调函数 `receiveEditCharacters` 中调用。

`addCharacter(Class characterClass, String titleString, String classString, String color, String selectText, String selectButton, String portrait, String characterID)`
* `character` - 角色实例
* `color` - 该角色的自定义颜色，例如： `MY_CUSTOM_COLOR.toString()` ， `MY_CUSTOM_COLOR` 是角色颜色的枚举值。
* `selectButtonPath` - 选择按钮的图像路径（以你的jar作为根目录）
* `portraitPath` - 选择界面的背景肖像（以你的jar作为根目录）（size: 1920px x 1200px）
* `characterID` - 应为 `MY_PLAYER_CLASS` ， `MY_PLAYER_CLASS` 是角色类的枚举

# 举个栗子

需要两步来添加自定义角色：定义，注册：

## 定义

```java
import java.util.ArrayList;

import com.badlogic.gdx.math.MathUtils;
import com.esotericsoftware.spine.AnimationState;
import com.megacrit.cardcrawl.actions.utility.ExhaustAllEtherealAction;
import com.megacrit.cardcrawl.characters.AbstractPlayer;
import com.megacrit.cardcrawl.core.EnergyManager;
import com.megacrit.cardcrawl.core.Settings;
import com.megacrit.cardcrawl.dungeons.AbstractDungeon;
import com.megacrit.cardcrawl.powers.AbstractPower;
import com.megacrit.cardcrawl.screens.CharSelectInfo;
import com.megacrit.cardcrawl.unlock.UnlockTracker;

public class MyCharacter extends CustomPlayer {
	public static final int ENERGY_PER_TURN = 3; // 每回合获得费用（能量）数
	public static final String MY_CHARACTER_SHOULDER_2 = "img/char/shoulder2.png"; // 篝火姿势
        public static final String MY_CHARACTER_SHOULDER_1 = "img/char/shoulder1.png"; // 另一个篝火姿势
	public static final String MY_CHARACTER_CORPSE = "img/char/corpse.png"; // 尸体
        public static final String MY_CHARACTER_SKELETON_ATLAS = "img/char/skeleton.atlas"; // 脊椎动画图集
        public static final String MY_CHARACTER_SKELETON_JSON = "img/char/skeleton.json"; // 脊椎动画json

	public MyCharacter (String name) {
		super(name, MyPlayerClassEnum.MY_PLAYER_CLASS);
		
		this.dialogX = (this.drawX + 0.0F * Settings.scale); // 文本气泡的位置
		this.dialogY = (this.drawY + 220.0F * Settings.scale); // 你只需要复制这些值
		
		initializeClass(null, MY_CHARACTER_SHOULDER_2, // 所需加载的视觉图案和能量/过载
				MY_CHARACTER_SHOULDER_1,
				MY_CHARACTER_CORPSE, 
				getLoadout(), 20.0F, -10.0F, 220.0F, 290.0F, new EnergyManager(ENERGY_PER_TURN));
		
		loadAnimation(MY_CHARACTER_SKELETON_ATLAS, MY_CHARACTER_SKELETON_JSON, 1.0F); // 如果您使用的是基础游戏动画的修改版或在spine中制作的动画，请确保定义了这里和下面几行
		
		AnimationState.TrackEntry e = this.state.setAnimation(0, "animation", true);
		e.setTime(e.getEndTime() * MathUtils.random());
	}

	public static ArrayList<String> getStartingDeck() { // 起始卡组
		ArrayList<String> retVal = new ArrayList<>();
		retVal.add("MyCard0");
		retVal.add("MyCard0");
		retVal.add("MyCard0");
		retVal.add("MyCard0");
		retVal.add("MyCard1");
		retVal.add("MyCard1");
		retVal.add("MyCard1");
		retVal.add("MyCard1");
		retVal.add("MyCard2");
		return retVal;
	}
	
	public static ArrayList<String> getStartingRelics() { // 起始遗物
		ArrayList<String> retVal = new ArrayList<>();
		retVal.add("MyRelic");
		UnlockTracker.markRelicAsSeen("MyRelic");
		return retVal;
	}
	
        public static final int STARTING_HP = 75;
        public static final int MAX_HP = 75;
        public static final int STARTING_GOLD = 99;
        public static final int HAND_SIZE = 5;

	public static CharSelectInfo getLoadout() { // 角色其余信息加载，比如选人界面的最大生命值等
		return new CharSelectInfo("My Character", "My character is a person from the outer worlds. He makes magic stuff happen.",
				STARTING_HP, MAX_HP, ORB_SLOTS, STARTING_GOLD, HAND_SIZE,
			this, getStartingRelics(), getStartingDeck(), false);
	}
	
}
```

## 注册

注意，在此之前你的角色必须已经添加过[自定义颜色](./Custom-Colors.md)了。

```java
@SpireInitializer
public class MyMod implements EditCharactersSubscriber {
	
        public MyMod() {
                BaseMod.subscribeToEditCharacters(this);
        }

        public static void initialize() {
                MyMod mod = new MyMod();
        }

        @Override
	public void receiveEditCharacters() {
		logger.info("begin editing characters");
		
		logger.info("add " + MyPlayerClassEnum.MY_PLAYER_CLASS.toString());
		BaseMod.addCharacter(new MyCharcter(CardCrawlGame.playerName),
				MY_CHARACTER_BUTTON,
				MY_CHARACTER_PORTRAIT,
				MyPlayerClassEnum.MY_PLAYER_CLASS);
		
		logger.info("done editing characters");
	}

}
```