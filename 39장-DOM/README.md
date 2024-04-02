## 39장. DOM

- 브라우저 렌더링 엔진은 HTML 문서를 파싱하여 브라우저가 이해할 수 있는 자료구조인 DOM을 생성

<br />

### 📍 DOM

- HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API
- 즉, 프로퍼티와 메서드를 제공하는 트리 자료구조

<br />

### 📍 Node

---

### 📌 HTML 요소와 노드 객체

- HTML 요소 = HTML 문서를 구성하는 개별적 요소

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbAerqp%2FbtrvkGjtEy8%2FNlEaA4Ad3KTdWN2tuPKHXk%2Fimg.png" />

<br />

- 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 반환
- 노드 객체들로 구성된 트리 자료구조를 DOM이라고 하고 DOM 트리라고 부른다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcJ0tB4%2FbtrviHwcZZm%2FtLnZ6FEirUhcf2UTsDv7MK%2Fimg.png" />

<br />

### 📌 노드 객체의 타입

- `문서 노드` : DOM 트리 최상위에 존재하는 루트 노드로서 document 객체를 가르킴
- `window.document`로도 참조할 수 있으며 HTML 문서당 하나인 유일한 객체
- `요소 노드` : `HTML 요소를 가르키는 객체`이며 중첩에 의해 부자 관계를 가지고 문서의 구조를 표현
- `어트리뷰트 노드` : `HTML 요소의 어트리뷰트를 가르키는 객체`로 어트리뷰트가 지정된 요소 노드와 연결
- `텍스트 노드` : HTML 요소의 텍스트를 가르키는 객체. 문서의 정보를 표현. 요소 노드의 자식 노드이면서 leaf 노드

<br />

### 📌 노드 객체의 상속 구조

- `Object -> EventTarget -> Node` 순서가 노드 객체의 상속 구조이며, 노드에서 문서 노드 / 요소 노드 / 어트리뷰트 노드 / 텍스트 노드 로 나뉜다.

ex) input 태그
Object -> EventTarget -> Node -> Element -> HTMLElement -> HTMLInputElement

```
프로토타입 또한 마찬가지로 위 순서대로 구성 / input 요소 노드 객체가 프로토타입 체인의 가장 아래 존재
따라서 프로토타입 체인 안에 있는 모든 프로토타입의 프로퍼티나 메서드를 상속 받아 사용할 수 있는 것
```

- `Object` : 객체
- `EventTarget` : 이벤트를 발생시키는 객체
- `Node` : 트리 자료구조의 노드 객체
- `Element` : 브라우저가 렌더링 할 수 있는 웹 문서의 요소를 표현하는 객체
- `HTMLElement` : 웹 문서의 요소 중 HTML 요소를 표현하는 객체
- `HTMLInputElement` : HTML 요소 중에서 input 요소를 표현하는 객체

<br />

- 각 객체마다 관련 기능을 제공하는 인터페이스가 존재하며 공통된 기능일수록 프로토타입 체인의 상위
- 개별적인 고유 기능일수록 프로토타입 체인의 하위에 프로토타입 체인을 구축하여 노드 객체에 필요한 기능인 프로퍼티 / 메서드를 제공하는 상속 구조를 갖는다.

- DOM = HTML 문서의 계층적 구조와 정보를 표현
- 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공
- DOM API를 통해 HTML의 구조, 내용, 스타일등 동적으로 조작 가능

<br />

### 📍 요소 노드 취득

- HTML의 구조, 내용, 스타일을 `동적으로 조작`하려면 먼저 `요소 노드를 취득`을 해야한다. -> 요소 노드의 취득이 HTML 요소를 조작하는 시작점
- DOM은 요소 노드를 취득할 수 있는 다양한 메서드 제공

<br />

### 📌 id를 이용한 요소 노드 취득

```javascript
const $elem = document.getElementById("id");
```

```javascript
id는 HTML 문서 내에서 '유일한 값'이어야 하지만 중복되는 id값이 있는 경우,
getElementById 메서드는 인수로 전달된 id 값을 갖는 첫 번째 요소 노드만 반환한다.
존재하지 않는 경우 null 반환.
```

- HTML 요소에 id 속성을 부여하면 id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고, 해당 노드 객체가 할당되는 부수효과가 있다.

<br />

```javascript
<div id="foo" />
```

```javascript
// id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당됨.
console.log(foo === document.getElementById("foo")); // true

// 암묵적 전역으로 생성된 전역 프로퍼티는 삭제되지만 전역 변수는 삭제 불가
delete foo;
console.log(foo); // <div id = "foo"></div>
```

```javascript
단 id 값과 동일한 이름의 전역 변수가 이미 선언되어 있으면, 이 전역 변수에 노드 객체가 재할당되지 않는다.
```

<br />

### 📌 태그 이름을 이용한 요소 노드 취득

> Document.prototype / Element.prototype.getElementsByTagName 메서드를 사용
> 차이점 : DOM 전체에서 요소 노드 탐색하여 반환 / 특정 요소 노드의 자손 중에서 요소 노드 탐색하여 반환

- 복수형이므로 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체 반환
- 이 객체는 `유사 배열 객체`이면서 `이터러블`

```javascript
// DOM 전체에서 탐색
const $lisFromDocument = documents.getElementsByTagName("li");

// 특정 요소 노드 자손 중에서 요소 노드 탐색
const $fruits = document.getElementById("fruits");
const $lisFromFruits = $fruits.getElementsByTagName("li");
```

> 인수로 전달된 태그 이름을 갖는 요소가 없는 경우 -> 빈 HTMLCollection 객체 반환

<br />

### 📌 class를 이용한 요소 노드 취득

> Document.prototype / Element.prototype.getElementsByClassName 메서드를 사용

- `getElementsByTagName` 메서드와 마찬가지로 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체 반환
- 만약 인수로 전달된 class 값을 갖는 요소가 존재하지 않는 경우, `빈 HTMLCollection 객체` 반환

<br />

### 📌 CSS 선택자를 이용한 요소 노드 취득

> Document.prototype / Element.prototype.querySelector 메서드

- 태그 이름으로 하는 방식과 비슷함
- 인수로 전달한 CSS 선택자를 만족시키는 요소 노드가 여러 개인 경우, `첫 번째 요소 노드`만 반환
- 인수로 전달된 CSS 선택자를 만족시키는 요소 노드가 존재하지 않는 경우, `null` 반환
- 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우, `DOMException` 에러 발생

> Document.prototype / Element.prototype.querySelectorAll 메서드

- 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 NodeList 객체를 반환하는데 이는 `유사 배열 객체`이면서 `이터러블`
- 인수로 전달된 CSS 선택자를 만족시키는 요소가 존재하지 않는 경우, `빈 NodeList 객체` 반환
- 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우, `DOMException` 에러 발생

<br />

```javascript
CSS 선택자 문법을 사용하는 'querySelector, querySelectorAll' 메서드는 'getElementById, getElementBy' 메서드 보다 다소 느리지만 좀 더 구체적인 조건과 일관된 방식으로 요소를 취득할 수 있다는 장점이 있다.
따라서 id의 경우 getElementById 메서드를 사용하고 그 외의 경우 querySelector, querySelectorAll 메서드 사용을 권장한다.
```

<br />

### 📌 특정 요소 노드를 취득할 수 있는지 확인

> Element.prototype.matches 메서드

```javascript
// apple이 ul id='fruits' 안의 li class='apple'이라고 가정
const $apple = document.querySelector(".apple");

console.log($apple.matches("#fruits > li.apple")); //true
console.log($apple.matches("#fruits > li.banana")); // false
```

<br />

### 📌 HTMLCollection / NodeList

- DOM API가 여러 개의 결과 값을 반환하기 위한 DOM 컬렉션 객체
- 2개 다 유사 배열 객체이면서 이터러블
- 💡 노드 객체의 상태 변화를 실시간으로 반영하여 살아있는 객체이다.
- NodeList의 대부분은 과거의 정적 상태를 유지하는 `none-live 객체`로 동작하지만 경우에 따라 `live 객체`로 동작할 때가 있다.

<br />

### 📌 HTMLCollection

- getElementsByTagName/ByClassName 메서드가 반환하는 HTMLCollection 객체는 노드의 상태 변화를 실시간으로 반영하는 살아있는 DOM 컬렉션 객체

```javascript
const $elem = document.getElementsByClassName("red");

console.log($elems); // HTMLCollection(3) [li.red, li.red, li.red]

// HTMLCollection 객체의 모든 요소의 class 값을 'blue'로 변경한다.
for (let i = 0; i < $elems.length; i++) {
  $elems[i].className = "blue";
}

console.log($elems); // HTMLCollection(1) [li.red]
```

```javascript
모든 요소가 blue 클래스로 변경될 것이라고 예상
하지만 2번째 li 요소는 그렇지 않다.
이유는 첫 번째 반복 시, elems[0]이 blue로 변경되면서 HTMLCollection 객체에서 제외되고 i가 1이 되면서 그 다음 요소인 3번재 요소를 가르킴
```

💡 **해결법**

- for문 역방향
- 객체가 남아있지 않을 때까지 while 사용하여 반복
- HTMLCollection 객체를 사용하지 않고 [...#elems]와 같이 배열로 변환하여 사용

<br />

### 📌 NodeList

- HTMLCollection 객체의 부작용을 해결하기 위해 querySelectorAll 메서드를 사용하여 NodeList 객체를 반환하게 만든다.
- 실시간으로 노드 객체의 상태를 반영하지 않는다.
- NodeList 객체는 NodeList.prototype.forEach 메소드를 상속받아 사용 가능
- 대부분의 객체는 non-live이지만, childeNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 객체와 같이 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작하므로 주의 필요

```javascript
const $fruits = document.getElementById("fruits");

// fruits라는 id를 가진 ul요소 아래의 모든 li요소들은 childNodes 이다.
// 이 노드객체는 live 객체로 동작!
const { childNodes } = $fruits;
```

> 노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 HTMLCollection이나 NodeList 객체를 배열로 변환하여 사용하는 것을 권장

<br />

### 📌 노드 탐색

- `Node.prototype`

  - parentNode
  - previousSibling
  - firstChild
  - childNodes

- Element.prototype
  - previousElementSibling
  - nextElementSibling
  - children

<br />

💡 **공백 텍스트 노드**

- html 요소 사이의 스페이스 / 탭 / 줄바꿈 등의 공백 문자는 텍스트 노드를 생성한다. = `공백 텍스트 노드`

<br />

💡 **자식 노드 탐색**

- `Node.prototype` -> childeNodes, firstChild, lastChilde가 있으면 이 프로퍼티가 반환하는 NodeList에는 요소노드뿐 아니라 텍스트 노드도 포함 가능성 있음
- `Element.prototype` -> children, firstElementChild, lastElementChild 프로퍼티가 반환한 HTMLCollection에는 텍스트 노드 포함하지 않음

💡 **자식 노드 존재확인**

- Node.prototype.hasChildeNodes 메서드 이용
- 존재하면 true / 아니라면 false 반환
- 자식 노드 중에 텍스트 노드가 아닌 노드가 있는지 확인하기 위해
  `children.length / Element 인터페이스`의 childElementCount 프로퍼티 사용

<br />

💡 **요소 노드의 텍스트 노드 탐색**

- 요소 노드의 텍스트 노드는 firstChilde 프로퍼티로 접근 가능 (첫번째 자식 노드 반환)
- firstChilde 프로퍼티가 반환한 노드 = 텍스트 노드 or 요소 노드

<br />

💡 **부모 노드 탐색**

- Node.prototype.parentNode 프로퍼티 사용
- 텍스트 노드는 DOM 트리의 최종단 노드인 `리프 노드`이므로 부모 노드가 텍스트 노드일 경우 ❌

<br />

💡 **형제 노드 탐색**

- 어트리뷰트 노드의 경우, 요소 노드와 연결되어 있지만 부모 노드가 같지 않기 때문에 반환 X
- Only 텍스트 노드 / 요소 노드 반환

```
Node.prototype -> previousSibling, nextSibling (요소 / 텍스트 노드)

Element.prototype -> previousElementSibling, nextElementSibling (요소 노드만)
```

<br />

### 📌 노드 정보 취득

- `Node.prototype.nodeType`
- 노드 객체의 종류 / 노드 타입을 나타내는 상수 반환

  - Node.ELEMENT_NODE: 요소 노드 타입을 나타내는 상수 1 반환
  - Node.TEXT_NODE: 텍스트 노드 타입을 나타내는 상수 3 반환
  - Node.DOCUMENT_NODE: 문서 노드 타입을 나타내는 상수 9 반환

- `Node.prototype.nodename`
- 노드의 이름을 문자열로 반환
  - 요소 노드: `대문자 문자열 태그 이름` 반환
  - 텍스트 노드: 문자열 `#text` 반환
  - 문서 노드: 문자열 `#document` 반환

<br />

### 📌 요소 노드의 텍스트 조작

💡 nodeValue

- Node.prototype.nodeValue 프로퍼티는 getter / setter 모두 존재하는 접근자 프로퍼티
- 참조 할당 가능

- nodeValue는 노드 객체의 값을 반환하는데 이 값이란 텍스트 노드의 텍스트 의미
- 문서 노드 / 요소노드의 nodeValue로 참조시, null 반환

- 요소 노드의 텍스트를 변경하기 위해 다음 순서를 따름
  1. 텍스트를 변경할 요소 노드 취득 후, firstChild 프로퍼티를 사용하여 탐색
  2. 탐색할 텍스트 노드의 nodeValue 프로퍼티를 사용하여 텍스트 노드의 값 사용

<br />

💡 textContent

- Node.prototype.textContent 프로퍼티는 getter / setter 모두 존재하는 접근자 프로퍼티
- 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경
- 요소 노드의 콘텐츠 영역내의 텍스트를 모두 반환 -> HTML 마크업 무시
- 어떤 요소 안에 자깃 요소 노드가 많다 -> textContent 사용이 더 좋다.
- 텍스트만 존재한다면 firstChild.nodeValue와 textContent 프로퍼티는 같은 결과를 반환한다.

- textContent를 사용하여 문자열 할당시, 모든 자식 노드가 제거 되고 HTML 마크업이 포함되어 있더라도 문자열 그대로 인식하기 때문에 마크업 피싱 ❌
- textContent와 비슷한 프로퍼티인 innerText가 존재하는데 다음과 같은 이유로 사용하지 않는 것이 좋다.
  1. CSS 순종적이어서 CSS에 의해 비표시로 자정된 요소 노드의 텍스트 반환 ❌
  2. CSS를 고려해야되서 textContent 프로퍼티 보다 느림

<br />

### 📌 DOM 조작

- DOM 조작은 리플로우 / 리페인트를 발생하는 원인이 되므로 성능 최적화를 위해 주의해서 사용

<br />

💡 **innerHTML**

- `Element.prototype.innerHTML` 프로퍼티
- innerHTML로 참조시, 모든 HTML 마크업을 `문자열`로 반환하여 참조
- innerHTML로 문자열을 할당하면 할당한 문자열을 포함되어 있는 `HTML 마크업`이 파싱되어 요소 노드의 자식 노드로 DOM 반영
- 이 때 사용자로부터 입력받은 데이터를 그대로 innerHTML 프로퍼티에 할당하는 것은 크로스 사이트 스크립팅 공격(XXS)에 취약하므로 위험
- 마크업 내에 자바스크립트 악성 코드가 포함되어 있다면 파싱 과정에서 그대로 실행될 가능성이 있기 때문이다.

- HTML5는 innerHTML 프로퍼티에 삽입된 script 자바스크립트 코드를 실행 ❌, HTML5를 지원하는 브라우저에서 동작 ❌

<br />

### 📌 노드 생성과 추가

- `Document.prototype.createElement` 프로퍼티
- 요소 노드를 생성하여 반환
- 인수에는 문자열로 전달

```javascript
// 1. 요소 노드 생성
const $li = document.createElement("li");

// 2. 텍스트 노드 생성
const textNode = document.createTextNode("Banana");

// 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
$li.appendChild(textNode);
```

<br />

### 📌 노드 삽입

- `Node.prototype.appendChild` 프로퍼티
- 인수로 전달받은 노드를 자신을 호출하는 노드의 마지막 자식 노드로 DOM에 추가
- 이 때 노드를 추가할 위치를 지정할 수 없고 언제나 마지막 자신 노드로 추가

- `Node.prototype.insertBefore(newNode, childeNode)` 프로퍼티
- 첫번째 인수로 전달받은 노드 -> 두번째 인수로 전달받은 노드 앞에 삽입

<br />

### 📌 노드 이동

- DOM에 이미 존재하는 노드를 appendChild / insertBefore 메서드를 이용하면 DOM에 다시 추가하여 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가한다. (= 즉, 노드 이동)

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
      <li>Orange</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");

    // 이미 존재하는 요소 노드를 취득
    const [$apple, $banana] = $fruits.children;

    // 이미 존재하는 $apple 요소 노드를 #fruits 요소 노드의 마지막 노드로 이동
    $fruits.appendChild($apple); // Banana - Orange - Apple

    // 이미 존재하는 $banana 요소 노드를 #fruits 요소의 마지막 자식 노드 앞으로 이동
    $fruits.insertBefore($banana, $fruits.lastElementChild);
    // Orange - Banana - Apple
  </script>
</html>
```

<br />

### 📌 노드 복사

- Node.prototype.cloneNode([deep: true | false]) 프로퍼티
- default = false
- 노드의 사본을 생성하여 반환
- true : 깊은 복사, 모든 자식 노드가 포함된 노드의 사본
- false : 얕은 복사, 노드 자신만의 사본 -> 자식이 없으니 텍스트 노드도 없음

<br />

### 📌 노드 교체

- Node.prototype.replaceChild(newChild, oldChild) 프로퍼티

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li>원본</li>
    </ul>
    <script>
      const table = document.querySelector("ul");
      const newNode = document.createElement("li");
      newNode.textContent = "교체";
      table.replaceChild(newNode, table.firstElementChild);
    </script>
  </body>
</html>
```

<br />

### 📌 노드 삭제

- Node.prototype.removeChild(child) 프로퍼티

---

### 📍 어트리뷰트

### 📌 어트리뷰트 노드 / attributes 프로퍼티

`<input id="user" type="text" value="eee">`

- HTML 요소의 시작 태그에 어트리뷰트 이름 = `어트리뷰트 값` 형식으로 정의
- HTML 문서가 파싱될 때, 각각의 어트리뷰트는 각각의 어트리뷰트 노드로 변환
- HTML 요소의 어트리뷰트가 3개이면 어트리뷰트 노드도 3개

- 모든 어트리뷰트 노드의 참조는 NamedNodeMap 객체에 담겨 요소 노드의 attributes 프로퍼티에 저장
- attributes 프로퍼티는 getter만 있는 접근자 프로퍼티
- 값에 접근 하려면 `요소.attributes.어트리뷰트명.value`로 접근

<br />

### 📌 HTML 어트리뷰트 조작

- attributes 프로퍼티를 통하지 않고 요소 노드에서 메서드로 바로 HTML 어트리뷰터 값 접근 가능
- Element.prototype.getAttribute(attributeName)
- Element.prototype.setAttribute(attributeName,attributeValue)
- Element.prototype.hasAttribute(attributeName)
- Element.prototype.removeAttribute(attributeName)

<br />

### 📌 HTML 어트리뷰트 vs DOM 프로퍼티

- 요소 노드 객체에는 HTML 어트리뷰트에 대응하는 프로퍼티 존재
- 이러한 DOM 프로퍼티들은 HTML 어트리뷰트 값을 초기값으로 가짐
- DOM 프로퍼티는 setter / getter 둘다 있는 접근자 프로퍼티

```
HTML 어트리뷰트 : HTML 요소의 초기 상태를 지정하고 이는 변하지 않음
DOM 프로퍼티 : 요소 노드의 최신 상태를 관리
```

- 요소 노드의 초기값과 최신값 모두 관리 해주어야 한다.
- 초기값을 알아야 새로고침을 했을 때 OK, 사용자 입력에 의한 최신값은 당연히 알아야함

- 단, 모든 DOM 프로퍼티가 사용자 입력에 의해 변경된 최신 상태를 관리하지 않음
  `<input type='text' id='name' value='Aj' />`

- id 프로퍼티는 사용자 입력과 아무관계 없다.
- id 어트리뷰트 / id 프로퍼티는 항상 `동일한 값 유지`
- 하나만 바뀌면 나머지도 바뀜

- 사용자 입력에 의한 상태변화와 관계있는 DOM 프로퍼티만 최신 상태 값 관리
- 그 외의 사용자 입력과 관계 없는 어트리뷰트와 DOM 프로퍼티는 항상 동일한 값 연동

<br />

### 📍 HTML 어트리뷰트와 DOM 프로퍼티의 대응 관계

- 대부분 1:1 대응이지만 아닌 애들도 있음

  - id 어트리뷰트: id 프로퍼티와 대응
  - class 어트리뷰트: className, classList 프로퍼티와 대응
  - for 어트리뷰트: htmlFor 프로퍼티와 대응
  - 어트리뷰트에 대응하는 어트리뷰트는 카멜케이스 (htmlFor)

<br />

### 📍 DOM 프로퍼티 타입

- getAttribute 메서드로 취득한 어트리뷰트의 값 = `항상 문자열`
- `DOM 프로퍼티`로 취득한 최신 값 = 문자열이 아닐 수 있다.

<br />

### 📌 data 어트리뷰트 / dataset 프로퍼티

- 사용자 정의 어트리뷰트
- data - 사용자정의 어트리뷰트 이름 = '값' 형식으로 선언
- HTMLElement.dataset 프로퍼티는 모든 data 어트리뷰트들 담은 DOMStringMap 반환
- DOMStringMap 객체는 사용자정의 어트리뷰트 이름을 카멜케이스로 변환한 프로퍼티를 갖는다.

<br />

### 📍 스타일

---

### 📌 인라인 스타일 조작

- HTMLElement.prototype.style 프로퍼티로 조작가능
- getter / setter 둘다 있는 접근자 프로퍼티
- 요소 노드의 인라인 스타일 취득
- style 프로퍼티 참조시, CSSStyleDeclaration 객체 반환
- CSSStyleDeclaration 객체의 프로퍼티 = 카멜케이스 / CSS = 케밥 케이스
- 프로퍼티에 값 할당시, 인라인 스타일 추가됨
- 단위 지정이 필요한 CSS 프로퍼티에 단위 없으면 적용 X

```html
<!DOCTYPE html>
<html>
  <body>
    <div style="width: 100px; height : 100px; background-color : red;"></div>
    <script>
      const $div = document.querySelector("div");
      console.log($div.style); // CSSStyleDeclaration 객체
      $div.style.backgroundColor = "blue"; // 파란색으로 변환
      $div.style["background-color"] = "green"; // css 표기법으로 적용하고 싶을 때
    </script>
  </body>
</html>
```

<br />

### 📌 클래스 조작

- 요소의 class 어트리뷰트를 조작하여 다른 스타일 적용
- class 어트리뷰트에 대응하는 DOM 프로퍼티인 className, classList를 통해 적용

<br />

### 📌 className

- getter / setter
- 참조시, class 어트리뷰트의 값을 `문자열`로 반환
- class가 여러개면 공백으로 구분된 문자열이 반환되어 다루기 불편

<br />

### 📌 classList

- Element.prototype.classList
- class 어트리뷰트의 정보를 DOMTokenList 객체에 담아 반환
- DOMTokenList 객체는 여러가지 메서드 제공

<br />

### 📌 DOMTokenList 객체의 메서드

- add(...className): 인수로 전달한 문자열을 class 어트리뷰트에 추가
- remove(...className): 삭제, 인수로 전달한 값이 없어도 에러발생 ❌
- item(index): index에 해당하는 클래스 반환
- contains(className): true / false 반환
- replace(oldClassName, newClassName)
- toggle(className, [조건식]): 있으면 삭제, 없으면 추가
  - 조건식은 선택사항, true면 강제 추가, false면 강제 삭제

<br />

### 📌 요소에 적용되어 있는 CSS 스타일 참조

- style 프로퍼티는 인라인 스타일만 반환
- 클래스나 상속을 통해 적용된 스타일을 알 수 없음
- 요소에 적용된 모든 스타일을 참조해야 하면 getComputedStyle 메서드
