# Send - 이벤트 전달자



`myState.second$.$goNext();` 형태의 메소드 외에는 `send` 형태의 이벤트 실행 방식을 사용할 수 있습니다. 앞에서 알아본 방식의 이벤트 트리거는 정확이 어떤 상태의 어떤 이벤트를 실행시킬 지 명확하게 지정한다면, 이번 챕터에서 알아볼 `send` 형태의 이벤트 트리거는 현재 상태를 신경쓰지 않고 자유롭게 호출할 수 있다는 장점이 있습니다.&#x20;

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
      //station.send('new'); // 위와 동일
    } else {
      print("Send 'finish' event");
      station.send.$finish();
      //station.send('finish'); // 위와 동일 
    }
    sleep(Duration(milliseconds: 1000));
  }
}

```

위의 예제에서 볼 수 있듯이, `send` 형태의 이벤트 트리거를 사용하면, 현재 상태를 굳이 체크하지 않고, 현재 상태를 접근하는 `on` 프로퍼티도 명시적으로 사용할 필요가 없습니다.&#x20;

`send` 를 통한 이벤트가 호출되면, `beat` 가 알아서 현재 상태를 판단하고, 해당 이벤트를 실행 시킬지 말지를 결정합니다. send를 사용하는 방식에는 `send.$finish()` 처럼 직접 이벤트를 호출하는 방법도 있지만, `send('finish')` 처럼 내가 설정한 이벤트 이름을 send 의 매개변수로 전달하는 방식도 사용가능합니다.&#x20;

### Verbose 와 Send 스타일은 언제 사용하면 좋은가?&#x20;

이 부분은 순전히 개발자의 자유입니다. 저는 보통 `send` 를 사용하는걸 선호합니다. 다만 몇가지 경우에는 둘을 구별하여 사용할 필요가 있습니다.&#x20;

1. 추후에 나오는 nested state를 적용한 경우 명확한 상태 전이를 위해 `on` 프로퍼티를 사용하여 이벤트를 트리거 합니다.&#x20;
2. 동일한 이벤트 이름이 여러개가 있을 때는 `send` 프로퍼티를 사용하여 `beat` 에 판단에 맡깁니다.&#x20;
3. 요청하고 싶은 이벤트가 실시간으로 변경되거나 유저의 입력에 따라 다른 이벤트를 트리거 할 필요가 있을 때는 `send('이벤트')` 스타일을 사용하여 `if/lese` 분기점의 수를 줄입니다.&#x20;
