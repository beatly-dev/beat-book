# 컨텍스트 업데이트 하기

&#x20;컨텍스트의 값을 변경하고자 할 때에는, `Beat` 어노테이션을 선언 할 때 `actions` 필드를 사용할 수 있습니다. `Action` 의 자세한 설명은 xstate 의 자료 혹은 [이 챕터](../../introduction/synchronous-effects-actions/)를 참고하세요.

Action 을 쓰도록 강요하는 이유는, 코드가 좀더 declarative 하도록 돕고, 한 곳에서 상태와 그 상태에 연결된 데이터의 변화를 한눈에 알아볼 수 있기 때문입니다.&#x20;

```dart
import 'package:beat/beat.dart';

@BeatStation(contextType: String)
enum User {
    @Beat(event: 'login', to: loggedIn, actions: [AssignAction(assignUserName)])
    loggedOut,
    loggedIn,
}

String assignUserName(BeatState state, EventData event) {
    final current = state.context as String;
    return 'Beatly';
}
```

> Action에 대한 자세한 내용은  [synchronous-effects-actions](../../introduction/synchronous-effects-actions/ "mention") 를 살펴보세요. &#x20;

&#x20;컨텍스트의 변경은 개발자가 직접하지 않아야 합니다. 위의 예제 처럼 `AssignAction` 클래스의 첫 번째 인자로, 이전 상태나 호출된 이벤트에 따라 새롭게 컨텍스틀 생성하는 콜백함수를 작성해야 합니다. 콜백 함수는 반드시 `contextType` 과 동일한 타입의 새로운 컨텍스트 데이터를 반환해야 합니다. 그렇지 않을 경우 에러가 발생할 수 있습니다.&#x20;

&#x20;콜백함수에는 첫 번째 인자로 현재의 상태가 전달됩니다. `BeatState.state` 를 통해 enum으로 표현된 상태 값을 접근할 수 있으며, `BeatState.context` 를 통해 현재의 컨텍스트 데이터를 사용할 수 있습니다. `BeatState` 는 개발자의 편의를 위해 제공되는 제너럴 클래스 이므로 실제 컨텍스트의 타입이나 enum 타입을 알지 못합니다. 그러므로 위의 예제 처럼 항상 `as` 구문을 통해 나의 컨텍스트 타입으로 변환후 사용하길 권장합니다. 혹은 `beat` 가 자동으로 생성할 BeatState 클래스를 사용해도 좋습니다.&#x20;

```dart
/// beat 가 자동으로 {내 enum 이름}BeatState 를 생성합니다.
String assignUserName(UserBeatState state, EventData event) {
    return '${state.context} - Beatly';
}
```

&#x20;콜백의 두 번째 인자로는 어떤 이벤트가 이 액션 콜백을 실행시키는지에 대한 정보가 전달됩니다. `EventData.event` 에는 개발자가 지정한 이벤트 이름이, `EventData.data` 에는 이벤트를 트리거할 때 전달한 추가 매개변수가 담겨있습니다. 예제에서는 `EventData.event` 에 `'login'` 이 담겨있게 됩니다.&#x20;

다음 장에서는 기존 BeatState 에만 의존하는 것이 아닌, 이벤트 트리거 당시의 데이터에 따라 동적으로 컨텍스트 데이터를 생성하는 간단한 방법을 살펴보겠습니다.
