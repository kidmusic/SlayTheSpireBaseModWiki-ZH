# 奖励
在基础游戏中你可以获得各种奖励，比如“卡牌”，“金币/被偷的金币”，“遗物”，“药水”等等，自定义奖励允许你提供给玩家这些以外的奖励，下面的例子将使玩家可以获得生命值奖励。

# API

## 注册奖励
`BaseMod.registerCustomReward(RewardType type, LoadCustomReward onLoad, SaveCustomReward onSave)` -  在 `receivePostInitialize` 中调用，确保你的奖励被正确的加载。

* `type` - 这应当是 `RewardType` 枚举中的一种，它应当是唯一的，使用游戏中的类型可能会造成未知错误
* `onLoad` - 一个会返回 `CustomReward` 实例的Lambda
* `onSave` - 一个会返回 `RewardSave` 实例（它存储了你的奖励的数据）的Lambda

# 举个栗子
为了创建这个新的奖励，我们需要在 `RewardItem.RewardType` 枚举中添加我们的新的奖励类型 `RewardType` 。
```Java
import com.evacipated.cardcrawl.modthespire.lib.SpireEnum;
import com.megacrit.cardcrawl.rewards.RewardItem;

public class HpRewardTypePatch {
    @SpireEnum
    public static RewardItem.RewardType MYMOD_HPREWARD;
}
```

接下来我们会创建具体的奖励类，它继承自 `CustomReward`。
```Java
import basemod.abstracts.CustomReward;
import com.badlogic.gdx.Gdx;
import com.badlogic.gdx.graphics.Texture;
import com.megacrit.cardcrawl.dungeons.AbstractDungeon;

public class HpReward extends CustomReward {
    private static final Texture ICON = new Texture(Gdx.files.internal("[pathtotexturefile]"));
    
    public int amount;

    public HpReward(int amount) {
        super(ICON, "Heal " + amount + " hp.", HpRewardTypePatch.MYMOD_HPREWARD);
        this.amount = amount;
    }

    @Override
    public boolean claimReward() {
        AbstractDungeon.player.heal(this.amount);
        return true;
    }
}
```

接下来我们要做的就是用 `BaseMod.registerCustomReward` 在你的初始化类中注册它。

```Java
...
    @Override
    public void receivePostInitialize() {
        BaseMod.registerCustomReward(
            HpRewardTypePatch.MYMOD_HPREWARD, 
            (rewardSave) -> { // 该类型加载时的处理
                return new HpReward(rewardSave.amount);
            }, 
            (customReward) -> { // 该类型保存时的处理
                return new RewardSave(customReward.type.toString(), null, ((HpReward)customReward).amount, 0);
            });
    }
...
```