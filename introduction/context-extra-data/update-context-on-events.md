# Update context on events

If you want to change the `context` with a transition, you can use `actions` property of the `Beat` annotation. See this example.

```dart
import 'package:beat/beat.dart';

@BeatStation(contextType: String)
enum User {
    @Beat(event: 'login', to: loggedIn, actions: [AssignAction(assignUserName)])
    loggedOut,
    loggedIn,
}

String assignUserName(BeatState state, EventData event) {
    return 'Beatly';
}
```

The details about `actions` can be found in [actions-side-effects](../actions-side-effects/ "mention").&#x20;

The function that is used to assign the context must return the same type of `contextType` field in `BeatStation` annotation.

The first parameter of the action function is the `BeatState` instance which has `state` and `context` field. `state` field represents the current state, which is type of your enum. `context` field represents your context data. `context` field can be `null`, so you must check its nullity.&#x20;

The second parameter of the action function is the `EventData` instance which has `event` and `data` field. `event` field is the name of the `event`, `login` in this example. `data` field contains the data you send when you call transition methods. The details is in [update-context-with-events-argument.md](update-context-with-events-argument.md "mention").

Defining `assign` actions by extending `AssignAction` class is currently not supported.&#x20;
