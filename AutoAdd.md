AutoAdd允许mod简易的注册所有的卡牌，遗物等，而不必像下面这样一个个注册：

```java
@Override
public void receiveEditCards()
{
    // No more of this
    BaseMod.addCard(new MyCard1());
    BaseMod.addCard(new MyCard2());
    BaseMod.addCard(new MyCard3());
    // etc.
}
```

# API
### 配置
* `AutoAdd(String modID)`
  * `modID` : 你想查询的mod的ID
* `overrideClassPool(ClassPool pool)`
  * `pool` : 用于查找类的Java类池
  * 默认值可以使用 `Loader.getClassPool()`
  * **注意:** 通常你只需要在 [Raw patch](https://github.com/kiooeht/ModTheSpire/wiki/SpirePatch#raw)   中使用AutoAdd时注意
* `setDefaultSeen(boolean seen)`
  * `seen` : 卡牌是否可见
  * 默认不可见
* `packageFilter(String packageName)`
  * `packageName` : 你要搜索的包名（例如： `"mymod.cards"` ），任何在此包以外的类会被忽略
* `packageFilter(Class<?> packageClass)`
  * `packageClass` : 你想从包中搜索的类的类型（例如： `MyAbstractCard.class` ），在这个类所在的包之外的类将会被忽略
* `filter(ClassFilter filter)`
  * `filter` : 为搜索到或者未搜索到的类创建过滤器

### 使用
* `<T> Collection<CtClass> findClasses(Class<T> type)`
  * 查找那些非抽象，非接口的类，他们继承自给定的 `type`
  * 这**包括**了所有具有 `@AutoAdd.Ignore` 注释的类
* `<T> void any(Class<T> type, BiConsumer<Info, T> add)`
  * 查找类，获取他们的注解（参考下面），然后通过类的实例调用 `add` 
  * 排除所有使用了`@AutoAdd.Ignore` 注解的类（参考下面）
  * 一个类必须有一个无参（或被忽略的参）构造函数，否则 `any` 将会崩溃
* `void cards()`
  * 使用BaseMod（ `BaseMod.addCard` ）查找所有卡牌并注册，并使它们可见

## 注解
如下的注解可以被添加到单独的类文件中，下面是如何使用他们：
* `@AutoAdd.Ignore`
  * 一个类使用此注解将会被AutoAdd完全忽略
* `@AutoAdd.Seen`
* `@AutoAdd.NotSeen`
  * 可见或者不可见将会复写默认的AutoAdd可见设置，可以用它去设置一张卡牌的可见/不可见的默认值。

# 举个栗子
## 简单的自动添加卡牌
```java
@Override
public void receiveEditCards()
{
    // 将会寻找并添加和MyAbstractCard同包或者子包的卡牌 
    // 并会设置所有添加的卡牌可见
    new AutoAdd(MyModID)
        .packageFilter(MyAbstractCard.class)
        .setDefaultSeen(true)
        .cards();
}
```

## 为mod角色添加指定遗物
```java
@Override
public void receiveEditRelics()
{
    // 这会添加所有同包中的自定义遗物
    // 比如MyRelic除了使用@AutoAdd.Seen注解的，其他都将会不可见
    new AutoAdd(MyModID)
        .packageFilter(MyRelic.class)
        .any(CustomRelic.class, (info, relic) -> {
            BaseMod.addRelicToCustomPool(relic, MY_CHARACTER_COLOR);
            if (info.seen) {
                UnlockTracker.markRelicAsSeen(relic.relicId);
            }
        });
}
```