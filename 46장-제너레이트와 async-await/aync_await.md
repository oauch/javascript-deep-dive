# async / await

- ES8 도입
- 간단하고 가독성 좋게 비동기 처리를 동기처럼 동작하도록 구현
- 프로미스 기반으로 동작
  - `프로미스의 후속처리 메서드 없이` 마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현 가능

```js
const fetch = require("node-fetch");

async function fetchData() {
  const url = "url";

  const res = await fetch(url);
  const data = await res.json();

  return data;
}
```

<br />

## async 함수

- await 키워드는 반드시 async 함수 내부에서 사용
- async 함수는 async 키워드를 사용해 정의
- 언제나 프로미스 반환
- async 함수는 명시적으로 프로미스를 반환하지 않더라도, 암죽적으로 반환 값을 resolve하는 프로미스 반환

```js
async function foo(n) {
  return n;
}

foo(1).then((v) => console.log(v)); // 1

const bar = async function (n) {
  return n;
};
bar(2).then((v) => console.log(v)); // 2

const bax = async (n) => n;
bar(3).then((v) => console.log(v)); // 3

const obj = {
  async foo(n) {
    return n;
  },
};
obj.foo(4).then((v) => console.log(v)); // 4
```

<br />

## await 키워드

- 프로미스가 settled 상태 (즉, 비동기 처리가 수행된 상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve 한 처리 결과 반환)
- 반드시 프로미스 앞에 사용

```js
const fetchData = async () => {
  // 인수로 전달한 URL로 HTTP 요청에 대한 서버의 응답이 도착해서 fetch 함수가 반환한 프로미스가 settled 상태가 될 때까지 대기
  // 이후, 프로미스가 settled 상태가 되면 프로미스가 resolve 한 처리 결과가 res 변수에 할당
  const res = await fetch("yourURL");

  // res 변수에 할당된 처리 결과(Response 객체, 정확히는 Promise 객체)에서 json 메서드로 Response Body 값을 역직렬화해서 반환받음
  const data = await res.json();

  return data;
};
```

- await 키워드는 다음 실행을 일시 중지시켰다가 프로미스가 settled 상태가 되면 다시 재개하는 방식

```js
async function foo() {
  const a = await new Promise((resolve) => setTimeout(() => resolve(1), 3000));
  const b = await new Promise((resolve) => setTimeout(() => resolve(2), 2000));
  const c = await new Promise((resolve) => setTimeout(() => resolve(3), 1000));

  console.log(a, b, c);
}

foo();
// ( 약 6초 뒤... )
// 1 2 3

/**
 * 첫 번째 비동기 처리가 약 3초 소요되는 동안 대기
 * 첫 번째 비동기 처리가 완료되고, 두 번째 비동기 처리가 약 2초 소요되는 동안 대기
 * 두 번째 비동기 처리가 완료되고, 세 번째 비동기 처리가 약 1초 소요되는 동안 대기
 *
 * 마지막으로 console 출력하고 종료
 *
 * 총 = 약 6초
 */
```

- 모든 프로미스에 await 키워드를 사용하는 것은 옳지 않음
  - 각각의 비동기 처리가 서로 연광성 없이 개별적으로 수행되도 상관 없는 것이면 -> 굳이 앞선 비동기 처리 완료가 될때까지 대기할 필요 없음
  - 모든 프로미스가 서로 연관되어 순서가 보장되면서 실행되어야 하는 경우에만 모든 프로미스의 await를 적용

```js
async function foo() {
  // 각각의 비동기 처리가 서로 연관이 없다.
  const a = new Promise((resolve) => setTimeout(() => resolve(1), 3000));
  const b = new Promise((resolve) => setTimeout(() => resolve(2), 2000));
  const c = new Promise((resolve) => setTimeout(() => resolve(3), 1000));

  // 모든 비동기 처리를 "병렬"로 실행
  const res = await Promise.all([a, b, c]);
  console.log(res);
}

foo();
// ( 약 3초 뒤... )
// 1 2 3
```

- 그렇지 않은 경우 오히려 모든 프로미스의 await를 적용하는 것은 전체 프로그램 실행 속도만 늦춤

```js
async function foo() {
  // 각각의 비동기 처리가 서로 연관되어 있다.
  // 첫 번째 비동기 처리 결과 -> 두 번째 비동기 처리에 필요
  // 두 번째 비동기 처리 결과 -> 세 번째 비동기 처리에 필요
  const a = await new Promise((resolve) => setTimeout(() => resolve(1), 3000));
  const b = await new Promise((resolve) =>
    setTimeout(() => resolve(a + 1), 2000)
  );
  const c = await new Promise((resolve) =>
    setTimeout(() => resolve(b + 1), 1000)
  );

  console.log(a, b, c);
}

foo();
// ( 약 6초 뒤... )
// 1 2 3
```

<br />

## 에러 처리

- async / await 방식의 비동기 처리를 하면 try - catch 문을 사용하여 에러 처리 가능
- 기존 콜백 패턴의 단점은 비동기 처리에서 발생 시, 에러가 호출자 방향으로 전파되는 원리를 이용해 에러를 캐치하지 못하는 단점 존재
  - 콜백 함수를 호출하는 것이 비동기 함수가 이미 콜 스택에서 pop 되어 추적 불가능

```js
try {
  setTimeout(() => {
    throw new Error("Error !");
  }, 1000);
} catch (error) {
  console.error(`캐치한 에러 : ${error}`);
}

// catch 문의 실행결과와 다름 (즉, 에러를 catch 하지 못함)
// Error: Error !
```

- 하지만 async / await 비동기 처리 방식은 항상 실행 결과로 프로미스 반환
  - 즉, 명시적으로 호출할 수 있기 때문에 호출자가 명확

```js
const foo = async () => {
  try {
    const wrongURL = "wrongURL !!!";

    const response = await fetch(wrongURL);
    const data = await response.json();

    return data;
  } catch (error) {
    console.error(`캐치한 에러 : ${error}`);
  }
};

foo();
// 캐치한 에러 : TypeError: Failed to fetch
```

- async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject 하는 프로미스 반환
  - 이 경우 async 함수를 호출하고 Promise.prototype.catch 후속 처리 메서드를 사용해 에러 캐치

```js
const foo = async () => {
  const wrongURL = "wrongURL !!!";

  const response = await fetch(wrongURL);
  const data = await response.json();

  return data;
};

foo()
  .then((res) => console.log(res))
  .catch((err) => console.error(err));
```
