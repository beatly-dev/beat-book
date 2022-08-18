# Installation

Add `beat`, `beat_generator` and `build_runner` to your `pubspec.yaml`. <mark style="color:blue;">If you are using</mark> <mark style="color:blue;"></mark><mark style="color:blue;">`flutter`</mark> <mark style="color:blue;"></mark><mark style="color:blue;">then add</mark> <mark style="color:blue;"></mark><mark style="color:blue;">`flutter_beat`</mark> <mark style="color:blue;"></mark><mark style="color:blue;">instead of</mark> <mark style="color:blue;"></mark><mark style="color:blue;">`beat`</mark><mark style="color:blue;">.</mark>

```yaml
dependencies: 
    # install only one of two according to your application
    beat: # for dart only applications
    flutter_beat: # for flutter applications
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
