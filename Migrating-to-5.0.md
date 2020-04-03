```Java
@Override
public String getTitle(PlayerClass playerClass) {}
```
返回玩家名字类名

```Java
@Override
public AbstractCard.CardColor getCardColor() {}
```
返回与你角色相关的颜色枚举

```Java
@Override
public AbstractCard getStartCardForEvent() {}
```
返回一个你想要出现在“gremlin match”事件中的稀有卡牌的实例

```Java
@Override
public Color getCardTrailColor() {}
```
返回一个颜色对象用于为卡牌的移动轨迹着色

```Java
@Override
public int getAscensionMaxHPLoss() {}
```
返回“提升14”以及之后的最大生命值削减的数值

```Java
@Override
public BitmapFont getEnergyNumFont() {}
```
返回一个BitmapFont对象，可以用来定义能量球的显示方式

```Java
@Override
public void doCharSelectScreenSelectEffect() {}
```
该方法会在你在角色选择页面选择好角色后调用。下面是战士的例子：
```Java
CardCrawlGame.sound.playA("ATTACK_HEAVY", MathUtils.random(-0.2f, 0.2f));
CardCrawlGame.screenShake.shake(ScreenShake.ShakeIntensity.MED, ScreenShake.ShakeDur.SHORT, true);
```

```Java
@Override
public String getCustomModeCharacterButtonSoundKey() {}
```
当你在自定义角色选择界面选则角色时，返回一个字符串来充当音效播放的key。这个key指向了一个hashmap `CardCrawlGame.sound.map` 。

```Java
@Override
public String getLocalizedCharacterName() {}
```
在运行界面时返回一个角色类名。

```Java
@Override
public AbstractPlayer newInstance() {}
```
返回角色实例，用 this.name 作为它的名字参数。

```Java
@Override
public String getSpireHeartText() {}
```
返回一个字符串，当你的角色攻击心脏时会显示它。

```Java
@Override
public Color getSlashAttackColor() {}
```
返回一个颜色对象，这是你的角色攻击心脏时的屏幕颜色。

```Java
@Override
public String getVampireText() {}
```
vampire事件将基础游戏角色分为“兄弟”，“姐妹”，“决裂” 。这个方法返回一个字符串，它包括了vampire第一屏的完整的文本。例如，战士的内容是：
```"在一条没有照明的街道上行驶时，你在一些黑暗中遇到几个蒙面的人物。 当你接近时，它们会一致地转向你。 其中最高的裸露的牙齿呈扇形，向着您伸出了一只苍白的长手。 战士，加入我们，我的哥哥，感受温暖"```

```Java
@Override
public Color getCardRenderColor() {}
```
返回一个颜色对象，它会为小的卡牌图片着色。

```Java
@Override
public AbstractGameAction.AttackEffect[] getSpireHeartSlashEffect() {}
```
返回一个攻击效果的数组（大于0），它会在你的角色击杀心脏时按顺序播放。

```Java
@Override
public ArrayList<String> getStartingDeck() {}, public ArrayList<String> getStartingRelics() {}
```
和之前的功能相同，但不再是静态的了。

```Java
@Override
public CharSelectInfo getLoadout() {}
```
不再是静态的了，且应该用this而不是枚举来作为第8个参数。

在主mod类中：
`BaseMod.addCharacter` 有新的参数，现在你需要添加类的实例，卡牌颜色的枚举，人物按钮的路径，人物肖像的路径和自定义累的枚举。
例如：
```Java
BaseMod.addCharacter(
    new MyCustomCharacter(CardCrawlGame.playerName),
    MyCardColorEnum.CHARACTER_COLOR,
    PATH_TO_CHAR_BUTTON,
    PATH_TO_CHAR_PORTRAIT,
    MyCharacterEnum.CHARACTER_CLASS
);
```