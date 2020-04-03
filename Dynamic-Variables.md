# 动态变量
游戏中，卡牌使用I !D! 或者 !M! 来展示数字。如果你想要在卡牌上加入数字，但是没有适合你的动态变量（比如!D!, !M!, !B!），或者它已经被使用过了，那么你可以自定义变量。

# 举个栗子
```Java
public class MyVariable extends DynamicVariable
{
    @Override
    public String key()
    {
        return "myKey"; 
        // 放在双!间，展示你的值。比如：!myKey!
    }

    @Override
    public boolean isModified(AbstractCard card)
    {
        return myBoolean;
        // 如果改变了基础值，设置为true
    }

    @Override
    public void setIsModified(AbstractCard card, boolean v)
    {
        // 写一些代码使它返回值v
        // 如果你想要正确运行，这个方法是必须的
    }

    @Override
    public int value(AbstractCard card)
    {
        return myInt;
        // 你卡牌被设置的动态变量值，通常来讲会是整型
    }

    @Override
    public int baseValue(AbstractCard card)
    {
        return myInt;
        // 一般和上面的值一样
    }

    @Override
    public boolean upgraded(AbstractCard card)
    {
        return myBoolean;
        // 如果改变了，设置为true
    }
```

接下来在 `receiveEditCards` 中注册。

`BaseMod.addDynamicVariable(new MyVariable());`