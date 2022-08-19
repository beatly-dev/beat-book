# Check if current state is X

Generated `SomeBeatState` provides some boolean getters.&#x20;

```dart
@BeatStation()
enum MyState {
    idle,
    loading,
    loaded,
}

///////// usage
station.currentState.isIdle$; // station.currentState.state == MyState.idle
station.currentState.isLoading$; // station.currentState.state == MyState.loading
station.currentState.isLoaded$; // station.currentState.state == MyState.loaded
```
