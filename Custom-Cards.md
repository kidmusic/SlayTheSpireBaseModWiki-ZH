# 自定义卡牌构造器
`CustomCard(String id, String name, String img, int cost, String rawDescription, CardType type, CardColor color, CardRarity rarity, CardTarget target)`

* `id` - 卡牌id
* `name` - 卡牌名字
* `img` - 卡牌图像路径 （从你的 ``jar`` 包根目录开始）（250px x 190px） ；较大尺寸图像 （500 x 380p）的路径最后应在图像名后面加“_p”，例如“my_card.png”的大版本应为“my_card_p.png”
* `cost` - 费用（能量）
* `rawDescription` - 描述
* `type` - 类型，例如： `ATTACK`, `SKILL`, `POWER`
* `color` - 颜色，基础选项： `RED`, `GREEN`, `COLORLESS`, `CURSE`, `STATUS` ，当然你也可以自定义颜色
* `rarity` - 稀有度，例如： `COMMON`, `UNCOMMON`, `RARE`
* `target` - 目标，例如： `ENEMY`, `ALL_ENEMIES`, `SELF`, etc...

## 自定义每张卡牌的样式
下列方法可以在构造其中调用，当然，你也可以在后续任何时间调用。

### 卡牌背景
`setBackgroundTexture(String smallTexturePath, String largeTexturePath)`

* `smallTexturePath` - 小图路径 （512 x 512p）
* `largeTexturePath` - 大图路径 （1024 x 1024p）
### 卡牌能量球
`setOrbTexture(String smallTexturePath, String largeTexturePath)`
* `smallTexturePath` - 小图路径 （512 x 512p）
* `largeTexturePath` - 大图路径 （164 x 164p）
### 卡牌稀有度横幅
`setBannerTexture(String smallTexturePath, String largeTexturePath)`
* `smallTexturePath` - 小图路径 （512 x 512p）
* `largeTexturePath` - 大图路径 （1024 x 1024p）

# 注册
为了使用下列方法添加/删除卡牌，你必须在你mod的主类中实现 `EditCardsSubscriber` 并复写 `receiveEditCards` 方法。该方法的内部是何时添加/删除卡牌。

* `BaseMod.addCard(AbstractCard card)` （注意： `CustomCard` 继承自 `AbstractCard`）
* `BaseMod.removeCard(AbstractCard card)` 从游戏中一出一张卡牌（注意：目前没有测试过移除一张事件使用到的卡牌）

# 举个栗子

例如你想创建一张名为 `Flare` 的卡牌，它会对一个敌人造成 `3` 点伤害和 ``1`` 层脆弱，升级后它会造成 `6` 点伤害和 `2` 层脆弱。

如果将该卡牌颜色定义为 `RED` 并且已经将他的贴图放在了 `img/my_card_img.png` 那么，下面的代码就可以创建这样一张卡牌：

```java
import com.megacrit.cardcrawl.actions.AbstractGameAction;
import com.megacrit.cardcrawl.actions.common.ApplyPowerAction;
import com.megacrit.cardcrawl.cards.AbstractCard;
import com.megacrit.cardcrawl.cards.DamageInfo;
import com.megacrit.cardcrawl.characters.AbstractPlayer;
import com.megacrit.cardcrawl.dungeons.AbstractDungeon;
import com.megacrit.cardcrawl.monsters.AbstractMonster;
import com.megacrit.cardcrawl.powers.VulnerablePower;

import basemod.abstracts.CustomCard;

public class Flare
extends CustomCard {
    public static final String ID = "myModID:Flare";
    private static CardStrings cardStrings = CardCrawlGame.languagePack.getCardStrings(ID);
    // Get object containing the strings that are displayed in the game.
    public static final String NAME = cardStrings.NAME;
    public static final String DESCRIPTION = cardStrings.DESCRIPTION;
    public static final String IMG_PATH = "img/my_card_img.png";
    private static final int COST = 0;
    private static final int ATTACK_DMG = 3;
    private static final int UPGRADE_PLUS_DMG = 3;
    private static final int VULNERABLE_AMT = 1;
    private static final int UPGRADE_PLUS_VULNERABLE = 1;

    public Flare() {
        super(ID, NAME, IMG_PATH, COST, DESCRIPTION,
        		AbstractCard.CardType.ATTACK, AbstractCard.CardColor.RED,
        		AbstractCard.CardRarity.UNCOMMON, AbstractCard.CardTarget.ENEMY);
        this.magicNumber = this.baseMagicNumber = VULNERABLE_AMT;
        this.damage=this.baseDamage = ATTACK_DMG;
        
        this.setBackgroundTexture("img/custom_background_small.png", "img/custom_background_large.png");

        this.setOrbTexture("img/custom_orb_small.png", "img/custom_orb_large.png");

        this.setBannerTexture("img/custom_banner_large.png", "img/custom_banner_large.png");
    }

    @Override
    public void use(AbstractPlayer p, AbstractMonster m) {
    	AbstractDungeon.actionManager.addToBottom(new com.megacrit.cardcrawl.actions.common.DamageAction(m,
				new DamageInfo(p, this.damage, this.damageTypeForTurn),
				AbstractGameAction.AttackEffect.SLASH_DIAGONAL));
    	AbstractDungeon.actionManager.addToBottom(new ApplyPowerAction(m, p, new VulnerablePower(m, this.magicNumber, false), this.magicNumber, true, AbstractGameAction.AttackEffect.NONE));
    }

    @Override
    public AbstractCard makeCopy() {
        return new Flare();
    }

    @Override
    public void upgrade() {
        if (!this.upgraded) {
            this.upgradeName();
            this.upgradeDamage(UPGRADE_PLUS_DMG);
            this.upgradeMagicNumber(UPGRADE_PLUS_VULNERABLE);
        }
    }
}
```

# 关于视图的注意事项

在牌组浏览界面显示的卡牌会比较大，相应的，也会使用较大的贴图，游戏会**自动**找到对应的加了 `_p` 的贴图。这类图片的大小应为500px x 380px。前面已经举例过： `img/my_card.png` => `img/my_card_p.png`。




