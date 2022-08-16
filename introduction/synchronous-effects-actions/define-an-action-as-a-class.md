# Define an action as a class

Similarly as described in [assign-change-context.md](assign-change-context.md "mention"), you can define other actions by extending `DefaultAction` class in the same way as extending `AssignActionBase`. Or, you can extend other action classes' base class, suffixed with `Base`. &#x20;

```dart
class ExampleAction<State extends BeatState, Context>
    extends DefaultAction<State, Context> {
  const ExampleAction(this.action);

  @override
  final Context Function(State currentState, EventData event) action;
}
```
