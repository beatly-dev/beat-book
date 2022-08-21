# Eventless - 자동으로 실행되는 트리거

&#x20;[asynchronous-effects-services](../undefined/asynchronous-effects-services/ "mention") 에서는 자동으로 실행되는 async 서비스를 지정할 수 있었듯이, 서비스 실행없이 자동으로 트리거 되는 이벤트 또한 지정할 수 있습니다.&#x20;

```dart
@BeatStation()
enum InfiniteLoop {
    @EventlessBeat(to: second)
    first,
    
    @EventlessBeat(to: first)
    second,
}
```

&#x20;위의 예제는 일단 `BeatStation.start()` 를 실행하면 끊임없이 자동으로 `first` 상태와 `second` 상태를 오가는 코드입니다.&#x20;

## 지연 시간을 통한 eventless 상태 이

&#x20;위의 예제처럼 코드가 작성된 경우 중단 시간이 없이 무한루프를 수행하게 되므로, 무한루프를 원한 경우이더라도 너무 많은 cpu 를 사용하는 문제가 발생합니다.&#x20;

`beat` 는 다양한 경우를 구현할 수 있도록 `after` 인자를 통해 지연시간을 설정할 수 있는 기능을 제공합니다.&#x20;

```dart
@BeatStation()
enum SlowInfiniteLoop {
    @EventlessBeat(to: second, after: Duration(milliseconds: 1000))
    first,
    
    @EventlessBeat(to: first, after: Duration(milliseconds: 1000))
    second,
}
```

&#x20;위의 예제는 자동으로 상태 전이가 발생하되, 상태 전이 이전에 1초간의 지연시간을 갖도록 하는 예제입니다.&#x20;

## EventlessBeat 와 after 지연시간이 필요한 예제

&#x20;그렇다면 어떤 경우에 이러한 기능들이 효과적으로 사용될 수 있는 지 아래의 에제를 통해 살펴봅시다.&#x20;

```dart
@BeatStation(contextType: LoginInfo)
enum User {
    @Invokes([/* Autologin service if */])
    // onDone: {to: loggedIn}, onError: {to: loggedOut}
    initial, 
    
    @Beat(event: 'login', to: connecting)
    loggedOut,
    
    @Invokes([/* Some login process */]) 
    // onDone: {to: loggedIn}, onError: {to: loggedOut}
    connecting, 
    
    // logout after three minutes
    @EventlessBeat(
        to: loggedOut,
        after: Duration(minutes: 3),
        actions: [/* some logout related actions */],
    )
    // self-routing state to cancel the previous eventless transition
    // after this transition, eventless logout transition is queued again.
    @Beat(event: 'keep login', to: loggedIn)
    loggedIn,
}
```

&#x20;위의 에제는 서비스와 evnetless 를 복합적으로 사용한, 자동로그인 / 로그인 타임아웃 기능을 시뮬레이션한 코드입니다.&#x20;

&#x20;스테이션이 시작되면 자동으로 `autologin` 서비스로 자동로그인을 테스트하고, `send.$login()` 을 시도하면 connecting 스테이트에서 자동으로 login 서비스를 실행하여 이후 동작을 결정합니다.&#x20;

일단 로그인이 되어 loggedIn 상태에 도달한 경우 3분간의 타임아웃을 실행합니다. 3분이내에 `send.$keepLogin()` 이벤트가 트리거 되지 않으면, 3분간 딜레이 된 `Eventless` 크리거가 실행되어 자동으로 로그아웃 액션들을 수행하고 loggedOut 상태로 전이합니다.&#x20;

3분 내에 `send.$keepLogin()` 을 실행했다면, 기존의 eventless 는 삭제되고 다시 3분간 지연된 eventless 가 이벤트 큐에 저장됩니다.&#x20;
