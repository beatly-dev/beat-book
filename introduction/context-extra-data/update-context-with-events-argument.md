# Update context with event's argument

Often, you may want to provide the contextual data to produce new context inside `assign` actions or other actions. `Beat` support this case using the `EventData` parameter. Let's start with an example.

```dart
import 'package:beat/beat.dart';

@BeatStation(contextType: String)
enum User {
    @Beat(event: 'login', to: loggedIn, actions: [AssignAction(assignUserName)])
    loggedOut,
    loggedIn,
}

String assignUserName(BeatState state, EventData<String> event) {
    final newName = event.data ?? 'Beatly';
    return newName;
}

void main() {
    final station = UserBeatStation()..start();
    station.send.$login('Provided name');
}
```

As you can see, just add an optional positional argument when you call the transitions. `data` field in the `EventData` instance will contain that data. `data` field can be null, so you should be carefully handle it.&#x20;

In the future, `beat` will support the `required` data on calling transitions.&#x20;
