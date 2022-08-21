# onDone/onError

&#x20;서비스의 실행이 끝난 뒤에 어떤 동작을 수행할지에 대해서도 지정할 수 있습니다. `onDone` 은 서비스가 에러 없이 종료된 경우, `onError` 는 서비스가 에러를 `throw` 한 경우에 수행되는 액션입니다. 아래의 예제를 통해 자세 알아보도록 하겠습니다.&#x20;

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

&#x20;서비스가 종료될 때 `return` 을 통해 무언가를 반환하는 경우 그 데이터는 이후에 `onDone` 에 의해 실행되는 액션의 `EventData.data` 에 전달됩니다. 서비스가 `throw` 를 통해 에러를 생성하는 경우 해당 에러는 `onError` 에 의해 실행되는 액션의 `EventData.data` 에 전달됩니다.&#x20;
