# onDone/onError

When the invoked service has done its job, you can indicate what to do next. You can define the next state and actions to execute using `AfterInvoke` class similar to the `Beat` annotation. You can also define the next behavior when the invoked service throws.&#x20;

```dart
@BeatStation(contextType: UserInfo)
enum User {
    @Invokes([
        InvokeFuture(
            autoLogin,
            onDone: AfterInvoke(
                to: loggedIn,
                actions: [AssignAction(assignUserInfo)],
            ),
            // There is nothing to in this scenario
            onError: AfterInvoke(),
        ),
    ])
    loggedOut,
    loggedIn,
}
Future<UserInfo> autoLogin(BeatState state, EventData event) async {
    /// CHECK USER'S PREVIOUS STATE
    if ( /* not logged in */ ) {
        throw 'there is no user information';
    }
    return UserInfo;
}

UserInfo assignUserInfo(_, EventData event) {
    return event.data;
}

```

The actions executed after the invoked service finishes take the returned data from the service in `EventData.data`. When the service throws, `onError` can retrieve thrown error from `EventData.data`.
