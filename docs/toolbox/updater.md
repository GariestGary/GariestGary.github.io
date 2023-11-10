`Updater` is a base class for process and initialize [`MonoCached`](monocached.md) classes. Also you can control time scale for all monos independently of basic Unity's time scale. You can get access to delta time impacted by setted time scale.

Here's list of all methods and properties available for managing objects:

|Method|Description|
|-|-|
|`UnscaledDelta`|Returns default `Time.deltaTime`|
|`TimeScale`|Returns current `Updater`'s time scale, or sets it to value between 0 and infinite|
|`Delta`|Returns current delta time multiplied by `Updater`'s time scale|
|`InitializeObjects(GameObject[] objs)`|Invokes Rise and Ready on given GameObjects, and then adds them to process|
|`RemoveObjectsFromUpdate(GameObject[] objs)`|Removes all GameObjects from process|
|`InitializeObject(GameObject obj)`|Invokes Rise and Ready on given GameObject, and then adds it to process|
|`InitializeMono(MonoCached mono)`|Invokes Rise and Ready on given MonoCached, and then adds it to process|
|`RemoveMonoFromUpdate(MonoCached mono)`|Removes given MonoCached from process|