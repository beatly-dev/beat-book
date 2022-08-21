# Callback - 간단한 액션

```dart
/// filename: mystate.dart
import 'package:beat/beat.dart';

part 'mystate.beat.dart';

@BeatStation()
enum MyState {
    @Beat(event: 'goToSecond', to: MyState.second, actions: [
        printTransition
    ])
    first, 
    @Beat(event: 'goToThird', to: MyState.third, actions: [
        printTransition
    ])
    second,
    @Beat(event: 'goToLast', to: MyState.last, actions: [
        printTransition
    ])
    third,
    last,
}
printTransition(BeatState state, EventData event) {
    final eventName = event.event;
    print('Transition from $state with event ${eventName}');
}
```

{% hint style="info" %}
Dart의 타입시스템 특성상 annotation 에는 반드시 const 값만이 들어가야 합니다. `() => null;`, `() {}` 과 같은 클로져는 다트에서 const 로 취급되지 않아서 액션에 들어갈 콜백 함수들은 반드시 이름이 있는 함수형으로 작성되어야 합니다.&#x20;
{% endhint %}

&#x20;위의 코드는 단순히 모든 이벤트 발생시 콘솔에 로그를 기록하는 예제입니다. 다른 패키지 혹은 `beat` 에서도 지원하는, `stream` 이나 `addListener()` 형태를 통해서도 이와 동일한 기능을 작성할 수 있지만, `beat` 에서는 최대한 action 을 활용하길 권장합니다.&#x20;

&#x20;특수한 경우를 제외하고 action 을 활용한다면, 동일한 기능의 액션에 대해 코드를 재사용하기 유리하고, 상태변화와 액션간의 관계를 하나의 파일에서 한눈에 쉽게 알아볼 수 있습니다. 처음에는 어색하고 적응이 안되겠지만 일단 적응이 된 이후에는 높은 생산성을 경험할 수 있습니다.&#x20;
