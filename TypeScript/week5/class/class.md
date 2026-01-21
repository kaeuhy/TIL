</br>
</br>

### **클래스란?**

클래스는 동일한 모양의 객체를 더 쉽게 생성하도록 도와주는 자바스크립트 ES6+ 문법입니다.

보통 자바스크립트에서 객체를 정의할 때는 다음과 같은 객체 정의 문법을 사용합니다.

</br>

```tsx
let max = {
	name: '막스 베르스타펜',
	team: '레드불'
};

let leclerc = {
	name: '르끌레르',
	team: '페라리'
};
```

두 객체는 모두 `name` 과 `team` 이라는 공통된 속성을 가지고 있습니다.

이렇게 모양이 유사한 객체는 일일이 객체를 정의하기보다 생성자 함수를 사용하는 것이 유리합니다.

</br>
</br>

```tsx
function Driver(name, team) {
	this.name = name;
	this.team = team;
}

let max = new Driver('막스 베르스타펜', '레드불');
let leclerc = new Driver('르끌레르', '페라리');
```

함수의 첫 글자를 대문자로 작성하면 생성자 함수라고 보는 것이 일반적인 관례입니다.

생성자 함수는 `new` 라는 키워드를 붙여서 호출하면 새로운 객체를 생성합니다.

이때, `this` 는 객체이며 현재 만들고 있는 객체를 의미합니다.

</br>
</br>

위 코드에서는 객체를 2개만 생성했지만 여러 개를 생성한다면 더 쉽게 유사한 구조의 객체를 만들 수 있습니다.

이렇게 객체를 쉽게 찍어내는 함수가 생성자 함수이고 이러한 생성자 함수를 최신 자바스크립트 문법으로 표현하면 다음과 같습니다.

```tsx
class Driver {
	constructor(name, team) {
		this.name = name;
		this.team = team;
	}
}
```

`Driver` 클래스는 앞서 살펴본 `Driver` 생성자 함수와 코드는 다르지만 역할은 동일합니다.

이처럼 생성자 함수라는 일반적인 관례를 문법 레벨로 끌어올린 것이 클래스입니다.

</br>
</br>
</br>
</br>

### 클래스 기본 문법

클래스 기본 문법을 이해하기전 생성자 함수의 예시를 보고 클래스와 비교해 봅시다.

여기서 `prototype` 은 생성자 함수가 공유하는 공용 공간을 말합니다.

`prototype` 에 메서드를 넣으면 `new Person()` 으로 만들어진 모든 객체가 해당 메서드를 사용할 수 있습니다.

```tsx
function Person(name, skill) {
	this.name = name;
	this.skill = skill;
}

Person.prototype.sayHi = function() {
	console.log('hi');
}
```

해당 코드에서는 `Person` 생성자 함수의 `prototype` 에 `sayHi` 메서드를 추가해주었습니다.

그렇기에 `Person` 으로 만들어지는 모든 객체는 `sayHi` 메서드를 사용할 수 있습니다.

</br>
</br>

**해당 문법을 쓰는 이유**

객체를 만들 때마다 메서드를 복사하지 않고 한 곳에만 저장해서 공유하기 위해서 해당 문법을 사용합니다.

예시 코드를 봅시다.

```tsx
function Person() {
	this.sayHi = function(){};
}
```

해당 코드처럼 메서드를 생성자 안에 넣으면 객체를 만들 때마다 메서드를 새로 복사하여 만들어야하므로 메모리 낭비가 발생합니다.

그래서 JavaScript에서는 공유 메서드 개념을 도입했고 그게 `prototype` 문법입니다.

</br>
</br>

해당 생성자 함수의 코드를 그대로 클래스로 옮기면 다음과 같습니다.

```tsx
class Person {
	constructor(name, skill) {
		this.name = name;
		this.skill = skill;
	}
	
	sayHi() {
		console.log('hi');
	}
}
```

해당 `Person` 클래스는 `name` 과 `skill` 값을 받아 객체를 생성할 수 있게 생성자 메서드인 `constructor` 를 선언하고, `sayHi` 클래스 메서드를 선언하였습니다.

여기에서 `name` 과 `skill` 속성을 클래스 필드 또는 클래스 속성이라고 합니다.

</br>
</br>

클래스도 앞의 생성자 함수와 동일하게 `new` 키워드를 붙여 객체를 생성합니다.

```tsx
let hyeon = new Person('은현', '프론트엔드');
```

이렇게 클래스로 생성된 객체를 클래스 인스턴스라고 합니다.

</br>
</br>
</br>
</br>

### **클래스의 상속**

클래스의 상속이란 부모 클래스의 속성과 메서드 등을 자식 클래스에서도 사용할 수 있게 물려준다는 의미입니다.

예시 코드를 통해 알아봅시다.

해당 코드는 `extends` 키워드를 사용하여  `Person` 클래스를 상속받아 `Developer` 클래스를 정의합니다.

```tsx
class Person {
	constructor(name, skill) {
		this.name = name;
		this.skill = skill
	}
	
	sayHi() {
		console.log('hi');
	}
}

class Developer extends Person {
	constructor(name, skill) {
		super(name, skill);
	}
	
	coding() {
		console.log('fun');
	}
}
```

`Developer` 클래스에서 `Person` 클래스의 생성자를 함께 호출해줘야 합니다.

그렇지 않으면 생성되는 객체의 필드 값이 제대로 설정되지 않습니다.

`super(name, skill);` 코드는 자식 클래스인 `Developer` 클래스에서 `new` 키워드로 객체를 생성할 때 부모 클래스인 `Person` 클래스의 생성자 메서드를 호출하겠다는 의미입니다.

</br>
</br>

이제 `Developer` 클래스로 새로운 객체를 생성해봅시다.

```tsx
let hyeon = new Developer('은현', '프론트엔드');

hyeon.sayHi();   // hi
hyeon.coding();  // fun

console.log(hyeon.name);  // 은현
console.log(hyeon.skill); // 프론트엔드
```

자식 클래스 `Developer` 에 정의된 `coding` 메서드와 부모 클래스 `Person` 에 정의된 `sayHi` 메서드도 정상적으로 호출할 수 있습니다.

상속받은 메서드뿐만 아니라 클래스 속성에도 모두 접근할 수 있습니다.

</br>
</br>

상속을 하면 클래스 인스턴스뿐만 아니라 자식 클래스 코드 내부에서도 부모 클래스의 속성이나 메서드를 접근할 수 있습니다.

```tsx
class Person {
	constructor(name, skill) {
		this.name = name;
		this.skill = skill;
	}
	
	sayHi() {
		console.log('hi');
	}
}

class Developer extends Person {
	constructor(name, skill) {
		super(name, skill);
		this.sayHi();
	}
	
	coding() {
		console.log('fun doing' + this.skill + 'by' + this.name);
	}
}
```

자식 클래스의 생성자 메서드에서 부모 클래스인 `Person` 의 `sayHi()` 메서드를 호출합니다.

그리고 자식 클래스의 `coding` 메서드에서 부모 클래스에서 정의한 `name` 과 `skill` 속성을 사용했습니다.

</br>
</br>

해당 자식 클래스로 객체를 생성하고 `coding` 메서드를 호출하면 다음과 같은 결과가 나옵니다.

```tsx
let hyeon = new Developer('은현', '프론트엔드');  // hi
hyeon.coding();  // fun doing 프론트엔드 by 은현
```

자식 클래스인 `Developer` 의 생성자 메서드에 부모 클래스의 메서드인 `this.sayHi` 를 정의했기에 `new` 키워드로 객체를 생성만해도 `hi` 가 출력됩니다.

그리고 자식 클래스의 `coding` 메서드가 호출될 때 부모 클래스의 `name` 과 `skill` 속성을 `this.skill` 과 `this.name` 으로 잘 가져오는 것을 확인 할 수 있습니다.

</br>
</br>
</br>
</br>

### 타입스크립트의 클래스

클래스의 기본적인 문법을 배웠으니 이제 자바스크립트 클래스에 타입을 입혀봅시다.

```tsx
class Chatgpt {
	constructor(name) {
		this.name = name;
	}
	
	sum(a, b) {
		return a + b;
	}
}
```

`Chatgpt` 클래스는 클래스 안에 생성자 메서드와 클래스 메서드 `sum` 을 선언합니다.

이제 해당 클래스에 타입을 입혀보겠습니다.

</br>
</br>

```tsx
class Chatgpt {
	constructor(name: string) {
		this.name = name;
	}
	
	sum(a: number, b: number): number {
		return a + b;
	}
}
```

생성자 메서드 함수의 파라미터인 `name` 타입을 `string` 으로 정의했고, `sum` 클래스 메서드 함수의 파라미터와 반환 타입은 모두 `number` 로 정의했습니다.

</br>
</br>

하지만 다음 코드는 오류가 발생합니다.

그 이유는 TypeScript로 클래스를 작성할 때는 생성자 메서드에서 사용될 클래스 속성들을 미리 정의해 주어야 하기 때문입니다.

```tsx
class Chatgpt {
	name: string;
	
	constructor(name: string) {
		this.name = name;
	}
	
	sum(a: number, b: number): number {
		return a + b;
	}
}
```

JavaScript는 동적 언어이기 때문에 클래스 속성들을 미리 정의할 필요가 없지만, TypeScript에서는 미리 정의를 해주어야 오류가 발생하지 않습니다.

</br>
</br>
</br>
</br>

### 클래스는 타입이다

타입스크립트의 클래스는 타입으로도 사용할 수 있습니다.

클래스를 타입으로 사용하면 해당 클래스가 생성하는 객체의 타입과 동일한 타입이 됩니다.

```tsx
class Person {
	name: string;
	skill: string;
	
	constructor(name: string, skill: string) {
		this.name = name;
		this.skill = skill;
	}
}

let hyeon: Person = {
	name: '은현',
	skill: '프론트엔드'
};

console.log(hyeon);
```

변수 `hyeon` 의 타입을 `Person` 클래스로 정의했습니다.

따라서 해당 변수는 `name`, `skill` 프로퍼터를 갖는 객체 타입으로 정의됩니다.

</br>
</br>

**클래스는 왜 타입인가?**

TypeScript는 구조, 프로퍼티가 같으면 같은 타입으로 보는 구조적 타입 시스템을 사용합니다.

즉, 어떤 객체가 어떤 클래스에서 만들어졌는지는 중요하지 않고, 객체가 어떤 프로퍼티를 갖고 있는지만 중요합니다.

</br>

`Person` 클래스를 TypeScript는 다음과 같이 인식합니다.

```tsx
{name: string; skill: string;}
```

윗 코드의 형태를 가진 모든 객체는 `Person` 타입으로 인정합니다.

즉, `new Person()` 으로 만든 객체뿐 아니라 프로퍼티 구조가 같으면 모두 `Person` 타입으로 인정합니다.

</br>
</br>
</br>
</br>


### 클래스 접근 제어자

클래스 속성의 노출 범위를 정의할 수 있는 접근 제어자는 복잡한 기능을 구현할 때 유용하게 쓰일 수 있습니다.

</br>

**클래스 접근 제어자의 필요성**

간단한 클래스 코드를 통해 알아봅시다.

```tsx
class Person {
	name: string;
	skill: string;
	
	constructor(name: string, skill: string) {
		this.name = name;
		this.skill = skill;
	}
}

let hyeon = new Person('은현', '프론트엔드');
console.log(hyeon.name); // 은현
```

해당 클래스로 생성한 객체는 `hyeon` 이라는 변수에 담겨 있습니다.

생성된 객체는 자유롭게 속성을 변경할 수 있습니다.

</br>
</br>

```tsx
let hyeon = new Person('은현', '프론트엔드');
console.log(hyeon.name); // 은현

hyeon.name = '현식';
console.log(hyeon.name); // 현식
```

해당 코드에서는 클래스로 생성된 객체 내용이 변하더라도 크게 문제없습니다.

하지만 클래스 속성의 내용이 변경되었을 때 영향을 주는 로직이 따로 있다면 어떻게 될까요??

</br>
</br>

해당 코드는 정수기를 의미하는 클래스 코드입니다.

```tsx
class WaterPurifier {
	waterAmount: number;
	
	constructor(waterAmount: number {
		this.waterAmount = waterAmount;
	}
	
	wash() {
		if (this.waterAmount > 0) {
			console.log('정수기 동작 성공');
		}
	}
}

let purifier = new WaterPurifier(30);
```

클래스 생성자 메서드로 클래스를 생성할 때 물의 양을 입력받을 수 있게 했습니다.

클래스 메서드 `wash` 는 물이 조금이라도 있어야 동작하게끔 `if` 문을 작성했고, 이 클래스를 사용하여 객체를 생성하고 `purifier` 라는 변수에 할당했습니다.

</br>
</br>

이제 `purifier` 객체를 사용하여 `wash` 메서드 함수를 실행할 수 있습니다.

```tsx
purifier.wash(); // 정수기 동작 성공
```

위 코드에서 `waterAmount` 에 `30` 을 넣었기에 정수기 동작 성공이라는 문자열이 콘솔에 출력되지만, 누군가가 `purifier` 객체에 접근하여 물의 양을 `0` 으로 바꾼다면 어떻게 될까요?

</br>
</br>

```tsx
purifier.waterAmount = 0;

purifier.wash();
```

물의 양을 `0` 으로 바꾸면 `waterAmount` 가 초깃값 `30` 에서 `0` 으로 바뀌기 때문에 `wash` 메서드를 실행하면 정상적으로 동작하지 않습니다.

이런 오류들을 속성 접근 제어자를 통해 미리 방지할 수 있습니다.

</br>
</br>

이제 속성 접근 제어자를 알아봅시다 !!

클래스의 접근 제어자는 `public`, `private`, `protected` 총 3개가 있습니다.

</br>
</br>

**public**

`public` 접근 제어자는 클래스 안에 선언된 속성과 메서드를 어디서든 접근 가능케 합니다.

클래스 속성과 메서드에 별도로 속성 접근 제어자를 선언하지 않으면 기본값은 `public` 입니다.

</br>

앞서 살펴본 정수기 클래스 코드에 `public` 접근 제어자를 붙여보겠습니다.

```tsx
public class WaterPurifier {
	public waterAmount: number;
	
	constructor(waterAmount: number {
		this.waterAmount = waterAmount;
	}
	
	public wash() {
		if (this.waterAmount > 0) {
			console.log('정수기 동작 성공');
		}
	}
}

let purifier = new WaterPurifier(50);
console.log(purifier.waterAmount); // 50
purifier.wash(); // 정수기 동작 성공
```

이렇게 `public` 접근 제어자를 붙여주면 클래스로 생성한 객체의 속성과 메서드를 어디에서나 접근할 수 있습니다.

</br>
</br>

> **왜 대부분 `constructor` 는 접근 제어자를 안 붙일까?**
>
- **`constructor` 는 원래 객체 생성용:**
    - 기본적으로 객체를 생성할 수 있어야 하므로 `public` 이 자연스럽습니다.
      → 그래서 대부분 생략
- **다른 접근자처럼 보호/은닉해야 하는 영역이 아님:**
    - 뒤에서 살펴볼 `private`, `protected` 는 내부 데이터의 보호 목적입니다.
    - 하지만 `constructor` 는 메서드라기보다는 객체 생성 규칙이므로 생성자를 은닉해야 하는 특별한 이유가 있을 때만 접근 제어자를 붙입니다.
- **생성자에 접근 제어자를 붙였을 때 의미가 크게 바뀜:**
    - `private` 를 붙이게 되면 클래스 외부에서 `new` 키워드를 사용하여 인스턴스를 생성할 수 없습니다.
    - `protected` 를 붙이게 되면 상속받은 클래스만 생성 가능합니다.
    - 즉, 클래스 디자인 자체를 바꾸는 강한 제약이므로 기본적으로 `public` 을 이용합니다.

</br>
</br>

**private**

`private` 접근 제어자는 클래스 코드 외부에서 클래스와 속성과 메서드를 접근할 수 없습니다.

`public` 과 반대되는 개념으로 클래스 안 로직을 외부 세상에서 단절시켜 보호할 때 주로 사용합니다.

앞서 살펴본 `Person` 클래스에 `private` 접근 제어자를 붙여봅시다.

```tsx
class Person {
	private name: string;
	private skill: string;
	
	constructor(name: string, skill: string) {
		this.name = name;
		this.skill = skill;
	}
	
	private sayHi() {
		console.log('hi');
	}
}
```

</br>
</br>

`private` 를 사용한 `Person` 클래스로 객체를 하나 생성하고 `name` 속성을 출력해 보겠습니다.

출력을 해보면 다음과 같은 에러 메시지가 나옵니다.

```tsx
let hyeon = new Person('은현', '프론트엔드');

// name 속성 접근
console.log(hyeon.name);  // 'name' 속성은 private이며 'Person' 클래스 내부에서만 액세스할 수 있습니다.

// sayHi 메서드 접근
hyeon.sayHi(); // 'sayHi' 속성은 private이며 'Person' 클래스 내부에서만 액세스할 수 있습니다.
```

해당 에러는 클래스의 `name` 속성이 `private` 으로 정의되어 있는데, 외부에서 해당 속성을 사용하려하여 발생했습니다.

메서드 `sayHi` 또한 마찬가지로 에러가 발생합니다.

이렇게 `private` 접근 제어자를 사용하면 클래스 코드 바깥에서는 해당 속성이나 메서드를 접근할 수 없습니다.

</br>
</br>

추가적으로 `private` 으로 지정된 속성과 메서드는 클래스 인스턴스에서 자동 완성을 지원하지 않습니다.

<img width="730" height="181" alt="image" src="https://github.com/user-attachments/assets/8e55bbe0-8275-4486-a650-f7cd138cbe60" />

이처럼 `private` 접근 제어자를 이용하여 클래스 외부에서 속성과 메서드가 사용되는 것을 막을 수 있습니다.

</br>
</br>

**protected**

`protected` 접근 제어자는 `private` 과 비슷하면서도 다릅니다.

`protected` 로 선언된 속성이나 메서드는 클래스 코드 외부에서는 사용할 수 없고 상속받은 클래스에서 사용할 수 있습니다.

</br>

`Person` 클래스를 상속받은 `Developer` 클래스 코드를 보면서 이해해봅시다.

```tsx
class Person {
	private name: string;
	private skill: string;
	
	constructor(name: string, skill: string) {
		this.name = name;
		this.skill = skill;
	}
	
	protected sayHi() {
		console.log('hi');
	}
}

class Developer extends Person {
	constructor(name: string, skill: string) {
		super(name, skill);
		this.sayHi();
	}
	
	coding(): void {
		console.log('fun doing' + this.skill + 'by' + this.name);
	}
}
```

`Person` 클래스의 `name` 과 `skill` 의 속성 타입을 모두 `string` 으로 지정하고 접근 제어자로 `private` 을 선언했습니다.

그리고 클래스 메서드 `sayHi` 에는 `protected` 를 선언했습니다.

</br>
</br>

하지만 해당 코드는 에러가 발생합니다.

<img width="1320" height="524" alt="image" src="https://github.com/user-attachments/assets/1990d6d6-7858-44c0-9519-feaacf023756" />

`coding` 메서드의 `this.skill` 과 `this.name` 은 `private` 으로 정의된 속성을 클래스 외부에서 접근하려고 했기 때문에 발생합니다.

</br>
</br>

<img width="922" height="382" alt="image" src="https://github.com/user-attachments/assets/487fe267-3bbc-4650-8f15-0443e74d8f39" />


`coding` 메서드와는 달리 `protected` 으로 정의한 `sayHi` 메서드는 `Person` 클래스를 상속받은 `Developer` 클래스에서 사용해도 문제가 발생하지 않습니다.

</br>
</br>

마지막으로 클래스로 객체를 생성하는 코드를 확인해봅시다.

```tsx
let hyeon = new Person('은현', '프론트엔드');
hyeon.sayHi;
```

<img width="1564" height="536" alt="image" src="https://github.com/user-attachments/assets/026c634d-0dc5-4891-aa31-e8d71d50b062" />


당연히 `sayHi` 메서드는 보호된 속성이므로 `private` 와 마찬가지로 클래스 외부에서 사용할 수 없습니다

</br>
</br>

**Private Fields `#`**

`private` 로 정의된 `name` 과 `skill` 속성을 사용하는 `coding` 메서드를 실행하면 에러가 발생할까요?

```tsx
class Person {
    private name: string;
    private skill: string;

    constructor(name: string, skill: string) {
        this.name = name;
        this.skill = skill;
    }

    protected sayHi() {
        console.log('hi');
    }
}

class Developer extends Person {
    constructor(name: string, skill: string) {
        super(name, skill);
        this.sayHi();
    }

    coding(): void {
        console.log('fun doing' + this.skill + 'by' + this.name);
    }
}

let hyeon = new Developer('은현', '프론트엔드');
hyeon.coding();  // fun doing 프론트엔드 by 은현
```

다음 코드는 문제없이 `fun doing 프론트엔드 by 은현` 이라는 문장을 출력합니다.

</br>
</br>

그 이유는 TypeScript의 작동 원리에 있습니다.

- TypeScript는 코드를 자바스크립트로 변환하기 전에 문법 검사를 수행하는 역할을 합니다.
- JavaScript로 변환되면 `private` 는 사라지게 됩니다.
- 실행은 JvaScript가 하기에 에러 없이 값이 출력됩니다.

즉, TypeScript는 에러를 보여줄뿐 실제 실행은 JavaScript가 수행하기에 실행시 에러가 발생하지 않습니다.

</br>
</br>

만약 런타임에서도 완벽하게 외부 접근을 차단하고 싶다면, 최신 자바스크립트 문법인 **Private Fields** `#` 키워드를 사용하면 됩니다.

`#` 을 사용하면 자바스크립트 엔진  레벨에서 강제로 접근을 막기 때문에 실행 자체가 불가능해집니다.

```tsx
class Person {
    #name: string;
    #skill: string;

    constructor(name: string, skill: string) {
        this.#name = name;
        this.#skill = skill;
    }

    protected sayHi() {
        console.log('hi');
    }
}

class Developer extends Person {
    constructor(name: string, skill: string) {
        super(name, skill);
        this.sayHi();
    }

    coding(): void {
        console.log('fun doing' + this.#skill + 'by' + this.#name);
    }
}

let hyeon = new Developer('은현', '프론트엔드');
hyeon.coding();
```

`pirvate` 대신에 `#` 키워드를 붙여줍니다.

</br>
</br>

<img width="1288" height="440" alt="image" src="https://github.com/user-attachments/assets/b65eb66f-cf4a-48bf-a571-35df93eb90ac" />


`#` 키워드를 사용하여 정의시 컴파일에서도 에러가 발생하여 실행되지 않습니다.

</br>
</br>
</br>
</br>

### 필드 생략

TypeScript에서는 생성자의 매개변수에 `public`, `private`, `protected` 같은 접근 제어자를 사용할 수 있습니다.

```tsx
constructor(public name: string, public skill: string) {
	this.name = name;
	this.skill = skill;
}
```

</br>
</br>

해당 문법을 사용하면 TypeScript가 자동으로 클래스 필드를 생성하고, 생성자에서 해당 값 할당까지 처리합니다.

해당 기능을 매개변수 프로퍼티라고 합니다.

```tsx
class Person {
    constructor(public name: string, public skill: string) {
        // this.name = name; 생략 가능
    }
}

let hyeon = new Person('은현', '프론트엔드');

console.log(hyeon); // Person { name: '은현', skill: '프론트엔드' }
```

따라서 프로퍼티 선언과 값 할당을 직접 작성하지 않아도, 프로퍼티  생성과 초기화가 모두 자동으로 처리되어 정상적으로 동작합니다.

</br>
</br>
</br>
</br>
