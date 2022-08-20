# Choose - conditional actions

If you want to define multiple actions at once and execute just one of them by the condition, you can use `ChooseAction`.&#x20;

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
```

In the above example, if you trigger `employ` event with a dog `data`, then the context will have `DogDog`. If not, the context will have `CatCat`.&#x20;

```dart
class Dog {
    bool get isDog => true;
}
class Cat {
    bool get isDog => false;
}

station.send.$employ(data: Dog()); 
// or 
station.send.$employ(data: Cat()); 
```
