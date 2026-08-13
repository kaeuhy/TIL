### Number

`Number` 는 주로 문자열을 숫자로 변환하거나 숫자의 상태를 검사하고 표시 형식을 정할 때 사용함

```tsx
Number('100');        // 100
Number('10.5');       // 10.5
Number('hello');      // NaN

Number.isNaN(NaN);    // true
Number.isFinite(10);  // true
Number.isInteger(10); // true
```

<br/>

사용자 입력값은 대부분 문자열로 들어오기 때문에 숫자로 변환하는 경우가 많음

```tsx
const value = '100';

const price = Number(value);

console.log(price);  // 100
```

<br/>

문자열에서 정수나 실수 부분만 추출해야 한다면 `parseInt` , `parseFloat` 를 사용함

```tsx
Number.parseInt('100px');      // 100
Number.parseFloat('10.5px');   // 10.5
```

<br/>

숫자를 화면에 표시할 때 특정 소수점 자릿수로 맞추고 싶다면 `toFixed` 를 사용함

```tsx
const price = 1234.5678;

console.log(price.toFixed(2)); // "1234.57"
```

이때, 반환값은 숫자가 아니라 문자열임

<br/>
<br/>

### Math

`Math` 는 숫자 계산에 필요한 메서드를 사용할 수 있게 해주는 객체로 반올림, 올림, 내림, 최댓값·최솟값, 랜덤 값 생성 등에 많이 사용함

특히 랜덤 값을 생성할 때 `Math.random()` 을 많이 사용함

```tsx
const result = Math.random();

console.log(result);  // 0.4574354224623213 -> 0과 1사이 무작위한 값
```

<br/>
<br/>

### Date

`Date` 는 현재 시간 확인, 날짜 비교, 서버에서 받은 날짜 처리, 날짜 문자열 변환 등에 많이 사용함

현재 날짜와 시간을 가져올 때는 `new Date()` 를 사용함

```tsx
const now = new Date();

console.log(now);  // 2026-08-13T05:38:43.149Z
```

<br/>

날짜의 각 값을 가져올 때는 `get` 계열 메서드를 사용함

```tsx
const date = new Date();

console.log(date.getFullYear());  // 2026
console.log(date.getMonth());     // 7
console.log(date.getDate());      // 13
console.log(date.getDay());       // 4
```

<br/>
<br/>

### RegExp

`RegExp` 는 문자열이 특정 패턴을 만족하는지 검사하거나 특정 패턴을 찾을 때 사용함

보통 이메일, 전화번호, 비밀번호, 사용자 입력값 검증 등에 사용됨

```tsx
const tel = '010-1234-567팔';

const regExp = /^\d{3}-\d{4}-\d{4}$/;

console.log(regExp.test(tel));  // false
```

`test()` 를 통해서 문자열이 해당 패턴과 일치하는지 검사함

<br/>

```tsx
const value = 'abc123!';

console.log(/[a-zA-Z]/.test(value));        // true
console.log(/[0-9]/.test(value));           // true
console.log(/[^a-zA-Z0-9\s]/.test(value));  // true
```

- `[a-zA-Z]`
    - 영문자가 하나라도 있는지 검사
- `[0-9]`
    - 숫자가 하나라도 있는지 검사
- `[^a-zA-Z0-9\s]`
    - 영문자, 숫자, 공백이 아닌 문자가 하나라도 있는지 검사
    - `!` , `@` , `#` 같은 특수문자

<br/>
<br/>

### String

`String` 은 문자열 검사, 추출, 치환, 분리, 공백 제거에 주로 사용함

문자열에 특정 값이 포함되어 있는지 확인할 때 `includes()` 를 사용함

```tsx
const text = 'JavaScript';

text.includes('Java');   // true
text.includes('React');  // false
```

<br/>

검색창이나 필터링 기능에서 자주 사용하는 형태임

```tsx
const keyword = 'react';
const title = 'React 개발 공부';

title.toLowerCase().includes(keyword.toLowerCase())  // ture
```

<br/>

특정 문자열로 시작하거나 끝나는지 확인할 때는 `startsWith` , `endsWith` 를 사용함

```tsx
const fileName = 'profile.png';

fileName.endsWith('.png');  // true
fileName.startsWith('profile')  // true
```

<br/>

문자열의 일부를 가져올 때는 `slice()` 를 사용함

```tsx
const text = 'JavaScript';

text.slice(0, 4);  // 'Java'
text.slice(4);     // 'Script'
```

<br/>

사용자 입력값의 앞뒤 공백을 제거할 때는 `trim()` 을 많이 사용함

```tsx
const value = '  hello  ';

value.trim();  // 'hello'
```

<br/>

대소문자를 통일해서 검색하거나 비교할 때는 `toLowerCase()` , `toUpperCase()` 를 사용함

```tsx
'Hello'.toLowerCase();  // 'hello'
'hello'.toUpperCase();  // 'HELLO'
```

<br/>