# Eventless Transitions

Like [asynchronous-effects-invoke-services.md](asynchronous-effects-invoke-services.md "mention"), there's a way to define the automatic transition when the station reaches a specific state. Like `Beat` annotation, you can use `EventlessBeat` annotation for this purpose.&#x20;

```dart
@BeatStation()
enum InfiniteLoop {
    @EventlessBeat(to: second)
    first,
    
    @EventlessBeat(to: first)
    second,
}
```

You can use other properties in `Beat` annotation, `actions` and `conditions.`

## Delayed eventless transitions

You can delay the transitions by setting `after` property of `EventlessBeat.`

```dart
@BeatStation()
enum SlowInfiniteLoop {
    @EventlessBeat(to: second, after: Duration(milliseconds: 1000))
    first,
    
    @EventlessBeat(to: first, after: Duration(milliseconds: 1000))
    second,
}
```

If the state is changed before the delayed transitions are triggered, all the delayed transitions are canceled.

## Why is this needed?&#x20;

The simplest example is login-timeout scenario.

```dart
@BeatStation()
enum User {
    @Invokes([/* Autologin service */])
    loggedOut,
    
    // logout after three minutes
    @Eventless(to: loggedOut, after: Duration(minutes: 3))
    // self-routing state to cancel the previous eventless transition
    // after this transition, eventless logout transition is queued again.
    @Beat(event: 'keep login', to: loggedIn)
    loggedIn,
}
```
