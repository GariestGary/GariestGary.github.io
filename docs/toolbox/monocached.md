## Basics

Toolbox has its own update system, which wraps around [Unity's basic update cycle](https://docs.unity3d.com/Manual/ExecutionOrder.html) and performs its own calculations.

`MonoCached` is the main component of this system. It is analogous to MonoBehaviour with some extra features described below.

`MonoCached` has the same update cycle-related methods, but they are named differently. It is closely related to [`Updater`](updater.md), so you can precisely control the lifetime and update behavior of each object. Here is a list of all "overridden" methods recommended for use instead:

| Basic       | Analogue   |
|-------------|------------|
| `Awake`     | `Rise`     |
| `Start`     | `Ready`    |
| `Update`    | `Tick`     |
| `FixedUpdate` | `FixedTick` |
| `LateUpdate` | `LateTick` |
| `OnDestroy` | `Destroyed` |
| `OnEnable`  | `OnActivate` |
| `OnDisable` | `OnDeactivate` |

Additionally, `MonoCached` has methods and properties to control its lifecycle:

| Method                     | Description                                                                                     |
|----------------------------|-------------------------------------------------------------------------------------------------|
| `Pause`                   | Pauses the execution of `Tick`, `FixedTick`, and `LateTick` methods.                          |
| `Resume`                  | Resumes the execution of `Tick`, `FixedTick`, and `LateTick` methods.                         |
| `EnableGameObject`        | Enables the `gameObject` on which the component is placed.                                    |
| `DisableGameObject`       | Disables the `gameObject` on which the component is placed.                                   |
| `ProcessIfInactiveSelf`   | Bool property. If enabled, `Tick`, `FixedTick`, and `LateTick` methods will be executed even if the `gameObject` is inactive self. |
| `ProcessIfInactiveInHierarchy` | Bool property. If enabled, `Tick`, `FixedTick`, and `LateTick` methods will be executed even if the `gameObject` is inactive in the hierarchy. |
| `delta`                   | Float property. Delta time, controlled by timescale, interval, etc.                             |
| `fixedDelta`              | Float property. Fixed delta time, controlled by timescale, interval, etc.                     |
| `Interval`                | Float property. Sets the interval between calling `Tick`, `FixedTick`, and `LateTick` methods. Delta time will then be the sum of deltas between the last call and the next. If set to 0, it works as usual. |

To utilize the full power of 'Toolbox', simply derive your class from `MonoCached` and override the methods you need.

Here's an example:

```C#
public class Rocket : MonoCached
{
    private float speed = 10;
    private SpriteRenderer spriteRenderer;

    protected override void Rise()
    {
        spriteRenderer = GetComponent<SpriteRenderer>();
    }

    protected override void Tick()
    {
        // Here we reduce update calls if we don't see the object
        if (/* There is some logic to check if the object is out of the screen */)
        {
            Interval = 1;
        }
        else
        {
            Interval = 0;
        }

        // Delta time will be the sum of deltas on every frame between the last call of this method and the current
        transform.position = transform.forward * speed * delta;
    }
}
```

Because `MonoCached` is derived from MonoBehaviour, you can still use `Awake`, `Update`, etc.