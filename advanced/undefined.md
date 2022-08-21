# 이벤트 트리거 지연시키기

[eventless-transitions.md](../introduction/eventless-transitions.md "mention")에서 `after` 를 통해 트리거를 지연시킬 수 있듯이, 일반적인 `Beat` 도 지연시간을 추가할 수 있으나, 사용법이 조금은 다릅니다.&#x20;

`Beat` 들의 경우에는 `$이벤트()` 호출 시 `after` 매개변수를 설정할 수 있습니다.&#x20;

```dart
// This will be triggered after 1 second.
myStation.send.$gotoNext(after: Duration(milliseconds: 1000));
myStation.send('gotoNext', after: Duration(milliseconds: 1000));
```

## 어차피 Timer 랑 Future.delayed 를 사용할 수 있는데 after 는 왜 지원하나요?

&#x20;이미 `Timer` 와 `Future.delayed` 에 대해 알고있는 분들은 굳이 이 기능을 왜 지원하는지 의아할 수도 있습니다. 이유는 한가지 입니다. `after` 문법을 사용하는 경우, 지연된 이벤트들이 실행되기 이전에 다른 이벤트로 인해 상태가 전이될 때 자동으로 큐잉된 모든 이벤트들을 취소합니다.&#x20;

&#x20;그러나 개발자가 직접 `Timer` 나 `Future.delayed` 를 사용해서 지연시간을 설정하는 경우 그서에 대한 취소나 사이드 이펙트에 대한 책임은 전적으로 개발자에게 있습니다.&#x20;
