# Provider 와 Consumer

`flutter_beat` 는 `{YourEnum}Provider` 와 `{YourEnum}Consumer`를 생성하고 이를 사용할 수 있도록 합니다. 사용법은 provider 패키지나 riverpod 패키지와 유사합니다. bloc, provider, riverpod 중 하나라도 사용해 봤다면 쉽게 이해하실 수 있습니다.

```dart
import 'package:flutter_beat/flutter_beat.dart';

part 'counter.beat.dart';

@BeatStation(contextType: CounterContext, withFlutter: true)
@addOneBeat
@takeOneBeat
enum Counter {
  @initializer
  loading,
  idle,
  added,
  taken,
}

class CounterContext {
  final int count;
  final Dummy dummy = const Dummy();

  CounterContext([this.count = 0]);
}

class Dummy {
  const Dummy();
}

const initializer = Invokes([
  InvokeFuture(
    initializeStation,
    onDone: AfterInvoke(
      to: Counter.idle,
      actions: [AssignAction(assignValue)],
    ),
  )
]);

Future<CounterContext> initializeStation(_, __) async {
  return CounterContext();
}

CounterContext assignValue(_, EventData event) {
  return event.data;
}

const addOneBeat =
    Beat(event: 'addOne', to: Counter.added, actions: [AssignAction(addOne)]);
const takeOneBeat =
    Beat(event: 'takeOne', to: Counter.taken, actions: [AssignAction(takeOne)]);

CounterContext addOne(BeatState state, _) {
  return CounterContext(state.context.count + 1);
}

CounterContext takeOne(BeatState state, _) {
  return CounterContext(state.context.count - 1);
}

```

&#x20;위의 에제는 일부러 복잡하게 만든 Counter 상태 코드입니다. 아래의 코드는 이를 사용하는 단순한 Counter +/- 애플리케이션 입니다. 해당 코드는 깃허브 레포지토리의 examples/counter 폴더에서 확인할 수 있습니다.&#x20;

{% code lineNumbers="true" %}
```dart
import 'package:flutter/material.dart';

import 'src/counter.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return CounterProvider(
      firstState: Counter.loading,
      initialContext: CounterContext(),
      child: MaterialApp(
        title: 'Flutter Demo',
        theme: ThemeData(
          primarySwatch: Colors.blue,
        ),
        home: const MyHomePage(title: 'Flutter Demo Home Page'),
      ),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({Key? key, required this.title}) : super(key: key);
  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              'You have pushed the button this many times:',
            ),
            CounterConsumer(
              builder: (context, ref) {
                final counter = ref.select(
                  (station) => (station.currentState.context?.count),
                );
                return Text(
                  '$counter',
                  style: Theme.of(context).textTheme.headline4,
                );
              },
            ),
            CounterConsumer(
              builder: (context, ref) {
                final count = ref.select(
                  (station) {
                    final count = station.currentState.context?.count ?? 0;
                    return (count ~/ 2) * 2;
                  },
                );
                return Text(
                  'Even number counter: $count',
                );
              },
            ),
          ],
        ),
      ),
      floatingActionButton: CounterConsumer(
        builder: (context, ref) {
          return Row(
            mainAxisSize: MainAxisSize.min,
            children: [
              FloatingActionButton(
                onPressed: () {
                  ref.readStation.send.$takeOne();
                },
                tooltip: 'Decrement',
                child: const Icon(Icons.remove),
              ),
              const SizedBox(width: 16),
              FloatingActionButton(
                onPressed: () {
                  ref.readStation.send.$addOne();
                },
                tooltip: 'Increment',
                child: const Icon(Icons.add),
              ),
            ],
          );
        },
      ), // This trailing comma makes auto-formatting nicer for build methods.
    );
  }
}
```
{% endcode %}

&#x20;예제 코드에서 보이듯이, Provider 를 사용할 때 초기 상태와 초기 컨텍스트 값을 제공할 수 있습니다. 추가로 station 의 시작 이전과, 종료 이전에 실행할 수 있는 `beforeStart` 와 `beforeDispose` 콜백함수도 지정할 수 있습니다.&#x20;

&#x20;Provider 의 하위 위젯트리에 존재하는 Consumer 에서는 builder 에 전달되는 ref 를 사용하여 station 에 접근할 수 있습니다. 위의 코드에서 한 부분만 살펴보면서 알아보도록 하겠습니다.&#x20;

```dart
CounterConsumer(
  builder: (contxt, ref) {
    final taken = ref.station.currentState.isTaken$;
    if (taken) {
      return const Text('Last transition: take');
    }
    return const SizedBox.shrink();
  },
),
```

`ref.station` 을 통해 `CounterBeatStation` 을 접근하고 해당 station 의 내용을 읽어옵니다.
