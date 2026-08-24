### Set

Set 객체는 중복되지 않는 유일한 값들의 집합으로 배열과 유사하지만 차이가 존재함

- 동일한 값을 중복하여 포함할 수 없음
- 요소 순서에 의미가 없음
- 인덱스로 요소에 접근할 수 없음

<br/>

`Set` 객체는 `Set` 생성자 함수로 이터러블을 인수로 전달받아 `Set` 객체를 생성하며 인수를 전달하지 않으면 빈 `Set` 객체가 생성됨

```tsx
const set = new Set();
console.log(set);  // Set(0) {}

const set1 = new Set([1, 2, 3, 3]);
console.log(set1);  // Set(3) { 1, 2, 3 }

const set2 = new Set('hello');
console.log(set2);  // Set(4) { 'h', 'e', 'l', 'o' }
```

이때 이터러블의 중복된 값은 `Set` 객체에 요소로 저장되지 않음

<br/>

이러한 특성을 활용하여 배열에서 중복된 요소를 제거할 수 있음

```tsx
const uniq = array => [...new Set(array)];
console.log(uniq[2, 1, 2, 3, 4, 3, 4]));  // [2, 1, 3, 4]
```

<br/>

`Set` 객체는 요소를 추가하거나 삭제하고, 특정 요소의 존재 여부를 확인하기 위한 메서드를 제공함

`add` 메서드는 `Set` 객체에 새로운 요소를 추가하며, 이미 존재하는 값을 추가하더라도 중복되어 저장되지 않음

```tsx
const set = new Set([1, 2]);

set.add(3);
set.add(3);

console.log(set);  // Set(3) { 1, 2, 3 }
```

<br/>

`delete` 메서드는 `Set` 객체에서 특정 요소를 삭제하며, 삭제에 성공하면 `true` , 존재하지 않는 요소를 삭제하려 하면 `false` 를 반환함

```tsx
console.log(set.delete(1));  // true

console.log(set.delete(3));  // false

console.log(set);  // Set(1) { 3 }
```

<br/>

`has` 메서드는 `Set` 객체에 특정 요소가 존재하는지 확인하며, 존재하면 `true` , 존재하지 않으면 `false` 를 반환함

```tsx
console.log(set.has(3));  // true

console.log(set.has(1));  // false
```

<br/>

`clear` 메서드는 `Set` 객체의 모든 요소를 제거함

```tsx
set.clear();

console.log(set);  // Set(0) {}
```

<br/>
<br/>

### Map

`Map` 객체는 키와 값의 쌍으로 이루어진 컬렉션으로 객체와 유사하지만 다음과 같은 차이가 있음

- 객체를 포함한 모든 값을 키로 사용할 수 있음
- 이터러블임

<br/>

`Map` 객체는 `Map` 생성자 함수로 이터러블을 인수로 전달받아 `Map` 객체를 생성하며 인수를 전달하지 않으면 빈 `Map` 객체가 생성됨

이때 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야함

```tsx
const map1 = new Map([['key1', 'value'], ['key2', 'value']]);
console.log(map1);  // Map(2) { 'key1' => 'value', 'key2' => 'value' }
```

<br/>

객체와 달리 `Map` 은 키로 문자열뿐만 아니라 객체, 배열, 함수 등 모든 값을 사용할 수 있음

```tsx
const user = { name: 'Kim' };

const map = new Map();

map.set(user, 'Fronted Developer');

console.log(map);  // Map(1) { { name: 'Kim' } => 'Fronted Developer' }
```

<br/>

`set` 메서드는 새로운 키와 값을 추가하며, 이미 존재하는 키에 값을 추가하면 기존 값을 새로운 값으로 변경함

```tsx
const map = new Map();

map.set('name', 'Kim');
map.set('age', 20);

map.set('age', 25);

console.log(map); // Map(2) { 'name' => 'Kim', 'age' => 25 }
```

<br/>

`get` 메서드는 특정 키에 연결된 값을 반환하고 존재하지 않는 키를 전달하면 `undefined` 를 반환함

```tsx
console.log(map.get('name')); // Kim
console.log(map.get('job'));  // undefined
```

<br/>

`has` 메서드는 특정 키가 존재하는지 확인하며, 존재하면 `true` , 존재하지 않으면 `false` 를 반환함

```tsx
console.log(map.has('name'));  // true
console.log(map.has('job'));   // false
```

<br/>

`delete` 메서드는 특정 키와 연결된 요소를 삭제하며, 삭제에 성공하면 `true` , 존재하지 않는 키를 전달하면 `false` 를 반환함

```tsx
map.delete('age');

console.log(map.has('age'));  // false
```

<br/>

`clear` 메서드는 `Map` 객체의 모든 요소를 제거함

```tsx
map.clear();

console.log(map);  // Map(0) {}
```

<br/>