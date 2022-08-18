# Selectable re-rendering

In the previous example, [provider-and-consumer.md](provider-and-consumer.md "mention"), I wrapped the entire widget tree with `CounterProvider` and wrapped the widgets that access to the `CounterBeatStation` with `CounterConsumer`.

You don't need to wrap the root widget with `CounterProvider`(line 15). You can wrap any widget which is the top-most common widget that uses `CounterBeatStation`. And you can use `CounterConsumer` with larger granularity rather than mine.&#x20;

## The beauty of selectable re-rendering

But the reason I wrapped widgets with `CounterConsumer` in a fine-grained way (line 51, 59, 64, 72, 83) is that `flutte_beat` provides a fine-grained notification on context/state change. I did it on purpose to show you how `flutter_beat` support a fine-grained notification system. If you run the[ example application](provider-and-consumer.md), you can see that only the widget associated with the changed property of `CounterBeatStation` will re-render as the state changes.&#x20;

Using `ref`, you can access `station`, `currentState`, `enumState` and `context` fields. Each of them refers `BeatStation`, `BeatState`, the state represented as your enum class, and context data associated with your state.&#x20;

Furthermore, if you assign your custom class context type, you can access its properties right away. In the [previous example](provider-and-consumer.md), I access the `count` field in the `CounterContext` class directly with `ref.$$count`. To prevent variable name duplication `flutter_beat` suffixes your class' fields with `$$`.&#x20;

```dart
CounterConsumer(
  builder: (context, ref, _) {
    return Text(
      '${ref.$$count ?? 0}',
      style: Theme.of(context).textTheme.headline4,
    );
  },
),
```

In the above case, `Text` widget will re-render only in two cases. The first is when the entire `CounterBeatStation` is reset or changed. The second is when the `$$count` field is changed. Change in the other part of the `CounterBeatStation` does not affect to `Text` widget.&#x20;

This is the beauty of selectable re-rendering.&#x20;
