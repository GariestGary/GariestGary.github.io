##Basics

Let's start this topic by defining what 'Pool' means. Here you can read all about this pattern: [Object pool pattern](https://en.wikipedia.org/wiki/Object_pool_pattern). In simple words, use pool patter when you are working with a lot of instantiating and destroying objects.

Toolbox has his own implementation of this pattern. Simple way you can use it is to define pools you need in [Toolbox settings](toolbox-settings.md) and call `Pooler.Spawn(...)` where you need.

<img src="pooler_use_example_settings.png">

```C#
...

public void Shoot()
{
    var bullet = Pooler.Spawn("Bullet", currentWeapon.tip.position, Quaternion.identity).GetComponent<Bullet>();
    bullet.StartMovingForward();
}

...
```

Here you can see that to create pool you need to define it's tag, prefab and capacity.

Capacity of the pool describes how many objects you can spawn without instantiating a new one, when it's all over.

You can create pool manually in code by calling `Pooler.TryAddPool(PoolData poolToAdd)` or `Pooler.TryAddPool(string tag, GameObject prefab, int capacity)`. Pool won't be added if other pool already uses specified tag.

Also you can remove pool by calling `Pooler.TryRemovePool(string tag)`.

##Creating objects

It is recommended to create all object by using `Pooler`, because it works with [`Updater`](updater.md) and initializes objects to work properly with Toolbox.

Alternative to basic Unity's `Instantiate(...)` method is `Pooler.Instantite(...)`. Use it when you need to create object without using pool.

##Destroying objects

IMPORTANT! Do not destroy objects you created by using `Pooler.Spawn(...)` method, use `Pooler.TryDespawn(GameObject obj)` instead. It will remove object from processing and returns it back to pool.

You can also use `Pooler.DespawnOrDestroy(GameObject obj)` method, so `Pooler` decide by itself to destroy or despawn object. If object are part of any existing pool it will be despawned otherwise it will be destroyed.

Both methods returns bool that indicates if objects despawned or destroyed or not.

##Scene operations handling

There might be situation when you unloading scene that contains spawned objects. What will happen to this objects? Pooler automatically checks if you are unloading scene, thanks to [Messenger](messenger.md), and returns all spawned objects back to its pools. So you can safely load and unload scenes, `Pooler` will handle all necessary stuff.

##Garbage Collector

