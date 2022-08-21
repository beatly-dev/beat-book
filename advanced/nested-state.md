# Nested state

{% hint style="warning" %}
&#x20;Nested state 에 대한 지원은 아직 불완전합니다. 중첩 상태로 선언하는 대부분의 경우는 독립적인 상태로도 관리할 수 있으므로 최대한 중첩 상태를 줄이고 독립적인 상태로 관리하는 것을 추천합니다.&#x20;
{% endhint %}

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

### Deeply nested stations

`beat` support any depth of the nesting.&#x20;

### Multiple substations in one state - not yet supported

One state can only have one child station. Multiple children are not supported yet. This will be naturally supported if I finish implementing [`Parallel State`](parallel-state.md) which is not yet supported.&#x20;

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
