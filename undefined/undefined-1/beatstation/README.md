# BeatStation - 상태 저장소

## \`BeatStation\` 어노테이

`beat` 는 현재 `enum` 자료형만을 지원하고 있습니다. 상태를 저장하는 상태 머신을 생성하기 위해서는 아래의 예제와 같이 간단하게 `BeatStation()` 어노테이션을 추가하기만 하면 됩니다. 다른 `build_runner` 기반의 패키지와 동일하게 생성할 아웃풋 파일의 이름을 미리 `part '{내파일명}.beat.dart';` 로 명시해야 합니다. &#x20;

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

미리 `flutter pub run build_runner watch` 를 실행해 놓았거나, 코드 작성 후 `flutter pub run build_runner build` 를 실행하면&#x20;

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
