# Actions - Side effects

### Define side effects (actions)

Side effects are `fire-and-forgot` actions when a transition is triggered. Each transition can have multiple `actions` to execute. `beat` execute all actions in order.&#x20;

Currently, `beat` support simple callback action and `assign` action.&#x20;

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

> Because Dart's type system does not allow the constant closure and annotations should be constant, you must define action handlers as function before using it.&#x20;

With the above code, you can see the print result every time you execute transitions.&#x20;

Typically, it's easy to pollute your code like `myState.addListener()` styled side effect execution. With `beat`'s actions system, you don't need to handle side effects in other places rather than state definitions. All you need to do is just define it!
