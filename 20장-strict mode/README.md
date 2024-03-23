## 20. strict mode

### 20-1. 암묵적 전역

- `use strict`모드를 반영하지 않을 경우, 일반적인 자바스크립트 동작 방식이다. -> 암묵적 전역 현상
- 개발자의 의도와는 상관없는 다양한 오류를 발생시키는 원인이 될 가능성이 높다.
  - ES6에 도입된 `let, const 키워드`를 사용하여 변수를 선언한 다음 사용하는 것이 일반적

```javascript
function foo() {
  x = 10;
}
foo();
console.log(x); // 10

/*
- foo 함수 실행
- foo 함수 내부에서 x를 선언한 것은 없기 때문에, 스코프 체인에 의해 변수 x를 검색한다.
- 전역 스코프까지와 왔지만, x는 존재하지 않는다. -> 자바스크립트 엔진은 암묵적으로 전역 객체에 x 프로퍼티를 동적 생성한다.
- 생성된 전역 객체에 x 프로퍼티에 10을 할당한다.
- foo 함수의 실행이 끝나고, console 에 암묵적으로 생성된 x를 참조하여 10이 출력된다.
*/
```

<br />

### 20-2. strict mode

- 자바스크립트 언어의 문법을 엄격히 적용하여 오류 발생이 높거나, 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러 발생을 시킨다.
- ✅ 참고로 ESLint와 같은 린트 도구를 사용해도 strict mode와 유사한 효과를 얻을 수 있다.
  - 📍 린트 도구는 정적 분석 기능을 통해 `소스코드를 실행하기 이전에` 소스코드를 검사해준다.
  - 📍 문법적 오류만 아니라 잠재적 오류까지 찾아내서 오류의 원인을 알려주는 기능까지 제공
  - 📍 코딩 컨벤션을 설정 파일 형태로 정의하고 강제할 수 있기 때문에 협업할 때 좋음

<br />

#### 20-2-1. strict mode 적용

- 전역의 선두 || 함수 몸체의 선두에 `use strict`를 추가
- 코드의 선두에 위치시키지 않으면 strict mode가 정상적으로 동작하지 않는다.

```javascript
"use strict"
...
```

```javascript
function foo() {
  'use strict'
  ...
}
```

<br />

#### 20-2-2. 전역 strict mode는 피하자

- 전역에 적용한 strict mode는 스크립트 단위로 적용
- 여러 스크립트가 통합되어 사용되는 프로그램이 일반적
- 프로그램 안에서 strict mode는 각 스크립트 단위로 적용
- 어떤 스크립트는 non-strict-mode인데, 혼용되어 사용하게 되면 오류 발생 가능성이 있다.
  - ex) 외부 라이브러리 사용시
- 그래서 strict mode 사용시, 즉시 실행 함수로 스크립트를 감싸서 스코프를 구분
- 다른 스크립트 파일에 영향이 받질 않도록 사용하는 것이 일반적

```javascript
(function() {
  'use strict';
  ...
})();
```

<br />

#### 20-2-3. 함수 단위로 strict mode 사용하는 것을 피하자.

- 모든 함수에 strict mode를 적용하는 것도 번거롭고, 함수마다 strict mode 적용 유/무를 구분하는 것은 바람직하지 않다.
- ❌ strict mode는 `즉시 실행 함수`로 감싼 스크리브 단위로 적용하는 것이 바람직

<br />

#### 20-2-4. strict mode가 발생시키는 에러

- 암묵적 전역 -> ReferenceError
- 선언하지 않은 변수를 참조할 경우 에러 발생

```javascript
(function () {
  ("use strict");

  x = 1;
  console.log(x); // ReferenceError: x is not defined
})();
```

- ✅ 변수, 함수, 매개변수의 삭제 -> SyntaxError
  - delete 연산자로 변수 / 함수 / 매개변수를 삭제하려고 할 경우 에러 발생
- ✅ 매개변수 이름의 중복 -> SyntaxError
  - 중복된 매개변수 이름을 사용할 경우 에러 발생
- ✅ with문 사용 -> SyntaxError
  - 사용할 경우 바로 에러 발생
  - with문은 성능과 가독성 측면 다 좋지 않음 -> 사용 권장하지 않음

### 20-3. strict mode 적용에 대한 변화

#### 20-3-1. 일반 함수의 this

- strict mode에서 함수를 일반 함수로서 호출하면 this에 undefined가 바인딩
- 생성자 함수가 아닌 일반 함수 내부에서 this를 사용할 필요 사라짐

```javascript
(function () {
  ("use strict");

  function foo() {
    console.log(this); // undefined
  }
  foo();

  function Foo() {
    console.log(this); // Foo
  }
  new Foo();
})();
```

#### 20-3-2. arguments 객체

- strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객채에 반영되지 않음

```javascript
(function (a) {
  ("use strict");

  a = 2;

  console.log(arguments); // { 0: 1, length: 1 }
})(1);
```
