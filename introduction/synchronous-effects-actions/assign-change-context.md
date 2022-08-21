# Assign - change context

Refer to [undefined.md](../../undefined/context/undefined.md "mention").

## Assign context with class

```dart
import 'package:beat/beat.dart';

@BeatStation(contextType: String)
enum User {
    @Beat(event: 'login', to: loggedIn, actions: [AssignNewName()])
    loggedOut,
    loggedIn,
}

class AssignNewName extends AssignActionBase {
  const AssignNewName();
  @override
  String Function(BeatState currentState, EventData event) get action => (_, __) {
    return 'Beatly';
  };
}
```

Like the above example, if you want to define `AssignAction` as a class, you can extend `AssignActionBase` class and override `action` getter.&#x20;

{% hint style="info" %}
Constructors should be `const` because dart's annotation system only allows constant construction.&#x20;
{% endhint %}
