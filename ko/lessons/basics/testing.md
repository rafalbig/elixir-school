---
layout: page
title: 테스트
category: basics
order: 12
lang: ko
---

소프트웨어 개발에서 테스트는 아주 중요합니다. 이번 수업에서는 ExUnit을 사용해서 Elixir 코드를 테스트하는 방법과 테스팅하는 데 있어서 가장 효율적인 절차를 함께 살펴보도록 하겠습니다.

{% include toc.html %}

## ExUnit

ExUnit은 우리가 짠 코드를 철저하게 테스트할 수 있는 모든 도구를 포함하고 있는 내장 테스트 프레임워크입니다. 시작하기 전에 먼저 이야기하자면, Elixir 스크립트로 테스트를 구현했기 때문에 `.exs` 파일 확장자를 사용하는 법을 알아둬야 합니다. 테스트를 실행하려면 `ExUnit.start()`로 ExUnit을 실행해야 하는데, 보통 `test/test_helper.exs` 파일에서 이 코드가 실행됩니다.

지난 수업에서 예제 프로젝트를 만들어 보았을 때, mix가 간단한 테스트를 이미 만들어놓았을 정도로 많은 도움이 되었습니다. `test/example_test.exs` 파일에서 확인해보도록 하지요:

```elixir
defmodule ExampleTest do
  use ExUnit.Case
  doctest Example

  test "the truth" do
    assert 1 + 1 == 2
  end
end
```

`mix test`를 입력하면 프로젝트에 있는 테스트를 실행할 수 있습니다. 지금 바로 테스트를 실행한다면 이런 결과를 볼 수 있겠네요:

```shell
Finished in 0.03 seconds (0.02s on load, 0.01s on tests)
1 tests, 0 failures
```

### assert

테스트 코드를 써 보신 적이 있다면 `assert`라는 단어에 익숙할 겁니다. 몇몇 프레임워크에서는 `assert` 대신 `should`나 `expect`를 사용하지만, 그래도 익숙하기는 마찬가지입니다.

해당 조건식이 참인지 테스트할 때 `assert` 매크로를 사용합니다. 참이 아니게 되는 일이 벌어진다면, 테스트가 실패하면서 에러를 던집니다. 테스트가 실패하는 경우를 확인해보기 위해서 샘플 테스트 코드를 수정하고 `mix test`를 실행해봅시다:

```elixir
defmodule ExampleTest do
  use ExUnit.Case
  doctest Example

  test "the truth" do
    assert 1 + 1 == 3
  end
end
```

이제 조금 색다르게 출력되는 것을 확인할 수 있습니다:

```shell
  1) test the truth (ExampleTest)
     test/example_test.exs:5
     Assertion with == failed
     code: 1 + 1 == 3
     lhs:  2
     rhs:  3
     stacktrace:
       test/example_test.exs:6

......

Finished in 0.03 seconds (0.02s on load, 0.01s on tests)
1 tests, 1 failures
```

테스트에 실패했을 때 ExUnit은 참이 아니라서 틀려버린 `assert`가 있던 곳과 예상했던 값, 실제 결과를 알려줄 것입니다.

### refute

`if`에 `unless`가 있다면 `assert`에는 `refute`가 있습니다. 실행 결과가 항상 거짓일 때 `refute`를 사용하면 됩니다.

### assert_raise

때로는 에러가 발생하는 상황을 테스트로 작성해야 할 때가 올텐데, 이 때에는 `assert_raise`를 사용하면 됩니다. 나중에 Plug를 다룰 수업에서 `assert_raise`를 사용하는 예제를 살펴보겠습니다.

## 테스트 준비

몇몇 인스턴스를 테스트하려면 테스트하기 전에 준비 과정을 거치고 싶을 때가 있는데, 이럴 때에는 `setup`과 `setup_all` 매크로를 사용하면 됩니다. `setup`은 매 테스트를 수행하기 전에 실행되고, `setup_all`은 해당 테스트 스위트 전체를 수행하기 전에 실행됩니다. 여기에서 `{:ok, state}` 형식으로 된 튜플을 리턴받아서 `state`를 테스트에서 사용할 수 있습니다.

예제로 한번 익혀볼 수 있게, `setup_all`을 쓰도록 코드를 수정해 봅시다.

```elixir
defmodule ExampleTest do
  use ExUnit.Case
  doctest Example

  setup_all do
    {:ok, number: 2}
  end

  test "the truth", state do
    assert 1 + 1 == state[:number]
  end
end
```

## 모의 객체 만들기

Elixir에서 모의 객체를 만들어서 테스트를 진행하는 가장 깔끔한 방법은 "안 하는 것"입니다. 다른 언어에서 테스트를 다루어 보셨다면 지금쯤 모의 객체(mock)을 다룰 때가 되었다고 본능적으로 느끼셨겠지만, Elixir 커뮤니티에서 사용하지 말자는 의견이 크기도 하고, 사용하지 않을 만한 멋진 이유도 있기 때문이에요. 좋은 설계 원칙을 지켜 코딩을 한다면 그 결과물도 각 부분으로 나누어 테스트하기 쉬워질 테니까요.

모의 객체에 대한 끓어오르는 열망을 억누르도록 하세요.
