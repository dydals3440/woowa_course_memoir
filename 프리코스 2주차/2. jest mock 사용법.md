### jest mock 사용법

이번에는, jest의 Mock에 대해서 알아보고자 합니다.

아래와 같은, check 함수가 있다고 가정해봅시다. 상세 기능은, 주석으로 설명을 남겨놓았으니 확인바랍니다!

```jsx
// 3가지 인자 모두 콜백함수
function check(judgement, trueCase, falseCase) {
  if (judgement()) {
    // true일시 2번쨰 인자인 trueCase에 'true'라는 문자를 전달해주면서 호출!
    trueCase('true');
  } else {
    // false일시 3번쨰 인자인 falseCase에 'false' 라는 문자를 전달해주면서 호출!
    falseCase('false');
  }
}

module.exports = check;
```

기본적인, `jest 동작 방식`은 이전 게시물을 통해 확인해주시면 감사하겠습니다!

[[우아한 테크코스] Jest 학습 내용](https://velog.io/@dydals3440/%EC%9A%B0%EC%95%84%ED%95%9C-%ED%85%8C%ED%81%AC%EC%BD%94%EC%8A%A4-Jest-%ED%95%99%EC%8A%B5-%EB%82%B4%EC%9A%A9)

`jest.fn()` 을 통해 함수를 불러올 수 있습니다.

그 후, 우리가 만든 함수 변수를 선언해주고, 매 테스트마다 독립적으로 실행시키기 위해서 beforeEach를 이용하여 함수를 초기화 해주겠습니다.

```jsx
const check = require('../check');

describe('check', () => {
  // trueCase, falseCase로 사용할 mock 함수 변수를 선언!
  let trueCase;
  let falseCase;
  // 각각 테스트코드가 실행하기 전에, beforeEach를 이용해서 함수들을 초기화시켜줍니다.
  beforeEach(() => {
    trueCase = jest.fn();
    falseCase = jest.fn();
  });
});
```

먼저, 우리가 해볼 테스트는 check 함수의 결과 값이 true 일 때, 우리가 만든 trueCase 함수를 실행시키고, true라는 결과값이 제대로 들어가 있는지 테스트를 해볼려고 합니다.

테스트 케이스를 일단은 글로 한번 작성해보고자 합니다.

1. check 함수가 true일 때, trueCase는 1번 호출되어진다.
2. trueCase의 호출 결과는, ‘true’이다.
3. falseCase는 호출되면 안된다.

이를 코드로 작성해보고자 합니다.

```jsx
const check = require('../check');

describe('judgement', () => {
  let trueCase;
  let falseCase;

  beforeEach(() => {
    trueCase = jest.fn();
    falseCase = jest.fn();
  });

  it('judgement가 true일 때, trueCase를 호출합니다.', () => {
    // check 함수의 결과값이 true일 때
    check(() => true, trueCase, falseCase);
    // 1. 호출의 길이는 1이어야 한다.
    expect(trueCase.mock.calls.length).toBe(1);
    // 2. trueCase의 호출 결과는 true라는 문자열이다.
    expect(trueCase.mock.calls[0][0]).toBe('true');
    // 3. falseCase의 호출 길이는 0이다. (true 이므로)
    expect(falseCase.mock.calls.length).toBe(0);
  });
});
```

대충 코드를 보면 이제 무슨 의미인지 이해하실 수 있을거라고 생각합니다. 하지만, `trueCase.mock` 이 어떻게 구성되어 있길래, 길이도 호출할 수 있고, 안에 들어가있는 값도 호출할 수 있는지에 대한 의문이 생길것입니다. 실제 `trueCase.mock`을 출력해보았습니다.

```jsx
  /*
      trueCase.mock의 구성
      {
        calls: [ [ 'yes' ] ],
        instances: [ undefined ],
        invocationCallOrder: [ 1 ],
        results: [ { type: 'return', value: undefined } ]
      }
    *
```

물론, 위의 테스트 코드처럼 `trueCase.mock.calls.length/calls` 이런식으로 활용해서 테스트를 진행해도 괜찮습니다. 하지만, 생각보다 번거롭기 떄문에 조금 더 유용한 api를 소개할려고 합니다.

바로, `toHaveBeenCalledTimes`와, `toHaveBeenCalledWith` 메소드이다.

```jsx
const check = require('../check');

describe('judgement', () => {
  let trueCase;
  let falseCase;

  beforeEach(() => {
    trueCase = jest.fn();
    falseCase = jest.fn();
  });

  it('judgement가 true일 때, trueCase를 호출합니다.', () => {
    // check 함수의 결과값이 true일 때
    check(() => true, trueCase, falseCase);

		/* 개선전 코드
    // 1. Before: 호출의 길이는 1이어야 한다.
    expect(trueCase.mock.calls.length).toBe(1);

    // 2. Before: trueCase의 호출 결과는 true라는 문자열이다.
    expect(trueCase.mock.calls[0][0]).toBe('true');

    // 3. Before: falseCase의 호출 길이는 0이다. (true 이므로)
    expect(falseCase.mock.calls.length).toBe(0);
		*/

		// 1. After: 호출의 길이는 1이어야 한다.
		expect(trueCase).toHaveBeenCalledTimes(1);
		
		// 2. After: trueCase의 호출 결과는 true라는 문자열이다.
		expect(trueCase).toHaveBeenCalledWith('true');

		// 3. After: falseCase의 호출 길이는 0이다. (true 이므로)
		expect(falseCase).toHaveBeenCalledTimes(0);

  });
});
```

Before일 떄 코드 보다, After일 때 코드가 조금 더 깔끔하다는 것을 알 수 있고, 테스트 또한 동일하게 동작함을 우리는 확인할 수 있을 것이다.

이제, 동일하게 `false인 케이스의 테스트`도 아래와 같이 작성해서 추가해주면 됩니다.

```jsx
it('judgement가 false일 때, falseCase를 호출합니다.', () => {
    check(() => false, trueCase, falseCase);

    expect(falseCase).toHaveBeenCalledTimes(1);

    expect(falseCase).toHaveBeenCalledWith('false');

    expect(trueCase).toHaveBeenCalledTimes(0);
  });
```

완성 테스트케이스 코드

```jsx
const check = require('../check');

describe('judgement', () => {
  let trueCase;
  let falseCase;

  beforeEach(() => {
    trueCase = jest.fn();
    falseCase = jest.fn();
  });

  it('judgement가 true일 때, trueCase를 호출합니다.', () => {
    // check 함수의 결과값이 true일 때
    check(() => true, trueCase, falseCase);

		/* 개선전 코드
    // 1. Before: 호출의 길이는 1이어야 한다.
    expect(trueCase.mock.calls.length).toBe(1);

    // 2. Before: trueCase의 호출 결과는 true라는 문자열이다.
    expect(trueCase.mock.calls[0][0]).toBe('true');

    // 3. Before: falseCase의 호출 길이는 0이다. (true 이므로)
    expect(falseCase.mock.calls.length).toBe(0);
		*/

		// 1. After: 호출의 길이는 1이어야 한다.
		expect(trueCase).toHaveBeenCalledTimes(1);
		
		// 2. After: trueCase의 호출 결과는 true라는 문자열이다.
		expect(trueCase).toHaveBeenCalledWith('true');

		// 3. After: falseCase의 호출 길이는 0이다. (true 이므로)
		expect(falseCase).toHaveBeenCalledTimes(0);

  });

	it('judgement가 false일 때, falseCase를 호출합니다.', () => {
    check(() => false, trueCase, falseCase);

    expect(falseCase).toHaveBeenCalledTimes(1);

    expect(falseCase).toHaveBeenCalledWith('false');

    expect(trueCase).toHaveBeenCalledTimes(0);
  });
});
```
---
### 마무리
함수를 테스트하는 방법을 추가적으로 학습했습니다. 우테코를 계기로 jest를 처음 사용해보게 되었는데, 매우 유용한 것 같지만, 테스트 코드를 작성해본 경험이 적어서 프리코스 테스트케이스 작성시 많은 어려움을 겪고 있네요!! 앞으로 프리코스 과정을 거치면서 테스트 코드를 작성하는 연습을 많이하고, 프리코스 과정을 통해 다양한 경험을 할 수 있어 매우 좋은 기회였습니다.