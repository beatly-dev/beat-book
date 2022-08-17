# Installation

Add `beat`, `beat_generator` and `build_runner` to your `pubspec.yaml`.

```yaml
dependencies: 
    beat:
dev_dependencies:
    build_runner:
    beat_generator: 
```

Now you can use `beat`.&#x20;

Highly recommend executing the below command while you are coding.

```
flutter pub run build_runner watch
# or 
dart run build_runner watch
```

If you don't execute the above command, then you have to run `flutter pub run build_runner build` or `dart run build_runner build` every time you edit your code.&#x20;

#### Lint options

You may want to add these lines to your `analysis_options.yaml` file to prevent linter's blaming.&#x20;

```yaml
# analysis_options.yaml

analyzer:
    exclude: ["**/*.beat.dart"]
```
