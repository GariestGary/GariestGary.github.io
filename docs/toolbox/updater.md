`Updater` is a base class for processing and initializing [`MonoCached`](monocached.md) classes. It also allows you to control the time scale for all monos independently of Unity's basic time scale. You can access delta time impacted by the set time scale.

Here is a list of all methods and properties available for managing objects:

| Method                         | Description                                                                                     |
|--------------------------------|-------------------------------------------------------------------------------------------------|
| `UnscaledDelta`                | Returns the default `Time.deltaTime`.                                                           |
| `TimeScale`                    | Returns the current `Updater`'s time scale or sets it to a value between 0 and infinity.       |
| `Delta`                        | Returns the current delta time multiplied by the `Updater`'s time scale.                      |
| `InitializeObjects(GameObject[] objs)` | Invokes `Rise` and `Ready` on the given GameObjects and then adds them to the process.         |
| `RemoveObjectsFromUpdate(GameObject[] objs)` | Removes all given GameObjects from the process.                                                |
| `InitializeObject(GameObject obj)` | Invokes `Rise` and `Ready` on the given GameObject and then adds it to the process.             |
| `InitializeMono(MonoCached mono)` | Invokes `Rise` and `Ready` on the given `MonoCached` and then adds it to the process.           |
| `RemoveMonoFromUpdate(MonoCached mono)` | Removes the given `MonoCached` from the process.                                                |