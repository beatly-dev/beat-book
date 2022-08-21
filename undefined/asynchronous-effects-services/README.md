# Asynchronous effects - Services

&#x20;현재 버전의 `beat` 는 action 들을 모두 `synchronous` 하게 실행합니다. 이로 인해서 액션 내부에서 `async` / `await` 를 사용할 경우 의도한대로 동작하지 않을 수 있습니다. 이런 경우에는 로직 코드에서 미리 async 루틴을 실행하고 그 결과 값을 EventData.data 로 전달하는 방식으로 구현해야 합니다.&#x20;

`beat` 이런 단점을 극복하고 상황에 따라 Asynchronous 한 동작들을 수행할 수 있도록 service 형태의 실행을 지원합니다.&#x20;

&#x20;액션과 서비스의 차이점은, 액션은 이벤트가 트리거 될 때 실행이 되고, 서비스는 상태가 변경된 이후에 실행된다는 것입니다. 현재는 일반 콜백과 `Future` 를 반환하는 서비스만을 지원하고 있으나 향후에는 xstate와 유사한 다양한 서비스들을 지원할 예정입니다.&#x20;

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

&#x20;위의 예제와 같이 서비스는 `Beat` 에 종속되지 않고 상태에 종속됩니다. BeatStation 이 특정한 상태에 도착했을 때, 해당 상태에 종속된 서비스들이 존재하면 해당 서비스들을 실행합니다. 위의 에제는 처음 시작시 자동 로그인 기능을 체크하는 코드입니다.&#x20;
