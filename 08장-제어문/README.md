## 8. 제어문

> 제어문은 조건에 따라 코드 블록을 실행하거나(조건문) 반복 실행할 때(반복문) 사용

- 제어문 코드의 실행 흐름을 인위적으로 제어할 수 있다.
  - 순차적으로 진행하는 직관적인 코드의 흐름을 혼란스럽게 만든다.
- 즉, 제어문은 코드의 흐름을 이해하기 어렵게 만들어 가독성을 떨어지게 만드는 단점을 가질 수 있다.
  - 가독성이 좋지 않은 코드는 오류를 발생시킬 수 있는 원인이 된다.

### 8-1. switch문

> 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case문으로 실행 흐름을 옮긴다.

```javascript
switch (표현식) {
  case 표현식1:
    실행문1;
    break;
  case 표현식1:
    실행문1;
    break;
  ...
  default:
    default 실행문;
}
```

- 조건문이 간단한 거는 `if - else`문으로 해결하는 것이 좋다.
- 조건문이 많아지면 `switch - case`문으로 해결하는 것이 좋다.

<br />

### 8-2. 반복문

```javascript
자바스크립트는 배열 순회 시, 주로 사용하는 "map"메서드
"객체의 프로퍼티르 열거 할 때", 주로 사용하는 "for - in"문
"ES6에서 도입된 이터러블을 순회 시" 사용하는 "for - of"문
```

```
while문 = 반복 횟수가 불명확할 때 사용
for문 = 반복 횟수가 명확할 때 사용
```

```javascript
for (초기값; 조건식; 증감식) {}
```

<br />

### 8-3. break문

> 코드 블럭 탈출

- 레이블 문, 반복문 또는 switch문을 탈출할 때 사용

```javascript
레이블 문 = '식별자'가 붙은 문을 말함

+ 레이블 문은 프로그램 실행순서를 제어하는데 사용
+ switch 문의 case문과 default문도 '레이블 문'
+ 레이블 문을 탈출 시, break문에 식별자 지정 필요


// foo 라는 레이블 식별자가 붙은 문
foo: console.log('foo');

// foo 라는 식별자가 붙은 레이블 블록문
foo: {
  console.log(1);
  break foo;
  console.log(2);
}
```

- 레이블 문은 중첩된 for문 외부로 탈출 할 때 유용하지만 권장하지 않는다.
- 레이블 문을 사용하면 프로그램 흐름이 복잡해져 가독성이 나빠지고 오류 발생 가능성이 높아진다.

<br />

### 8-4. continue문

```
💡 반복문의 코드 블록 실행을 현 시점에서 중단하고, 반복문의 증감식으로 실행 흐름을 옮긴다.
💡 break 문처럼 반복문 탈출을 하는 것은 아니다.
```

```javascript
// if문 내에서 여러 코드를 작성해야 할 경우 -> continue 문 사용하지 않아될 경우
let arr = [1, 2, 3, 4, 5];
let target = 3;
let count = 0;

for (let i = 0; i < arr.length; i++) {
  if (arr[i] <= target) {
    count++;
    // 코드
    // 코드...
  }
}

// if문 내에서 여러 코드를 작성해야 할 경우 -> continue 문 사용한 경우 -> depth가 하나 줄어든다.
let arr = [1, 2, 3, 4, 5];
let target = 3;
let count = 0;

for (let i = 0; i < arr.length; i++) {
  // arr[i] 가 target 초과이면 count 증감하지 않는다.
  if (arr[i] > target) continue;

  count++;
}
```
