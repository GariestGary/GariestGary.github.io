## Basics

Let's start this topic by defining what 'Pool' means. Here you can read all about this pattern: 

[Object pool pattern](https://en.wikipedia.org/wiki/Object_pool_pattern).

In simple words, use the pool pattern when you are working with a lot of instantiating and destroying objects.

Toolbox has its own implementation of this pattern. A simple way you can use it is to define pools you need in [Toolbox settings](toolbox-settings.md) and call `Pooler.Spawn(...)` where you need.

```C#
...

public void Shoot()
{
    var bullet = Pooler.Spawn("Bullet", currentWeapon.tip.position, Quaternion.identity).GetComponent<Bullet>();
    bullet.StartMovingForward();
}

...
```

To configure where pooled objects will be contained, go to the 'MAIN' scene. In the `Pooler` component of the 'Toolbox Container' game object, set your own `Transform` in the `Predefined Root` field.

<img src="predefined_root.png">

By default, it will create a new root at runtime in the 'MAIN' scene.

## Adding pool

To create pools, which will be initialized on start, go to 'Toolbox Settings' and click on the 'Pooler' tab. Here you can add or remove pools, change their tags, prefabs and capacity, and set the garbage collector's interval.

<img src="pooler_empty.png">

You can create a new pool by clicking the 'Add Pool' button.

<img src="pool_added.png">

Each pool has the following parameters:

- `Pool Tag`: The tag through which you can retrieve a pooled object.
- `Prefab`: Prefab which the pool will instantiate objects from.
- `Initial Pool Size`: Count of objects that will be added to the pool at start.

<img src="pool_filled.png">

To delete a pool, simply click the button with the trash icon.

The size of the pool also describes how many objects you can spawn without instantiating a new one when it's all over.

You can create a pool manually in code by calling `Pooler.TryAddPool(PoolData poolToAdd)` or `Pooler.TryAddPool(string tag, GameObject prefab, int capacity)`. The pool won't be added if another pool already uses the specified tag.

You can also remove a pool by calling `Pooler.TryRemovePool(string tag)`.

## Scene pools

What if you want to use some pools in a specific scene? Manually creating and removing pools each time seems boring and complicated. For this reason, Toolbox has a `Scene Pool` component, which automatically creates specified pools when the scene is loaded and removes them when it is unloaded.

<img src="scene_pool.png">

## Creating objects

It is recommended to create all objects by using `Pooler`, because it works with [`Updater`](updater.md) and initializes objects to work properly with Toolbox.

An alternative to Unity's basic `Instantiate(...)` method is `Pooler.Instantiate(...)`. Use it when you need to create an object without using a pool.

## Destroying objects

<span style="color:red">IMPORTANT!</span> Do not destroy objects that are created by the `Pooler.Spawn(...)` method, use `Pooler.TryDespawn(GameObject obj)` instead. It will remove the object from processing and return it back to the pool.

If you do not know exactly what object you are working with, use the `Pooler.DespawnOrDestroy(GameObject obj)` method, so `Pooler` decides for itself whether to destroy or despawn the object. If the object is part of any existing pool, it will be despawned; otherwise, it will be destroyed.

Both methods return a bool that indicates whether the object was despawned or destroyed or not.

## Scene operations handling

There might be a situation when you are unloading a scene that contains spawned objects. What will happen to these objects? Pooler automatically checks if you are unloading a scene, thanks to [Messenger](messenger.md), and returns all spawned objects back to their pools. So you can safely load and unload scenes, `Pooler` will handle all necessary stuff.

## Garbage Collector

When you spawn a lot of objects, after some amount of time you end up with a bunch of objects that you don't need, but they are still in memory. 
To solve this problem, `Pooler` has a 'Garbage Collector' that cleans up all objects in pools until they hold an initial capacity of objects.

The garbage collector works by checking all pools every amount of time you specified in 'Toolbox Settings'. If a pool holds more objects than the initial capacity, then the extra objects will be destroyed.

You can manually tell the garbage collector to clean up pools by calling `Pooler.ForceGarbageCollector()`.