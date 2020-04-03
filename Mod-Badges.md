# 介绍

这些 `32x32` 图像被用于主页标题下显示。点击会打开mod的设置菜单。
* `BaseMod.registerModBadge(Texture texture, String modName, String author, String description, ModPanel settingsPanel)` - 通过BaseMod注册你的Mod图标
* `ModPanel.addButton(float x, float y, Consumer<ModButton> clickEvent)` - 在设置菜单添加一个按钮
* `ModPanel.addLabel(String text, float x, float y, Consumer<ModLabel> updateEvent)` - 在设置菜单添加一个标签

# 举个栗子

```java
ModPanel settingsPanel = new ModPanel();
settingsPanel.addLabel("", 475.0f, 700.0f, (me) -> {
    if (me.parent.waitingOnEvent) {
        me.text = "Press key";
    } else {
        me.text = "Change console hotkey (" + Keys.toString(DevConsole.toggleKey) + ")";
    }
});

settingsPanel.addButton(350.0f, 650.0f, (me) -> {
    me.parent.waitingOnEvent = true;
    oldInputProcessor = Gdx.input.getInputProcessor();
    Gdx.input.setInputProcessor(new InputAdapter() {
        @Override
        public boolean keyUp(int keycode) {
            DevConsole.toggleKey = keycode;
            me.parent.waitingOnEvent = false;
            Gdx.input.setInputProcessor(oldInputProcessor);
            return true;
        }
    });
});

Texture badgeTexture = new Texture(Gdx.files.internal("img/BaseModBadge.png"));
registerModBadge(badgeTexture, MODNAME, AUTHOR, DESCRIPTION, settingsPanel);
```

# 更进一步

查看 `basemod.BaseModInit` 去查看BaseMod是如何创建mod标志的。