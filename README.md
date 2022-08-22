# Introduction

{% hint style="danger" %}
WARNING

I am currently rewriting everything to follow the [https://statecharts.dev/](https://statecharts.dev/) . ALL of the APIs will be changed.&#x20;
{% endhint %}

{% hint style="warning" %}
`beat` is actively changing. None of the APIs is guaranteed to be stable. You need to check this book if you want to update `beat`, `flutter_beat`, and `beat_generator` versions. This book will always be up to date.&#x20;

When the `beat` reaches v1.0, API will be stabilized.&#x20;
{% endhint %}

## Beat - State management with a state chart

This project is my toy project to simplify the state management flows in `Flutter`. Whole functionalities are heavily inspired by [`xstate.js`](https://xstate.js.org/docs/). I highly recommend reading the docs of [`xstate.js`](https://xstate.js.org/docs/).&#x20;

Like `xstate.js` provides type-sensitive state management with its typescript implementation, `beat` aims to provide typed state management. `beat` will enable developers to write code quickly without errors thanks to `Dart`'s type system and auto-completion.&#x20;

You can find a detailed introduction to a state chart in the [`xstate.js's introduction section.`](https://xstate.js.org/docs/guides/introduction-to-state-machines-and-statecharts/#states)

### The power of code generation

`beat` is built on top of `build_runner`'s code generation systems. With **beat**, you can write a code as small as possible. **beat** will generate all the code needed to manage the state. **beat** will even generate helper classes and widgets for whom to use this package for their `dart` and `flutter` applications.&#x20;

## Alternatives

`beat` is dependent on Dart's code generation system `build_runner`. If you are looking for other state chart packages which do not depend on `build_runner`, there are several packages.

* xstate \[[pub.dev](https://pub.dev/packages/xstate)], \[[github](https://github.com/sahandevs/xstate.dart)] - As you can see its name, it's almost the same as xstate.js
* statecharts \[[pub.dev](https://pub.dev/packages/statecharts)], \[[github](https://github.com/sarahec/statecharts)] - declarative, immutable, and compatible with [SCXML](https://www.w3.org/TR/scxml/).
* automata \[[pub.dev](https://pub.dev/packages/automata)], \[[github](https://github.com/rows/automata)] - developed by rows.com

And you can find others via searching `state machine` or `state chart`.

## Disclaimer

Although I said `beat` as state management with **a state chart**, `beat` would not strictly follow the state chart XML (SCXML) specifications. And also, `beat` would not strictly follow the `xstate.js`.
