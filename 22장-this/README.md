### 22. this

- 자신이 속한 객체 / 자신이 생성할 인스턴스를 가르키는 `자기 참조 변수`
- this를 통해 자신이 속한 객체 / 자신이 생성할 인스턴스의 프로퍼티나 메서드 참조
- this는 자바스크립트 엔진에 의해 암묵적 생성

```javascript
📍 this 바인딩
- 바인딩 : `식별자`와 `값`을 연결화는 과정
- this 바인딩 : `this`와 `this가 가르키는 객체`를 바인딩
```

- 객체 리터럴의 메서드 내부에서 this는 메서드를 호출하는 객체

```javascript
// 객체 리터럴
const circle = {
  radius: 5,

  getDiameter() {
    // 여기서 this 는 메서드를 호출한 객체
    return 2 * this.radius;
  },
};

console.log(circle.getDiameter()); // 10
```

- 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스

```javascript
// 생성자 함수
function Circle(radius) {
  // 여기서 this는 생성자 함수 Circle이 생성할 인스턴스
  this.radius = radius;
}

Circle.prototype.getDiameter = function () {
  // 여기서 this는 생성자 함수가 생성할 인스턴스
  return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

- this가 가르키는 값 = this 바인딩은 함수 호출 방식에 의해 동적으로 결정
- 클래스 기반 언어의 this는 언제나 클래스가 생성하는 인스턴스를 가르킴.
- 하지만, `자바스크립트의 this`는 함수가 호출되는 방식에 따라 this에 바인딩될 값 = this 바인딩이 동적으로 결정
- strict mode는 this 바인딩에 영향을 준다.
  - this는 코드 어디에서도 참조 가능
  - 전역에서도 함수 내부에서도 참조 가능
  - this의 본질은 객체의 프로퍼티 / 메서드를 참조하기 위한 자기 참조 변수의 개념
  - 따라서, 객체가 아닌 일반 함수 내부의 this는 필요가 없다.
  - strict mode에서 this를 참조할 경우 undefined 반환

### 22-1. 함수 호출 방식 / this 바인딩

- 일반 함수 호출
- 메서드 호출
- 생성자 함수 호출
- Function.prototype.apply / call / bind 메서드에 의한 간전 호출

#### 22-1.1. 일반 함수 호출

- this에는 전역 객체 바인딩
- 전역 함수는 물론 중첩 함수를 일반함수로 호출된 모든 함수 내부의 this에는 전역 객체 바인딩
- strict mode에서 this 참조시, undefined 바인딩

```javascript
function foo() {
  console.log(`전역 함수 foo의 this : ${this}`); // 전역 함수 foo의 this : [object global]

  function bar() {
    console.log(`중첩 함수 bar의 this : ${this}`); // 중첩 함수 bar의 this : [object global]
  }

  bar();
}
foo();
```

```javascript
// 전역 변수 value ( 전역 객체 프로퍼티 )
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log(`foo 메서드의 this : ${this}`); // foo 메서드의 this : [object Object]
    console.log(`foo 메서드의 this가 가리키는 객체의 value : ${this.value}`); // foo 메서드의 this가 가리키는 객체의 value : 100

    // 메서드 내, 정의한 중첩 함수
    function bar() {
      console.log(`메서드 내 중첩 함수 bar의 this : ${this}`); // 메서드 내 중첩 함수 bar의 this : [object global]
      console.log(
        `중첩 함수 bar의 this가 가리키는 객체의 value : ${this.value}`
      ); // 중첩 함수 bar의 this가 가리키는 객체의 value : 1
    }

    // 메서드 내 중첩 함수를 일반 함수 처럼 호출
    bar();
  },
};

obj.foo();
```

```javascript
// 전역 변수 value ( 전역 객체 프로퍼티 )
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log(`foo 메서드의 this : ${this}`); // foo 메서드의 this : [object Object]
    console.log(`foo 메서드의 this가 가리키는 객체의 value : ${this.value}`); // foo 메서드의 this가 가리키는 객체의 value : 100

    // 메서드 내 콜백 함수
    setTimeout(function () {
      console.log(`callback 함수의 this : ${this}`); // callback 함수의 this : [object global]
      console.log(
        `callback 함수의 this가 가리키는 객체의 value : ${this.value}`
      ); // callback 함수의 this가 가리키는 객체의 value : 1
    }, 100);
  },
};

obj.foo();
```

- 중첩 함수 / 콜백 함수의 경우, 용도가 일반적으로 외부 함수를 돕는 헬퍼 함수 역할하므로, this가 가르키는 객체가 전역 객체일 경우 헬퍼 함수 역할을 하기 힘듬
- 메서드 내부의 중첩함수 / 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키기 위한 방법은 다음과 같다.
  - 메서드의 this를 바인딩할 객체에 변수 할당
  - ` Function.prototype.apply`` or  `[Function.prototype.call](http://Function.prototype.call)` 에 의한 바인딩

```javascript
// 전역 변수 value ( 전역 객체 프로퍼티 )
var value = 1;

const obj = {
  value: 100,
  foo() {
    // 콜백 함수에 바인딩할 obj 객체를 가리키는 this를 변수 that에 할당
    const that = this;

    setTimeout(function () {
      console.log(`callback 함수의 this : ${that}`); // callback 함수의 this : [object Object]
      console.log(
        `callback 함수의 this가 가리키는 객체의 value : ${that.value}`
      ); // callback 함수의 this가 가리키는 객체의 value : 100
    }, 100);
  },
};

obj.foo();
```

```javascript
// 전역 변수 value ( 전역 객체 프로퍼티 )
var value = 1;

const obj = {
  value: 100,
  foo() {
    // Function.prototype.bind 메서드로 obj 객체를 가리키는 this를 콜백함수에 명시적으로 바인딩
    setTimeout(
      function () {
        console.log(`callback 함수의 this : ${this}`); // callback 함수의 this : [object Object]
        console.log(
          `callback 함수의 this가 가리키는 객체의 value : ${this.value}`
        ); // callback 함수의 this가 가리키는 객체의 value : 100
      }.bind(this),
      100
    );
  },
};

obj.foo();
```

#### 22-1-2. 메서드 호출

- 기본적으로 `메서드 내부의 this`에는 `메서드를 호출하는 객체` 바인딩
- 메서드를 호출할 때 메서드 앞에 마침표 연산자(.) 앞에 기술한 객체 바인딩
  - ⚠️ 메서드 내부의 this = 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩

```javascript
const person = {
  name: "WI",
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩
    // getName 메서드는 person 객체에 포함된 것이 아닌, 독립적으로 존재하는 별도의 객체 개념
    return this.name;
  },
};

console.log(person.getName()); // WI
```

- 메서드 = 특정 객체에 포함된 것이 아닌 `독립적으로 존재하는 별도의 객체`
  - 다른 객체의 프로퍼티로 할당 가능
  - 일반 변수에 할당하여 일반 함수로 호출도 가능

```javascript
const person = {
  name: "WI",
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩
    // getName 메서드는 person 객체에 포함된 것이 아닌, 독립적으로 존재하는 별도의 객체 개념
    return this.name;
  },
};

const anotherPerson = {
  name: "KIM",
};

// getName 메서드를 anotherPerson 객체의 메서드로 할당 (getName 메서드는 독립적인 객체이기 때문)
anotherPerson.getName = person.getName;

// getName을 호출한 객체는 이 시점에선 person이 아닌 anotherPerson이다.
console.log(anotherPerson.getName()); // KIM

// getName을 getName 변수에 할당 (getName 메서드는 독립적인 객체이기 때문)
const getName = person.getName;

// getName을 호출한 객체는 이 시점에서는 전역 객체다.
// 전역 객체에 프로퍼티에는 name 이라는 프로퍼티가 존재하지 않다.
// 참조 시, 자바스크립트 엔진이 암묵적으로 undefined 로 초기화한다.
console.log(getName()); // undefined
```

- 프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체 바인딩

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입에 getName 메서드 할당
Person.prototype.getName = function () {
  return this.name;
};

// me 인스턴스 생성
const me = new Person("WI");

// 이 시점에서 getName 메서드를 호출한 주체는 me 객체
console.log(me.getName()); // WI

Person.prototype.name = "KIM";

// 이 시점에서 getName 메서드를 호출한 주체는 Person.prototype 객체
console.log(Person.prototype.getName()); // KIM
```

#### 22-1-3. 생성자 함수 호출

- 생성자 함수 내부의 this에는 함수가 생성할 인스턴스가 바인딩
- 생성자 함수의 역할을 객체를 생성하는 함수

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
  this.getName = function () {
    return `안녕하세요. 저는 ${this.name}입니다.`;
  };
}

const person1 = new Person("WI");
const person2 = new Person("KIM");

console.log(person1.getName()); // 안녕하세요. 저는 WI입니다.
console.log(person2.getName()); // 안녕하세요. 저는 KIM입니다.
```

### Function.prototype.apply / call / bind 메서드에 의한 간접 호출

- apply / call / bind는 Function.prototype의 메서드
- 모든 함수가 이 메서드들을 상속받아 사용가능

- Function.prototype.apply(this로 사용할 객체, arguments 리스트(배열 or 유사배열객체))
- Function.prototype.call(this로 사용할 객체, arguments 인수 리스트(,로 구분하여 전달))
- Function.prototype.bind(this로 사용할 객체)

#### apply, call

- 본질적인 기능 = 함수 호출
- 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩
- 두 번째 인수를 함수에 전달하는 방식만 다를 뿐 동일하게 작동
- 대표적으로 arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우 효과적

```javascript
function convertArgsToArray() {
  console.log(arguments); // [Arguments] { '0': 1, '1': 2, '2': 3 }

  // 유사 배열 객체인 arguments 객체는 배열이 아니다.
  // 그런 arguments 객체에 대해, 배열 메서드인 Array.prototype.slice 메서드를 Function.prototype.call 메서드로 arguments 객체에 바인딩한다.
  // 유사 배열 객체 arguments 객체임에도 배열 메서드인 slice 메서드를 적용하여 반환된 새로운 배열 객체를 반환할 수 있게 된다.
  const arr = Array.prototype.slice.call(arguments);

  console.log(arr); // [ 1, 2, 3 ]

  return arr;
}

convertArgsToArray(1, 2, 3);
```

#### bind

- 본질적인 기능 = 함수를 호출하지 않고 this로 사용할 객체만 전달
- 대표적으로 메서드의 this / 메서드의 내부 중첩 함수 / 콜백 함수의 this가 불일치하는 문제 해결에 효과적

```javascript
const person = {
  name: "WI",
  foo(callback) {
    // bind 를 적용하지 않는다면, foo 메서드 내부에 콜백 함수에 정의된 this는 전역 객체(window 또는 global)를 가리킨다.
    // 전역 객체에는 name 프로퍼티가 없기 때문에, 원래는 undefined 를 출력하는 것이 맞다.
    // 하지만, Function.prototype.bind 메서드로 콜백 함수의 주체를 person 객체로 동적 바인딩 해주었다.
    // 때문에 person 객체의 name 프로퍼티에 접근할 수 있게 되었다.
    setTimeout(callback.bind(this), 100);
  },
};

person.foo(function () {
  console.log(`안녕하세요. ${this.name}입니다.`); // 안녕하세요. WI입니다.
});
```
