# Class 형태로 일반 액션 선언하기

&#x20;[assign.md](assign.md "mention") 의 마지막에 설명한 것과 동일하게 일반적인 다른 액션들 또한 클래스 형태로 작성이 가능합니다. `AssignAction` 의 경우에만 `AssignActionBase` 를 상속해야 하고, 그 외의 경우에는 `DefaultAction` 클래스를 상속하면 됩니다.&#x20;

```dart
class ExampleAction<State extends BeatState, Context>
    extends DefaultAction<State, Context> {
  const ExampleAction(this.action);

  @override
  final Context Function(State currentState, EventData event) action;
}
```
