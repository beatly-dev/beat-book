# Next events

현재 상태에서 실행가능한 다음 이벤트의 종류를 알아보고 싶다면 아래의 예제를 참조하세요.&#x20;

```dart
myStation.nextEvents; // this will return possible next events as a list of strings
myStation.commonEvents; // this will return common events defined onto enum itself.
```

이를 통해 `send('이벤트')` 와 조합하여 동적으로 다양한 이벤트를 실행할 수 있습니다.&#x20;
