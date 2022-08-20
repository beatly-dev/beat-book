# Listen to changes

You can listen to changes in the station.&#x20;

* `addListener`/ `removeListener`: listen to `currentState.state` changes
* `addContextListener` / `removeContextListener`: listen to `currentState.context` changes

In addition, there are more fine-grained listeners.&#x20;

```dart
@BeatStation
enum Sample {
    one,
    two,
    three,
}
sampleStation.addListener(() {}); // for every changes of enum state
sampleStation.addContextListener((){}); // for changes of context

/// Callback is called if station reaches the specific state. 
sampleStation.addListenerOnOne(() {});
sampleStation.addListenerOnTwo(() {});
sampleStation.addListenerOnThree(() {});
```

Like the above example, other listeners are auto-generated.&#x20;

## Listen to changes by stream

`beat` provides three streams to use.&#x20;

* `stateStream`: notified when `currentState.state` and `currentState.context` changes
* `enumStream`: notified when `currentState.state` changes
* `contextStream:` notified when `currentState.context` changes

```dart
@BeatStation
enum Sample {
    one,
    two,
    three,
}

sampleStation.stateStream.addListener(() {});
sampleStation.enumStream.addListener(() {});
sampleStation.contextStream.addListener() {});
```

If you are using `flutter`, `stream` is much more easier to utilize.&#x20;
