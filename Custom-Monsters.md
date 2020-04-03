# 添加一个怪物

事实上你不是添加了“怪物”而是添加了”遭遇“。添加一个遭遇需要两步：首先注册一个怪物，接下来将这个遭遇添加到地牢中。

API应该在 `receivePostInitialize`调用。

## 注册怪物

`addMonster(String encounterID, BaseMod.GetMonster monster)`

`addMonster(String encounterID, String name, BaseMod.GetMonster monster)`

`addMonster(String encounterID, BaseMod.GetMonsterGroup group)`

`addMonster(String encounterID, String name, BaseMod.GetMonsterGroup group)`

* `encounterID`: 这个/类怪物的ID
* `name` (可选): 显示在游戏界面的怪物的名字，如果不指定，BaseMod会自动生成一个名字
* `monster`: 一个返回怪物实例的lambda
* `group`:一个返回 `MonsterGroup`实例的lambda， `MonsterGroup` 包括了你创建的怪物（们）

## 添加遭遇到地牢

`addMonsterEncounter(String dungeonID, MonsterInfo encounter)`

`addStrongMonsterEncounter(String dungeonID, MonsterInfo encounter)`

`addEliteEncounter(String dungeonID, MonsterInfo encounter)`

* `dungeonID`:你想要添加遭遇的地牢的ID（例如： `Exordium.ID`, `TheCity.ID`, `TheBeyond.ID`）
* `encounter`: `MonsterInfo` 包括了你注册的 `encounterID` 以及其他的各种东西


## 举个栗子
```Java
public void receivePostInitialize()
{
    // 添加一个怪物
    BaseMod.addMonster(MyMonster.ID, () -> new MyMonster());
    // 添加多个怪物
    BaseMod.addMonster("MyMonsters", () -> new MonsterGroup(new AbstractMonster[] {
            new MyMonster(),
            new MyMonster2()
    }));

    BaseMod.addMonsterEncounter(TheCity.ID, new MonsterInfo(MyMonster.ID, 5));
    BaseMod.addStrongMonsterEncounter(TheBeyond.ID, new MonsterInfo("MyMonsters", 10));
}
```

# 添加boss

添加一个新的boss和添加怪物的方法类似。

在注册完boss后，使用如下的方法将他添加到地牢：

`addBoss(String dungeon, String bossID, String mapIcon, String mapIconOutline)`

* `dungeon`: 你想要添加遭遇的地牢的ID（例如： `Exordium.ID`, `TheCity.ID`, `TheBeyond.ID`）
* `bossID`: 通过 `addMonster`注册的boss的ID
* `mapIcon`: 地图中boss的图标路径
* `mapIconOutline`: 地图中boss的图标边框路径

## 举个栗子
```Java
public void receivePostInitialize()
{
    BaseMod.addMonster(MyBoss.ID, () -> new MyBoss());

    BaseMod.addBoss(Exordium.ID, MyBoss.ID,
            "images/mymod/ui/map/boss/myBoss.png",
            "images/mymod/ui/map/bossOutline/myBoss.png");
}
```