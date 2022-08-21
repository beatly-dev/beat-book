# 동적으로 context 생성하기

&#x20;저장하고자 하는 데이터가 단순히 기존 상태의 형태나 컨텍스트에 의존할 뿐만 아니라, 유저의 입력등 다양한 요소에 의해 결정되는 경우가 많습니다. `beat` 는 `EventData.data` 를 통해 이러한 상황을 구현할 수 있도록 지원합니다.&#x20;

```dart
import 'package:beat/beat.dart';

@BeatStation(contextType: String)
enum User {
    @Beat(event: 'login', to: loggedIn, actions: [AssignAction(assignUserName)])
    loggedOut,
    loggedIn,
}

String assignUserName(BeatState state, EventData event) {
    final newName = event.data ?? 'Beatly';
    return newName;
}

void main() {
    final station = UserBeatStation()..start();
    station.send.$login(data: 'Provided name');
}
```

&#x20;앞 장에서 설명했듯이, `EventData.data` 에는 이벤트를 트리거할 때 전달하는 추가적인 데이터가 담겨있습니다. 이벤트를 트리거 할 때 `data` 속성을 지정하면 이 데이터가 자동으로 action 에 전달이 됩니다.&#x20;

`BeatState.context` 에 접근할 때와 마찬가지로 역시나 `as` 구문으로 `EventData.data` 의 타입을 변환하고 사용하길 추천합니다. 혹은 `if (EventData.data is String)` 과 같이 타입체킹을 우선적으로 진행하여 잘못된 데이터를 전달하는 것을 방지할 수 있습니다.&#x20;
