# Nested state

Defining a nested state is the same as defining a normal state. Define your new station first and then annotate a parent station with it.&#x20;

```dart
import 'package:flutter_beat/flutter_beat.dart';

part 'dog.beat.dart';

@BeatStation()
enum Dog {
  @Beat(event: 'gotoWalk', to: onWalking)
  home,
  @Beat(event: 'goHome', to: home)
  @Substation(Tail)
  onWalking,
}

@BeatStation()
enum Tail {
  @Beat(event: 'wag', to: wagging)
  stopped,
  @Beat(event: 'stop', to: stopped)
  wagging,
}
```

When `DogBeatStation` enters `Dog.onWalking`, `TailBeatStation` starts.&#x20;

### Code reusability - not yet supported

Currently, `beat` does not allow to use of the same substation in more than one parent station. If you do like the below, its behavior is undefined. **This will be fixed soon.** The example below will not work properly.&#x20;

```dart
import 'package:flutter_beat/flutter_beat.dart';

part 'dog.beat.dart';

@BeatStation()
enum Dog {
  @Beat(event: 'gotoWalk', to: onWalking)
  home,
  @Beat(event: 'goHome', to: home)
  @Substation(Tail)
  onWalking,
}

/// Reuse tail station, and it will not work 

@BeatStation()
enum Cat {
  idle,
  @Substation(Tail)
  meow,
}

@BeatStation()
enum Tail {
  @Beat(event: 'wag', to: wagging)
  stopped,
  @Beat(event: 'stop', to: stopped)
  wagging,
}
```
