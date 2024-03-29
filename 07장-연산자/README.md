## 7. 연산자

- 연산자는 하나 이상의 표현식을 대상으로 산술, 할당, 비교, 논리, 타입, 지수 연산등을 수행하여 하나의 값을 만든다.

```javascript
// 산술 연산자
5 * 4; // 20

// 문자열 연결 연산자
"My name is " + "YoonHyuk"; // My name is YoonHyuk

// 할당 연산자
color = "red"; // 'red'

// 비교 연산자
3 > 5; // false

// 논리 연산자
true && false; // false

// 타입 연산자
typeof "shooooo"; // string
```

---

### 7-1. 산술 연산자

```
피연산자를 대상으로 수학적 계산을 수행해 새로운 숫자 값을 만든다.
산술 연산이 불가능한 경우, NaN 반환
```

<br />

#### 7-1-1. 이항 산술 연산자

```
2개의 피연산자를 산술 연산하여 숫자 값을 만든다.
산술 연산은 피연산자의 값이 바뀌는 경우는 없고, 언제나 새로운 값을 만든다.
```

```javascript
5 + 2; // 7
5 - 2; // 3
5 * 2; // 10
5 / 2; // 2.5
5 % 2; // 1
```

<br />

#### 7-1-2. 단항 산술 연산자

```
1개의 피 연산자를 연산하여 숫자 값을 만든다.
증가(++) / 감소(--) 연산자는 피연산자의 값을 변경하는 부수효과를 가진다.
```

```javascript
var x = 1;

x++;
console.log(x); // 2

x--;
console.log(x); // 1
```

- 증가 / 감소 연산자는 위치에 따라 의미가 달라진다.

```javascript
var x = 5;
var result;

// 선할당 후증가
result = x++;
console.log(result, x); // 5, 6

// 선증가 후할당
result = ++x;
console.log(result, x); // 7, 7
```

- `+` 단항 연산자는 피연산자에 어떠한 효과도 없다.
- 숫자 타입이 아닌 피연산자에 `+` 단항 연산자를 사용하게 되면 피연산자를 숫자 타입으로 변환하여 반환
- 피연산자를 변경하는 것이 아닌 `숫자 타입으로 변환한 값`을 생성해서 반환

```javascript
var x = "1";

console.log(+x); // 1
console.log(x); // '1'

// 불리언 값을 숫자 -> 숫자로 반환
var x = true;
console.log(+x); // 1
console.log(x); // true

// 문자열은 숫자로 반환할 수 없음 -> NaN 반환
x = "Hello";
console.log(+x); // NaN
console.log(x); // 'Hello'
```

- `-` 단항 연산자는 피연산자의 부호를 반전한 값을 반환
- 숫자 타입이 아닌 피연산자에 적용 가능
- 피연산자를 숫자 타입으로 반환

<br />

#### 7-1-3. 문자열 연결 연산자

- `**+` 연산자는 피연산자 중 하나 이상이 문자열인 경우, 문자열 연결 연산자로 동작

```javascript
// 문자열 연결 연산자
"1" + 2; // '12'
1 + "2"; // '12'

// 산술 연산자
1 + 2; // 3

// true는 1로 타입 반환
1 + true; // 2

// false는 0으로 타입 반환
1 + false; // 1

// null은 0으로 타입 반환
1 + null; // 1

// undefined는 숫자로 타입 반환 X
+undefined; // NaN
1 + undefined; // NaN
```

- 자바스크립트는 개발자의 의도와는 상관 없이 자바스크립트 엔진에 의해 자동으로 타입이 변환
- 이를 암묵적 타입변환 / 타입 강제 변환으로 `true, false, null`은 숫자 타입으로 변환 (1, 0, 0)
