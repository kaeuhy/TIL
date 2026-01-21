</br>
</br>

### **이넘 - enum**

`enum` 은 관련된 이름(레이블)들을 묶어 숫자나 문자열 같은 값으로 관리하기 편하도록 만드는 TypeScript 전용 문법을 말합니다.

이넘은 알아보기 전에 상수의 의미를 먼저 복습해봅시다.

</br>
</br>

```tsx
function getDinnerPrice() {
	return 10000 + 2000;
}
```

해당 코드는 저녁 식사 값을 계산하는 함수로, 반환값으로 10000원과 2000원을 더한 값을 돌려주고 있습니다.

하지만 해당 코드만 봐서는 어떤 메뉴의 가격인지 알 수 없습니다.

이때 상수를 사용하여 각 숫자 값에 의미를 부여할 수 있습니다.

</br>
</br>

상수를 사용할때는 보통 모두 대문자로 작성해서 일반 변수와 구분합니다.

→ 코딩 규칙은 프로젝트마다 다를 수 있습니다.

```tsx
function getDinnerPrice() {
	const RICE = 10000;
	const COKE = 2000;
	return RICE + COKE;
}
```

해당 메뉴들은 이미 지출된 값이기 때문에 `const` 를 사용하여 고정 값으로 정의했습니다.

상수는 단순히 고정된 값을 저장하는 것뿐만 아니라 이 값이 어떤 의미를 갖는지 알려 줌으로써 가독성을 높이는 장점이 있습니다.

</br>

앞서 살펴본 여러 개의 상수를 하나의 단위로 묶으면 이넘이 됩니다.

비슷한 성격이나 같은 범주에 있는 상수를 하나로 묶어 더 큰 단위의 상수로 만드는 것이 이넘의 역할입니다.

</br>
</br>

신발 브랜드를 `enum` 타입을 이용하여 하나의 단위로 묶은 예제입니다.

```tsx
enum ShoesBrand {
	Nike,
	Adidas,
	NewBalance
}
```

</br>
</br>

해당 이넘 값의 각 속성은 다음과 같은 형태로 사용합니다.

```tsx
let myShoes = ShoesBrand.Nike;

let yourShoes = ShoesBrand.NewBalance;
```

객체의 속성에 접근하듯이 이넘의 이름을 쓰고 점 접근자를 이용하여 속성 이름을 붙입니다.

`myShoes` 와 `yourShoes` 에는 각각 `0`과 `1`이 대입됩니다.

`enum` 멤버에 점 접근자로 접근할 수 있는 이유는 컴파일 결과물이 객체이기 때문입니다.

</br>
</br>
</br>
</br>

### **숫자형 이넘**

이넘에 선언된 속성은 기본적으로 숫자 값을 가집니다.

이넘을 선언하면 첫 번째 속성부터 `0`, `1`, `2`, `3` 이 할당됩니다.

```tsx
enum Direction {
	Up,   // 0
	Down, // 1
	Left, // 2
	Right // 3
}

console.log(Direction.Up); // 0
```

이처럼 속성 값이 숫자로 지정되는 이유는 타입스크립트의 내부 규칙 때문에 그렇습니다.

이넘을 자바스크립트로 컴파일하여 알아봅시다.

</br>
</br>

```tsx
"use strict";
var Direction;
(function (Direction) {
    Direction[Direction["Up"] = 0] = "Up";
    Direction[Direction["Down"] = 1] = "Down";
    Direction[Direction["Left"] = 2] = "Left";
    Direction[Direction["Right"] = 3] = "Right";
})(Direction || (Direction = {}));

console.log(Direction.Up);  // 0
```

`Direction` 은 변수이고 변수의 `Up` 속성에 `0`을 할당했기 때문에 `Direction` 객체의 `Up` 속성에 접근하는 형태의 `Direction.Up`을 입력하면 `0` 이라는 값이 나옵니다.

</br>
</br>

**TypeScript `enum` 이 JavaScript로 변활될 때의 핵심 원리**

`enum` 키워드는 JavaScript에는 존재하지 않는 키워드이므로 컴파일 과정에서 일반 JavaScript 객체로 변환됩니다.

```tsx
Direction[Direction["Up"] = 0] = "Up";
```

해당 코드를 보면 TypeScript는 `enum` 을 양방향 매핑, 리버스 매핑을 가진 객체로 만듭니다.

그렇기에 `“Up” → 0`, `0 → “Up”` 이 둘 다 가능하게됩니다.

해당 한 줄에 두 가지 할당이 한꺼번에 일어나므로 충분히 헷갈릴 수 있습니다.

순서대로 코드를 풀어봅시다.

</br>
</br>

```tsx
Direction["Up"] = 0;
```

먼저 객체 `Direction` 의 프로퍼티 `“Up”` 에 숫자 `0` 을 할당합니다.

할당 표현식의 값은 할당된 값 자체인 0을 반환합니다.

</br>
</br>

```tsx
Direction[0] = "Up";
```

그다음 반환값 `0`을 인덱스로 사용하여 `Direction[0] = "Up"` 을 실행합니다.

결국 `Direction` 객체에 숫자 인덱스처럼 동작하는 문자열 `0` 을 추가하여 `“Up”`을 저장합니다.

그렇게에 `Direction["Up"] === 0` 이고 `Direction[0] === "Up"`  이 되게 됩니다.

</br>
</br>

결과적으로 `Direction` 은 아래와 같은 객체가 됩니다.

```tsx
{ '0': 'Up', '1': 'Down', Up: 0, Down: 1 }
```

</br>
</br>

**자동 증가 규칙**

기본값이 존재하지 않는다면 첫 멤버가 `0` 이 되고, 그 다음 순차적으로 `1`, `2`, … 로 자동 증가합니다.

첫 값에 특정 숫자를 지정하게 된다면 그 다음부터는 특성 숫자에 `+ 1` 값으로 자동 증가됩니다.

```tsx
enum Direction {
	Up = 10,
	Down,  // 11
	Left,  // 12
	Right, // 13
}
```

실제 코드를 작성할 때는 `Down = 11` 처럼 명시적으로 값을 설정하는 것이 컴파일 결과를 보지 않고도 값을 빠르게 파악할 수 있습니다.

</br>
</br>
</br>
</br>

### **문자형 이넘**

문자형 이넘이란 이넘의 속성 값에 문자열을 연결한 이넘을 의미합니다.

숫자형 이넘과는 다르게 모든 속성 값을 다 문자열로 지정해 주어야 하고, 선언된 속성 순서대로 값이 증가하는 규칙도 없습니다.

```tsx
enum Direction {
	Up = "Up",
	Down = "Down",
	Left = "Left",
	Right = "Right"
}

console.log(Direction.Up);  // Up
```

해당 문자형 이넘도 자바스크립트 코드로 변환시켜봅시다.

</br>
</br>

```tsx
"use strict";
var Direction;
(function (Direction) {
    Direction["Up"] = "Up";
    Direction["Down"] = "Down";
    Direction["Left"] = "Left";
    Direction["Right"] = "Right";
})(Direction || (Direction = {}));
```

숫자형 이넘과 다르게 문자열 이넘은 역방향 매핑이 생성되지 않습니다.

`Direction.Up === "Up"` 은 가능하지만 `Direction["Up"]` 는 `undefined` 입니다.

</br>
</br>

```tsx
enum Direction {
	UP = "UP",
	DOWN = "DOWN",
	LEFT = "LEFT",
	RIGHT = "RIGHT"
}

enum ArrowKey {
	KEY_UP = "KEY_UP",
	KEY_DOWN = "KEY_DOWN"
}
```

추가적으로 이넘을 사용할때, 모두 파스칼 케이스로 작성했지만 모두 대문자로 적거나 언더스코어를 사용해도 상관없습니다.

</br>
</br>
</br>
</br>


### 알아 두면 좋은 이넘의 특징

숫자형 이넘과 문자형 이넘 말고도 다양한 형태로 선언할 수 있는 이넘의 문법을 알아보겠습니다.

</br>
</br>

**혼합 이넘**

이넘을 사용할때 숫자와 문자열을 섞어서 선언할 수 있습니다.

```tsx
enum Answer {
	Yes = "Yes",
	No = 1
}
```

하지만 이넘 값은 일괄되게 숫자나 문자열 둘 중 하나의 데이터 타입으로 관리하는 것이 좋습니다.

**→ 이유:**

- **타입 혼란을 막기 위해:**
    - 숫자와 문자열을 섞으면 멤버마다 동작 방식이 달라져, 코드 읽기와 유지보수가 어려워집니다.
- **역방향 매핑 일관성 유지:**
    - 숫자 `enum` 은 역매핑이 생기지만, 문자열 `enum` 은 역매핑이 없기 때문입니다.
- **버그 발생 가능성 감소:**
    - API 통신, DB 저장, 비교 연산에서 값 타입이 섞여 있으면 의도치 않은 비교 오류나 타입 변환 문제가 자주 발생합니다.
- **가독성 및 명확한 설계 유지:**
    - `enum` 의 목적은 정해진 값 집합을 명확하고 안정적으로 관리하는 것인데, 타입이 섞이면 설계 기준이 모호해집니다.

</br>
</br>

**다양한 이넘 속성 값 정의 방식**

이넘의 속성 값은 고정 값뿐만 아니라 다양한 형태로 값을 할당할 수 있습니다.

```tsx
enum Authorization {
	User,
	Admin,
	SuperAdmin = User + Admin,
	God = "abc".lenth
}
```

</br>
</br>

해당 코드의 값을 보면 다음과 같습니다.

```tsx
enum Authorization {
	User,                        // 0
	Admin,                       // 1
	SuperAdmin = User + Admin,   // 1
	God = "abc".lenth            // 3
}
```

`User` 와 `Admin` 속성은 이넘의 기본 규칙에 따라 값이 0과 1이 됩니다.

`SuperAdmin` 속성은 `User` 와 `Admin` 의 값을 더한 결과인 1을 갖습니다.

여기서 먼저 선언되어 있는 이넘의 속성을 활용할 수 있다는 것과 덧셈 연산자를 사용하여 계산한 값을 속성 값으로 할당할 수 있다는 것입니다.

</br>

`God` 속성은 문자열의 길이를 뜻하는 속성 값을 정의하여 길이가 `3` 이므로 `God` 속성 값은 `3` 이 됩니다.

</br>
</br>

**`const` 이넘**

`const` 이넘은 컴파일 시 완전히 사라지는 즉, `enum` 객체를 만들지 않는 특별한 `enum` 을 말합니다.

런타임에 `enum` 이라는 객체 자체가 존재하지 않습니다.

```tsx
const enum logLevel {
	Debug = "Debug",
	Info = "Info",
	Error = "Error"
}

var appLevel = logLevel.Error
```

JavaScript로 컴파일을 하여 코드로 알아봅시다.

</br>
</br>

```tsx
var appLevel = "Error" /* logLevel.Error */;
```

`const` 를 사용하여 이넘으로 선언하면 `Error` 문자열 값이 할당되는 것을 볼 수 있습니다.

실제로 `logLevel.Error` 이라는 객체는 생성되자않아 JavaScript 에 존재하지 않기 때문입니다.

</br>
</br>

**일반 이넘과 `const` 이넘의 차이**

- **enum 객체 생성:**
    - 일반 이넘은 이넘 객체가 생성되지만 `const` 이넘은 생성되지 않습니다.
- **리버스 매핑:**
    - 일반 이넘은 가능하지만 객체가 없는 `const` 이넘은 불가능합니다.
- **메모리 사용:**
    - 객체가 없는 `const` 이넘은 메모리를 사용하지 않아 접근 비용도 없습니다.

</br>
</br>