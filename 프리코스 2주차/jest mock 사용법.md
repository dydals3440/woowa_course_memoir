### jest mock 사용법
이번에는, jest의 Mock에 대해서 알아보고자 합니다.

```js
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