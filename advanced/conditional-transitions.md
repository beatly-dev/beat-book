# Conditional Transitions

If you want to trigger the transitions only when some conditions meet, then you can use `conditions` property. When all the results of the `conditions` are `true`, then transitions are triggered.&#x20;

```dart
@BeatStation()
enum Mike {
    @Beat(event: 'eat', to: full, conditions: [smellsGood])
    hungry,
    full,
}
bool smellsGood(BeatState state, EventData event) {
    return event.data.smellsGood();
}
```

As you can see, the functions used in `conditions` field takes the same arguments of actions.&#x20;

Note again that all conditions should be true to trigger the transitions.&#x20;

`conditions` can also be used in `EventlessBeat`.&#x20;
