# 선택전 이벤트 트리거

&#x20;Choose 액션을 통해서 액션을 선택적으로 실행할 수 있었듯이, 이벤트가 발생시 반드시 상태 전이를 하지 않고 특정한 조건을 만족해야만 상태 전이를 수행할 수 있도록 선언할 수도 있습니다.&#x20;

```dart
@BeatStation()
enum Mike {
    @Beat(event: 'eat', to: full, conditions: [smellsGood])
    hungry,
    full,
}
bool smellsGood(BeatState state, EventData event) {
    return event.data.smellsGood();
}
```

&#x20;위의 예제처럼 `Beat` 나 `EventlessBeat` 선언 시 `conditions` 필드를 사용하면 됩니다. 해당 필드에는 `bool` 을 반환하는 콜백들을 선언할 수 있습니다. 콜백의 인자는 액션의 정의와 동일합니다.&#x20;

&#x20;이렇게 선언된 이벤트들은 `conditions` 필드의 <mark style="background-color:red;">**모든 조건을 만족해야만**</mark> 트리거 됩니다.&#x20;
