## Basics

Toolbox implements a publisher-subscriber pattern, that allows you to write minimally coupled code in fewer lines. 
You can read more about publisher-subscriber pattern [Here](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern).

To use this pattern you need to work with `Messenger` class, which allows you to subscribe and send messages by calling `Messenger.Subscribe<T>(...)` and `Messenger.Send<T>()` correspondingly.

## Message

To work with messages you need to create one. `Message` is a basic class for all messages, just derive your message from it, add fields and properties and that's it.

```C#
public class PlayerReloadingMessage: Message { }

public class EnemyDiedMessage: Message
{
    public Enemy enemy;
}
```

## Subscribing

To subscribe to a message, you need provide its type and a callback to a `Messenger.Subscribe<T>(...)` method.

```C#
...

protected override void Rise()
{
    Toolbox.Messenger.Subscribe<PlayerReloadingMessage>(_ => OnPlayerReloading());
    Toolbox.Messenger.Subscribe<EnemyDiedMessage>(m => OnEnemyDied(m));
}

private void OnPlayerReloading()
{
    // Do something when player reloading
}

private void OnEnemyDied(Enemy enemy)
{
    m_KilledEnemies.Add(enemy);
}

...
```

You can provide type you need in two ways:

1. By passing type as Generic argument: `Toolbox.Messenger.Subscribe<SomeMessage>(...)`
2. By passing type directly (in case you have instance of a message you want to subscribe to): `Toolbox.Messenger.Subscribe(_someMessage.GetType(), ...)`

## Sending

You can send messages in several ways:

###By type

If you do not have additional data to provide with message, you can just provide message's type.

```C#
...

Toolbox.Messenger.Send<PlayerReloadingMessage>();

...
```

### By instance
If you want to pass additional data to a message, you need to create an instance of it and provide it as a parameter.

```C#
...

Toolbox.Messenger.Send(new EnemyDiedMessage { enemy = this });

...
```

### By cached variable

If you often send a one type of message with different data, you can cache it and send it without creating a new instance.

```C#
...

private GainedXPMessage _gainedXPMessage = new GainedXPMessage();

...

public void OnXpGained(int amount)
{
    //do related stuff
    
    //update message's variable
    m_GainedXPMessage.amount = amount; 
    //then send a message
    Toolbox.Messenger.Send(_gainedXPMessage);
}

...
```

## Subscriber object

Every time you subscribing to a message by `Subscribe` method it returns `Subscriber` object, which defines this subscription. You can use it to manually unsubscribe from it.

```C#
...
private Subscriber _damageTakenSubscriber;
...
public void Awake()
{
    _damageTakenSubscriber = Toolbox.Messenger.Subscribe<DamageTakenMessage>(...);
}
...
public void OnDestroy()
{
    Toolbox.Messenger.RemoveSubscriber(_damageTakenSubscriber);
}

```

You can also remove list or array of subscribers by calling `Toolbox.Messenger.RemoveSubscribers(_listOfSubscribers)`.

To remove all subscribers just call `Toolbox.Messenger.ClearSubscribers()`.

## Binding

You can bind your subscribe to a gameobject lifetime, so it will be automatically removed if gameobject destroyed to prevent memory leaks and errors. Just provide gameobject you want to bind as second parameter.

```C#
Toolbox.Messenger.Subscribe<PlayerReloadingMessage>(_ => OnPlayerReloading, gameObject);
```