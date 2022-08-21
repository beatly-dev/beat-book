# 선택적 렌더링

&#x20;기본 예제를 보면 이상하고 불편한 부분들이 있습니다. 바로 전체 위젯을 Consumer 로 감싸지 않고, station 을 사용하는 일부분만을 Consumer 로 감싸고 있다는 점입니다. 특히나, 단순히 `ref.station` 을 사용하지 않고 `ref.select` 혹은 `ref.readStation` 을 사용하는 부분들도 존재하고 있습니다.&#x20;

&#x20;이는 `flutter_beat` 가 지원하는 선택적 렌더링을 설명하기 위해 일부러 그렇게 만들어놓은 것이기 때문입니다. Consumer 를 사용할 때 `ref.station` 을 통해 스테이션을 접근하는 경우 스테이션의 enum 상태나 context 정보가 바뀌는 경우에 항상 다시 렌더링을 하게 됩니다.&#x20;

&#x20;하나의 위젯에서 station 내부의 매우 다양한 정보를 사용하는 경우에는 어쩔 수 없는 선택지가 되겠지만, 카운터 예제와 같이 단순히 상태정보도 필요없고 카운터 값이 바뀔 때만 렌더링을 하면 되는 경우에는 불필요한 컴퓨팅 리소스를 사용하는 단점이 있습니다.&#x20;

&#x20;`flutter_beat` 는 이를 방지하고자 어느 시점에 렌더링을 다시 수행할 지 지정할 수 있는 `select` 문법을 제공합니다.&#x20;

## flutter\_beat 사용 시 위젯이 렌더링 되는 시점

flutter\_beat 의 consumer 는 크게 3가지의 사용방법을 제공하고, 이에 따라서 렌더링 하는 시점이 달라지게 됩니다.&#x20;

### 1. ref.station - 무조건 렌더링

`ref.station` 을 통해 스테이션에 접근하는 경우에는 enum 상태가 변하거나 context 상태가 변하면 무조건 다시 렌더링이 됩니다.&#x20;

```dart
CounterConsumer(
    builder: (context, ref) {
        final state = ref.station.currentState;
        return Text("${state.state}");
    }
)
```

### 2. ref.select() - 원하는 값이 변하는 경우만 렌더링&#x20;

&#x20;`ref.select()` 메소드를 사용하는 경우에는 select 메소드의 매개변수로 제공하는 콜백 함수의 반환값이 변할 때만 렌더링을 수행합니다. select 메소드에 전달하는 콜백함수는 첫 인자로 station 을 제공받습니다.&#x20;

&#x20;아래의 예제에서는 context 에 저장된 count 의 값이 변경 될 때만 다시 렌더링합니다.&#x20;

```dart
CounterConsumer(
    // This will be called only once
    builder: (context, ref) {
        // rebuilt only when the refreshed count is changed. 
        final count = ref.select(
            (station) => station.currentState.context.count
        );
        
        return Text("Counter: $count");
    }
)
```

### 3. ref.readStation - 처음 접근 이후로 렌더링을 다시 하지 않음

&#x20;어떤 경우에 있어서는 station 내부의 값이 절대 변하지 않을 것을 미리 알고 있을 수도 잇습니다. 이런 경우에는 `ref.readStation` 을 통해 station 을 접근하면, Provider 자체가 rebuild 되지 않는 한은 그 어떤변화에도 다시 렌더링 하지 않습니다.&#x20;

```dart
CounterConsumer(
    // This will be called only once
    builder: (context, ref) {
        final state = ref.readStation.currentState;
        // Same result, same effect
        // final state = ref.read((station) => station.currentSate); 
        return Text("${state.state}");
    }
)
```

{% hint style="warning" %}
&#x20;여기서 주의할 점은 ref.station 과 ref.select() 방식은 비동기적으로 사용되어서는 안됩니다. 예를 들어 버튼 위젯의 `onPressed` 와 같은 콜백에서 사용하고자 하는 경우에는 반드시 `ref.readStation` 만을 사용해야 합니다.&#x20;
{% endhint %}
