# 개요

{% hint style="danger" %}
&#x20;현재 패키지의 모든 부분을 완전히 다시 작성하고 있습니다. 처음 이 패키지를 만들 때는 대충 생각하던거 + xstatejs 써본 기억으로 작성했으나, 패키지를 사용하고자 하는 분들과의 대화후 견고하게 재작성할 필요가 있다고 판단하였습니다.&#x20;
{% endhint %}

{% hint style="warning" %}
`beat` 패키지는 현재 활발하게 개발되고 있으며, 예고 없이 API가 변경될 수 있습니다. 사용중에 최신 버전으로 패키지를 업그레이드 하고 싶은 경우에는 반드시 이 사이트를 먼저 확인하세요. 본 사이트의 내용은 항상 최신 버전의 `beat의 사용법에 대해서만 설명합니다.`&#x20;
{% endhint %}

## Beat - 코드 자동 생성을 통한 상태관리

> 개인 목적으로 생성한 프로젝트이므로 상용 제품에 사용하여 발생하는 문제들에는 책임을 지지 않습니다.&#x20;

기존 상태관리 패키지들은 의존성 주입에만 그치는 경우가 대부분입니다. 실제 상태관리에 대한 기능이 있는 패키지들이더라도 몇가지 문제를 안고 있습니다. 물론 워낙 이해하기 쉽고, 사용하기 쉽고, 안정성도 뛰어나고, 활발하게 유지보수가 되기 때문에 이러한 단점들을 초월하는 수많은 장점이 존재합니다.&#x20;

1. 상태관리와 관련된 코드들에 대해 IDE가 제공하는 자동완성 기능을 사용하기 어렵습니다. 자동완성 기능을 주로 사용하는 경우, 이를 활용하게 위해서는 상태관리 코드 뿐 아니라, 자동완성을 해주도록 부가적인 변수나 클래스, 함수들을 선언하는 불편함을 감수해야 합니다.
2. 상태의 변화를 한눈에 파악하기 힘듭니다. `provider`, `riverpod` 에서 주로 사용 되는 `Notifier` 모델은 imperative 한 코드로 구성되기 쉽고, `bloc` 에서 사용되는 reducer 모델은 하나의 리듀서 함수에서 너무 많은 종류의 이벤트를 처리하는 코드로 구성되기 쉽습니다.&#x20;
3. 관리해야 하는 상태의 종류가 많아질수록 직접 작성해야하는 코드가 기하급수적으로 늘어납니다.&#x20;

이 외에도 다양한 단점을 찾으려면 찾을 수 있겠지만, 위의 세가지가 개인적으로 느꼈던 불편함이었습니다. 1번의 이유로 인해 생산성이 저하되고, 2번과 3번의 이유로 인해 다른 사람과의 협업 시 코드 퀄리티에 따라 협업과정의 난이도가 달라지는 문제가 있었습니다. 이를 해결하기 위해 오토마타 이론의 state machine을 도입하고자 하였고, 이미 존재하는 다른 많은 state machine 패키지들은 여전히 1번의 문제를 안고 있었습니다.&#x20;

다양한 조사 중에 가장 눈에 띄었던 프로젝트는 자바스크립트 진영에서 사용되는 xstate 프로젝트였습니다. State chart 스펙을 따르는 state machine 기반의 상태관리 라이브러리인 xstate와 그의 파생 애플리케이션인 stately.ai 와 유사한 정도의 기능을 제공하고, 자동완성 기능까지 제공하기 위해서 `beat` 프로젝트를 시작하게 되었습니다.&#x20;

이와 관련해서는 [`xstate.js`](https://xstate.js.org/docs/) 문서를 읽어보길 강력히 권장합니다. State chart 자체에 관심이 있으신 분들은 [https://statecharts.dev/](https://statecharts.dev/) 도 읽어보길 권장합니다.&#x20;

### 코드 자동생성의 이점&#x20;

`beat` 는 `build_runner`를 활용하여 필요한 코드를 자동생성합니다. 이를 통해 개발시에 다양한 자동완성을 제공하여 생산성을 향상시킵니다. 사실 이 기능에 제일 필요했습니다.&#x20;

### Declarative 한 상태 선언의 이점

imperative 한 상태관리에서는 상태관리와 연관된 코드들이 어디서 실행되는지 어떻게 실행되는지 모두 파악해야 하지만, `beat` 를 사용하여 좀 더 `declarative` 한 상태 선언을 수행하면, 한 곳에서 한눈에 상태의 변경을 알아볼 수 있습니다. 물론 `beat` 가 완벽하게 declarative 하지도 않고, 개발자의 구현 방식에 따라서 당연히 imperative 스타일로 변형되기도 쉽지만, 익숙해진다면 엄청난 생산성을 기대할 수 있습니다.&#x20;

## 그 밖의 state machine 기반 패키지

이미 다양한 종류의 state chart 기반 패키지들이 존재하고 있으니, `build_runner` 에 의존하고 싶지 않거나, 다른 방식으로 상태관리를 시도하고 싶으신 분들은 아래의 패키지들 혹은 다른 패키지들을 살펴보셔도 좋습니다.&#x20;

* xstate \[[pub.dev](https://pub.dev/packages/xstate)], \[[github](https://github.com/sahandevs/xstate.dart)] - As you can see its name, it's almost the same as xstate.js
* statecharts \[[pub.dev](https://pub.dev/packages/statecharts)], \[[github](https://github.com/sarahec/statecharts)] - declarative, immutable, and compatible with [SCXML](https://www.w3.org/TR/scxml/).
* automata \[[pub.dev](https://pub.dev/packages/automata)], \[[github](https://github.com/rows/automata)] - developed by rows.com

## Disclaimer

state machine 기반, state chart 기반, xstate 기반이라고는 하지만 그들의 스펙을 그대로 구현하고 지원하지는 않습니다. 물론, 최종적으로는 scxml을 완전히 지원하고 싶지만, 아주 먼 얘기가 될 것 같습니다.&#x20;

개발은 제가 사용하면서 필요한 기능들 위주로 먼저 개발될 예정이며, API 또한 제 입맛대로 작성됩니다. 혹시라도 후원이 들어오게 된다면, 후원 규모에 따라 개발의 방향성이 달라질 수 있습니다 :smile:
