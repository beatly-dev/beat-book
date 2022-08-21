# Assign - 컨텍스트 변경

자세한 내용은 앞선 챕터인 [undefined.md](../../undefined/context/undefined.md "mention")를 참조 하세요&#x20;

## 클래스 형태로 Assign 액션 구현하기&#x20;

`beat` 는 단순한 콜백 형태의 액션 뿐만 아니라 클래스 형태로도 액션을 구현할 수 있도록 기반 클래스를 제공합니다.&#x20;

`AssignActionBase` 클래스를 상속해서 `action` getter를 원하는 함수로 작성하면 됩니다.&#x20;

```dart
@BeatStation(contextType: String)
enum User {
    @Beat(event: 'login', to: loggedIn, actions: [AssignNewName()])
    loggedOut,
    loggedIn,
}

class AssignNewName extends AssignActionBase {
  const AssignNewName();
  @override
  String Function(BeatState currentState, EventData event) get action => (_, __) {
    return 'Beatly';
  };
}
```

{% hint style="info" %}
&#x20;앞에서 언급했듯이 annotation 에 들어가는 모든 값은 `const` 값이어야 합니다. 그러므로 클래스 형태로 액션을 작성할 때 생성자가 반드시 `const` 생성자를 지원해야 합니다.&#x20;
{% endhint %}
