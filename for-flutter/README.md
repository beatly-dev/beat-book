# For Flutter

&#x20;개인 프로젝트의 대부분이 flutter 이기 때문에, 플러터를 최우선 타겟으로 생각하면서 `beat` 를 개발/사용하고 있습니다. 플러터와 함께 `beat` 를 사용하는 경우에는 `beat` 패키지가 아니라 `flutter_beat` 패키지를 설치해야 합니다.&#x20;

&#x20;`flutter_beat` 패키지는 플러터 프레임워크 내에서 Provider / Consumer 패턴을 지원합니다. `flutter_beat` 패키지를 설치하고 `BeatStation` 어노테이션의 `withFlutter` 속성을 `true` 로 설정하면, 자동으로 필요한 Provider 와 Consumer 를 생성합니다.&#x20;

```dart
@BeatStation(contextType: int, withFlutter: true)
enum Counter {
    added,
    taken,
}
```

&#x20;위의 예제에서는 `CounterProvider` 와 `CounterConsumer` 가 생성됩니다. CounterProvider 는 이름 그대로 counter staion 을 위젯 트리에 주입시키는 역할을 하고, CounterConsumer 는 주입된 counter station 을 이용하는 역할을 합니다. 이 밖에도 Consumer 를 자유롭게 만들어 사용할 수 있도록 상속가능한 `CounterConsumerWidget` 을 제공하고, StatefulWidget 과 같은 형태로 상속할 수 있도록 `StatefulCounterConsumerWidget` 과 `CounterConsumerState` 를 제공합니다.&#x20;
