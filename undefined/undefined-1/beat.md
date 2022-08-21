# Beat - 상태 전이

## \`Beat\` annotation - 상태 전이 이벤트 지정하기

상태를 저장하는 저장소를 만들었다면, 상태를 변경하는 이벤트도 지정할 수 있습니다. 이제 부터 `part` 구문이나 `build_runner` 실행에 대해서는 따로 언급하지 않겠지만, 여전히 앞 챕터에서 말한 대로 `part` 를 포함하고 `build_runner` 를 실행시켜야 합니다.&#x20;

```dart
/// filename: mystate.dart
import 'package:beat/beat.dart';

part 'mystate.beat.dart';

@BeatStation()
enum MyState {
    @Beat(event: 'gotoSecond', to: MyState.second)
    first, 
    @Beat(event: 'gotoThird', to: MyState.third)
    second,
    @Beat(event: 'gotoLast', to: MyState.last)
    third,
    last,
}
```

위 예제의 `Beat` 어노테이션이 상태 전이 이벤트를 지정하는 방법 입니다. `event` 속성을 내가 원하는 이벤트 이름으로, `to` 속성에는 어느 상태로 이동할 것인지를 지정하면 됩니다.&#x20;

아래와 같이 상태 변경 이벤트를 미리 선언하여, 조금 더 깔끔한 코드로도 작성이 가능합니다.&#x20;

```dart
/// filename: mystate.dart
import 'package:beat/beat.dart';

part 'mystate.beat.dart';

@BeatStation()
enum MyState {
    @gotoSecond
    first, 
    @gotoThird
    second,
    @gotoLast
    third,
    last,
}
const gotoSecond = Beat(event: 'gotoSecond', to: MyState.second);
const gotoThrid = Beat(event: 'gotoThird', to: MyState.third);
const gotoLast = Beat(event: 'gotoLast', to: MyState.last);
```

> Dart의 코드 분석 제약사항으로 인해 annotation 에 사용되는 객체는 반드시 `const` 객체이어야 합니다.&#x20;

`beat` 가 알아서 생성한 코드에는 상태를 변화시키는 다양한 코드들이 포함됩니다. 위에 예제에서는 `gotoSecond`, `gotoThird`, `gotoLast` 이벤트에 대응하는 코드들이 생성됩니다. 사용법은 단순합니다. 내가 실행시키고자 하는 이벤트를 직접 실행시키면 됩니다.&#x20;

```dart
/// filename: main.dart
import 'mystate.dart';
main() {
    final myState = MyStateBeatStation(
        firstState: MyState.first
    )..start();
    
    print(myState.currentState.state); // will print `MyState.first`
    
    // all transition event methods are prefixed with `$` signs. 
    myState.onFirst$.$gotoSecond();
    print(myState.currentState.state); // will print `MyState.second`
    
    // This does nothing because it's not in `MyState.first` state.
    // `beat` automatically prevents unrelated transitions from being executed
    myState.onFirst$.$gotoSecond(); 
    print(myState.currentState.state); // will print `MyState.second`
    
    myState.onSecond$.$gotoThird();
    print(myState.currentState.state); // will print `MyState.third`
    
    myState.onThird$.$gotoLast();
    print(myState.currentState.state); // will print `MyState.last`
    
    // Because `MyState.last` is our last state 
    // which does not have any transitions (`beat` annotation),
    // all the transitions followed will do nothing. 
    myState.onSecond$.$gotoThird();
    myState.onThird$.$gotoLast();
    myState.onFirst$.$gotoSecond();
    print(myState.currentState.state); // will print `MyState.last`
}
```

enum 에 정의된 각각의 값들에 대해서 `BeatStation` 내부의 필드로 제공합니다. 다만 충돌 방지를 위해 앞에 `on`을 뒤에  `$` 를 붙여서 생성합니다. 위의 예제에서는 `first`, `second`, `third`, `last` 상태 각각에 대해 `stati`o`n.onFirst`, `station.onSecond`, `station.onThird`, `station.onLast` 를 생성합니다.

생성된 상태 필드에는 사용가능한 이벤트 메소드들이 존재합니다. 각 메소드들의 이름은 개발자가 지정한 `event` 필드의 값에 따라 결정되며, event 필드에 포함된 특수문자 (영문, 숫자를 제외한 모든 문자) 를 제거하고, 앞에 `$` 을 붙여서 생성됩니다.&#x20;

위의 예제처럼 이벤트 트리거 메소드를 실행하면 현재 상태에 따라 이벤트가 발생합니다. 예를 들어 처음 시작을 `MyState.first` 로 시작해서 현재 상태가 `MyState.first` 에 있다, `station.onFirst.gotoSecond()` 는 동작하지만, `station.onSecond.gotoThird()`  는 아무런 동작을 하지 않습니다.&#x20;
