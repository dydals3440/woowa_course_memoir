### Jest 학습 자료를 준비한 이유!

---

우테코, 1주차를 진행하면서 테스트 케이스 jest 코드를 보게 되었다. 들어는 보았지만 실제로 사용해본 적은 없기에 굉장히 생소한 개념이었습니다.

그리고, 1주차 프리코스 후에 다른 사람들의 코드를 보았는데, 다른 사람들이 Jest를 통해 예외 사항을 찾아서 테스트 케이스를 작성하고 예외처리를 해준 것을 보고, 나도 한번 시험이 끝난 후 Jest를 개인적으로 학습한 후에 1주차 코드를 수정하고, 2주차 프리코스 부터는 jest를 적극 활용해서 코드를 작성해보고 싶어 따로 학습을 진행해 보았습니다.

실제로 2주차 때는 멘토님께서 아래와 같이 함수 분리와 더불어서 함수별 테스트를 중요시 하였기 떄문에 저처럼, jest에 익숙하지 않은 사람들에게 도움이 되지 않을까 자료를 공유합니다!

<aside> 💡 2주 차 미션에서는 1주 차에서 학습한 것에 더해 **함수를 분리**하고, 각 **함수별로 테스트를 작성하는 것**에 익숙해지는 것을 목표로 하고 있어요. 이번에 테스트를 처음 접하시는 분들은 언어별 테스트 도구를 학습하고 작은 단위의 기능부터 테스트를 작성해보길 바랍니다

</aside>

---

### Unit Test 그리고 Jest
`**Test Runner**` 테스트를 실행후 결과 생성

**`Assertion`** 예상값, 실제값 비교 수행 테스트 조건, 비교를 통한 테스트 로직

`Test Runner`, `Assertion` 구분해서 각각 필요한 라이브러리를 사용을 해도 되지만!

요즘은, `Jest 라이브러리`를 사용하여 구분 없이 모든 기능을 통합하여 사용할 수 있습니다!

---

### Jest 공식문서 읽어보기!

1. `zero config`

이것 저것, 설치할 번거로움 없이 간편하게 사용가능!

2. `snapshots`

단위 테스트와는, 별개의 테스트 브라우저 환경 UI 테스트할 때 사용, 그 순간을 찰칵하고 찍어 상태를 비교!

3.  `isolated`

속도 성능이 더 뛰어남

4.  `great api`

it, expect와 같은 api를 통해 간편하게 테스트를 사용할 수 있다.
![[스크린샷 2023-10-28 오후 1.48.37.png]]

[Jest](https://jestjs.io/)

---

### Jest 기본 환경 설정

1. 이 프로젝트는 노드에서 패키지 매니저가 관리하는 것이라고 명시! (package.json 이 생긴것이 확인 가능)

```css
npm init --yes 
```

2. Jest 설치

```css
npm install jest --global
```

3.  jest —init (이 프로젝트에서 jest사용할 수 있게 init 해줌)
![[3.png]]
![[4.png]]

4. package.json 확인 (test라고 시작하면 jest가 자동으로 동작)

```jsx
"scripts": {
    "test": "jest"
  },

// 이렇게 추가됨을 확인할 수 있다.
```

5.  그리고, 우리 프로젝트를 레포에 올렸을 떄 다른 개발자도 동일한 테스팅 환경을 갖길 원하는 개발 모드에 jest추가!

```java
npm install --save-dev jest

/* 아래와 같이 개발의존성이 추가된 것을 확인할 수 있다.
"devDependencies": {
    "jest": "^29.7.0"
  }
*/
```

6. 추가적으로, 개발의 용이성을 위해 test 코드의 타입을 알 수 있게 jest의 type을 설치해주겠습니다.

```jsx
npm install @types/jest
```

---

### jest 사용하여 테스트 해보기!

이제 add라는 함수를 만들어서, 이에 대한 테스트 코드를 작성해보겠습니다.

```jsx
// add.js
function add(a, b) {
	return a + b;
}

module.exports = add;
```

그 후, test 폴더를 별도로 만들어 분리하여 test 코드를 작성해보겠습니다.

```jsx
// node 환경 import 방식
const add = require('../add');

test('4+3은 7인 테스트코드', () => {
  // 테스트 코드 작성
  expect(add(4, 3)).toBe(7);
});
```

예상하는 값을, expect에 우리가 만든 함수의 파라미터를 전달해 넣어주면 되고, 그거에 대한 결과 값을 toBe의 인자로 넣어주면 됩니다.

모든, 테스트 코드 작성이 완료되었습니다. 이제 `npm run test`를 통해 결과값을 확인해 보면 됩니다.
![[5.png]]

그러면, 임의로 우리가 `add 함수를 a - b로 변경`해보겠습니다. 예상값은 7인데, 실제 값은 1이므로 테스트가 통과되지 않아야겠죠?
![[6.png]]

이렇게 실패됨을 확인할 수 있고, 어느 부분에서 잘못되었는지 바로 알 수 있습니다.
![[7.png]]
우리가, 우테코 하면서 테스트를 돌렸을 때, 이 부분은 안나온 것으로 기억합니다. 이것을 우리는 `collectCoverage`라고 부릅니다.

```jsx
// jest.config.js
collectCoverage: false, // 기존 true로 된것을 false로 변경해줌!
```

이제는, `**collectCoverage**`가 나오지 않음을 확인할 수 있습니다.
![[8.png]]

```jsx
// false라 해도 coverage를 보고 싶은 경우가 있다면!
jest --coverage // 이렇게 실행하면 된다!
```

---

### 자동 환경 설정

매번 코드를 수정할 떄 마다, test 코드를 자동으로 실행하고 싶다면!?

```jsx
"scripts": {
    "test": "jest --watchAll" // jest를 --watchAll을 추가해주면 된다.
  },
```

작업 변경할 때마다, 수행합니다.

코드를, 수정하고 파일만 저장하면 테스트가 다시 실행이 되니, 실시간으로 결과를 확인하면서 코딩하기 편합니다. 프로젝트 규모가 크고, 수많은 테스트파일이 있다면 내가 작업하고 있는 파일이 변경될 떄 마다, 수백개 수천개의 테스트 코드가 실행된다면 이는 번거로울 것 입니다. 원하지 않는 결과도 볼 수 있어 불편할 수 있습니다.

이미 커밋된, 변경되지 않은 파일에 대해서는 테스트 코드를 수행하지 않고, 지금 내가 활발하게 작업중인 것 에 대해서만 테스트를 수행할 수 있습니다.

```jsx
"scripts": {
    "test": "jest --watch"
  },
```

깃을 활용해야 합니다.

```jsx
git init // 깃 추가
```

사실 이런 `node_modules` 파일은 git에 저장되는 것은 좋지 않습니다. ignore 해주겠습니다.

`.gitignore` 파일을 만든 후, `node_modules/*` 을 파일에 추가해주시면 됩니다.
![[9.png]]
```jsx
// .gitignore
node_modules/*
```

그 후, 이제 `jest —watch`모드는, 파일에 변동이 있을 때만, 동작한다고 했으니 파일에 변동이 있는 것을 알기 위해서는 `git add/commit`을 해주어야 합니다.

```jsx
git add .
git commit -am "Jest setup & add"
```

그 후, 이제 `npm run test`를 해주면 우리가 `**커밋한 상태와, 지금 테스트 코드를 비교**`해서 `**수정**`이 되었다면 `**그 부분만 테스트를 실행**`하는 것을 확인할 수 있습니다.

```jsx
npm run test
```

---

### Calculator Class Jest Test

Calculator Class를 만들고, 계산기에는 다양한 기능들이 있기에 메소드로 정의해주었다. 각각의 덧셈, 초기화, 뺄셈 등의 기능들이 제대로 동작하는지 테스트를 해보겠다.

```jsx
class Calculator {
  constructor() {
    this.value = 0;
  }

  set(num) {
    this.value = num;
  }

  clear() {
    this.value = 0;
  }

  add(num) {
    const sum = this.value + num;
    if (sum > 200) {
      throw new Error('계산된 값은 200이상 넘을 수 없습니다.');
    }
    // 200 이하일떄만, sum으로 할당
    this.value = sum;
  }

  subtract(num) {
    this.value = this.value - num;
  }

  multiply(num) {
    this.value = this.value * num;
  }

  divide(num) {
    this.value = this.value / num;
  }
}

module.exports = Calculator;
```

이처럼, 여러개의 기능을 테스트 할 떄는, `describe()`를 사용하여 내부에, 정의해주면 된다. 그리고, `it`을 활용해서 각각의 기능들에 해당하는 테스트를 진행해주면 된다. `it 대신 test라고 작성해도 동일하게 동작`한다.

```jsx
const Calculator = require('../calculator');

// describe를 통해 관련있는 테스트들을 묶을 수 있다.
// it 은 Calculator을 가리킴 test라해도됨
describe('Calculator', () => {
  it('inits with 0', () => {
		const cal = new Calculator();
    expect(cal.value).toBe(0);
  });
  // 이전 테스트에 영향 안받는게 좋음.
  it('sets', () => {
		const cal = new Calculator();
    cal.set(9);
    expect(cal.value).toBe(9);
  });
});
```

---

### beforeEach를 사용하는 이유?

이런식으로, 매번 `**const cal = new Calculator();**` 이렇게 인스턴스를 만들어 주는 것은 불필요한 반복이란게 느껴질 것이다.

```jsx
const Calculator = require('../calculator');

describe('Calculator', () => {
	// 이렇게 위로 뺴줄 수는 있는데, 이 떄 문제가 발생한다.
	const cal = new Calculator();
  it('inits with 0', () => {
    expect(cal.value).toBe(0);
  });
  it('sets', () => {
    cal.set(9);
    expect(cal.value).toBe(9);
  });
});
```

물론, 위의 테스트는 정확히 동작할 것이다. cal.value가 0이고 단순 9로만 변경해주기 떄문이다. 하지만 아래의 코드를 보자

```jsx
const Calculator = require('../calculator');

describe('Calculator', () => {
  // 이렇게 위로 뺴줄 수는 있는데, 이 떄 문제가 발생한다.
  const cal = new Calculator();
  it('inits with 0', () => {
    expect(cal.value).toBe(0);
  });
  it('sets', () => {
    cal.set(9);
    expect(cal.value).toBe(9);
  });
	// 뺄셈
  it('subtract', () => {
    cal.subtract(3);
    expect(cal.value).toBe(-3);
  });
});
```

이 경우에 `npm run test`를 실행하면 우리는 subtract에서 `0 - 3` 이렇게해서, `-3이란 값을 예측할 텐데` 실제로는 그러지 않은 것을 확인할 수 있다.
![[10.png]]
매번, cal 값이 각각의 메소드들을 실행할 떄 마다, 초기화 되는 것이 아닌 한번 초기화 시켜놓고 그 cal 값으로 계속해서 계산이 진행되기 떄문에, `0 → 9(set) → 6(subtract(-3))` 이런식으로 계산이 되서, 6의 값이 나오게 됩니다.

이렇게 테스트 코드를 작성한다면, 예상치 못한, 테스트 에러를 마주하게 되기 떄문에 이를 방지하기 위해 `beforeEach` 라는 메소드가 있습니다.

`beforeEach`는 매번 테스트를 실행하기 전에, 계속해서 안에 있는 콜백함수를 동작시켜주는 것 입니다. 아래 처럼 코드를 작성하면 이제 예기치 못한 문제를 해결할 수 있습니다.

```jsx
const Calculator = require('../calculator');

describe('Calculator', () => {
  // 초기화 후 beforeEach로 매번 재할당시켜줌
  let cal;
  beforeEach(() => {
    cal = new Calculator();
  });

  it('inits with 0', () => {
    expect(cal.value).toBe(0);
  });
  it('sets', () => {
    cal.set(9);
    expect(cal.value).toBe(9);
  });

  it('subtract', () => {
    cal.subtract(3);
    expect(cal.value).toBe(-3);
  });
});
```

---

### 에러 처리

```jsx
// calculate.js
add(num) {
    const sum = this.value + num;
    if (sum > 200) {
      throw new Error('계산된 값은 200이상 넘을 수 없습니다.');
    }
    // 200 이하일떄만, sum으로 할당
    this.value = sum;
  }
```

우리가, 위에서 200이 넘는 값은 에러가 나오도록 처리를 했는데, 아래 테스트 코드를 작성해 보았는데, 해당하는 테스트 코드는 사실 200 값이 넘지 않기 떄문에 에러는 발생하지 않기 떄문에 정상적으로 테스트는 통과할 것 이다.

```jsx
// calculate.test.js (17-18 줄)
it('adds', () => {
    cal.set(9);
    cal.add(3);
    expect(cal.value).toBe(12);
  });
```

하지만, `jest --coverage` 를 통해 커버리지를 확인해보면 아래와 같은 문제를 발견할 수 있다.

![스크린샷 2023-10-28 오후 6.25.57.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/e57bc1c9-a051-46a7-9c15-f5e2f1fea50c/fd1e4de2-d533-4abb-9115-82fb51823f08/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-10-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.25.57.png)

우리가, 에러를 따로 테스트 처리를 안해주었기 떄문에, 해당 줄의 부분이 커버되지 않았다라는 에러를 확인할 수 있었습니다. 100%가 아닌 94.4%만 처리되었다는 의미입니다. 이를 `expect`, `toThrow`를 통해 한번 처리해주겠습니다.

```jsx
it('adds', () => {
    cal.set(9);
    cal.add(3);
    expect(cal.value).toBe(12);
  });

  it('덧셈 함수의 실행 값은 200이상 넘을 수 없습니다.', () => {
    // Error 예상 (expect안에 콜백함수에 에러 예상 코드 전달)
    expect(() => {
      cal.add(201);
    }).toThrow('계산된 값은 200이상 넘을 수 없습니다.');
  });
```

---

### 나누기 예외 처리

나눗셈은, 생각보다 까다로운 테스트 절차가 필요합니다.

1. `0 / 0` 은 수학적으로, 숫자가 아니기 떄문에 `NaN`이 나오게 됩니다.
2. `정수 / 0 은 무한대`입니다. Inifinity
3. 그냥, 나눗셈 테스트 케이스

나눗셈에 해당하는 경우가 여러개 있기 떄문에 이 경우 `describe` 내부에 또 `describe`를 선언해주어도 문제가 없습니다.

```jsx
describe('divides', () => {
    it('0/0 === NaN', () => {
      cal.divide(0);
      expect(cal.value).toBe(NaN);
    });

    it('1/0 === Infinity', () => {
      cal.set(1);
      cal.divide(0);
      expect(cal.value).toBe(Infinity);
    });

    it('4/4 === 1', () => {
      cal.set(4);
      cal.divide(4);

      expect(cal.value).toBe(1);
    });
  });
```

---

### 계산기 전체 테스트 코드

```jsx
const Calculator = require('../calculator');

test('add', () => {});

// 관련있는 테스트들을 묶을 수 있다.

// it 은 Calculator을 가리킴 test라해도됨
describe('Calculator', () => {
  // 독립적으로, 만들어야하기 떄문에 beforeEach메소드 사용
  // calculator 그룹안에, 새로운 오브젝트를 항상 테스트 코드가 실행되기전에 초기화 될 수 있도록 beforeEach 사용
  //   const cal = new Calculator();
  let cal;
  beforeEach(() => {
    cal = new Calculator();
  });

  it('inits with 0', () => {
    expect(cal.value).toBe(0);
  });
  // 이전 테스트에 영향 안받는게 좋음.
  it('sets', () => {
    cal.set(9);
    expect(cal.value).toBe(9);
  });

  it('clear', () => {
    cal.set(9);
    cal.clear();
    expect(cal.value).toBe(0);
  });

  it('adds', () => {
    cal.set(9);
    cal.add(3);
    expect(cal.value).toBe(12);
  });

  it('덧셈 함수의 실행 값은 200이상 넘을 수 없습니다.', () => {
    // Error 예상 (expect안에 콜백함수에 에러 예상 코드 전달)
    expect(() => {
      cal.add(201);
    }).toThrow('계산된 값은 200이상 넘을 수 없습니다.');
  });

  it('subtracts', () => {
    cal.subtract(1);
    expect(cal.value).toBe(-1);
  });

  it('multiply', () => {
    cal.set(2);
    cal.multiply(7);
    expect(cal.value).toBe(14);
  });

  // 나누는건 여러개 다시 describe로묶음
  describe('divides', () => {
    it('0/0 === NaN', () => {
      cal.divide(0);
      expect(cal.value).toBe(NaN);
    });

    it('1/0 === Infinity', () => {
      cal.set(1);
      cal.divide(0);
      expect(cal.value).toBe(Infinity);
    });

    it('4/4 === 1', () => {
      cal.set(4);
      cal.divide(4);

      expect(cal.value).toBe(1);
    });
  });
});
```

---

### Testing Asynchronous Code

```jsx
// async.js

function fetchProduct(error) {
  if (error === 'error') {
    return Promise.reject('네트워크 에러');
  }
  return Promise.resolve({ name: 'yong-min', englishName: 'matthew' });
}

module.exports = fetchProduct;
```

```jsx
const fetchProduct = require('../async.js');

describe('Async', () => {
  // 비동기는, 조금있다 실행되니 fetchProduct() 호출해놓고, 
  // then이 호출됬는지 안됬는지 결과 나왔는지 안나왔지에 상관않고, 
  // 함수를 호출한 후에 코드블럭은 끝나고, it은 끝 it이 별도의 에러 없이 끝난거
  it('async', () => {
    fetchProduct().then((item) => {
      expect(item).toEqual({ name: 'yong-min', englishName: 'matthew' });
    });
  });

  // 1. 비동기 done으로 처리 (5초 정도 딜레이 있음) 보통 2,3번으로 처리 많이함.
  it('async - done', (done) => {
    fetchProduct().then((item) => {
      expect(item).toEqual({ name: 'yong-min', englishName: 'matthew' });
      done();
    });
  });
	
	// 2. 비동기 return으로 처리
  it('async - return', () => {
    return fetchProduct().then((item) => {
      expect(item).toEqual({ name: 'yong-min', englishName: 'matthew' });
    });
  });

  // 3. async / await 처리
  it('async - await', async () => {
    const product = await fetchProduct();
    expect(product).toEqual({ name: 'yong-min', englishName: 'matthew' });
  });

	// 4. resolves 테스트
  it('async - resolves', () => {
    // 비동기기에 return 해주어야함
    return expect(fetchProduct()).resolves.toEqual({
      name: 'yong-min',
      englishName: 'matthew',
    });
  });
	
	// 5. rejects 테스트
  it('async - rejects', () => {
    return expect(fetchProduct('error')).rejects.toBe('네트워크 에러');
  });
});
```
---
### 마무리
Jest를 사용함으로서, 더 안정적인 app을 만들 수 있음을 이번에 학습하면서 확인할 수 있게 되었다. 아직 Jest를 많이 사용해보지 않아 익숙하지는 않지만, 이번 우테코 프리코스 과정을 거치면서 계속해서 jest 라이브러리를 사용하다 보면 익숙해지지 않을까 싶다.

현재 지금 우테코에서 프리코스를 시작으로, 향후에는 리액트와 같은 프로젝트에서도, jest 라이브러리를 활용하여 TDD를 적용해서 개발을 진행해보고 싶은 생각이 들었다!