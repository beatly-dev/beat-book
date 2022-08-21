# Nested state 사용하기

중첩된 스테이트는 [nested-state.md](../advanced/nested-state.md "mention") 에서 설명하는 방식 그대로 선언하면 됩니다. 다만, flutter 와 함께 사용하기 위해서는 중첩된 모든 enum 들의 `BeatStation` 에 `withFlutter: true` 를 추가해야 합니다. 향후에 root enum 만 `withFlutter: true` 를 선언하면 나머지는 선언하지 않아도 되도록 수정할 예정입니다.&#x20;

모든 중첩 enum에 `withFlutter: true` 를 적용하였다면 각 스테이션들에 맞는 Provider/Consumer가 생성됩니다.&#x20;

다만, 굳이 모든 Provider 를 사용할 필요없이 root enum 의 Provider 만을 사용하면 됩니다.&#x20;

```dart
@BeatStation(withFlutter: true) 
enum Root {
    @Substation(Child)
    someState,
}

@BeatStation(withFlutter: true)
enum Child {
    @Substation(GrandChild)
    someState,
}

@BeatStation(withFlutter: true)
enum GrandChild {
    someState,
}
```

&#x20;위와 같이 중첩된 상태를 선언했다면 아래와 같이 사용할 수 있습니다.&#x20;

```dart
//// root 위젯 혹은 상태를 사용하고자 하는 위젯트리의 최 상단
RootProvider(
    child: MyWidget(),
),

/// MyWidget 의 build 함수

return Column(
    children: [
        RootConsumer(builder: (context, ref, _) {
            return SomeWidget();
        }),
        ChildConsumer(builder: (context, ref, _) {
            return SomeChildWidget();
        }),
        GrandChildConsumer(builder: (context, ref, _) {
            return SomeGrandChildWidget();
        }),
    ],
);
```

&#x20;위의 예제 코드에서 볼 수 있듯이, 단순히 `RootProvider` 로 감싸기만 하면, 원하는 레벨의 Consumer 를 사용해서 위젯을 구성할 수 있습니다.&#x20;

&#x20;또는[ nested state에 접근하는 법](../advanced/nested-state/access-nested-states.md) 에서 설명한 방식을 사용해도 좋습니다.&#x20;

```dart
/// BeatState.of()를 사용한 예
RootConsumer(builder: (context, ref, _) {
    final child = ref.select(
        (station) => station.currentState.of(Child)
    );
}),
```

## Child state 가 아직 시작하지 않은 경우

&#x20;위의 예제에서 각 중첩 상태들은 `someState` 에 도달해야만 실행이 됩니다. 그 이전에는 비활성화된 상태로 남아있습니다. Consumer 들은 의존하고 있는 station 이 비활성화 상태인 경우 builder 를 호출하지 않고, child 에 있는 위젯을 바로 사용합니다. 위와 같이 child에 아무 위젯도 전달하지 않은 경우에는 `SizedBox.shrink()` 를 사용합니다. 아래의 예제를 통해 앞의 예제를 발전시켜 보도록 하겠습니다.&#x20;

```dart
[
    RootConsumer(builder: (context, ref, _) {
        return SomeWidget();
    }),
    ChildConsumer(
        builder: (context, ref, _) {
        return SomeChildWidget();
        }, 
        child: Text('아직 Root.someState에 도달하지 못했습니다.'),
    ),
    GrandChildConsumer(
        builder: (context, ref, _) {
            return SomeGrandChildWidget();
        },
        child: Text('아직 Child.someState에 도달하지 못했습니다.'),
    ),
],
```

&#x20;이제 Child 와 GrandChild가 비활성화 상태인 경우에 그에 맞는 텍스트가 표기됩니다.&#x20;
