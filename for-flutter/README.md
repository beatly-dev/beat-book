# For Flutter

Flutter is the first citizen I aim to support. `flutter_beat` package is for this reason. <mark style="color:blue;">If you are using flutter rather than Dart-only, then you</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**should**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">add</mark> <mark style="color:blue;"></mark><mark style="color:blue;">`flutter_beat`</mark> <mark style="color:blue;"></mark><mark style="color:blue;">instead of</mark> <mark style="color:blue;"></mark><mark style="color:blue;">`beat`</mark><mark style="color:blue;">.</mark>

`flutter_beat` mainly provides two classes. The first is `consumer` and the second is `provider`. Usage is as same as `beat`. But you have to mark your enum classes with `WithFlutter` annotation. Then, `beat_generator` generates provider and consumer widgets for you.

```dart
@WithFlutter()
@BeatStation(contextType: int)
enum Counter {
    added,
    taken,
}
```

This will generate `CounterProvider` and `CounterConsumer` widgets. If you want to customize the behavior of the consumer, you can extend `CounterConsumerWidget` or `StatefulConsumerWidget`.

Let's see the details in the next chapter.&#x20;
