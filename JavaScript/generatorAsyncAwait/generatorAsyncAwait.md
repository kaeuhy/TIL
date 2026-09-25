### 제너레이터란?

제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수임

제너레이터와 일반 함수의 차이는 다음과 같음

- 함수 호출자에게 함수 실행의 제어권 양도 가능
- 함수 호출자와 함수의 상태를 주고 받을 수 있음
- 제너레이터 함수 호출시 제너레이터 객체를 반환함

<br/>
<br/>

### 제너레이터 함수의 정의

제너레이터 함수는 `function*` 키워드로 선언하고 하나 이상의 `yield` 표현식을 포함함

```tsx
// 제너레이터 함수 선언문
function* getDecFunc() {
  yield 1;
};

// 제너레이터 함수 표현식
const getExpFunc = function* () {
  yield 1;
};

// 제너레이터 메서드
const obj = {
  * genObjMethod() {
    yield 1;
  }
};

// 제너레이터 클래스 메서드
class MyClass {
  * genClsMethod() {
    yield 1;
  }
}
```

애스터리스트(`*`)의 위치는 `function` 키워드와 함수 이름 사이라면 어디든지 상관없음

단, 제너레이터 함수는 화살표 함수로 정의할 수 없고 `new` 연산자와 함께 생성자 함수로 호출할 수 없음

<br/>
<br/>

### 제너레이터 객체

제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환함

제너레이터 객체는 이터러블이면서 동시에 이터레이터임

```tsx
function* genFunc(){
  yield 1;
  yield 2;
  yield 3;
}

const generator = genFunc();

console.log(Symbol.iterator in generator);  // ture
console.log('next' in generator);           // ture
```

제너레이터 객체는 이터레이터에는 없는 `return` , `throw`  메서드도 가짐

<br/>

`next` 메서드 호출시 제너레이터 함수의 `yield` 표현식까지 코드 블록을 실행하고 `yield` 된 값을 `value` 프로퍼티 값으로, `false` 를 `done` 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환함

`return` 메서드 호출시에는 인수로 전달받은 값을 `value` 프로퍼티 값으로, `true` 를 `done` 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환함

```tsx
function* genFunc(){
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.error(e);
  }
}

const generator = genFunc();

console.log(generator.next());                 // { value: 1, done: false }
console.log(generator.return('End!'));    // { value: "End!", done: true }
```

<br/>

`throw` 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 `undefined` 를 `value` 프로퍼티 값으로, `true` 를 `done` 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환함

```tsx
function* genFunc(){
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.error(e);
  }
}

const generator = genFunc();

console.log(generator.next());              // { value: 1, done: false }
console.log(generator.throw('Error!'));     // { value: undefined, done: true }
```

<br/>
<br/>

### 제너레이터의 일시 중지와 재개

제너레이터는 `yield` 키워드와 `next` 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수 있음

제너레이터 객체의 `next` 메서드를 호출하면 제너레이터 함수의 코드 블록을 실행함

→ 계속 짚고넘어가자면 제너레이터 함수를 호출하면 코드 블록 실행이 아니라 제너레이터 객체를 반환

이때 코드 블록을 실행하는 것이 아니라 `yield` 표현식까지만 실행함

<br/>

`yield` 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 `yield` 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환함

```tsx
function* genFunc(){
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.error(e);
  }
}

const generator = genFunc();

console.log(generator.next());  // { value: 1, done: false }

console.log(generator.next());  // { value: 2, done: false }

console.log(generator.next());  // { value: 3, done: false }

console.log(generator.next());  // { value: undefined, done: true }
```

제너레이터 객체의 `next` 메서드에 전달한 인수는 제너레이터 함수의 `yield` 표현식을 할당받는 변수에 할당됨

단, `const x = yield 1;` 처럼 변수에 할당되지 않음

<br/>
<br/>

### async/await

제너레이터 함수는 `next` 메서드와 `yield` 표현식을 통해 함수 호출자와 함수의 상태를 주고받아 프로미스를 사용한 비동기 처리를 동기 처리처럼 구현할 수 있음

→ 하지만 코드가 어렵고 가독성이 나쁨

이를 해결하고자 비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있는 `async` / `await` 이 도입됨

<br/>

`async` / `await` 은 프로미스를 기반으로 동작하고 `Promise` 의 `then` / `catch` / `finally` 후속 처리 메서드에 콜백 함수를 전달해서 비동기 처리 결과를 후속 처리할 필요없음

```tsx
const fetch = require('node-fetch');

async function fetchTodo() {
  const url = 'https://jsonplaceholder.typicode.com/todos/1';
  
  const response = await fetch(url);
  const todo = await response.json();
  console.log(todo);
}

fetchTodo();
```

<br/>

`await` 키워드는 반드시 `async` 함수 내부에서 사옹해야함

`async` 함수는 `async` 키워드를 사용해 정의하며 언제나 `Promise` 를 반환함

→ 명시적으로 `Promise` 를 반환하지 않더라도 `async` 함수는 암묵적으로 반환값을 `resolve` 하는 `Promise` 를 반환함

```tsx
async function foo(n) { return n }
foo(1).then(v => console.log(v));  // 1

const bar = async function (n) { return n }
bar(2).then(v => console.log(v));  // 2

const baz = async n => n;
baz(3).then(v => console.log(v));  // 3

const obj = {
  async foo(n) { return n }
};
obj.foo(4).then(v => console.log(v));  // 4

class MyClass {
  async bar(n) { return n }
}

const myClass = new MyClass();
myClass.bar(5).then(v => console.log(v));  // 5
```

<br/>

단 인스턴스를 반환해야하는 `constructor` 메서드에서는 `async` 메서드가 될 수 없음

→ `async` 함수는 항상 `Promise` 를 반환해야하므로

```tsx
class MyClass {
	async constructor() {}
	// SyntaxError: Class constructor may not be an async method
}

const myClass = new Myclass();
```

<br/>

`awati` 키워드는 `Promise` 가 `settled` 상태가 될 때까지 대기하다가 `settled` 상태가 되면 `Promise` 가 `resolve` 한 처리 결과를 반환함

```tsx
const fetch = require('node-fetch');

const getGithubUserName = async (id) => {
  const res = await fetch(`https://api.github.com/users/${id}`);
  const { name } = await res.json();
  console.log(name);
};

getGithubUserName('hyeon');
```

<br/>