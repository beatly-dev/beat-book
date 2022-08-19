# Access nested states

Refer to the previous example, if you want to access ancestors' or descendants' `BeatState`, you have two methods to use.&#x20;

The first one is:&#x20;

```dart
station.child$.currentState; 
station.child$.grandChild$.currentState;
```

The second one is:

```dart
station.currentState.of(Child);
station.currentState.of(GrandChild);
station.currentState.of(Root);
```

Choose what you want.&#x20;

This method can be useful when you are writing actions, services, or conditions.&#x20;

```dart
bool transitionGuardForRoot(BeatState state, _) {
    final BeatState rootState = state.of(GrandChild);
    return rootState.currentState.isIdle$;
}
```
