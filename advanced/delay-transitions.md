# Delay transitions

Like [eventless-transitions.md](../introduction/eventless-transitions.md "mention"), the normal `Beat` can be delayed by the user.&#x20;

You can just pass `after` argument on the trigger.&#x20;

```dart
// This will be triggered after 1 second.
myStation.send.$gotoNext(after: Duration(milliseconds: 1000));
myStation.send('gotoNext', after: Duration(milliseconds: 1000));
```

As same with [delayed eventless transitions](../introduction/eventless-transitions.md), if the station reaches the other state, then the delayed transitions are all canceled.

### I can use \`Timer\` and \`Future.delayed\`. Why do I need this?

You can use Dart's `Timer` or `Future.delayed` yourself. But canceling the delayed transitions is your responsibility in this case. &#x20;
