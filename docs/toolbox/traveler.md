The `Traveler` class allows you to easily load and unload scenes (additively or not) and provides UniTasks to manage your call orders in code. It also handles initial logic in scenes.

## Scene Handler

Imagine you need some initial preparations in a scene before everything starts working. To do this, you can define your own `SceneHandler` for the scene.

```C#
public class GameSceneHandler : SceneHandler<GameSceneArgs>
{
    protected override void SetupScene(GameSceneArgs args)
    {
        // Initialization logic here
    }
}

public class GameSceneArgs : SceneArgs
{
    // Define additional data here
}
```

### What is the `GameSceneArgs` Class?

The `GameSceneArgs` class allows you to provide additional data to the scene when loading it. For example, you might need to create several levels with different difficulties. Instead of creating a separate scene for each difficulty, you can set up one scene and change its logic based on the data provided to it.

Here's an example:

```C#
public class PingPongSceneHandler : SceneHandler<PingPongSceneArgs>
{
    [SerializeField] private Enemy m_Enemy;

    protected override void SetupScene(PingPongSceneArgs args)
    {
        m_Enemy.SetSpeed(args.EnemySpeed);
    }
}

[Serializable]
public class PingPongSceneArgs : SceneArgs
{
    public float EnemySpeed;
}
```

In this example, we set the enemy's speed defined by `PingPongSceneArgs`. The `SceneArgs` class is derived from `ScriptableObject`, so you can create multiple `PingPongSceneArgs` assets for each difficulty and bind them to difficulty buttons in your menu scene.

You can also override the `OnUnloadCallback` method to perform some logic before the scene unloads.

## Scene Loading

To load a scene, simply call:

```C#
Traveler.LoadScene("MySceneName", new MySceneArgs { data = "Hello World!" });
```

This method returns a UniTask, so you can await it to manage your calls if needed:

```C#
await Fader.In(0.2f);
await Traveler.LoadScene("MySceneName");
await Fader.Out(0.2f);
```

## Scene Unloading

To unload a scene, simply call:

```C#
Traveler.UnloadScene("MySceneName");
```

You can also unload all scenes except `MAIN` by calling:

```C#
Traveler.UnloadAllScenes();
```

These methods also return a UniTask.

The `LoadScene` method has an overload that allows you to get the `SceneHandler`. If you know the type of `SceneHandler` for the scene you are loading, you can use this type as a generic:

```C#
var handler = await Traveler.LoadScene<PingPongSceneHandler>();
```

If you want to get the `SceneHandler` after opening a scene, you can still do that using the following syntax:

```C#
var handler = Traveler.TryGetSceneHandler<PingPongSceneHandler>();
```

or

```C#
var handlers = Traveler.TryGetAllSceneHandlers<PingPongSceneHandler>();
```

if you have multiple scenes with this type of handler.

## Loading Order

After calling the `LoadScene` method of the `Traveler` class, it will wait for the current operations (loading/unloading scenes) to finish. Then the scene will be loaded as usual, and a `SceneLoadingMessage` will be fired. After the scene finishes loading, a `SceneLoadedMessage` fires, and all objects in the scene will be traversed. If any object contains a `SceneHandler`, it will be initialized by the [`Updater`](updater.md) and will start preparing the scene. Then all objects in the scene will be initialized, and a `SceneOpenedMessage` will fire.

<span style="color:red">IMPORTANT!</span> The scene handler must be a root game object in the scene.

On the `UnloadScene` call, it will wait until the current operations (loading/unloading scenes) are done. Then a `SceneUnloadingMessage` fires. If a `SceneHandler` exists on this scene, the `OnSceneUnload` method will be invoked, and all objects in the scene will be removed from the [`Updater`](updater.md) processing. Finally, it unloads the scene as usual, waits for it to finish, and fires a `SceneUnloadedMessage`.