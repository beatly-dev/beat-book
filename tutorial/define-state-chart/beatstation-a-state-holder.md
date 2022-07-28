# BeatStation - A state holder

## Define state holder with \`BeatStation\` annotation

`beat` currently supports `enum` only.&#x20;

```dart
/// filename: mystate.dart
import 'package:beat/beat.dart';

/// !! You should write this `part '{filename}.beat.dart';` 
/// !! to properly generate code and use the `beat`.
part 'mystate.beat.dart';

@BeatStation()
enum MyState {
    first,
    second,
    third,
    last,
}
```

When you run `dart run build_runner build` or `flutter pub run build_runner build`, above code will generate a `MyStateBeatStation` class and the other stuff to handle state management.&#x20;

You can use `MyStateBeatStation` as a start point.&#x20;

```dart
/// filename: main.dart
import 'mystate.dart';
main() {
    final myState = MyStateBeatStation(MyStateBeatState(
        state: MyState.first
    ));
    final currentState = myState.currentState;
    final realState = currentState.state;
    print(realState); // this will print `MyState.first`
}
```
