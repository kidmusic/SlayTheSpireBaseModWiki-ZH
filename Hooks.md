# 订阅系统

BaseMod使用了订阅模式来处理hook。如果你获得某一hook那么请使用 `subscribeTo` 订阅它，同样，如果你不再想使用某一hook请使用 `unsubscribeFrom` 取消对它的订阅。

通常，你应该只在mod主文件中使用BaseMod的订阅者（@SpireInitialize），然后去调用它的方法。这是为了防止多个实例重复订阅却不取消订阅。

## 新建订阅
* `BaseMod.subscribe(this)` 这里的 `this` 是一些订阅者的实例，该方法将会使 `this` 订阅**所有**它实现的订阅者。
* `BaseMod.unsubscribe(this)` 这里的 `this` 是一些订阅者的实例，该方法将会使 `this` 取消订阅**所有**它实现的订阅者。
* `BaseMod.subscribe(this, Class<? extends ISubscriber> toAddClass)` 这里的 `this` 是一些订阅者的实例，该方法将会使 `this` 仅订阅 `toAddClass` 这**一个**hook。
* `BaseMod.unsubscribe(this, Class<? extends ISubscriber> toRemoveClass)` 这里的 `this` 是一些订阅者的实例，该方法将会使 `this` 仅取消订阅 `toRemoveClass` 这**一个**hook。

## 过时的订阅接口 (不再适用)
* `BaseMod.subscribeTo...(this)`
* `BaseMod.unsubscribeFrom...(this)`

## UnsubscribeLater
有时你需要在某事件触发一次后取消对它的订阅。比如你只想监听第一次打出卡牌或者第一次生成地牢。在这种情况下，你可以在某hook的回调函数中取消对它的订阅。但是如果你想直接在你的 `receiveSomeReallyCoolEvent` 方法中使用 `BaseMod.unsubscribe` ，那将会抛出一个 `ConcurrentModificationException` 异常，这是因为你在列表迭代时尝试对它做出修改。为了避免这种错误，你应该使用 `BaseMod.unsubscribeLater` ，它会在那些可能抛出异常的代码执行完毕后再取消订阅。

# Hook列表

这是一份**不完整**的hook列表。请到 [`src/main/java/basemod/interfaces`](https://github.com/daviscook477/BaseMod/tree/master/mod/src/main/java/basemod/interfaces) 查看完整列表。

## 卡牌
* `PostDrawSubscriber` - 当抽取卡牌后
* `PostExhaustSubscriber` - 当消耗卡牌后
* `OnCardUseSubscriber` - 当卡牌被使用后立即调用（可用于增加卡牌的功能）

## 费用（能量）
* `PostEnergyRechargeSubscriber` - 在玩家的每个回合开始前，所有能量重置触发。

## 渲染
* `PreRenderSubscriber`
* `ModelRenderSubscriber` - 结合 `receiveCameraRender` 使用
* `PreRoomRenderSubscriber` - 在玩家，敌人和背景后
* `RenderSubscriber` - 在提示和光标下，其他所有上
* `PostRenderSubscriber` - 在所有之上

## 游戏
* `PostInitializeSubscriber` - 仅一次，在 `CardCrawlGame.initialize()` 后
* `PostDungeonInitializeSubscriber` - 在地牢初始化完成后
* `StartActSubscriber` - 开始新动作后 - 仅在玩家两次动作间**或者**开始新游并开始“前言”动作
* `PostCampfireSubscriber` - 在篝火之后。返回false将允许执行其他操作
* `PreStartGameSubscriber` - 在生成或继续游戏之后，在生成或加载玩家之前
* `StartGameSubscriber` - 在生成或继续游戏之后，在生成或加载玩家之后，在地牢生成之前
* `PreUpdateSubscriber` - 读取输入后立即生效
* `PostUpdateSubscriber` - 处理输入之前立即生效
* `PreDungeonUpdateSubscriber` - 在 `AbstractDungeon.update()` 发生任何事之前调用
* `PostDungeonUpdateSubscriber` - 在 `AbstractDungeon.update()` 发生任何事之后调用
* `PrePlayerUpdateSubscriber` - 在 `AbstractPlayer.update()` 发生任何事之前调用
* `PostPlayerUpdateSubscriber` - 在 `AbstractPlayer.update()` 发生任何事之后调用
* `PostBattleSubscriber` - 在 `AbstractRoom.endBattle()` 中的多有逻辑执行完后调用
* `OnPowersModifiedSubscriber` - 在 `AbstractDungeon.onModifyPower()` 后调用
* `PostPowerApplySubscriber`
* `MaxHPChangeSubscriber` - 在生命值上限因 `AbstractCreature` 改变时立即调用，允许对改变的值进行更改
* `PostEnergyRechargeSubscriber`

## 起始套牌/遗物
* `PostCreateStartingDeckSubscriber` - 在人物的初始套牌被创建后立即调用，返回true将会从默认起始套牌中移除所有卡牌。使用 `cardsToAdd` 向起始套牌中增加卡牌。
* `PostCreateStartingRelicsSubscriber` - 在人物的初始遗物被创建后立即调用，返回true将会从默认起始遗物中移除所有遗物使用 `relicsToAdd` 向起始遗物中增加遗物。
* `RelicGetSubscriber`

## 商店
* `PostCreateShopRelicSubscriber` - 商店生成遗物后立即调用，修改 `relics` 将会修改商店中的遗物。 `screenInstance` 是 `ShopScreen` 的一个实例。
* `PostCreateShopPotionSubscriber` - 商店生成药水后立即调用，修改 `potions` 将会修改商店中的药水。 `screenInstance` 是 `ShopScreen` 的一个实例。

## 药水
* `PrePotionUseSubscriber` - 在药水使用前立即调用
* `PostPotionUseSubscriber` - 在药水使用后立即调用
* `PotionGetSubscriber` - 当玩家获得药水时调用

## 自定义内容
* `EditCardsSubscriber` - 当你想使用 `BaseMod.addCard` 和 `BaseMod.removeCard`  去添加/删除卡牌时调用。**不要**在该处理器之外初始化任何卡牌或添加/删除卡牌。Slay the Spire需要做一些工作以确保处理器会正确的执行。请注意，移除游戏中的任何卡牌现在仍是未定义的行为。
* `EditRelicsSubscriber` - 遗物，同上。
* `EditCharactersSubscriber` - 人物，同上。
* `SetUnlocksSubscriber` - 当你注册任何自定义解锁时，请注意，无法从游戏中移除任何的解锁（这里应该抛出异常，但事实上并没有）。不要在该处理器之外设置任何解锁。Slay the Spire需要做一些工作以确保处理器会正确的执行。
* `AddAudioSubscriber` - 当你需要使用 `BaseMod.addAudio` 添加音乐时使用

# 举个栗子

这里是一个简单的例子，它展示了当玩家抽牌时你可以做的事情。例如这里做的就是当玩家抽牌时，把卡牌名记录到控制台。

```java
@SpireInitializer
public class MyMod implements PostDrawSubscriber {
	
        public MyMod() {
                BaseMod.subscribe(this);
        }

        public static void initialize() {
                MyMod mod = new MyMod();
        }

        @Override
	public void receivePostDraw(AbstractCard card) {
		logger.info("card ID: " + card.cardID + " was drawn!");
	}

}
```