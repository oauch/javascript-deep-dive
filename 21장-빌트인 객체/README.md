## 21. 빌트인 객체

- 자바스크립트 객체는 3가지로 분류

1. 표준 빌트인 객체

- ECMAScript 사양에 정의된 객체를 의미
- 애플리케이션 전역에 공통 기능을 제공
- 전역 객체의 프로퍼티로 제공되므로 별도의 선업 없이 전역 변수처럼 언제나 참조 가능

2. 호스트 객체

- ECMAScript 사양에 정의되어 있지는 않지만 브라우저 환경 or Node.js 환경에서 추가로 제공하는 객체

3. 사용자 정의 객체

- 사용자가 직접 정의한 객체

### 21-1. 표준 빌트인 객체

- 인스턴스를 생성할 수 있는 생성자 함수 객체
- 자바스크립트는 Object / String / Number / Boolean / Symbol / Array / Map,Set / Function 등 40개 정도의 표준 빌트인 객체를 제공
- 프로토타입 메서드 / 정적 메서드 제공
- Math, Reflect, JSON은 정적 메서드만 제공
- 생성자 함수인 표준 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩
  - ex) 표준 빌트인 객체인 String을 생성자 함수로서 호출하여 생성한 String 인스턴스의 프로토타입은 String.prototype

```javascript
const strObj = new String("WI");

console.log(Object.getPrototypeOf(strObj) === String.prototype); // true
```

- 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체는 다양한 기능의 빌트인 프로토타입 메서드 제공
- 인스턴스 생성 없이도 호출 가능한 빌트인 정적 메서드 제공

```javascript
// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(1.5);

// Number.prototype 에 프로토타입 메서드인 toFixed
console.log(numObj.toFixed()); // 2

// Number 빌트인 객체의 정적 메서드
console.log(Number.isInteger(0.5)); // false
```

### 21-2. 원시값과 래퍼 객체

- 문자열 / 숫자 / 불리언 같은 원시값에 대해 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체라고 한다.
- 문자열 / 숫자 / 불리언 등의 원시값이 마치 객체처럼 마침표(.), 대괄호 표기법으로 접근하여 메서드 호출
  - 자바스크립트 엔진이 일시적으로 원시값으로 연관된 객체로 반환해준다.
  - 암묵적으로 엔진이 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드로 호출하고 다시 원시값으로 되돌림

```javascript
const str = "Hello";

// 원시 타입인 문자열 -> 래퍼 객체인 String 인스턴스로 변환
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
// 생성되었던 래퍼 객체는 "가비지 컬렉션"의 대상이 된다.
console.log(typeof str); // string  << 🔎 String 표준 빌트인 생성자 함수 타입이 아니다.
```

- 래퍼 객체의 역할이 끝나면 원시값은 다시 식별자에 연결되고 래퍼 객체는 가바지 컬렉션 대상이 된다.

```javascript
// 식별자(변수) str은 'Hello' 문자열 원시값을 가진다.
const str = "Hello";

// 식별자 str은 암묵적으로 생성된 "래퍼 객체"를 가리킨다.
// 식별자 str의 값 'Hello' 는 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된다.
// 래퍼 객체의 "name 프로퍼티가 동적 추가"된다.
str.name = "WI";

// 래퍼 객체의 역할이 끝나고, 식별자 str은 다시 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.
// 생성되었던 래퍼 객체는 아무도 참조하지 않게 되고, 가비지 컬렉션의 대상이 된다.

// 식별자 str 은 더 이상 래퍼 객체를 가리키고 있지 않다.
// 그러므로, 래퍼 객체에 추가된 name 프로퍼티는 더 이상 str 식별자로 접근할 수 없다. 즉 undefined 를 반환한다.
console.log(str.name); // undefined

// 실제로 식별자 str이 가리키는 데이터를 참조해보면 타입이 string 문자열이다.
console.log(typeof str, str); // string Hello
```

- 원시값 중 Number / String / Boolean은 래퍼 객체를 가지며 ES6에 도입된 Symbol 또한 래퍼 객체를 가진다.
- undefined / null 원시값은 래퍼 객체를 가지지 않는다. -> 래퍼 객체처럼 사용하려면 에러 발생
- 이처럼 래퍼 객체가 생성되는 원시값들은 암묵적으로 표준 빌트인 객체의 프로토타입에서 제공하는 메서드 || 프로퍼티 참조 가능
  - 그래서 굳이 new 연산자와 함께 인스턴스를 생성할 필요도 없고, 권장하는 방법 ❌

### 21-2. 전역 객체

- 자바스크립트 코드가 실행되기 이전(런타임 이전) 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체
- 브라우저 환경에서 window / self / this / frames로 사용하며 Node.js 환경에서 global로 전역 객체 접근

```
globalThis

- ES11에서 도입된 globalThis는 브라우저 환경 / Node.js 환경에서 여러 방식으로 전역 객체를 가르키던 것을 통일한 형태
- globalThis는 표준 사양이므로 ECMAScript 표준 사양을 준수하는 모든 환경에서 사용 가능
```

- 전역 객체는 표준 빌트인 객체 / 호스트 객체 / var 키워드로 선언한 전역 변수, 함수를 프로퍼티로 가짐
  - 계층적 구조상 어떤 객체에도 속하지 않은 모든 빌트인 객체(표준 빌트인 객체, 호스트 객체)의 최상위 객체

💡 최상위 객체 : 프로토타입 상속 관계상에서 최상위 객체를 의미가 아닌, 어떤 객체의 프로퍼티도 아니며, 객체의 계층적 구조상 표준 빌트인 객체 / 호스트 객체를 프로퍼티로 소유한다는 의미

✅ 전역 객체 특징

1. 전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않는.
2. 전역 객체의 프로퍼티를 참조할 때, window / global 생략 가능
3. 전역 객체는 표준 빌트인 객체를 프로퍼티로 가짐
4. 브라우저 / Node.js등 환경에 따라 추가적인 프로퍼티, 메서드를 가짐
5. var 키워드로 선언한 전역 변수 / 선언하지 않은 변수에 값을 할당한 암묵적 전역, 전역 함수는 전역 객체의 프로퍼티가 된다.
6. let, const 키워드로 선언한 전역변수 = 전역 객체의 프로퍼티 ❌ (window / global로 접근 불가능)

- 해당 키워드는 보이지 않는 개념적 블록이 존재하게 된다.

7. 모든 자바스크립트 코드는 하나의 전역 객체를 공유

<br />

### 21-3. 빌트인 전역 프로퍼티

- 전역 객체의 프로퍼티를 의미
- 주로 애플리케이션 전역에서 사용하는 값 제공

#### Infinity

- 무한대를 나타내는 숫자 값

#### NaN

- 숫자가 아님을 나타내는 값

#### undefined

- 원시타입 undefined

### 21-4. 빌트인 전역 함수

#### eval 함수

- 자바스크립트 코드를 나타내는 문자열을 인수로 전달 받음
- 전달받은 문자열 코드가 `표현식`이면 문자열 코드를 런타임에 평가하여 값을 생성
- 전달받은 문자열 코드가 `문`이면 문자열 코드를 런타임에 실행
- 자신이 호출된 위치에 해당하는 기존의 스코프를 런타임에 동적으로 수정
- strict mode 존재시, 기존 스코프를 수정하지 않고 eval 함수 자신의 자체적인 스코프 생성
- 다만, eval 함수를 통해 사용자로부터 입력받은 콘텐츠를 실행하는 것은 보안상 매우 취약 / 자바스크립트 엔진에 의해 최적화가 수행되지 않는 코드 특성상 처리속도가 느리다.

> eval 함수 사용은 옳지 않음

#### inFinite 함수

- 전달받은 인수가 정상적인 유한수인지 검사
- 반환값 boolean
- 유한수 -> true
- 무한수 -> false
- 전달받은 인수의 타입이 숫자가 아닌 경우, 숫자로 타입 변환후 검사 진행

#### isNaN 함수

- 전달받은 인수가 NaN인지 검사
- 반환값 boolean
- 숫자가 아님 -> false
- 숫자 -> true
- 전달받은 인수의 타입이 숫자가 아닌 경우, 숫자로 타입 변환후 검사 진행

#### parseFloat 함수

- 전달받은 `문자열 인수`를 `실수`로 해석하여 반환

#### parseInt 함수

- 전달 받은 `문자열 인수`를 `정수`로 해석하여 반환
- 인수가 문자열이 아니면, 문자열로 변환후, 정수로 해석하여 반환
- 두 번째 인수로 진법을 나타내는 기수를 전달 할 수 있음
  - 첫 번째 인수로 전달된 문자열을 해당 기수의 숫자로 해석하여 반환
  - 반환값은 언제나 10진수
- `parseInt(값, 변환하고 싶은 진수)`

```javascript
// '10' -> 10진수로 해석(parsing) -> 10진수 정수로 반환(10)
console.log(parseInt("10")); // 10

// '10' -> 2진수로 해석(parsing) -> 10진수 정수로 반환(2)
console.log(parseInt("10", 2)); // 2

// '10' -> 8진수로 해석(parsing) -> 10진수 정수로 반환(8)
console.log(parseInt("10", 8)); // 8

// '10' -> 16진수로 해석(parsing) -> 10진수 정수로 반환(16)
console.log(parseInt("10", 16)); // 16
```

- 10진수 숫자 -> 해당 진수의 문자열로 반환
- `toString()` 메서드 사용

```javascript
const x = 15;

console.log(x.toString(2)); // '1111'
console.log(parseInt(x.toString(2), 2)); // 15

console.log(x.toString(8)); // '17'
console.log(parseInt(x.toString(8), 8)); // 15

console.log(x.toString(16)); // 'f'
console.log(parseInt(x.toString(16), 16)); // 15

console.log(x.toString()); // '15'
console.log(parseInt(x.toString())); // 15
```

- 첫 번째 문자열 내에, 두 번째 인수로 전달한 radix 로 나타내는 숫자가 아닌 문자와 마주치면 전부 무시되고 해석된 정수값만 반환

```javascript
// A은 10진법에서 벗어남 -> '10' 만 변환 -> 5
console.log(parseInt("1A0")); // 1

// 2은 2진법에서 벗어남 -> '10' 만 변환 -> 2
console.log(parseInt("102", 2)); // 2

// 8은 8진법에서 벗어남 -> '5' 만 변환 -> 5
console.log(parseInt("58", 8)); // 5

// G는 16진법에서 벗어남 -> 'F' 만 변환 -> 15
console.log(parseInt("FG", 16)); // 15
```

#### encodeURI / decodeURI 함수

```javascript
📍 인코딩 : URI의 문자들을 이스케이프 처리하는 것
📍 디코딩 : 인코딩된 URI를 이스케이프 이전 상태로 복원하는 것
```

- 이스케이프 처리는 네트워크를 통해 정보를 공유할 때 어떤 시스템에서도 읽을 수 있는 아스키 문자 셋으로 변환하는 것
- URL은 아스키 문자 셋으로 구성되어야 하며 한글을 포함한 대부분의 외국어나 아스키 문자셋에 정의 되지 않은 특수 문자를 포함하지 않아야 한다.
- 따라서 URL 내부에서 의미를 갖고 있는 `문자(%, ?, #)`나 `한글, 공백` 등 또는 시스템에 의해 해석 될 수 있는 문자를 이스케이프 처리하여 야기될 수 있는 문제를 예방해야함
- `알파벳, 0~9 숫자, -, \_, ., !, ~, \*, ', (, )` 문자는 이스케이프 처릴에서 제외

```javascript
// 완전한 URI
const uri = "http://example.com?name=임윤혁&job=programmer&student";

// 원래 URI 형태 -> 이스케이프 처리된 URI 형태로
const enc = encodeURI(uri);
console.log(enc); // http://example.com?name=%EC%9C%84%EC%98%81%EB%AF%BC&job=programmer&student

// 인코딩(이스케이프 처리)된 URI -> 원래 URI 형태로
const dec = decodeURI(enc);
console.log(dec); // http://example.com?name=임윤혁&job=programmer&student
```

#### encodeURIComponent / decodeURIComponent 함수

- `encodeURI / decodeURI와의 차이점`은 URI 전체가 아닌 `URI의 구성요소인 쿼리 스트링의 일부`로 간주
- 따라서, 쿼리 스트링 구분자로 사용되는 (=, ?, &)까지 인코딩
- encodeURI 함수는 매개변수로 전달된 문자열을 URI 전체라고 간주
- 쿼리스트링을 구분하는 구분자는 인코딩 안함

```javascript
// URI의 쿼리 스트링(구성요소만)
const uriComp = "name=&job=programmer&student";

// encodeURIComponent 는 쿼리 스트링 구분자(=, ?, &)까지 인코딩한다.
let enc = encodeURIComponent(uriComp);
console.log(enc); // name%3D%EC%9C%84%EC%98%81%EB%AF%BC%26job%3Dprogrammer%26student

let dec = decodeURI(enc);
console.log(dec); // name%임윤혁%26job%3Dprogrammer%26student

// encodeURI 는 쿼리 스트링 구분자(=, ?, &)는 인코딩 하지 않는다.
enc = encodeURI(uriComp);
console.log(enc); // name=%EC%9C%84%EC%98%81%EB%AF%BC&job=programmer&student

dec = decodeURI(uriComp);
console.log(dec); // name=임윤혁&job=programmer&student
```
