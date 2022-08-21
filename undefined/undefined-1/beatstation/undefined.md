# 현재 상태 확인하기

기본적으로 `station.currentState` 를 통해 현재 상태를 확인할 수 있습니다. `currentState`의 `state` 프로퍼티에 개발자가 생성한 `enum` 값을 저장하고 있습니다. 직접 enum 값을 확인할 수도 있지만, 향후 설명할 중첩된 상태관리등을 위해 상태 확인 접근자를 제공합니다. 아래의 코드 예제를 확인하길 바랍니다. &#x20;

```dart
@BeatStation()
enum MyState {
    idle,
    loading,
    loaded,
}

///////// usage
station.currentState.isIdle$; // station.currentState.state == MyState.idle
station.currentState.isLoading$; // station.currentState.state == MyState.loading
station.currentState.isLoaded$; // station.currentState.state == MyState.loaded
```

위에서 보듯이, enum 의 값 앞에 `is` 를 뒤에 `$` 사인을 붙여서 생성합니다. `is` 를 붙이는 이유는 상태 확인 (boolean) 접근자임을 명시하기 위해서이고, `$` 사인을 붙이는 이유는, `beat` 가 발전하면서 생길 자체 변수들과의 충돌을 방지하기 위해서입니다.&#x20;

어찌되었든, 자동완성을 사용하시는 분이라면 어차피 자동완성이 되므로 외우고 있을 필요는 없습니다 :)
