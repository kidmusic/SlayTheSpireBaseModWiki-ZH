# 一个完整的例子：为战士制作一个起始遗物mod，它会用 `黑暗之血` 代替 `燃烧之血`

```java
import java.util.ArrayList;

import basemod.ModLabel;
import com.megacrit.cardcrawl.characters.AbstractPlayer;
import com.megacrit.cardcrawl.characters.Ironclad;
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

import com.badlogic.gdx.graphics.Texture;
import com.evacipated.cardcrawl.modthespire.lib.SpireInitializer;
import com.megacrit.cardcrawl.core.Settings;
import com.megacrit.cardcrawl.unlock.UnlockTracker;
import com.megacrit.cardcrawl.characters.AbstractPlayer.PlayerClass;

import basemod.BaseMod;
import basemod.ModPanel;
import basemod.interfaces.PostCreateStartingRelicsSubscriber;
import basemod.interfaces.PostInitializeSubscriber;

@SpireInitializer // 这个注解告诉ModTheSpire用这个类来初始化我们的mod
public class SampleMod implements PostCreateStartingRelicsSubscriber, PostInitializeSubscriber {
    public static final Logger logger = LogManager.getLogger(SampleMod.class.getName()); // 输出日志

    public static final String MODNAME = "Sample Mod"; // mod名
    public static final String AUTHOR = "You"; // 作者名
    public static final String DESCRIPTION = "v1.0.0 - makes the Ironclad OP"; // 介绍，你可以在此添加版本号

    public SampleMod() {
        logger.info("subscribing to PostCreateStartingRelics and postInitialize events");
        // 当mod被加载时告诉BaseMod我们想在角色的遗物被创建时做一些事情
        // 同样也是游戏初始化完成时。它会在更早时定义
        BaseMod.subscribe(this);
    }

    public static void initialize() { // ModTheSpire将调用该方法来初始化，以为上面我们使用的注解
        @SuppressWarnings("unused")
        SampleMod mod = new SampleMod();
    }

    @Override
    public void receivePostInitialize() {
        // mod标识
        Texture badgeTexture = new Texture("badge_img.png");
        ModPanel settingsPanel = new ModPanel();
        ModLabel label = new ModLabel("My mod does not have any settings (yet)!", 400.0f, 700.0f, settingsPanel, (me) -> {});
        settingsPanel.addUIElement(label);
        BaseMod.registerModBadge(badgeTexture, MODNAME, AUTHOR, DESCRIPTION, settingsPanel); // 游戏初始化后我们会设置一个标识，告诉玩家我们的mod已经加载
    }

    @Override
    public void receivePostCreateStartingRelics(PlayerClass playerClass, ArrayList<String> relicsToAdd) {
        if (playerClass == PlayerClass.IRONCLAD) { // 仅能在角色为战士时通过它得到遗物
            relicsToAdd.add("Black Blood"); // 我们在这里替换了遗物
            UnlockTracker.markRelicAsSeen("Black Blood");
        }
    }

}
```

# Some mods using BaseMod
* (https://github.com/Gremious/StS-DefaultModBase) - This Mod is highly recommended. Please use this as an excellent jumping off point with examples and good practices! Some of the others below are outdated.
* (https://github.com/gskleres/FruityMod-StS)
* (https://github.com/daviscook477/CustomClimb)
* (https://github.com/ShikiSeiren/NecroMod)
* (https://github.com/t-larson/ColoredMap)
* (https://github.com/twanvl/sts-hybrid-character)
* (https://github.com/Modernkennnern/STS_AlwaysWhale)