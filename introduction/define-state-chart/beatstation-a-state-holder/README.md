# BeatStation - 상태 저장소

## \`BeatStation\` 어노테이션&#x20;

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

미리 `flutter pub run build_runner watch` 를 실행해 놓았거나, 코드 작성 후 `flutter pub run build_runner build` 를 실행하면 필요한 파일들이 생성됩니다.&#x20;

주로 사용할 클래스는 `{enum이름}BeatStation` 과 `{enum이름}BeatState` 클래스입니다.&#x20;

* BeatStation 클래스: 상태를 저장하고 관리하는 주체
* BeatState 클래스: 상태를 표현하는 객체. `state` 프로퍼티에 개발자가 만든 enum 타입으로 상태를 표시, `context` 프로퍼티에 상태와 연관된 데이터를 저장.&#x20;

아래의 코드와 같이 `BeatStation` 을 생성하고 `start()` 메소드를 호출하면 상태저장소를 사용할 수 있습니다.&#x20;

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

{% hint style="warning" %}
다른 작업들을 진하기 이전에 반드시 `start()` 메소드를 호출해야 합니다.&#x20;
{% endhint %}

<mark style="color:red;">****</mark>
