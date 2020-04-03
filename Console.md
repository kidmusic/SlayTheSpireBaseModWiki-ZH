# 开启

使用 `` ` ``键 开启控制台。该快捷键可以在**BaseMod**的设置中更改。

# 命令

## 套牌修改
* `deck add [id] {cardcount} {upgrades}` 向套牌中添加卡牌 （可选参数cardcount：整数类型 # 添加该卡牌的数量）（可选参数upgrades：整数类型 # 卡牌升级)
* `deck remove [id]` 从套牌中移除卡牌
* `deck remove all` 移除套牌中所有卡牌

## 战斗中
* `draw [num]` 抽卡
* `energy add [amount]` 获得费用
* `energy inf` 无限费用
* `energy remove [amount]` 失去费用
* `hand add [id] {cardcount} {upgrades}` 向手牌中添加卡牌 可选参数cardcount：整数类型 # 添加该卡牌的数量）（可选参数upgrades：整数类型 # 卡牌升级)
* `hand remove all` 移除所有手牌
* `hand remove [id]` 从手牌中移除卡牌
* `kill all` 杀死当前战斗中所有敌人
* `kill self` 自杀
* `power [id] [amount]` 使敌人/自己获得力量

## 战斗外
* `fight [name]` 进入指定的战斗遭遇
* `event [name]` 进入指定的事件遭遇

## 任何时间
* `gold add [amount]` 获得金币
* `gold lose [amount]` 失去金币
* `info toggle` Settings.isInfo
* `potion [pos] [id]` 获得药水并放入指定的位置 (0, 1, or 2)
* `hp add [amount]` 获得生命值
* `hp remove [amount]` 失去生命值
* `maxhp add [amount]` 获得生命上限
* `maxhp remove [amount]` 失去生命上限
* `debug [true/false]` sets `Settings.isDebug`
## 遗物
* `relic add [id]` 添加遗物
* `relic list` 显示所有遗物
* `relic remove [id]` 失去遗物

## 解锁
* `unlock always` 总是在死亡时获得解锁
* `unlock level [level]` 指定解锁等级

## 行为
* `act boss` 直抵达boss处
* `act [actname]` 回到起始点

## Keys to Act 4
* `key add [color | all]` 添加相应的key
* `key lose [color | all]` 移除相应的key

## 历史
* `history random`随机获得你当前角色曾经通关的套牌和遗物
* `history last` 获得你当前角色最后一次通关的套牌和遗物

# 添加自定义命令
添加自定义命令需要：

* 一个用于提交命令的class
*  一个未被占用的关键词

## Java类
所有的命令必须继承自``basemod.devcommands.ConsoleCommand``这个抽象类。

你可以在该类的构造函数中设置多种参数去提交你的自定义命令。

注意，这些都不是必须的并且它们都有默认值。

```java
public class YourCommand extends ConsoleCommand {
    public YourCommand() {
        maxExtraTokens = 2; //你命令后的附加词的最大数量。如果不指定，则默认值为1。
        minExtraTokens = 0; //你命令后的附加词的最小数。如果不指定，则默认值为1。
        requiresPlayer = true; //如果为true，该命令将仅能在冒险中使用。如果不指定，则默认值为false。
        simplecheck = true;
        /**
         * 如果该值为true且你没有对check方法进行复写，它将会检查输入 的内容是否存在你的命令的选项中。
         * 注意，这仅适用于控制台的自动补全功能，与命令执行时无关。
         * 如果不指定，则默认值为false。
        */
        followup.put("whateveryouwantmetobebaby", YourSecondCommand.class);
        /**
         * 这样做会将第一个参数作为该命令的后续，并传给YourSecondCommand。
         * 你可以像这样添加多条
        */
    }
    ...
}
```

每条命令都必须使用的方法： `void execute(String[] tokens, int depth)`.
* ``tokens``是控制台中输入的全部命令通过 `/\s/` (空格)分割
* ``depth`` 是你当前所在的tokens的位置的索引
例如： `tokens[depth]` 将是引导进你的命令的单词后的第一个单词

这个方法包括了对你的命令的语法检查以及它的实际作用。

对于过渡命令来说， `execute(...)` 通常仅会调用 `errorMsg();`

你也可以重写以下的方法：
* `ArrayList<String> extraOptions(String[] tokens, int depth)`
* `void errorMsg(String[]? tokens)`

``extraOptions`` 返回的值和你在构造函数中添加到 `followup` 的内容将会显示在控制填的自动补全功能。

```java
...
    public ArrayList<String> extraOptions(String[] tokens, int depth) {
        ArrayList<String> result = new ArrayList<>();
        result.add("add");
        result.add("lose");
        
        if(tokens[depth].equals("add") || tokens[depth].equals("lose")) {
            complete = true;
            /**
             * 值为true时将会显示"Command is complete"在自动补全窗口。
             * 如果你在构造函数中指定了"simplecheck = true"且没有添加额外的逻辑代码，那么你就不需要这么做。
            */
        }
        
        return result;
    }
...
```
这样当控制台认为你想要使用你的命令时将会显示 `add` 和 `lose` 自动补全选项。

`errorMsg()` 方法用于显示 `execute(...)`过程中产生的错误.
满足上述示例 `extraOptions(...)` 错误消息如下:

```java
    @Override
    public void errorMsg() {
        DevConsole.couldNotParse();
        DevConsole.log("options are:");
        DevConsole.log("* add [amt]");
        DevConsole.log("* lose [amt]");
    }
```
注意，必须在执行过程中专门调用此方法。

## 注册你的命令
在你的mod的``main file``中，在``in receivePostInitialize``中，调用如下的静态方法：
```java
    public void receivePostInitialize() {
        ...
        ConsoleCommand.addCommand("yourphrasehere", YourCommand.class);
        ...
    }
```
此处的关键词只能由字母和冒号组成。如果该关键词已经被使用或者无效，BaseMod将不会注册你的命令并在日志窗口抛出错误信息，但这并不会使游戏崩溃。