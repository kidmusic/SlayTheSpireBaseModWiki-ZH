# Sprite动画（使用 https://github.com/kiooeht）

从 https://brashmonkey.com/下载**Spriter** 并用它创建动画。它会将角色模型分成单独的材质，以便制作一些单独的骨骼动画。你可以在YouTube上查看基础教程。这是BrashMonkey的官方教程： https://www.youtube.com/playlist?list=PL8Vej0NhCcI5sxOT64-Z31LW59VUyAVfr。制作好Spriter文件后将他放入mod资源目录中。当我们编写继承自 `CustomPlayer` 的类的构造函数时，你可以在 `super` 使用它：

```Java
	public CustomPlayer(String name, PlayerClass playerClass, String[] orbTextures, String orbVfxPath, float[] layerSpeeds, AbstractAnimation animation)
```

关于 `animation` 参数，你可以传入一个 `basemod.animations.SpriterAnimation` 实例，他有非常简单的够赞函数： `public SpriterAnimation(String filepath)`。传入你的Spriter文件路径到 `SpriterAnimation` 构造函数，你就完成了设置Spriter动画的全过程。


# G3DJ 动画（过时的，难以使用)（所以就不翻了）

To make custom character animations you need Blender and fbx-conv:
* Blender (https://www.blender.org/)
* fbx-conv (https://github.com/libgdx/fbx-conv)

In Blender you should just create a bunch of textured quads with your character art on them. (You need to be able to import images as planes). Look up some tutorial for 2d animation to see how to do this and make animations. You need these animations: (TODO list required animations for StS).

You have to make your character quads in the xy-plane with the textures facing in the negative Z direction. Turn on `Backface Culling` in Blender so you can tell if your texture has its normals facing properly.

When exporting to `fbx` from Blender, you **should** be able to leave things at default but here are the requirements:
* scale should be `1.0` to start out, up the scale to what looks right as you test
* `Y-up`
* `forward: -Z`

Then when using fbx-conv to convert the fbx into either a g3db or g3dj file you need to use the `-f` flag for flipping textures because otherwise everything will be upside down.

There are two file formats that libgdx supports: `g3db` and `g3dj`. `g3db` is a binary format whereas `g3dj` is a json format. Right now we only support loading from the json format but I'll get `g3db` loading working soon enough.

Take a look at (https://github.com/gskleres/FruityMod-StS) to see an example of how to get the character to use these files once you've made them. This feature isn't particularly well supported yet so hop on the StS discord on the `unsupported-modding` channel if you need help with it.

Here's an example of how to load a g3dj file:

```java
		// Model loader needs a binary json reader to decode
		JsonReader jsonReader = new JsonReader();
		
		// Create a model loader passing in our json reader
		G3dModelLoader modelLoader = new G3dModelLoader(jsonReader);
		
		// Now load the model by name
		myModel = modelLoader.loadModel(Gdx.files.internal("data/seeker.g3dj"));

                // Necessary to get transparent textures working - I don't know why
 		for (Material mat : myModel.materials) {
 			mat.set(new BlendingAttribute(GL20.GL_SRC_ALPHA, GL20.GL_ONE_MINUS_SRC_ALPHA));
 		}
		
		// Now create an instance. Instance holds the positioning data, etc
		// of an instance of your model
		myInstance = new ModelInstance(myModel, 0, 0, 10.0f);

		// fbx-conv is supposed to perform this rotation for you... it
		// doesnt seem to
		myInstance.transform.rotate(1, 0, 0, -90);

		// You use an AnimationController to um, control animations. Each
		// control is tied to the model instance
		controller = new AnimationController(myInstance);
		// Pick the current animation by name
		controller.setAnimation("bottom|idle", 1, new AnimationListener() {

			@Override
			public void onEnd(AnimationDesc animation) {
				// this will be called when the current animation is done.
				// queue up another animation called "Jump".
				// Passing a negative to loop count loops forever. 1f for
				// speed is normal speed.
				controller.queue("bottom|idle", -1, 1f, null, 0f);
			}

			@Override
			public void onLoop(AnimationDesc animation) {
				// TODO Auto-generated method stub

			}

		});
```

Code required in the `receiveModelRender(ModelBatch batch, Environment env)` function:

```java
		controller.update(Gdx.graphics.getDeltaTime());
		batch.render(myInstance, env);
```