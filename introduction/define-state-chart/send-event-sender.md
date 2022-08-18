# Send - Event sender

Although `myState.second$.$goNext();`-styled event trigger is somewhat verbose, it's a bit bothering to type same event name.

To solve this bother, **beat** provides a sender-styled event trigger. Let's see an example.

```dart
import 'dart:io';
import 'dart:math';

import 'package:beat/beat.dart';

part 'use_sender.beat.dart';

@BeatStation()
enum Assignment {
  @Beat(event: 'finish', to: Assignment.done)
  doing,

  @Beat(event: 'new', to: Assignment.doing)
  done,
}

void main(List<String> arguments) {
  final station =
      AssignmentBeatStation(firstState: Assignment.doing)..start();

  station.addListener(() {
    print("State changed to ${station.currentState}");
  });

  while (true) {
    if (Random().nextBool()) {
      print("Send 'new' event");
      station.send.$new();
    } else {
      print("Send 'finish' event");
      station.send.$finish();
    }
    sleep(Duration(milliseconds: 1000));
  }
}

```

The verbose-styled trigger should type the exact state to call transition like `station.doing$.$finish()`. But the sender-styled trigger is simpler. You don't need to type the state. The `send` field has all the events you defined.&#x20;

This approach is easier to handle events when your events are similar to the previous example - [Beat transition tutorial](beat-transitions.md). If you have same events name for many states, you can just type `station.send.$sameEventName()`

Let's revise the [Beat transition tutorial](beat-transitions.md).

```dart
/// filename: main.dart
myState.send.$goToNext();

myState.send.$goToNext(); 

myState.send.$goToNext();

myState.send.$gotToNext();
    
```

Now you don't need to care about whether the current state is `first`, `second`, `third`, or `last`. The sender will care about everything. It will trigger an appropriate event for you.&#x20;

### When to use verbose? sender?

It's up to you! If you want to specify the event then you can use the verbose-styled trigger. If you don't want to care about the state then you can use the sender-styled trigger.&#x20;
