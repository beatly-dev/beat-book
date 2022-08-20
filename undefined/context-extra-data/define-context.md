# Define context

## Define context (data that the state holds)

> Do not confuse `beat`'s context with flutter's `BuildContext`.&#x20;

The beat station can hold the data you want.&#x20;

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

`contextType` can be anything you want! It can be primitives like `int`, `String`, or it can be custom classes and enums.&#x20;

Now you can instantiate the beat station with an initial context.&#x20;

```dart
// main function in main.dart 
final myState = MyStateBeatStation(
    firstState: MyState.first,
    initialContext: MyContextType('beatly.dev'),
)..start();

// Accessing the context
final currentContext = myState.currentState.context;
    
```

If you want to update the context, you should use [`AssignAction`](update-context-on-transitions.md).&#x20;

<mark style="color:red;">**YOU MUST NEVER DO**</mark> `currentState.context = something;`
