# Beat - Transitions

## Define state transitions with \`Beat\` annotation

Now it's time to define state transitions with `Beat` annotation.

```dart
/// filename: mystate.dart
import 'package:beat/beat.dart';

part 'mystate.beat.dart';

@BeatStation()
enum MyState {
    @Beat(event: 'goToSecond', to: MyState.second)
    first, 
    @Beat(event: 'goToThird', to: MyState.third)
    second,
    @Beat(event: 'goToLast', to: MyState.last)
    third,
    last,
}
```

That's it! Run `build_runner` again, and then it will generate everything you need. Now, let's use transitions. All transition events you defined will be prefixed with `$` signs. See the below.&#x20;

```dart
/// filename: main.dart
import 'mystate.dart';
main() {
    final myState = MyStateBeatStation(MyStateBeatState(
        state: MyState.first
    ));
    
    print(myState.currentState.state); // will print `MyState.first`
    
    // `beat` automatically generate these fields and methods. 
    // all transition event methods are prefixed with `$` signs. 
    myState.first.$goToSecond();
    print(myState.currentState.state); // will print `MyState.second`
    
    // This does nothing because it's not in `MyState.first` state.
    // `beat` automatically prevent unrelated transitions being executed
    myState.first.$goToSecond(); 
    print(myState.currentState.state); // will print `MyState.second`
    
    myState.second.$goToThird();
    print(myState.currentState.state); // will print `MyState.third`
    
    myState.third.$gotToLast();
    print(myState.currentState.state); // will print `MyState.last`
    
    // Because `MyState.last` is our last state 
    // which does not have any transitions (`beat` annotation),
    // all the transitions followed will do nothing. 
    myState.second.$goToThird();
    myState.third.$goToLast();
    myState.first.$goToSecond();
    print(myState.currentState.state); // will print `MyState.last`
}
```
