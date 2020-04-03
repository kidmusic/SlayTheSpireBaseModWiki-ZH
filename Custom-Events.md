# 自定义事件

首先你要做的就是创建一个继承自AbstractEvent的类。（或者任何继承自它的抽象类） 

下面是一个简单的例子：

```java
public class MyFirstEvent extends AbstractImageEvent {

    //这在技术上不是必须的，但后面会有用
    public static final String ID = "My First Event";

    public MyFirstEvent(){
        super(ID, "My body text", "img/events/eventpicture.png");
        
        //这里创建提示选项
        this.imageEventText.setDialogOption("My Dialog Option"); //把选项加入了选项列表
    }

    @Override
    protected void buttonEffect(int buttonPressed) {
        //最好查看游戏源码中的事件是如何编写的，但事实上但你点击了事件选项时，它就会索引你到该方法。
    }
}
```

你可以在[这里](https://github.com/GraysonnG/InfiniteSpire/blob/master/src/main/java/infinitespire/events/EmptyRestSite.java)学习自定义事件的高级用法。

# 添加事件

现在我们已经创建了事件，接下来我们将把这个事件添加到事件池中，好在BaseMod提供了这样一个处理器。

你所需要做的就是在receivePostInitialize中调用下面的方法： 

`BaseMod.addEvent(String eventID, Class<? extends AbstractEvent> class, String dungeonID)`
* `eventID` : 事件ID，必须和你在eventStrings.json中设置的ID保持一致
* `class` : 你要添加的事件类
* `dungeonID` : 你想添加事件的地牢的ID（例如： `Exordium.ID`, `TheCity.ID`, `TheBeyond.ID` ），忽略此参数将会把事件添加到所有池中

一个小例子:

```java
@Override
public void receivePostInitialize() {
    BaseMod.addEvent(MyFirstEvent.ID, MyFirstEvent.class);
    BaseMod.addEvent(MySecondEvent.ID, MySecondEvent.class, TheCity.ID);
}
```