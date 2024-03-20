## 9. 타입변환과 단축평가

---

### 9-1. 타입 변환

```javascript
`명시적 타입변환` / `타입 캐스팅`은 개발자가 의도적으로 값의 타입을 변환하는 것
`암묵적 타입변환` / `타입 강제 변환`은 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 타입이 변환되는 것
```

- 명시적 / 암묵적 타입변환이 원시 값을 직접 변경하는 것은 아니다 -> 원시값 = 변경 불가능한 값
- 기존 원시값을 사용해 `다른 타입의 새로운 원시 값`을 생성 하는 것

#### 9-1-1. 암묵적 타입변환

```javascript
자바스크립트 엔진이 표현식을 평가할 때, 개발자의 의도와는 상관없이
"코드의 문맥을 고려해 암묵적으로" 데이터 타입을 강제 변환하는 것
```

> 문자열 타입 변환

```javascript

// 숫자 타입
NaN + ""; // "NaN"
Infinity + ""; // "Infinity"

// null 타입
null + ""; // "null"

// undefined 타입
undefined + ""; // "undefined"

// 심벌 타입
Symbol() + ""; // TypeError: ~

// 객체 타입
({}) + "";            // "[object Object]"
Math + "";            // "[object Math]"
[] + "";              // ""
[10, 20] + "";        // "10, 20"
(function(){}_ + ""); // "function(){}"
Array + "";           // function Array() { [native code] }"
```

<br />

> 숫자 타입으로 변환

```javascript

// 문자열 타입
+"";           // 0
+"0";          // 0
+"1";          // 1
+"string" + ;  // 0
+true;         // 1
+false;        // 0

// null 타입
+null;  // 0

// undefined 타입
+undefined;  // NaN

// 심벌 타입
+Symbol();  // TypeError: ~

// 객체 타입
+{}; // NaN
+[]; // 0
+[10, 20]; // NaN
+function () {}; // NaN
```

<br />

> 불리언 타입으로 변환

```javascript
if ("") console.log(x);
```

- if문, for문, 삼항 조건 연산자의 조건식은 불리언 값으로 평가되어야 하는 표현식
- 자바스크립트 엔진은 조건식의 평가 결과를 불리언 타입으로 암묵적 타입 변환

```javascript
if ("") console.log(1);
if (true) console.log(2);
if (0) console.log(3);
if ("str") console.log(4);
if (null) console.log(5);

// 2 4
```

- 자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy 값 / Falsy 값으로 구분
- 제어문의 조건식과 같이 불리언 값으로 평가되어야 할 문맥에서 true / false로 암묵적 타입 변환

<br />

`false`로 평가되는 falsy 값

- false
- undefined
- null
- 0, -0
- NaN
- ' ' (빈 문자열)

다음과 같은 값을 제외한 모든 값은 `true`로 평가되는 Truthy 값으로 평가

<br />

#### 9-1-2. 명시적 타입변환

```javascript
'개발자의 의도에 따라' 명시적으로 타입을 변경하는 방법
ex) 생성자 함수 (String, Number, Boolean)
```

> 문자열 타입 변환

- 문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법
  1. String 생성자 함수를 new 연산자 없이 호출
  2. Object.prototype.toString 메서드
  3. 문자열 연결 연산자를 이용

```javascript
// 1. String 생성자 함수를 new 연산자 없이 호출하는 방법
// 숫자 타입 -> 문자열 타입
String(1); // '1'
String(NaN); // 'NaN'
String(Infinity); // 'Infinity'

// 불리언 타입 -> 문자열 타입
String(true); // 'true'
String(false); // 'false'

// 2. Object.prototype.toString 메서드를 이용하는 방법
(1).toString(); // '1'
NaN.toString(); // 'NaN'
Infinity.toString(); // 'Infinity'
true.toString(); // 'true'
false.toString(); // 'false'

// 3. 문자열 연결 연산자를 이용하는 방법
// 숫자 타입 -> 문자열 타입
1 + ""; // '1'
NaN + ""; // 'NaN'
Infinity + ""; // 'Infinity'

// 불리언 타입 -> 문자열 타입
true + ""; // 'true'
false + ""; // 'false'
```

<br />

> 숫자 타입으로 변환

- 숫자 타입이 아닌 값을 숫자 타입으로 변환하는 방법
  1. Number 생성자 함수를 new 연산자 없이 호출
  2. parseInt, parseFloat 함수를 사용하는 방법 (문자열만 가능)
  3. `+` 단항 산순 연산자를 이용하는 방법
  4. `*` 산술 연산자를 이용하는 방법

```javascript
// 1. Number 생성자 함수를 new 연산자 없이 호출
// 문자열 타입 -> 숫자 타입
Number("0"); // 0
Number("-1"); // -1
Number("10.53"); // 10.53

// 불리언 타입 -> 숫자 타입
Number(true); // 1
Number(false); // 0

// 2. parseInt, parseFloat 함수를 사용하는 방법 (문자열만 가능)
// 문자열 타입 -> 숫자 타입
parseInt("0"); // 0
parseInt("-1"); // -1
parseInt("10.53"); // 10.53

// 3. + 단항 산술 연산자를 이용하는 방법
// 문자열 타입 -> 숫자 타입
+"0"; // 0
+"1"; // 1
+"10.53"; // 10.53

// 불리언 타입 -> 숫자 타입
+true; // 1
+false; // 0

// 4. * 산술 연산자를 이용한 방법
// 문자열 타입 -> 숫자 타입
"0" * 1; // 0
"-1" * 1; // -1
"10.53" * 1; // 10.53

// 불리언 타입 -> 숫자 타입
true * 1; // 1
false * 1; // 0
```

<br />

> 불리언 타입으로 변환

- 불리언 타입이 아닌 값을 불리언 타입으로 변환하는 방법
  1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
  2. ! 부정 논리 연산자를 2번 사용하는 방법

<br />

```javascript
// 1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 -> 불리언 타입
Boolean("x"); // true
Boolean(""); // false
Boolean("false"); // true

// 숫자 타입 -> 불리언타입
Boolean(0); // false
Boolean(1); // true
Boolean(NaN); // false
Boolean(Infinity); // true

// null 타입 -> 불리언 타입
Boolean(null); // false

// undefined 타입 -> 불리언 타입
Boolean(undefined); // false

// 객체 타입 -> 불리언 타입
Boolean({}); // true
Boolean([]); // true

// 2. ! 부정 논리 연산자를 2번 사용하는 방법
// 문자열 타입 -> 불리언 타입
!!"x"; // true
!!""; // false
!!"false"; // true

// 숫자 타입 -> 불리언 타입
!!0; // false
!!1; // true
!!NaN; // false
!!Infinity; // true

// null 타입 -> 불리언 타입
!!null; // false

// undefined 타입 -> 불리언 타입
!!undefined; // false

// 객체 타입
!!{}; // true
!![]; // true
```

---

### 9-2. 단축 평가

- 논리 연산의 결과를 결정하는 피연산자를 타입 변환 없이 그대로 반환
- 표현식을 평가하는 도중 평가결과가 확정된 경우 = 나머지 평가 과정 생략

```javascript
true || abcd; // true
false || abcd; // false
true && abcd; // abcd
false && abcd; // false
```

<br />

#### 9-2-1. 논리 연산자를 사용한 단축 평가

`논리곱 (&&)` : 논리 연산의 결과를 결정하는 것 = 2번째 피연산자

```javascript
"Apple" && "Banana"; // Banana
```

- 2개의 피연산자가 `모두 true로 평가되면`` -> true 반환
- 좌항 -> 우항으로 평가 진행

<br />

`논리합 (||)` : 논리 연산의 결정하는 것 = 1번째 피연산자

```javascript
"Apple" || "Banana"; // Apple
```

- 2개의 피연산자 중 `하나만 true로 평가되어도`` -> true 반환
- 좌항 -> 우항으로 평가 진행

<br />

```javascript
// 💡 논리 연산자 단축 평가 - 기본

// 논리합(||) 연산
"Cat" || "Dog"; // "Cat"
false || "Dog"; // "Dog"
"Cat" || false; // "Cat"

// 논리곱(&&) 연산
"Cat" && "Dog"; // "Dog"
false && "Dog"; // "false"
"Cat" && false; // "false"
```

```javascript
// if문을 대체하여 사용 가능하다 -> 깔끔한 코드로 만들 수 있다.
let done = true;
let msg = "";

// nope
if (done) msg = "값";

// good
msg = done && "값";
```

```javascript
// 💡 객체를 가르키기를 기대하는 변수가 null / undefined인지 확인하고 프로퍼티 참조시
// 객체는 { 키: 값 }으로 구성된 프로퍼티의 집합
// 객체를 가르키기를 기대하는 변수의 값이 아닌 null / undefined 경우, 객체 참조 시 TypeError가 발생 -> 프로그램 강제 종료

// nope
let elem = null;
let val = elem.val; // TypeError: ~

// good
let elem = null;
let elem = elem && elem.val; // null
```

```javascript
// 💡 함수 매개변수에 기본값을 설정할 때
// 함수를 호출할 때 인수를 전달하지 않으면 매개변수에  undefined 할당

// nope : 인수를 전달하지 않을 경우
function getStrLength(str) {
  return str.length;
}
getStrLength(); // TypeError: ~

// good : 단축 평가를 사용한 매개변수의 기본값 설정
function getStrLength(str) {
  str = str || "";
  return str.length;
}
getStrLength(); // 0

// good : ES6의 매개변수 default parameter 설정
function getStrLength(str = "") {
  return str.length;
}
getStrLength(); // 0
```

<br />

#### 9-2-2. 옵셔너러 체이닝 연산자

- 옵셔널 체이닝 연산자 = `?`

- 좌항의 피연산자가 `null / undefined` -> `undefined` 반환
- 그렇지 않으면, 우항의 프로퍼티 참조

```javascript
let elem = null;
let val = elem?.value; // undefined
```

- 객체를 가르키기를 기대하는 변수가 null / undefined가 아닌지 확인하고 프로퍼티를 안전하게 참조할 때 유용
- 옵셔널 체이닝 도입 이전
  - 논리곱(&&)을 사용한 단축 평가를 통해, 변수가 null / undefined인지 확인 했다

```javascript
let elem = null;
let val = elem && elem.value; // null
```

```javascript
// 💡 논리곱(&&) 연산자 vs 옵셔널 체이닝 연산자

// nope : 논리곱 연산자 = 좌항 피연산자가 Falsy값이면, 좌항 피연산자를 그대로 반환 (단, 0 또는 ''은 객체로 평가될 때도 있다.)
let str = "";
let length = str && str.length; // ''

// good : 옵셔널 체이닝 = 좌항 피연산자가 Falsy값이라도 null / undefined만 아니면, 우항의 프로퍼티 참조
let str = "";
let length = str?.length; // 0
```

<br />

#### 9-2-3. null 병합 연산자

- 병합 연산자 = `??`
  - 좌항의 피연산자가 `null / undefined` -> `우항의 피연산자` 반환
  - 그렇지 않으면 -> `좌항의 프로퍼티` 참조

```javascript
let foo = null ?? "Car"; // 'Car'
```

- null 병합 연산자 도입 이전
  - `논리합(||)`을 사용한 단축평가를 통해 변수의 기본값 설정

```javascript
// 💡 논리합(||) 연산자 vs null 병합 연산자

// nope : 논리합(||) 연산자 = 좌항의 피연산자가 Falsy값이면, 우항의 피연산자 반환 (단, 0 또는 ''은 기본값으로서 유효하다면 예기치 않은 동작 발생)
let foo = "" || "Car"; // "Car"

// good : null 병합 연산자 = 좌항의 피연산자가 Falsy값이라도 null / undefined가 아니면, 좌항의 피 연산자 그대로 반환;
let foo = "" ?? "Car"; // ""
```
