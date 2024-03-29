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

The main classes you will use are `{YourEnumClassName}BeatStation` and `{YourEnumClassName}BeatState`.&#x20;

* {Some}BeatStation is holding the state which is {Some}BeatState
* {Some}BeatState represents the state&#x20;

You can use `MyStateBeatStation` as a start point and `MyStateBeatState` to define an initial state.

```dart
/// filename: main.dart
import 'mystate.dart';
main() {
    final myState = MyStateBeatStation(
        firstState: MyState.first
    )..start();
    final currentState = myState.currentState;
    final realState = currentState.state;
    print(realState); // this will print `MyState.first`
}
```

> <mark style="color:red;">**You must call**</mark><mark style="color:red;">** **</mark><mark style="color:red;">**`start()`**</mark><mark style="color:red;">** **</mark><mark style="color:red;">**method before using the station.**</mark>&#x20;

If you don't provide the `firstState` then the first field in your enum will be the initial state.&#x20;

<mark style="color:red;">****</mark>
