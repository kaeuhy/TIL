### 스프레드 문법

스프레드 문법은 전개 문법인 `…` 으로 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만듦

스프레드 문법을 사용할 수 있는 대상은 이터러블에 한정됨

```tsx
console.log(...[1, 2, 3]);  // 1 2 3

console.log(...'Hello');  // H e l l o

// TypeError: Spread syntax requires ...iterable[Symbol.iterator] to be a function
console.log(...{ a: 1, b: 2});
```

스프레드 문법의 결과는 값이 아니라 개별적인 값들의 목록임

그렇기에 스프레드 문법의 결과는 변수에 할당할 수 없음

<br/>

스프레드 문법의 결과물은 다음과 같이 쉼표로 구분한 값의 목록을 사용하는 문맥에서만 사용할 수 있음

- **함수 호출문의 인수 목록**
- **배열 리터럴의 요소 목록**
- **객체리터럴의 프로퍼티 목록**

<br/>

함수 호출문의 인수 목록에서 사용하는 예시는 다음과 같음

```tsx
const arr = [1, 2, 3];

const max = Math.mac(...arr);  // -> 3
```

<br/>

Rest 파라미터와 형태가 동일하여 혼동할 수 있지만 서로 반대의 개념임

Rest 파라미터는 다음과 같이 함수애 전달된 인수들의 목록을 배열로 전달받기 위해 매개변수 이름 앞에 `…` 을 붙임

```tsx
function foo(...rest) {
	console.log(rest);  // [ 1, 2, 3 ]
}

foo(...[1, 2, 3]);
```

<br/>

배열 리터럴 내부에서 사용하는 경우는 기존의 방식보다 더욱 간결하고 가독성 좋게 표현할 수 있음

```tsx
// 기존 concat 사용 방식
let arr = [1, 2].concat([3, 4]);
console.log(arr)  // [ 1, 2, 3, 4 ]

// 스프레드 문법 사용 방식
const arr = [...[1, 2], ...[3, 4]];
console.log(arr)  // [ 1, 2, 3, 4 ]
```

<br/>

기존에는 어떤 배열의 중간에 다른 배열의 요소들을 추가하거나 제거하려면  `splice` 메서드를 사용했음

하지만 `splice` 메서드는 배열 자체가 추가됨

```tsx
let arr1 = [1, 4];
let arr2 = [2, 3];

arr1.splice(1, 0, arr2);

console.log(arr1);  // [ 1, [ 2, 3 ], 4 ]
```

<br/>

스프레드 문법을 사용하면 더욱 간결하고 가독성 좋게 표현할 수 있음

```tsx
const arr1 = [1, 4];
const arr2 = [2, 3];

arr1.splice(1, 0, ...arr2);
console.log(arr1);  // [1, 2, 3, 4]
```

<br/>

또, 스프레드 문법을 사용하여 객체 리터럴 내부를 쉽게 바꿀 수 있음

```tsx
const merged = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3 } };
console.log(merged)  // { x: 1, y: 10, z: 3 }

const changed = { ...{ x: 1, y: 2 }, y: 100 };
console.log(changed)  // { x: 1, y: 100 }

const added = { ...{ x: 1, y: 2 }, z: 0 };
console.log(added)  // { x: 1, y: 2, z: 0 }
```

객체 병합시에 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 가짐

<br/>