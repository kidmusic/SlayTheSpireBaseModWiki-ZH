可选卡牌允许卡牌在玩家使用它时展示选项。

比如，你想要卡牌给玩家随机的红，绿或者无色的卡牌，并允许玩家选择他们想要的卡牌，你可以使用可选卡牌来实现它。 (https://streamable.com/gxx7t). 该类的代码可以在下面查看。

# API #

## 选项构建 ##

你需要首先创建 `ModalChoiceBuilder` 类

#### `setTitle` ####
设置标题，如果不设置，将会显示“选择一个选项”

#### `addOption` ####
这里有三个设置可以用来添加选项：
* `addOption(String description, AbstractCard.CardTarget target)`
    * 设置选项为"选项 X" ，X是选项的数字，如果选择了，会调用回调。受 `setColor` 和 `setCallback` 的影响
* `addOption(String title, String description, AbstractCard.CardTarget target)`
    * 带标题的设置，同上
* `addOption(AbstractCard card)`
    * 添加相同内容，选择选项时会调用 `use()` 

#### `setColor` ####
设置之后会选择的卡的颜色

#### `setCallback` ####
设置选卡后的回调函数

#### `create` ####
完成创建并返回 `ModalChoice` 对象

## 选项 ##

#### `open` ####
打开选项界面，并为玩家提供选项。通常会使用你的卡牌的 `use()` 方法。

#### `generateTooltips` ####
为选项生成工具提示。如果你的卡牌继承自`CustomCard`，使用 `getCustomTooltips()` 来实现。

#### `Callback` ####
回调函数必须实现 `ModalChoice.Callback` 接口，当选项被选择时，它们的回调会被调用。

# 举个栗子 #

```Java
import basemod.abstracts.CustomCard;
import basemod.helpers.ModalChoice;
import basemod.helpers.ModalChoiceBuilder;
import basemod.helpers.TooltipInfo;
import com.megacrit.cardcrawl.actions.common.MakeTempCardInHandAction;
import com.megacrit.cardcrawl.cards.AbstractCard;
import com.megacrit.cardcrawl.characters.AbstractPlayer;
import com.megacrit.cardcrawl.dungeons.AbstractDungeon;
import com.megacrit.cardcrawl.helpers.CardLibrary;
import com.megacrit.cardcrawl.monsters.AbstractMonster;

import java.util.List;

public class ModalTest extends CustomCard implements ModalChoice.Callback
{
    public static final String ID = "ModalTest";
    public static final String NAME = "Modal Test";
    public static final String DESCRIPTION = "Choose Red, Green, or Colorless. NL Gain a random card of that color.";
    private static final int COST = 0;
    private ModalChoice modal;

    public ModalTest()
    {
        super(ID, NAME, null, COST, DESCRIPTION, CardType.SKILL, CardColor.COLORLESS, CardRarity.BASIC, CardTarget.NONE);

        modal = new ModalChoiceBuilder()
                .setCallback(this) // 为下面的选项设置回调
                .setColor(CardColor.RED) // 把卡牌设置成红色
                .addOption("Add a random Red card to your hand.", CardTarget.NONE)
                .setColor(CardColor.GREEN) // 把卡牌设置成绿色
                .addOption("Add a random Green card to your hand.", CardTarget.NONE)
                .setColor(CardColor.COLORLESS) // 把卡牌设置成无色
                .addOption("Add a random Colorless card to your hand.", CardTarget.NONE)
                .create();
    }

    // 设置标题和描述
    @Override
    public List<TooltipInfo> getCustomTooltips()
    {
        return modal.generateTooltips();
    }

    @Override
    public void use(AbstractPlayer p, AbstractMonster m)
    {
        modal.open();
    }

    // 当选项被选择时调用
    @Override
    public void optionSelected(AbstractPlayer p, AbstractMonster m, int i)
    {
        CardColor color;
        switch (i) {
            case 0:
                color = CardColor.RED;
                break;
            case 1:
                color = CardColor.GREEN;
                break;
            case 2:
                color = CardColor.COLORLESS;
                break;
            default:
                return;
        }

        AbstractCard c;
        if (color == CardColor.COLORLESS) {
            c = AbstractDungeon.returnTrulyRandomColorlessCard(AbstractDungeon.cardRandomRng).makeCopy();
        } else {
            c = CardLibrary.getColorSpecificCard(color, AbstractDungeon.cardRandomRng).makeCopy();
        }
        AbstractDungeon.actionManager.addToBottom(new MakeTempCardInHandAction(c, true));
    }

    @Override
    public void upgrade()
    {
        if (!upgraded) {
            upgradeName();
        }
    }

    @Override
    public AbstractCard makeCopy()
    {
        return new ModalTest();
    }
}
```