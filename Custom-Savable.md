# 自定义存储器
当你存/取一个卡牌或者遗物的值，而它又不是遗物的 `counter` 值或者卡牌的 `misc` 值，那么你可以通过自定义存储器来实现。

# API

`CustomSavable<Object>` - 在你想存储值的类中实现这个接口，并把你想存的值的类型传入

# 举个栗子

```java
public class MyCustomBottleRelic extends CustomRelic implements CustomBottleRelic, CustomSavable<Integer>
{
    private AbstractCard card;
    // 你想要保存的字段值。 

      @Override
    public Integer onSave()
    {
        return AbstractDungeon.player.masterDeck.group.indexOf(card);
        // 返回卡牌在你牌组中的位置，抽象卡牌不能被序列化，所以我们用了整型代替。
    }

     @Override
    public void onLoad(Integer cardIndex)
    {
    // 当游戏调用onSave时onLoad会自动的保存Integer值

        if (cardIndex == null) {
            return;
        }
        if (cardIndex >= 0 && cardIndex < AbstractDungeon.player.masterDeck.group.size()) {
            card = AbstractDungeon.player.masterDeck.group.get(cardIndex);
            if (card != null) {
                MyCustomBottledField.inCustomBottle.set(card, true);
                setDescriptionAfterLoading();
            }
        }
        // 在搜索前使用卡牌的索引将其保存，并存入一个自定义的位置
    }
}
```
如果这方法没有被调用的话，尝试下面的代码：
```java
@Override
public Type savedType()
{
    return new TypeToken</*yourTypeHere*/>(){}.getType();
}
```
如果你的存储器存储的不是卡牌或者遗物，那你必须使用BaseMod.addSaveField(String key, CustomSavableRaw saveField)` 注册它。