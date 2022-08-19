# Send event

Refer to[ the previous example](./), you can use `DogBeatStation` to send the events to the nested stations.&#x20;

```dart
final myStation = DogBeatStation()..start();

myStation.send.$gotoWalk(); 
// DogBeatStation enters `onWalk` state, `TailBeatStation` starts automatically.

// now you can send events one of these.
myStation.send.$wag(); // this changes `TailBeatStation`'s state to `wagging`. 
myStation.send.$goHome(); // this will stop `TailBeatSTation`, and change DogBeatStation's state to `home`
```

If you don't like `send`-styled transitions, you can specify the events.&#x20;

```dart
myStation.tail$.send.$wag(); // same as the myStation.send.$wag();
myStation.tail$.onIdle$.$wag(); // same with the above
```

## Duplicated event name resolution

If nested stations have the same event names, then the first station from the root will handle that event.&#x20;

```dart
@BeatStation()
enum Root {
    @Beat(event: 'travel', to: traveling)
    idle,
    @Beat(event: 'stop', to: idle)
    @Substation(Child)
    traveling,
}

@BeatStation()
enum Child {
    @Beat(event: 'travel', to: traveling)
    idle,
    @Beat(event: 'stop', to: idle)
    @Substation(GrandChild)
    traveling,
}

@BeatStation()
enum GrandChild {
    @Beat(event: 'travel', to: traveling)
    idle,
    @Beat(event: 'stop', to: idle)
    traveling,
}
```

In the above example, if you call `station.send.$stop()` then `Root` station goes to idle and the other station stops because the root station exit from the traveling state.&#x20;

#### Specify nested transition

But if you don't want this behavior, you can specify the transition of children by using verbose-styled trigger.&#x20;

```dart
station.child$.grandChild$.send.$stop(); 
station.child$.grandChild$.send.$travel(); 
```

The parents will not be triggered by this.&#x20;
