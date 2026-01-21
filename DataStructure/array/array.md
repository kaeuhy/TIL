## 배열

배열은 컴퓨터에서 리스트를 저장하는 데이터 타입 중 하나

연속된 메모리 공간에 순차적으로 데이터가 저장, 다음과 같은 특징을 가짐

</br>

> 특징
>
- 배열은 같은 타입의 데이터를 여러개 나열한 선형 자료구조
- 대부분에 프로그램 언어에서 동일 타입의 데이터를 저장
- 배열은 생성시 크기를 정하면 해당 크기로 고정
- 배열을 구성하는 각각의 값을 요소(element)라고 하며, 배열에서의 위치를 가리키는 숫자는 인덱스(index)
- 반복문과 결합하여 많은 데이터를 효율적으로 처리

</br>
</br>

## 배열 선언 - TypeScript

> 기본적인 방법 - 가장 많이 사용
>

```tsx
const numbers: number[] = [1, 2, 3];
const student: string[] = ['준택', '우민', '은현'];
```

</br>


> Array<타입> 제네릭 사용 방법
>

```tsx
const numbers: Array<number> = [1, 2, 3];
const student: Array<string> = ['준택', '우민', '은현'];
```

</br>

> 배열에 들어가는 요소들의 타입이 다른 경우
>

```tsx
const multiArr: (number | string)[] = [1, "hello"];
```

</br>

> 다차원 배열
>

```tsx
let doubleArr: number[][] = [
    [1, 2, 3],
    [4, 5],
];
```

</br>
</br>

## 배열 내 연산

배열 내 연산은 크게 접근(read), 검색(search), 추가(insert), 삭제(delete)로 나뉨

</br>

> TypeScript 기준 배열의 시간복잡도
>

| Operation | 평균 시간복잡도 |
| --- | --- |
| read | O(1) |
| insert | O(n) |
| delete | O(n) |
| search | O(n) |


</br>
</br>

## 시간 복잡도

어떤 코드(연산) 동작시, 입력값의 크기(n)에 따라 얼마나 시간이 걸리는지를 수학적으로 나타낸 것

이때, O는 빅오 표기법을 사용

</br>

**배열의 시간복잡도란?**

배열에서 어떤 작업 수행시, 얼마나 빠르게 끝나는가를 알려주는 지표

> 예시
>

```tsx
const arr = [10, 20, 30];
```

- arr[2]: 바로 해당 인덱스인 30값을 가르키기에 시간복잡도가 빠르다라고 할 수 있음
  → O(1) - 길이가 커도 시간은 동일
- 값 30찾기: 값 30이 들어가있는 인덱스를 알 수가 없어 처음부터 끝까지 순차적으로 비교
  → O(n) - 시간 복잡도가 큼
</aside>

</br>

> 접근 - read
>

```tsx
const student: string[] = ['준택', '우민', '은현'];
console.log(student[2]); // '은현'
```

</br>

> 검색 - search
>

```tsx
const student: string[] = ['준택', '우민', '은현'];
const target = '준택';

const found = arr.find(item => item === target);
```

</br>

> 추가/삭제 - insert/delete
>

```tsx
let student: string[] = ['준택', '우민', '은현'];

// 추가
student.unshift('재영'); // ['재영', '준택', '우민', '은현']
student.splice(2, 0, '지수'); // ['재영', '준택', '지수', '우민', '은현']
student.push('예은'); // ['재영', '준택', '지수', '우민', '은현', '예은']

// 삭제
student.shift(); // ['준택', '지수', '우민', '은현', '예은']
student.splice(2, 1); // ['준택', '지수', '은현', '예은']
student.pop(); // ['준택', '지수', '은현']
```

</br>
</br>

## 배열 메서드

배열(객체)의 동작(기능)을 정의한 함수

</br>

> **`unshift/splice/push`**
>

순서대로 맨 앞, 중간, 뒤 추가

```tsx
student.unshift('재영'); // ['재영', '준택', '우민', '은현']

student.push('재영'); // ['준택', '우민', '은현', '재영']

student.splice(1, 0, '재영'); // ['준택', '재영', '우민', '은현']
student.splice(1, 1, '재영'); // ['준택', '재영', '은현']
```

- splice 진행시 두 번째 인자의 의미
    - 0 → 삭제 없이 추가
    - 1 → 기존 요소 1개 삭제 후 추가

</br>

> **`shift/splice/pop`**
>

순서대로 맨 앞, 중간, 뒤 삭제

```tsx
student.shift(); // ['우민', '은현']

student.pop(); // ['준택', '우민']

student.splice(1, 1); // ['준택', '은현']
```

</br>

> **`map/filter/forEach`**
>

배열을 순회하면서 변형(map)하거나, 조건 필터링(filter)하거나, 단순 반복(forEach) 할 때 사용

```tsx
let nums: number[] = [1, 2, 3, 4, 5];

let doubled = nums.map(n => n * 2); 
// 각 요소를 2배로 바꿈 → [2, 4, 6, 8, 10]

let evens = nums.filter(n => n % 2 === 0); 
// 짝수만 남김 → [2, 4]

nums.forEach(n => console.log(n)); 
// 하나씩 출력 → 콘솔: 1 2 3 4 5
```

</br>