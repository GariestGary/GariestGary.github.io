## Basics

If you want to use parts of the Toolbox as dependencies to inject, here's a guide on how to do it using [Zenject](https://assetstore.unity.com/packages/tools/utilities/extenject-dependency-injection-ioc-157735?srsltid=AfmBOopX-iTgJndLt6cApiZS7ZYxSwiMYTm6l63iptcGN4dlXiExqWRy) as an example:

1. Let's assume that we want to bind all Toolbox-related components to the project context. First, move the 'Toolbox Container' game object from the 'MAIN' scene to the 'Project Context' prefab in the `Resources` folder.

2. Uncheck the `Initialize On Start` toggle on the Toolbox Entry component.

3. Set up bindings from instances.

    ```C#
    public class ToolboxInstaller : MonoInstaller
    {
        [SerializeField] private Messenger _Messenger;
        [SerializeField] private Traveler _Traveler;
        [SerializeField] private AudioPlayer _AudioPlayer;
        [SerializeField] private Pooler _Pooler;
        [SerializeField] private ToolboxEntry _Entry;
        [SerializeField] private Updater _Updater;

        public override void InstallBindings()
        {
            Container.Bind<Messenger>().FromInstance(_Messenger).AsSingle();
            Container.Bind<Traveler>().FromInstance(_Traveler).AsSingle();
            Container.Bind<AudioPlayer>().FromInstance(_AudioPlayer).AsSingle();
            Container.Bind<Pooler>().FromInstance(_Pooler).AsSingle();
            Container.Bind<ToolboxEntry>().FromInstance(_Entry).AsSingle();
            Container.Bind<Updater>().FromInstance(_Updater).AsSingle();

            Container.QueueForInject(_Messenger);
            Container.QueueForInject(_Traveler);
            Container.QueueForInject(_AudioPlayer);
            Container.QueueForInject(_Pooler);
            Container.QueueForInject(_Entry);
            Container.QueueForInject(_Updater);
        }
    }
    ```

4. If you want game objects spawned by the pool to be automatically injected, use [this](pooler.md#spawn-and-instantiate-actions) approach and define how your game object needs to be injected.

5. Finally, initialize Toolbox anywhere you need.

    ```C#
    public class MainBootstrapper : MonoBehaviour
    {
        [Inject] private ToolboxEntry _Entry;
        [Inject] private Pooler _Pool;
        [Inject] private DiContainer _Container;

        private void Awake()
        {
            _Pool.SetSpawnAction(OnGameObjectSpawn);
        }

        private void Start()
        {
            _Entry.Init().Forget();
        }

        private void OnGameObjectSpawn(GameObject go)
        {
            _Container.InjectGameObject(go);
        }
    }
    ```

    Notice that setting the spawn action for the pooler must be done before initializing Toolbox.