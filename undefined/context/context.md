# context 정의하기

{% hint style="danger" %}
다시 말하지만 flutter의 `BuildContext` 가 아닙니다. 이와 관련하여 헷갈린다는 피드백이 많아질경우 추후에 이름을 변경할수도 있습니다. 현재는 xstate를 따라하기 위해서 (순전히 네이밍 편의를 위해) context 라는 이름을 사용하고 있습니다.&#x20;
{% endhint %}

상태와 같이 저장하고자 하는 context 는 `BeatStation` 어노테이션 선언시 `contextType` 속성을 원하는 타입으로 지정하면 됩니다.&#x20;

```dart
@BeatStation(contextType: MyContextType)
enum MyState {
    // .... code ....
}
class MyContextType {
    final String myName;
    // Don't need to be `const`
    const MyContextType(this.myName);
}
```

앞서 언급한대로 타입은 원시 타입부터 개발자가 선언한 클래스까지 다양한 타입들이 가능합니다. Map, List, Set 등도 당연히 가능합니다. context 타입을 지정하지 않은 경우에는 자동으로 `dynamic` 타입으로 설정됩니다.&#x20;

context를 사용하고 싶다면 station 을 초기화 할 때 첫 context 를 지정할 수 있습니다.&#x20;

```dart
// main function in main.dart 
final myState = MyStateBeatStation(
    firstState: MyState.first,
    initialContext: MyContextType('beatly.dev'),
)..start();

// Accessing the context
final currentContext = myState.currentState.context;
    
```

If 저장된 컨텍스트는 `station.currentState.context` 를 통해서 접근할 수 있습니다. 초기화 하지 않는 경우를 대비하여 `context` 는 항상 `nullable` 합니다. 예를 들어 컨텍스트 타입을 int 로 지정했다면 context의 타입은 `int?` 입니다. 위의 예제에서는 `MyContextType?` 타입이 되겠네요.&#x20;

상태에 저장된 컨텍스트의 값을 변경하는 것은 반드시 `action` 을 사용해야 합니다. 직접 `context` 내부 변수를 변경하거나 `currentState.context` 자체를 변경할 경우에는 imperative 코드로 변하기 쉬우며, `beat` 가 생성한 코드들의 온전한 동작도 보장되지 않습니다.&#x20;

컨텍스트의 변경은 다음 장에서 살펴보도록 하겠습니다.&#x20;

<mark style="color:red;">**YOU MUST NEVER DO**</mark> `currentState.context = something;`
