# Selectable re-rendering

In the previous example, [provider-and-consumer.md](provider-and-consumer.md "mention"), I wrapped the entire widget tree with `CounterProvider` and wrapped the widgets that access to the `CounterBeatStation` with `CounterConsumer`.

You don't need to wrap the root widget with `CounterProvider`(line 15). You can wrap any widget which is the top-most common widget that uses `CounterBeatStation`. And you can use `CounterConsumer` with larger granularity rather than mine.&#x20;

## When does beat rebuild widgets?&#x20;

You have three options to use `{STATE}Consumer`. `beat` rebuilds your widgets depending on your choice.&#x20;

### 1. Rebuild every time enum state and context changes

If you use `ref.station` syntax to read values from it, `beat` will rebuild your widgets every time it changes its state or context.&#x20;

```dart
CounterConsumer(
    builder: (context, ref, _) {
        final state = ref.station.currentState;
        return Text("${state.state}");
    }
)
```

### 2. Rebuild only when the computed value you want is changed

If you use `ref.select` method, `beat` will rebuild your widget only when the computed value has a different result. Through this, you can manage your widget tree finer-grained way.&#x20;

If you just want to retrieve `BeatStation` by testing it, you can use `ref.stationWhen()` method. This method returns `BeatStation` and rebuild your widgets only when the `test` callback returns true.

```dart
CounterConsumer(
    // This will be called only once
    builder: (context, ref, _) {
        // rebuilt only when the refreshed count is changed. 
        final state = ref.select(
            (station) => station.currentState.context.count
        );
        
        // rebuilt only when the counter is an even number
        final station = ref.stationWhen(
            (station) => station.currentState.context.count % 2 == 0
        );
        
        return Text("Is counter an even number? ${state.state}");
    }
)
```

### 3. Never rebuild your widget

If you use `ref.readStation` syntax, `beat` will never rebuild your widget even when the station itself changes to other instances. You can also use `ref.read()` method.

```dart
CounterConsumer(
    // This will be called only once
    builder: (context, ref, _) {
        final state = ref.readStation.currentState;
        // Same result, same effect
        // final state = ref.read((station) => station.currentSate); 
        return Text("${state.state}");
    }
)
```

