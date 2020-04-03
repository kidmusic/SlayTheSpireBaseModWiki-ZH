# API
想要通过BaseMod来添加一个顶部选项，你需要在`PostInitializeSubscriber`  的回调函数 `receivePostInitialize` 中调用下面的方法：

* `BaseMod.addTopPanelItem(TopPanelItem topPanelItem)` 

# 举个栗子
你需要继承 `TopPanelItem` 类来实现该功能：
```Java
public class TopPanelItemExample extends TopPanelItem {
    private static final Texture IMG = new Texture("yourmodresources/images/icon.png");
    public static final String ID = "yourmodname:TopPanelItemExample";

    public TopPanelItemExample() {
	super(IMG, ID);
    }

    @Override
    protected void onClick() {
	// 被点击后的事件
    }
}
```