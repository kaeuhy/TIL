### 정적 타이핑 & 동적 타이핑

분류는 언제 타입 검사가 이루어지는가에 대한 기준입니다.

</br>

**정적 타이핑 - Static Typing**

정적 타이핑은 코드가 실행되기 전, 즉 컴파일 시점에 타입 검사를 합니다.

그렇기에 변수나 함수 매개변수, 반환 값에 대해 개발자가 코드에 명시적으로 타입을 선언해야 합니다.

```tsx
// java
int age = 30;

// TypeScript
const age: number = 30;
```


- **장점**
    - 높은 안정성: 코드가 실행되기 전에 타입 관련 오류를 대부분 잡을 수 있습니다.
    - 유지보수성: 코드의 의도가 명확해져 프로젝트 관리가 용이해집니다.
- **단점**
    - 타입을 일일이 선언해야 하므로 코드가 늘어납니다.
    - 유연성이 다소 떨어집니다.

</br>
</br>

**동적 타이핑 - Dynamic Typing**

동적 타이핑은 코드가 실행되는 도중, 즉 런타임 시점에 타입 검사를 합니다.

그렇기에 변수를 선언할 때 타입을 명시하지 않고, 타입은 변수에 값이 할당될 때 결정 됩니다.

```tsx
// JavaScript
let age = 30;  // number 타입

let age = "hello";  // string 타입
```


- **장점**
    - 높은 유연성: 코드 작성이 빠르고 유연합니다.
    - 타입 선언이 없어 코드가 간결해 보입니다.
- **단점**
    - 런타임 에러: 실행 전까지 타입 오류를 알 수 없어 안정성이 낮습니다.
    - 타입 추적이 어려워 유지보수가 어려워집니다.

</br>
</br>

### 강타입 & 약타입

서로 다른 타입 간의 연산을 얼마나 엄격하게 제한하는가에 대한 기준을 말합니다.

</br>

**강타입 - Strong Typing**

강타입은 서로 다른 타입의 연산을 엄격히 금지합니다.

그렇기에 프로그래머가 명시적으로 타입을 변환하지 않는 한, 언어 시스템이 알아서 타입을 바꿔주지 않습니다.

- **장점**
    - 예측 가능성: 코드의 동작이 명확하고 예측 가능합니다.
    - 암시적 변환으로 인한 버그가 발생할 여지가 거의 없습니다.
- **단점**
    - 개발자가 직접 타입을 변환해줘야 하므로 코드가 다소 길어질 수 있습니다.

</br>

**약타입 - Weak Typing**

약타입은 서로 다른 타입의 연산을 만나면, 언어가 임의로 타입을 변환하여 실행합니다.

그렇기에 연산을 수행하기 위한 언어 내부적으로 암시적 타입 변환이 활발하게 일어납니다.


- **장점**
    - 코드가 유연하고 간편하게 느껴질 수 있습니다.
- **단점**
    - 예측 불가능성: 개발자의 의도와 다른 암시적 변환으로 인해 심각한 버그를 유발할 수 있습니다.

</br>

**JavaScript와 TypeScript**

- **JavaScript**
    - 동적 타이핑 + 약타입 언어
    - 타입 에러를 런타임에 가서야 알 수 있지만 그마저도 언어가 알아서 타입을 바꾸기 때문에 이상한 값으로 실행되어 버릴 수 있습니다.

</br>

- **TypeScript**
    - 정적 타이핑 + 약타입 언어
    - 컴파일 시점에 미리 타입 경고를 하여 개발자가 수정하도록 하는 것이 런타임에 발생하는 JavaScript의 약 타입 특성 자체를 없애주지는 못합니다.

      → TypeScript 코드는 결국 JavaScript 코드로 변환되어 실행되기 때문

</br>
</br>

### 인터페이스란?

타입스크립트에서 인터페이스는 객체 타입을 정의할 때 사용하는 문법을 말합니다.

</br>

**인터페이스를 이용한 객체 타입 정의**

```tsx
let seho = { name: '세호', age: 36 };
```

세호라는 문자열을 갖는 `name` 속성과 숫자 `36`을 갖는 `age` 속성으로 구성된 객체입니다.

해당 객체의 타입을 인터페이스로 정의하면 다음과 같습니다.

`interface`라는 예약어를 이용하여 인터페이스를 선언합니다.

```tsx
interface User {
	name: string;
	age: number;
}

let seho: User = {
	name: '세호',
	age: '36',
};
```

`seho`라는 객체에 `User`라는 인터페이스를 지정하였고, 해당 객체는 인터페이스의 타입에 맞는 타입을 사용했기에 인터페이스를 지정하더라도 에러가 발생하지 않습니다.

이처럼 인터페이스를 이용하여 객체의 속성과 들어갈 데이터 타입을 정확하게 정의할 수 있습니다.

</br>
</br>

### **인터페이스를 이용한 함수 타입 정의**

인터페이스는 객체가 활용되는 모든 곳에 인터페이스를 사용할 수 있습니다.

객체는 함수의 파라미터로도 사용되고 반환값으로도 사용될 수 있습니다.

</br>

**함수 파라미터 타입 정의**

특정 데이터에 속성이 존재하려면 해당 데이터가 객체여야 합니다.

그렇기에 함수의 파라미터를 좀 더 명시적으로 선언하려면 다음과 같이 인터페이스를 이용하여 타입을 선언하면 매개변수가 객체인지 쉽게 알 수 있습니다.

```tsx
interface Person {
	name: string;
	age: number;
}

function logAge(someone: Person) {
	console.log(someone.age);
}
```

`Person`이라는 인터페이스를 선언한 후 함수의 파라미터 `someone`에 `Person` 타입을 정의합니다.

이렇게 `Person` 인터페이스를 통해 `someone`이 객체임을 알 수 있습니다.

</br>
</br>

```tsx
interface Person {
	name: string;
	age: number;
}

function logAge(someone: Person) {
	console.log(someone.age);
}

let eunhyeon = { name: 'eunhyeon' }
logAge(eunhyeon);  // Error
```

`eunhyeon` 객체에는 `name` 속성만 존재하기에 `logAge` 파라미터 타입과 일치하지 않아 에러가 발생합니다.

</br>
</br>

**함수 반환 타입 정의**

Person 인터페이스 타입의 데이터를 받아 그대로 반환시 함수의 반환 타입이 추론이 됩니다.

함수의 반환 타입을 명시적으로 표시하기 위해 인터페이스로 함수의 반환 타입을 정의할 수 있습니다.

```tsx
interface Person {
	name: string;
	age: number;
}

function getPerson(someone: Person): Person {
	return someone;
}

// let eunhyeon: Person
let eunhyeon = getPerson({ name: 'eunhyeon', age: 25 });
```

이때, `getPerson` 함수의 호출 결과를 변수에 할당하면 변수가 `Person` 인터페이스 타입으로 추론됩니다.

</br>
</br>

### 인터페이스의 옵션 속성

인터페이스로 정의된 객체의 속성을 선택적으로 사용하고 싶을 때 옵션 속성을 사용합니다.

이때 옵션 속성, 옵셔널 프로퍼티를 사용하여 객체의 속성을 선택적으로 사용합니다.

</br>

선택적으로 사용하고 싶은 객체 오른쪽에 `?`을 붙여줍니다.

```tsx
interface Person {
	name?: string;
	age: number;
}

function logAge(someone: Person) {
	console.log(someone.age);
}

let eunhyeon = { age: 100 };
logAge(eunhyeon);  // 100
```

`name` 속성에 옵셔널 프로퍼티가 사용되었기에 `age` 속성만 있더라도 에러가 발생하지 않습니다.

이렇게 상황에 따라서 유연하게 인터페이스 속성의 사용 여부를 결정할 수 있는 것이 옵션 속성입니다.

</br>
</br>

### 인터페이스 상속

상속은 객체 간 관계를 형성하는 방법이며, 상위 클래스의 내용을 하위 클래스가 물려받아 사용하거나 확장하는 기법을 의미합니다.

</br>

**인터페이스의 상속이란?**

클래스를 상속받을 때 `extends`란 예약어를 사용했지만, 인터페이스를 상속받을 때도 동일하게 `extends` 예약어를 사용합니다.

```tsx
interface Person {
	name: string;
	age: string;
}

interface Developer extends Person {
	skill: string;
}

let ironman: Developer = {
	name: '아이언맨',
	age: 21,
	skill: '만들기'
}
```

`Person` 인터페이스를 선언하고 `Developer` 인터페이스에 `extends`로 상속을 하였습니다.

</br>

`Person` 인터페이스에 정의된 `name`과 `age` 속성 타입이 Developer 인터페이스에 상속되었기 때문에 다음 코드와 같이 정의한 효과가 나타납니다.

```tsx
interface Developer {
	name: string;
	age: number;
	skill: string;
}
```

`extends` 키워드를 사용해서 인터페이스의 타입을 상속받아 확장하여 사용할 수 있습니다.

</br>
</br>

**인터페이스를 상속할 때 참고 사항**

상위 인터페이스의 타입을 하위 인터페이스에서 상속받아 타입을 정의할때 상위 인터페이스의 타입과 호환이 되어야 합니다.

호환이 된다는 것은 상위 클래스에서 정의된 타입을 사용해야 합니다.

</br>
</br>

```tsx
interface Person {
	name: string;
	age: number;
}

interface Develop extends Person {
	name: number;
}
```

이런식으로 상위 인터페이스에서의 `name` 속성의 타입과 하위 인터페이스에서의 타입을 다르게 정의하면 에러가 발생합니다.

</br>
</br>

상속할 때 또 알아 두어야 할 점은 상속을 여러 번 할 수 있습니다.

```tsx
interface Hero {
	power: boolean;
}

interface Person extends Hero {
	name: string;
	age: number;
}

interface Developer extends Person {
	skill: string;
}

let ironman: Developer = {
	name: '아이언맨',
	age: 21,
	skill: '만들기',
	power: true
}
```

`Hero` 인터페이스를 `Person` 인터페이스가 상속받고, `Person` 인터페이스를 `Developer` 인터페이스가 상속받습니다.

이처럼 상속을 여러 번 받아서 정의할 수도 있습니다.

</br>
</br>

### 인터페이스를 이용한 인덱싱 타입 정의

인덱싱이랑 다음과 같이 객체의 특정 속성을 접근하거나 배열의 인덱스로 특정 요소에 접근하는 동작을 의미합니다.

</br>

```tsx
let user = {
	name: '은현',
	admin: true
};
console.log(user['name']);  // 은현

let companies = ['토스', '토스 플레이스', '토스 인컴'];
console.log(companies[0]);  // 토스
```

이렇게 `user[’name’]` 형태로 객체의 특정 속성에 접근하거나 `companies[0]` 형태로 배열의 특정 요소에 접근하는 것을 인덱싱이라고 합니다.

</br>
</br>

**배열 인덱싱 타입 정의**

배열을 인덱싱할 때 인터페이스로 인덱스와 요소의 타입을 정의할 수 있습니다.

</br>



>**근데 인터페이스는 객체 타입을 정의할 때 사용하는 문법이라면서 어떻게 배열을 사용할 수 있나요?**

핵심은 인터페이스로 배열의 형태를 정의할 수도 있다가 정확한 표현입니다.

</br>

```tsx
const arr = ['은현', '막스', '르클레르'];

console.log(typeof arr);  // object
console.log(arr[0]);  // 은현
console.log(arr[1]);  // 막스
console.log(arr[2]);  // 르클레르
```

콘솔창에 배열을 선언한 후에 typeof 를 통해 자료형을 확인해 보면 object가 출력되는 것을 확인할 수 있습니다.

자바스크립트에서 배열은 내부적으로 **객체**입니다.

</br>

```tsx
{
  0: "은현",
  1: "막스",
  2: "르클레르",
  length: 3,
  push: function() { ... },
  pop: function() { ... },
  // 등등...
}
```

즉, 배열은 0, 1, 2 같은 숫자 인덱스를 속성 이름처럼 사용하고, 값으로 실제 배열 요소가 들어가는 객체입니다.

</br>

그렇기에 배열의 형태를 인터페이스로 표현하면 아래 코드처럼 구현할 수 있습니다.

```tsx
interface StringArray {
	[index: number]: string;
}

let companies: StringArray = ['토스', '토스 플레이스', '토스 인컴']
```

즉, `[index: number]` 코드는 어떤 숫자든 모두 속성의 이름이 될 수 있고, `[index: number]: string;` 에서 속성 이름은 숫자고 그 속성 값으로 문자열 타입이 와야 한다는 의미입니다.

</br>
</br>

```tsx
interface StringArray {
	[index: string]: string;
}

let companies: StringArray = ['토스', '토스 플레이스', '토스 인컴']
```

만약 인덱스 타입을 문자열로 하면 당연히 오류가 발생합니다.

→ 배열이기 때문에 문자열이 아닌 `number` 타입만 가능하다.

</br>
</br>

**객체 인덱싱 타입 정의**

배열 인덱싱과 마찬가지로 객체 인덱싱의 타입도 인터페이스로 정의할 수 있습니다.

</br>

`Map` 인터페이스는 속성 이름이 문자열 타입이고 속성 값이 숫자 타입인 인터페이스입니다.

```tsx
interface Map {
	[level: string]: number;
}

let hyeon: Map = {
	junior: 100
};

let money = hyeon.junior;
```

`hyeon` 객체에서는 속성 이름이 문자열이기때문에 오류가 발생하지 않습니다.

</br>
</br>

**인덱스 시그니처란?**

정확히 속성 이름을 명시하지 않고 속성 이름의 타입과 속성 값의 타입을 정의하는 문법을 인덱스 시그니처라고 합니다.

</br>
</br>

```tsx
interface CountryCodes {
	Korea: string;
	UnitedState: string;
	UnitedKingdom: string;
}

let Country: CountryCode {
	Korea: "ko",
	UnitedState: "us",
	UnitedKingdom: "uk",
};
```

해당 코드처럼 구현을 하게된다면 객체의 `key` 값이 생성 될때마다 `interface` 에서도 마찬가지로 `key` 값을 구현해줘야합니다.

</br>
</br>

인덱스 시그니처를 사용하면 쉽게 해결 할 수 있습니다.

```tsx
interface CountryCodes {
	[key: string]: string;
}

let Country: CountryCode {
	Korea: "ko",
	UnitedState: "us",
	UnitedKingdom: "uk",
};
```

이렇게 `key` 값을 `string` 타입 `value` 값을 `string` 타입으로 지정해주면 해당 형태의 타입으로 들어오는 모든 객체들을 구현할 수 있습니다.

</br>
</br>

```tsx
interface CountryCodes {
	[key: string]: string;
}

let Country: CountryCode {};
```

하지만 인덱스 시그니처는 해당 형태가 불일치 형태만 신경쓰기 때문에 객체에 아무런 내용이 없어도 오류가 발생하지 않습니다.

</br>
</br>

```tsx
interface CountryCodes {
	[key: string]: string;
	Korea: string;
}

let Country: CountryCode {
	Korea: "ko",
};
```

그렇기 때문에 꼭 구현이 필요한 객체는 `key` 와 `value` 를 지정해줘야합니다.

이때 인덱스 시그니처의 `key` 와 `value` 타입과 동일해야합니다.

</br>
</br>