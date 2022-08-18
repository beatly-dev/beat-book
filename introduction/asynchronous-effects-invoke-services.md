# Asynchronous effects - Invoke services

`Action` only can invoke synchronous calls on a transition. To overcome this limitation, `beat` support asynchronous side effects, called services.&#x20;

The services are automatically invoked when the state is reached. Services can be defined using `Invokes` annotation and `Invoke{Type}` classes.&#x20;

```dart
@BeatStation()
enum User {
    @Invokes([
        InvokeFuture(autoLogin),
    ])
    loggedOut,
    loggedIn,
}
Future autoLogin(BeatState state, EventData event) async {
    /// CHECK USER'S PREVIOUS STATE
}
```

`Invokes` annotation takes the list of `Invoke` classes. All the items in the list are called asynchronous, and the order is not deterministic, so services must not depend on others.

Whenever `UserBeatStation` reaches to `User.loggedOut` state including the start time, the `Invokes` will be invoked. &#x20;
