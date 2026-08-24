### 디스트럭처링 할당

디스트럭처링 할당은 구조화된 배열과 같은 이터러블 또는 객체를 비구조화하여 1개 이상의 변수에 개별적으로 할당하는 것을 말함

배열 디스트럭처링 할당을 위해서는 할당 연산자 왼쪽에 값을 할당받을 변수를 선언해야함

```tsx
const arr = [1, 2, 3];

const [one, two, three] = arr;

console.log(one, two, three);  // 1 2 3
```

배열 디스트럭처링 할당의 대상은 이터러블이어야 하며, 할당 기준은 배열의 인덱스임

<br/>

배열 디스트럭처링 할당을 위한 변수에 기본값을 설정할 수 있음

```tsx
const [a, b, c = 3] = [1, 2];
console.log(a, b, c);  // 1 2 3

const [e, f = 10, g = 3] = [1, 2];
console.log(e, f, g)  // 1 2 3
```

이때 기본값보다 할당된 값이 우선시 됨

<br/>

다음과 같이 배열 디스트럭처링 할당을 사용하여 이터러블에서 필요한 요소만 추출 할 수 있음

```tsx
function parseURL(url = '') {
    const parsedURL = url.match(/^(\w+):\/\/([^/]+)\/(.*)$/);
    console.log(parsedURL);

    if (!parsedURL) return {};

    const [, protocol, host, path] = parsedURL;
    return { protocol, host, path };
}

const parsedURL = parseURL('https://developer.mozilla.org/ko/docs/Web/JavaScript');
```

<br/>

객체 디스트럭처링 할당은 객체의 각 프로퍼티를 객체로부터 추출하여 1개 이상의 변수에 할당함

이때 객체 디스트럭처링 할당의 대상은 객체이어야 하며, 할당 기준은 프로퍼티 키임

```tsx
const user = { firstName: 'Ungmo', lastName: 'Lee' };

const { firstName, lastName } = user;

console.log(firstName, lastName);  // Ungmo Lee
```

<br/>

객체 리터럴 형태로 선언한 변수는 `lastName` , `firstName` 으로 프로퍼티 축약 표현을 통해 선언한 것임

```tsx
const user = { firstName: 'Ungmo', lastName: 'Lee' };

// 둘은 동일함
const { firstName, lastName } = user
const { firstName: firstName, lastName: lastName} = user;
```

<br/>

따라서 객체의 프로퍼티 키와 다른 변수 이름으로 프로퍼티 값을 할당받으려면 다음과 같이 변수를 선언함

```tsx
const user = { firstName: 'Ungmo', lastName: 'Lee' };

const { firstName: fn, lastName: ln} = user;

console.log(fn, ln); // Ungmo Lee
```

<br/>

객체 디스트럭처링 할당도 마찬가지로 변수에 기본값을 설정할 수 있음

```tsx
const { firstName = 'Ungmo', lastName } ={ lastName: 'Lee' };
console.log(firstName, lastName)  // Ungmo Lee

const { firstName: fn = 'Ungmo', lastName: ln } = { lastName: 'Lee' };
console.log(fn, ln);  // Ungmo Lee
```

<br/>

프로퍼티 키로 필요한 프로퍼티 값만 추출하여 변수에 할당할 수 도 있어서 지금까지 다룬 내용을 종합하면 다음과 같이 사용할 수 있음

```tsx
const queryResult = {
  data: [
    { id: 1, content: 'HTML', completed: true },
    { id: 2, content: 'CSS', completed: false },
  ],
  isLoading: false,
  isError: false,
};

const { data: todoItems } = queryResult;

console.log(todoItems)
/*
[
  { id: 1, content: 'HTML', completed: true },
  { id: 2, content: 'CSS', completed: false }
]
*/
```

<br/>

배열의 요소가 객체인 경우 배열 디스트럭처링 할당과 객체 디스트럭처링 할당을 혼용할 수 있음

```tsx
const todos = [
  { id: 1, content: 'HTML', completed: true },
  { id: 2, content: 'CSS', completed: false },
  { id: 3, content: 'JS', completed: false },
];

const [, { id }] = todos;
console.log(id);  // 2
```

<br/>

객체 중첩일 경우는 다음과 같이 사용함

```tsx
const user = {
  name: 'Lee',
  address: {
    zipCode: '03068',
    city: 'Seoul'
  }
};

const { address: { city }} = user;
console.log(city);  // Seoul
```

<br/>