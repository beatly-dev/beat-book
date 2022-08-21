# Access to nested substations

You can use [nested-state.md](../advanced/nested-state.md "mention") method. If you want all of them with a Provider/Consumer you should annotate them with `withFlutter: true` . Let's see an example.&#x20;

> I am struggling to find the way to resolve `withFlutter` only from the root state. If you know how to do this please answer my question from [https://gitter.im/flutter/flutter](https://gitter.im/flutter/flutter). My id is [wurikiji](https://app.gitbook.com/u/5617125ad46d061000ac5bcb "mention")&#x20;

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

From this example, `flutter_beat` will generate all three of `RootProvider/Consumer`, `ChildProvider/Consumer`, `GrandChildProvider/Consumer`.&#x20;

You just need to wrap the root widget with `RootProvider`. You don't need to wrap it with `ChildProvider`, `GrandChildProvider`.&#x20;

```dart
//// root widget
RootProvider(
    child: MyWidget(),
),

/// MyWidget's build method

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

As you can see, I just wrapped the root with `RootProvider` and I can use any level of Consumer. Or you can use [access-nested-states.md](../advanced/nested-state/access-nested-states.md "mention").&#x20;

```dart
/// Using BeatState.of()
RootConsumer(builder: (context, ref, _) {
    final child = ref.select(
        (station) => station.currentState.of(Child)
    );
}),
```

## When the child state is not yet started

Child state is not started until its parent state reaches a specific state which is associated with the child state. In this case, Consumer will not call  its `builder`, and use `child`. If you don't provide a child widget like the above example, then `SizedBox.shrink()` is used. Let's modify the example.&#x20;

```dart
[
    RootConsumer(builder: (context, ref, _) {
        return SomeWidget();
    }),
    ChildConsumer(
        builder: (context, ref, _) {
        return SomeChildWidget();
        }, 
        child: Text('Root.someState is not yet reached'),
    ),
    GrandChildConsumer(
        builder: (context, ref, _) {
            return SomeGrandChildWidget();
        },
        child: Text('Child.someState is not yet reached'),
    ),
],
```

Now you can show the proper text to your users.&#x20;
