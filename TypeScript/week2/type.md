</br>
</br>

### TypeScript?

타입스크립트는 자바스크립트의 확장판으로 기존의 자바스크립트를 더 안전하게 사용할 수 있도록 타입 관련된 여러가지 기능들을 추가한 언어입니다.

</br>
</br>

이러한 타입스크립트를 왜 배워야 하나요?

> **에러의 사전 방지**
>

타입스크립트를 사용하면 코드를 실제로 실행하기 전에 미리 에러를 어느 정도 검출 할 수 있습니다.

```tsx
function sum(a, b) {
	return a + b;
}
```

매개변수 **`a`**, `b`에 작성자가 의도하지 않은 문자열과 같은 매개변수를 넘기게 되면 의도와는 다른 결과가 출력됩니다.

</br>
</br>

```tsx
function sum(a: number, b: number) {
	return a + b;
}

sum(10, "hello");  // error
```

매개변수에 숫자 타입을 지정해주면 의도대로 사용할 수 있습니다.

또, **`sum()`** 함수를 호출할 때 인자의 타입이 맞지 않으면 개발  툴에서 미리 경고를 해주어 사소한 실수를 미연에 방지할 수 있습니다.

</br>
</br>

> **코드 가이드 및 자동 완성**
>

타입스크립트를 사용하면 코드를 빠르고 정확하게 작성할 수 있도록 도와줍니다.

</br>
</br>

### JSDoc

JSDoc은 자바스크립트 코드에 주석을 다는 표준 방식을 말합니다.

```tsx
// @ts-check

/**
	* @description 두 수의 합을 구하는 함수
	* @param {number} a 첫 번째 숫자
	* @param {number} b 두 번째 숫자
	*/

function sum(a, b) {
 return a + b;
}
```

- `// @ts-check`는 JSDoc으로 정의한 타입과 맞지 않는 경우 미리 에러를 표시해줍니다.
- `@description`은 해당 함수의 역할을 설명합니다.
- `@param`은 각 함수의 각 파라미터 역할과 타입을 정의합니다.

</br>
</br>

> **“TypeScript말고 JSDoc 쓰면 되는거 아니야?”**
>

JSDoc으로도 충분히 타입스크립트 효과를 낼 수 있습니다.

하지만 간단한 타입이 아니라 객체와 배열 더 나아가 복잡한 타입을 다룰 때는 타입 정의를 위해 작성해야 하는 코드가 많아집니다.

또, 타입스크립트의 모듈 시스템을 사용하지 않는다면 파일마다 동일한 코드를 중복으로 작성해야 하는 단점이 생깁니다.

**그렇기에 결론은 뭐다? 타입스크립트를 열심히 공부하자 !**

</br>
</br>

첫 번째로 알아볼 타입은 문자열 타입입니다.

### 문자열 타입: string

`string` 타입은 문자열을 의미하는 타입입니다.

**`str1, 2, 3, 4`** 이라는 변수의 이름 뒤에 콜론 **`:`** 과 함께 변수의 타입을 정의하는 이런 문법을 **타입 주석** 또는 **타입 어노테이션**이라고 부릅니다.

여기서 콜론 **`:`** 은 **타입 표기**라고 합니다.

```tsx
let str1: string = "hello";
let str2: string = 'hello';
let str3: string = `hello`;
let str4: string = `hello ${str1}`;
```

쌍따옴표 문자열 뿐만 아니라 작은 따옴표, 백틱, 템플릿 리터럴로 만든 모든 문자열을 포함합니다.

</br>
</br>

### 숫자 타입: number

`number` 타입은 자바스크립트에서 숫자를 의미하는 모든 값을 포함하는 타입입니다.

```tsx
let num1: number = 123;
let num2: number = -123;
let num3: number = 0.123;
let num4: number = -0.123;
let num5: number = Infinity;
let num6: number = -Infinity;
let num7: number = NaN;
```

단순 정수 뿐만 아니라 소수, 음수, **`Infinity`**, **`NaN`** 등의 특수한 숫자들도 포함합니다.

</br>
</br>

```tsx
num1 = 'hello'; // error

num1.toUpperCase(); // error
```

**`number`** 타입으로 정의한 변수에는 **`number`** 타입을 제외한 값을 할당할 수 없으며, **`number`** 타입의 값이 사용할 수 없는 **`toUpperCase`** 등의 메서드는 사용할 수 없습니다.

</br>
</br>

### 진위 타입: boolean

참과 거짓을 구분하는 진위 값을 다루는 경우 boolean 타입을 사용합니다.

```tsx
let bool1: boolean = true;
let bool2: boolean = false;
```

**`true`** 또는 **`false`** 만 이 타입에 해당됩니다.

</br>
</br>

### null & undefined 타입

```tsx
let unde1: undefined = undefined;
```

**`undefined`** 타입은 오직 **`undefined`** 값만 포함하는 타입입니다.

</br>
</br>

```tsx
let null: null = null;
```

**`null`** 타입 역시 **`undefined`** 타입과 마찬가지로 오직 하나의 값 **`null`** 만 포함하는 타입입니다.

</br>
</br>

> **`null`** **값을 다른 타입의 변수에 할당하기**
>

자바스크립트에서는 아직 값이 정해지지 않은 상태에서는 변수에 `null`을 임시로 넣어둡니다.

```tsx
// js
let numA = null;

// ts
let numA: number = null;
```

타입스크립트에서는 타입이 **`null`** 타입이 아닐 경우에는 오류가 발생합니다.

</br>
</br>

### any 타입

**`any`** 타입은 타입스크립트에서만 제공되는 특별한 타입으로 타입 검사를 받지 않는 특수한 타입을 말합니다.

```tsx
let anyVar: any = 10;
anyVar = "hello";

anyVar = true;
anyVar = {};
```

**`any`** 타입은 아무 타입의 값이나 범용적으로 담아 사용할 수 있습니다.

</br>
</br>

또 **`any`** 타입의 값은 어떤 타입으로 정의된 변수던 문제 없이 다 할당할 수 있습니다.

```tsx
let anyVar: any = 10;
anyVar = "hello";

let num: number = 10;
num = anyVar;
```

그렇기에 다음과 같이 할당을 해도 문제가 발생하지 않습니다.

</br>
</br>

### 리터럴 타입

리터럴은 우리말로 **값**이라는 뜻으로 딱 하나의 값만 포함하는 타입을 말합니다.

```tsx
let numA: 10 = 10;
let strA: "hello" = "hello";
let boolA: true = true;
let boolB: false = false;
```

숫자 값 뿐만 아니라 문자열이나 불리언 타입의 값도 모두 리터럴 타입으로 만들 수 있습니다.

</br>
</br>

### 객체 타입: object

객체 유형의 데이터를 취급할 때는 **`object`** 라는 타입을 사용합니다.

```tsx
let user: object = {
  id: 1,
  name: "강은현",
};
```

타입스크립트의 **`object`** 타입은 단순 값이 객체임을 표현하는 것 외에는 아무런 정보를 제공하지 않는 타입입니다.

그렇기에 해당 타입은 객체의 프로퍼티에 대한 정보를 전혀 가지고 있지 않아 프로퍼티에 접근하려고 하면 오류가 발생합니다.

</br>
</br>

이럴 때에는 `object`가 아닌 객체 리터럴 타입을 이용해야 합니다.

> **객체 리터럴 타입**
>

객체 리터럴 타입은 중괄호를 열고 객체가 갖는 프로퍼티를 직접 나열해 만드는 타입입니다.

```tsx
let user: { id: number; name: string; } = { id: 1, name: "강은현", };

console.log(user.id);  // 1
console.log(user.name);  // 강은현
```

객체 리터럴 타입을 사용하면 타입내에 정의되어 있는 프로퍼티에 이상 없이 접근할 수 있게 됩니다.

</br>
</br>

### 배열 타입: Array

배열의 데이터 타입은 배열을 구성할 요소의 타입을 의미합니다.

배열 타입은 두 가지 방법으로 선언할 수 있습니다.

```tsx
// 1.
let 변수: 배열요소타입[] = [...];

// 2.
let 변수: Array<배열요소타입> = [...];
```

두 가지 방법은 문법적으로만 다를 뿐 역할은 같습니다.

그렇기에 키보드 입력도 더 적고 직관적인 `배열요소타입[]` 형태의 문법을 사용하는게 좋습니다.

</br>
</br>

```tsx
// 1.
let strArr: string[] = ["hello", "im", "eunhyeon"];

// 2.
let boolArr: Array<boolean> = [true, false, true];
```

이때 **`Array<배열요소타입>`** 형태에서 꺽쇠와 함께 타입을 작성하는 문법을 타입스크립트에서는 제네릭이라고 부릅니다.

</br>
</br>

> **다양한 타입 요소를 갖는 배열  타입 정의하기**
>

여러 타입중 하나를 만족하는 타입을 정의하는 문법을 유니온 타입이라고 부릅니다.

유니온 타입을 이용해 여러 타입 중 하나를 만족하는 경우 허용하는 범용적인 타입을 만들 수 있습니다.

```tsx
let multiArr: (number | string)[] = [1, "hello"];
```

소괄호 내부의 `number | string`은 배열 요소의 타입이 **`string`** 이거나 **`number`** 일 것을 의미합니다.

</br>
</br>

> **다차원 배열 타입 정의하기**
>

```tsx
let doubleArr: number[][] = [
	[1, 2, 3],
	[4, 5],
];
```

`[]`를 연달아 작성해 다차원 배열 타입도 간단하게 정의할 수 있습니다.

</br>
</br>

### 튜플 타입: tuple

튜플은 타입스크립트의 특수한 타입으로 길이와 타입이 고정된 배열을 의미합니다.

```tsx
let tup1: [number, number] = [1, 2];
```

이런식으로 길이가 2로 고정된 2개의 **`number`** 타입 요소를 갖는 튜플 타입을 정의합니다.

</br>
</br>

> **튜플 = 배열이다**
>

튜플 타입이 정의된 타입스크립트 코드를 컴파일 하면 자바스크립트 배열로 변환이 되어 나타납니다.

```tsx
let tup1: [number, number] = [1, 2];

tup1.push(1);
tup1.push(1);
tup1.push(1);
tup1.push(1);

console.log(tup1);  // [1, 2, 1, 1, 1, 1]
```

그렇기에 배열 메서드인 **`push`** 나 **`pop`** 을 이용해 고정된 길이를 무시하고 요소를 추가하거나 삭제할 수 있습니다.

</br>
</br>

### 함수에 타입을 정의하는 방법

자바스크립트에서 함수는 다음과 같이 선언합니다.

함수는 `function`이라는 키워드(예약어)와 함수 이름으로 함수를 선언할 수 있고, `return`을 추가해서 값을 반환하거나 함수 실행을 종료할 수 있습니다.

```tsx
function sayWord(word) {
	return word;
}

sayWord('hello');  // hello 반환
sayWord('bye');  // bye 반환
```

`word`는 함수의 파라미터(매개변수)라고 하고, 파라미터가 있으면 함수를 호출할 때 값을 넘길 수 있습니다.

이때 함수를 호출할 때 넘긴 문자열 `hello, bye`를 인자라고 합니다.

</br>
</br>

> **함수의 타입 정의: 파라미터와 반환값**
>

함수의 타입을 정의할 때는 먼저 입력 값과 출력 값에 대한 타입을 정의합니다.

```tsx
function sayWord(word): string {
	return word;
}
```

함수의 반환값 타입은 함수 이름 오른쪽에 `‘: 타입이름’`으로 지정할 수 있습니다.

</br>
</br>

```tsx
function sayWord(word: string): string {
	return word;
}
```

파라미터 오른쪽에 **`‘: 타입이름’`** 을 넣으면 파라미터의 타입이 정의됩니다.

</br>
</br>

### 타입스크립트 함수의 인자 특징

자바스크립트 함수에서는 파라미터와 인자의 개수가 일치하지 않아도 프로그래밍상 문제가 없습니다.

```tsx
function sayWord(word) {
	return word;
}

sayWord('hi', 'capt');  // hi
```

파라미터의 숫자보다 인자의 개수가 많더라도 에러가 발생하지 않고 정상적으로 실행되지만, 초과된 인자는 실행에 영향을 미치지 않고 무시됩니다.

하지만 타입스크립트에서는 파라미터와 인자의 개수가 다르면 에러가 발생합니다.

</br>
</br>

### 옵셔널 파라미터

함수의 파라미터를 선택적으로 사용하기 위해서 옵셔널 파라미터가 필요합니다.

```tsx
function sayMyName(firstName: string, lastName?: string): string {
	return 'my name: ' + firstName + ' ' + lastName;
}

sayMyName('Captain');  // my name : captain
```

두 번째 파라미터에 `?`를 붙여서 옵셔널 파라미터로 정의하면 `lastName`은 호출할때 넘겨도 되고 넘기지 않아도 상관없습니다.