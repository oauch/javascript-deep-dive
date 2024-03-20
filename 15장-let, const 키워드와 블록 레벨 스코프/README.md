## 15. let, const 키워드와 블록 레벨 스코프

### 15-1. var 키워드 문제점

#### 1. 변수 중복 선언 허용

- 동일한 이름의 변수가 이미 선언되어 있는 것을 모르고 변수를 중복 선언하면서 값까지 할당하게 되면 의도치 않게 먼저 선언된 변수 값이 변경된다 👀

```javascript
var x = 1; // x 변수 선언 및 초기화
var y = 1; // y 변수 선언 및 초기화

// 초기화 문이 있는 변수 선언문 = 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작
var x = 100;

// 초기화문이 없는 변수 선언문 -> 무시
var y;

console.log(x); // 100
console.log(y); // 1
```

#### 2. 함수 레벨 스코프

- 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 된다.
  - `함수 레벨 스코프`
- 함수 레벨 스코프는 전역 변수르르 남발할 가능성이 높음⬆️
  - 의도치 않게 전역 변수가 중복 선언되는 경우가 발생

```javascript
var x = 1;

// 코드 블록(블록 레벨) -> var는 함수 레벨 스코프
// 그래서, 블록 내 x 변수는 전역 변수처럼 스코프 적용
{
  var x = 10;
}

console.log(x); // 10
```

#### 3. 변수 호이스팅

- 에러를 발생시키지는 않지만 프로그램 흐름상 맞지 않다.
- 가독성 ⬇️
- 오류를 발생시킬 가능성이 있다.

```javascript
console.log(foo); // undefined <- foo 변수 암묵적 선언 및 초기화 (호이스팅)

foo = 123; // foo 변수 값 할당

console.log(foo); // 123

var foo;
```

<br />

### 15-2. let 키워드

- `var 키워드 단점`을 보완하기 위해 `ES6`에 도입

#### 1. 변수 중복 선언 금지

- 이름이 같은 변수를 중복 선언하면 문법 에러 발생 (SyntaxError)

```javascript
// var 변수 = 중복 선언 ⭕️
var foo = 123;
var foo = 456;
console.log(foo);

// let 변수 = 중복 선언 ❌
let bar = 123;
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
console.log(bar);
```

- 선언 단계에서 체크 -> 위에서 var 변수인 foo에 대한 출력문 실행이 진행 안됨

#### 2. 블록 레벨 스코프

- 모든 코드 블록을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.
- ex) 함수, if, for, while 등등

```javascript
let foo === 1;
{
  let foo = 2;
  let bar = 3;
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined;
```

- foo 변수는 블록 밖에 선언 되어 있다.
- bar 변수는 블록 안에 선언 되어있기 때문에 `ReferenceError`가 발생하는 것이다.

<br />

  <img width="449" alt="스크린샷 2024-03-20 15 56 23" src="https://github.com/CodeJaws/Coding-Study/assets/49686619/5fdefdda-6a2c-4d89-9376-a8dad19db771" />

#### 3. 변수 호이스팅

- 변수 선언문 이전에 참조하면 참조 에러가 발생
- var 키워드와 변수 선언 / 초기화 시점 차이가 있다.

```javascript
// 변수 호이스팅 - var 변수
console.log(foo); // undefined

var foo;
console.log(foo); // undefined

foo = 1;
console.log(foo); // 1

// 변수 호이스팅 - let 변수
console.log(foo); // 일시적 사각지대(TDZ) - ReferenceError: Cannot access 'foo' before initialization ( 사실상, 여기서 프로그램 종료 )

let foo; // 변수 선언문에서 초기화 단계가 실행
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행
console.log(foo); // 1
```

- 📍 전역 객체

  - var 키워드로 선언한 전역 변수 / 전역 함수는 선언하지 않은 변수에 값을 할당한 암묵적 전역은 `전역 객체 window 프로피티`가 된다.
  - 전역 객체의 프로퍼티를 참조할 때 window 생략 가능
  - 단, 전역 객체는 `브라우저 환경내에서 참조 가능`

- 하지만 `let 키워드`로 선언한 전역 변수는 전역 객체의 프로퍼티 ❌
  - window 객체에 접근 ❌
  - let 전역 변수는 보이지 않는 개념적 블록이 존재

<br />

### 15-3. const 키워드

- 상수를 선언하기 위해 사용 (반드시는 아님)
- `let 키워드`랑 비슷한 성질을 가짐

#### 1. 선언과 동시에 초기화

- 반드시 선언과 동시에 초기화 해줘야한다.

```javascript
const foo; // SyntaxError: Missing initializer in const declaration
```

#### 2. 재할당 금지

- 값의 재할당이 안된다.

```javascript
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable.
```

#### 3. const로 상수

- const 키워드로 선언한 변수에 `원시 값`을 할당할 경우 변수 값 변경 ❌
- 원시 값은 `변경 불가능한 값`이므로 재할당 없이 값을 변경할 수 있는 방법 ❌

`상수(constant)` = 재할당이 금지된 변수

- 상수도 `변수`이다.
- 상수도 `메모리 공간을 가지며 값을 가짐`
- 변수는 언제든지 재할당 할 수 있지만 `상수는 재할당이 금지된 것`이다.
- 상태 유지, 가독성, 유지보수의 편의를 위해 적극 사용권장
- 일반적으로 상수 이름은 대문자로 선언
- 여러 단어로 이루어지면 언더 스코어 (\_)로 구분
- 스네이크 케이스로 표현

```javascript
// TAX_RATE 라는 상수(constant)를 적용하므로써, 코드의 가독성이 증가한다.
const TAX_RATE = 0.1;

let preTaxPrice = 100;
let afterTaxPrice = preTaxPrice + preTaxPrice * TAX_RATE;

console.log(afterTaxPrice); // 110
```

#### 4. const 객체

- const 키워드로 객체를 할당할 경우 변수의 값을 변경할 수 있다.
- 객체는 `재할당 없이도 직접 값을 변경할 수 있기 때문이다.`
- const 키워드는 재할당을 금지할뿐 `불변`을 의미하지 않는다.
  - 새로운 값을 재할당 하는 것은 불가능
  - 프로퍼티 동적 생성 / 삭제 / 프로퍼티 값 변경을 통하나 객체의 변경 가능
  - 객체가 변경되더라고 `변수에 할당된 참조 값`은 변경되지 않음

```javascript
const person = {
  name: "Yoon",
};

// 객체는 변경 가능한 값(mutable value) == 재할당 없이 변경 가능
person.name = "HYUK";
console.log(person.name); // { name: "HYUK" };
```
