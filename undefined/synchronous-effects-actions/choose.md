# Choose - 조건별 액션 실행

&#x20;이벤트가 트리거 될 때 단순히 일련의 액션들을 실행하는 것이 아니라, 이벤트가 트리거 되는 시점의 조건에 따라서 서로 다른 액션을 실행하고 싶을 수도 있습니다. 이를 위해 `Choose` 액션을 지원합니다.&#x20;

&#x20;`ChooseAction` 의 매개변수로 `ChoosActionItem` 리스트를 전달하면 됩니다. 해당 액션들은 `if` / `else` 와 동일하게 가장 처음 조건을 만족하는 액션 **하나만** 수행되고 나머지는 무시됩니다.&#x20;

```dart
@BeatStation(contextType: String)
enum Companion {
    @Beat(
        event: 'employ', 
        to: followed, 
        actions: [
            ChooseAction([
                ChooseActionItem(
                    conditions: [isDog],
                    actions: [AssignAction(assignDogName)],
                ),
                ChooseActionItem(
                    conditions: [isNotDog],
                    actions: [AssignAction(assignCatName)],
                ),
            ]),
        ]
    )
    none, 
    followed,
}

String assignDogName(_, __) {
    return 'DogDog';
}

String assignCatName(_, __) {
    return 'CatCat';
}

bool isDog(_, EventData event) {
    return event.data.isDog;
}

bool isNotDog(_, EventData event) {
    return !isDog(_, event);
}

class Dog {
    bool get isDog => true;
}
class Cat {
    bool get isDog => false;
}
```

&#x20;위의 예제와 같은 Station 이 존재하는 경우 `send.$employ(data: Dog())` 을 실행하면 `assignDogName` 액션이 실행되고, `send.$employ(Cat())` 을 실행하면 `assignCatName` 액션이 실행됩니다. 그 외의 이벤트 데이터를 전달할 경우에는 `isNotDog` 함수로 인해서 항상 `assignCatName` 이 실행됩니다.&#x20;
