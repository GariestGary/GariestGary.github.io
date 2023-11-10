`Traveler` class allows you to easily load/unload scenes (additevely or not) and provides UniTasks for you to manage your call orders in code. It also handles initial logic in scenes.

##Scene Handler
Let's imagine that you need some initial preparations in scene before all starts working. To do this, you can define your own `SceneHandler` for this.

```C#
public class GameSceneHandler: SceneHandler<GameSceneArgs>
{
    protected override void SetupScene(GameSceneArgs args)
    {
        
    }
}

public class GameSceneArgs: SceneArgs
{

}
```

Wait a minute! What this GameSceneArgs class means?! 

Exactly what you're thinking about! You can provide additional data to the scene when loading it, e.g. you need to create several levels with different difficulties. Creating a scene for each difficulty seems weird, because you have to recreate most of the objects each time if your scene stays the same in general. Instead of that you can set up one scene and change it's logic depending on data provided to it.

Here's some example:

```C#
public class PingPongSceneHandler: SceneHandler<PingPongSceneArgs>
{
    [SerializeField] private Enemy m_Enemy;

    protected override void SetupScene(PingPongSceneArgs args)
    {
        m_Enemy.SetSpeed(args.EnemySpeed);
    }
}

[Serializable]
public class PingPongSceneArgs: SceneArgs
{
    public float EnemySpeed;
}
```

In that example we're setting up enemy's speed defined by `PingPongSceneArgs`. `SceneArgs` class derived from `ScriptableObject`, so you can create some `PingPongSceneArgs` assets and bind it to difficulty buttons in your menu scene.

You can also override `OnSceneUnload` method, to do some logic before scene unloads.

##Scene Loading

To load scene simply call:

```C#
Traveler.LoadScene("MySceneName", new MySceneArgs { data = "Hello World!" });
```

It will return a UniTask, so you can await it, to manage your calls if you need:

```C#
await Fader.In(0.2f); 
await Traveler.LoadScene("MySceneName");
await Fader.Out(0.2f);
```

##Scene Unloading

To unload scene simply call:

```C#
Traveler.UnloadScene("MySceneName");
```

You can also unload all scenes, except `MAIN`, by calling:

```C#
Traveler.UnloadAllScenes();
```

This methods will also returns a UniTask.

##Loading order

After calling `LoadScene` of [Traveler](traveler.md)'s class, it will be waiting before current operations (loading/unloading scenes) finished his work. Then scene will be loaded usually, and `SceneLoadingMessage` will be fired. After scene finished loading `SceneLoadedMessage` fires and all objects in scene will be traversed. If any object contains `SceneHandler`, then it will be initialized by [`Updater`](updater.md) and starts preparing scene. Then all objects in scene will be initialized and `SceneOpenedMessage` fires.

On `UnloadScene` call it will be wait until current operations (loading/unloading scenes) done it's work. Then `SceneUnloadingMessage` fires, if `SceneHandler` on this scene exists, then `OnSceneUnload` method invokes and all objects in scene removes from [`Updater`](updater.md) processing. Finally, it unloads scene usually, waits for it's finish and fires `SceneUnloadedMessage`.

