恭喜！ 如果你已经完成所有配置，相信你已经准备好了开始制作mod。在开始之前，我们会介绍一些**ModTheSpire** 和 **BaseMod** 是如何运行的基础知识，以便你更好的使用它们。**ModTheSpire** 是Slay The Spire的mod加载器，可以让你直接更改游戏代码。这个过程有些无聊并且在每次游戏更新后都会改变，但这对于添加一些（尤其是那些复杂的）Mod是必须的。**BaseMod**提供了更方便的 **API** 因此大部分mod可以不用去直接更改游戏代码。因此请使用BaseMod和ModTheSpire组合来制作mod。BaseMod的 **API** 目的是触发事件并让mod去订阅/监听这些事件。比如你想要制作一个每当卡牌被消耗时回复5点生命的mod，那么你必须使用BaseMod去订阅 `PostExhaust` 事件，接下来你将可以使用事件处理器去监听这个事件。在本例中我们将创建一个简单的样例mod来展示如何使用BaseMod的订阅系统。将来会有关于如何使用自定义角色以及GUI系统的教程，但是如果你现在就像使用这些系统，请查看这些wiki或者那些已经使用了他们的mod。

## 添加你的Mod信息

再开始制作你的mod前，确保你已经添加了mod信息以便于MTS可以发现它。文件被存放于`src/main/resources` 文件夹的根目录。你可以从该[教程](./ModInfo)学习如何格式化文件。

## @SpireInitializer
为了使用BaseMod去制作mod，你需要从如下的样板代码开始：

```Java
package example_mod;

import com.evacipated.cardcrawl.modthespire.lib.SpireInitializer;

@SpireInitializer
public class ExampleMod {

    public ExampleMod() {
        // TODO: 尽情制作你的Mod吧!
    }

    public static void initialize() {
        new ExampleMod();
    }

}
```

注意：如果Eclipse不能找到`@SpireInitializer` ，那么你需要通过 **Dependencies** 再次进行设置，因为当你改变了你的资源文件夹时，Eclipse似乎会重置依赖设置。

上述代码告诉ModTheSpire我们有一个叫作 `ExampleMod` 的类中的 `initialize` 方法想要在mod加载后但游戏加载前调用。在 `initialize` 方法中我们创建了该mod的一个实例，并且在这个实例中我们会告诉BaseMod我们想要监听的不同事件。

## ISubscriber
在这个例子中，我们将计算一张卡牌被消耗的次数，并将它发送给日志。我们还将记录卡牌消耗的总数，并在每次战斗后将其报告。我们将实现类似这样的功能：``例如计算出玩家在第一场战斗中消耗了2张卡牌，接下来在第二场战斗中消耗了3张卡牌，共计消耗了5张卡牌。`` 要做到上述功能，我们BaseMod提供的三个事件：`PostExhaust`, `PostBattle`, `PostDungeonInitialize`。

我们将使用如下的hooks。在`PostDungeonInitialize` 中我们将把本次冒险的卡牌消耗数置位0。在PostBattle` 中我们将打印出卡牌消耗的总数和本次战斗中的卡牌消耗数。在PostExhaust` 中我们将计算卡牌消耗数。

要让我们的mod订阅这些事件，我们需要让我们的`ExampleMod`类实现相应的接口： ``PostExhaustSubscriber`` ， ``PostBattleSubscriber`` ， ``PostDungeonInitializeSubscriber`` 。（你可以在[Hook页面](./Hooks.md)找到全部的接口列表。）接下来我们通过BaseMod.subscribe(this)来告诉BaseMod我们想要监听这些事件。

```Java
package example_mod;

import com.evacipated.cardcrawl.modthespire.lib.SpireInitializer;
import com.megacrit.cardcrawl.cards.AbstractCard;
import com.megacrit.cardcrawl.rooms.AbstractRoom;

import basemod.BaseMod;
import basemod.interfaces.PostBattleSubscriber;
import basemod.interfaces.PostDungeonInitializeSubscriber;
import basemod.interfaces.PostExhaustSubscriber;

@SpireInitializer
public class ExampleMod implements PostExhaustSubscriber,
	PostBattleSubscriber, PostDungeonInitializeSubscriber {

	public ExampleMod() {
		BaseMod.subscribe(this);
	}
	
	public static void initialize() {
		new ExampleMod();
	}
	
	@Override
	public void receivePostExhaust(AbstractCard c) {

	}
	
	@Override
	public void receivePostBattle(AbstractRoom r) {
		
	}
	
	@Override
	public void receivePostDungeonInitialize() {
		
	}
	
}
```

## Mod逻辑

```Java
package example_mod;

import com.evacipated.cardcrawl.modthespire.lib.SpireInitializer;
import com.megacrit.cardcrawl.cards.AbstractCard;
import com.megacrit.cardcrawl.rooms.AbstractRoom;

import basemod.BaseMod;
import basemod.interfaces.PostBattleSubscriber;
import basemod.interfaces.PostDungeonInitializeSubscriber;
import basemod.interfaces.PostExhaustSubscriber;

@SpireInitializer
public class ExampleMod implements PostExhaustSubscriber,
	PostBattleSubscriber, PostDungeonInitializeSubscriber {

	private int count, totalCount;
	
	private void resetCounts() {
		totalCount = count = 0;
	}
	
	public ExampleMod() {
		BaseMod.subscribe(this);
		resetCounts();
	}
	
	public static void initialize() {
		new ExampleMod();
	}
	
	@Override
	public void receivePostExhaust(AbstractCard c) {
		count++;
		totalCount++;
	}
	
	@Override
	public void receivePostBattle(AbstractRoom r) {
		System.out.println(count + " cards were exhausted this battle, " +
			totalCount + " cards have been exhausted so far this act.");
		count = 0;
	}
	
	@Override
	public void receivePostDungeonInitialize() {
		resetCounts();
	}
	
}
```