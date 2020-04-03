# 自定义字串
为了集中管理所有的卡牌，遗物，事件，UI等等，我们要将所有的文本集中到一起，以方便其他人的阅读和修改，或者使你的mod更容易被翻译成其他语言。

# API
`loadCustomStringsFile(Class class, String filePath)` - 仅在`EditStringsSubscriber` 的回调函数`receiveEditStrings` 中调用，除非你的关键词还没初始化。

`class` - 你用到的字串的类，一个完整的字串列表就在下面

`filePath` - json的文件路径

你可以通过调用 `CardCrawlGame.languagePack.get(StringType)(myKey)` 来获得一个包含这些字串的对象。用正确的类型替换StringType。

# 字串类型的列表

```
MonsterStrings
PowerStrings
CardStrings
RelicStrings
EventStrings
PotionStrings
CreditStrings
TutorialStrings
KeywordStrings
ScoreBonusStrings
CharacterStrings
UIStrings
OrbStrings
RunModStrings
BlightStrings
AchievementStrings
```

# 举个栗子

```java
 @Override
    public void receiveEditStrings() {
        BaseMod.loadCustomStringsFile(RelicStrings.class, "example_mod/localization/ExampleModRelicStrings.json");
        // 加载的字串包括遗物名字，描述等
    }
```

```java
private static CardStrings cardStrings = CardCrawlGame.languagePack.getCardStrings(myCardID);
// 从你的卡牌字串中通过卡牌ID获得它的名字和描述
```