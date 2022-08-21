# Listen to changes

&#x20;아래의 메소드들을 통해 다양한 변화에 반응하는 콜백들을 등록할 수 있습니다.&#x20;

* `addListener`/ `removeListener`: listen to `currentState.state` changes
* `addContextListener` / `removeContextListener`: listen to `currentState.context` changes

&#x20;뿐만 아니라 `beat` 는 좀더 작은 단위의 변화에 대한 콜백 리스너도 지원합니다.&#x20;

```dart
@BeatStation
enum Sample {
    one,
    two,
    three,
}
sampleStation.addListener(() {}); // for every changes of enum state
sampleStation.addContextListener((){}); // for changes of context

/// Callback is called if station reaches the specific state. 
sampleStation.addListenerOnOne(() {});
sampleStation.addListenerOnTwo(() {});
sampleStation.addListenerOnThree(() {});
```

## Listen to changes by stream

&#x20;다트의 기본적인 reactive 데이터인 `stream` 또한 지원하고 있습니다.&#x20;

* `stateStream`: notified when `currentState.state` and `currentState.context` changes
* `enumStream`: notified when `currentState.state` changes
* `contextStream:` notified when `currentState.context` changes

```dart
@BeatStation
enum Sample {
    one,
    two,
    three,
}

sampleStation.stateStream.addListener(() {});
sampleStation.enumStream.addListener(() {});
sampleStation.contextStream.addListener() {});
```

&#x20;플러터를 메인 프레임으로 사용하는 개발자라면  이 stream을 통해서 `StreamBuilder` 로 쉽게 위젯을 생성할 수 있습니다.&#x20;
