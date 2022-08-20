# 설치하기

설치는 당연히 일반 패키지와 동일합니다. 다만 코드 생성을 위해 몇 가지 추가적인 패키지를 설치해야 합니다.&#x20;

`beat` 패키지를 `dependencies` 에, `beat_generator` 와 `build_runner` 를 `dev_dependencies` 에 추가해야 합니다. Flutter와 함께 사용하고자 하는 경우에는 `beat` 패지지가 아닌 `flutter_beat` 패키지를 추가해야 합니다.&#x20;

```dart
dependencies: 
    # install only one of two according to your application
    beat: # for dart only applications
    flutter_beat: # for flutter applications
dev_dependencies:
    build_runner:
    beat_generator: 
```

혹은 `flutter pub add` 커맨드를 사용하셔도 좋습니다.&#x20;

```bash
# for flutter applications
flutter pub add flutter_beat
flutter pub add --dev beat_generator
flutter pub add --dev build_runner

#or 

# for dart only applications
dart pub add beat 
dart pub add --dev beat_generator
dart pub add --dev build_runner
```

다른 `build_runner` 기반의 패키지들과 동일하게 코드를 수정할 때마다 아래의 커맨드를 실행해야 합니다.&#x20;

```
flutter pub run build_runner build
# or 
dart run build_runner build
```

매번 커맨드를 실행하는게 귀찮은 경우에는 `build` 옵션 대신 `watch` 옵션을 사용하시면 됩니다.&#x20;

#### .gitignore

`beat` 는 최종 결과물로 `*.beat.dart` 파일을 생성합니다. 이는 `beat` 버전이 변경되거나 코드가 변경되면 항상 변화하는 파일이므로 `.gitignore` 에 추가하여 버전관리가 되지 않도록 하는 것을 권장합니다. 다른 머신에서 재생성 할 수 있으므로 걱정하지 않아도 됩니다.&#x20;

```
**/*.beat.dart
```

#### Lint options

`beat` 가 생성한 코드는 여러분들의 취향에 맞는 linter 옵션에 적합하지 않을 수 있습니다. Analyzer 가 자꾸 에러를 뿜거나, 빨간줄 노란줄이 보이는게 싫으신 분들은 `analysis_options.yaml` 파일에 아래와 같이 추가하시면 됩니다.&#x20;

```yaml
# analysis_options.yaml

analyzer:
    exclude: ["**/*.beat.dart"]
```
