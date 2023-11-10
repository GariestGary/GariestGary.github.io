##Basics

Toolbox has own update system, which wraps around basic Unity's update cycle and doing his own calculations.

Main component in this system is `MonoCached`, which are analogue to MonoBehaviour with some extra described below.

`MonoCached` has same update cycle related methods, but named differently. Here are list of all "overrided" methods:

|Basic|Analogue|
|-|-|
|`Awake`|`Rise`|
|`Start`|`Ready`|
|`Update`|`Tick`|
|`FixedUpdate`|`FixedTick`|
|`LateUpdate`|`LateTick`|
|`OnDestroy`|`Destroyed`|
|`OnEnable`|`OnActivate`|
|`OnDisable`|`OnDeactivate`|

Also, `MonoCached` has methods and properties to control lifecycle:

|Method|Description|
|-|-|
|`Pause`|Pauses execution of `Tick`, `FixedTick` and `LateTick` methods|
|`Resume`|Resumes execution of `Tick`, `FixedTick` and `LateTick` methods|
|`EnableGameObject`|Enables `gameObject` on which component placed|
|`DisableGameObject`|Disables `gameObject` on which component placed|
|`ProcessIfInactiveSelf`|Bool property. If enabled, then `Tick`, `FixedTick` and `LateTick` methods will be executed even if gameObject inactive self|
|`ProcessIfInactiveInHierarchy`|Bool property. If enabled, then `Tick`, `FixedTick` and `LateTick` methods will be executed even if gameObject inactive in hierarchy|
|`delta`|Float property. Delta time, controlled by timescale, interval etc|
|`fixedDelta`|Float property. Fixed delta time, controlled by timescale, interval etc|
|`Interval`|Float property. Sets the interval between calling `Tick`, `FixedTick` and `LateTick` methods. Delta time then will be a sum of deltas between last call and next. If setted to 0, then it works usually|

To use full power of 'Toolbox', just derive your class from `MonoCached` and override method you need.

Here's some example:

```C#
public class Rocket: MonoCached
{
    private float speed = 10;
    private SpriteRenderer sr;

    protected override void Rise()
    {
        sr = GetComponent<SpriteRenderer>();
    }

    protected override void Tick()
    {
        //Here we reducing update calls if we don't see the object
        if(/*There are some logic to check if object is out of screen*/)
        {
            Interval = 1;
        }
        else
        {
            Interval = 0;
        }

        //Delta time will be a sum of deltas on every frame between last call of this method and next
        transform.position = transform.forward * speed * delta;
    }
}
```

Because of `MonoCached` derived from MonoBehaviour you can still use `Awake`, `Update`, etc...