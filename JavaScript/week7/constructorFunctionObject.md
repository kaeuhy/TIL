### Object 생성자 함수

`new` 연산자와 함께 `Object` 생성자 함수를 호출하면 빈 객체를 생성하여 반환함

빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있음

```tsx
const person = new Object();

person.name = 'Lee';
person.sayHello = function () {
	console.log('Hi! My name is ' + this.name);
};

console.log(person);
person.sayHello();
```

</br>

자바스크립트는 `Object` 생성자 함수 이외에도 `String` , `Number` , `Boolean` , `Function` , `Array` , `Date` , `RegExp` , `Promise` 등의 빌트인 생성자 함수를 제공함

이 생성자 함수들도 `new` 연산자와 함께 호출하면 인스턴스를 생성함

```tsx
const strObj = new String("Lee");
const numObj = new Number(123);
const boolObj = new Boolean(true);
const arr = new Array(1, 2, 3);
const date = new Date();
```

만약 `new` 연산자를 사용하지 않으면 객체를 생성하지 않고 기본 값을 반환하거나 다른 역할을 수행함

</br>
</br>

### 생성자 함수에 의한 객체 생성 방식의 장점

생성자 함수에 의한 객체 생성 방식은 마치 인스턴스를 생성하기 위한 클래스처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있음

이때, 생성자 함수로 사용할 수 없는 함수는 non-constructor 함수임

→ 화살표 함수, 메서드

```tsx
function Circle(radius) {
	this.radius = redius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);
```

</br>
</br>

### 생성자 함수의 인스턴스 생성 과정

생성자 함수는 인스턴스를 생성하고 생성된 인스턴스를 초기화함

이때 인스턴스를 생성하는 것은 필수이지만 초기화하는 것은 선택임

</br>

위에 코드를 보면 `Circle` 함수 내부에 인스턴스를 생성하고 반환하는 코드는 보이지 않음

원래라면 다음과 같이 코드를 작성해야함

```tsx
function Circle(radius) {
  // 빈 객체를 생성하고 this가 빈 객체를 참조
	const this = {};
	// this가 빈 객체를 가리킴
	this.radious = radius;
	
	this.getDiameter = function() {
		return 2 * this.radius;
	};
	
	// 객체 반환
	return this;
}
```

하지만 생성자 함수에서는 이러한 코드를 작성하지 않음

`new` 연산자와 함께 생성자 함수를 호출하면 자바스크립트 엔진이 인스턴스 생성과 반환을 암묵적으로 처리해주기 때문임

</br>

```tsx
const circle = new Circle(5);
```

만약 다음과 같이 실행하면 자바스크립트 엔진은 내부적으로 다음과 같은 과정을 거침

</br>

먼저 생성자 함수의 본문이 실행되기 전에 빈 인스턴스를 생성함

그 다음 생성한 인스턴스를 생성자 함수 내부의 `this` 에 바인딩함

그렇기에 생성자 함수가 실행되는 동안 `this` 는 방금 생성한 인스턴스를 참조하게 됨

</br>

이제 생성자 함수의 코드가 한 줄씩 실행됨

```tsx
this.radius = radius;

this.getDiameter = function () {
  return 2 * this.radius;
};
```

</br>

위에서 `this` 는 이미 새롭게 생성된 인스턴스를 참조하고 있으므로 실제로는 다음과 같은 코드가 실행되는 것과 같음

```tsx
인스턴스.radius = radius;

인스턴스.getDiameter = function () {
  return 2 * 인스턴스.radius;
};
```

즉, 생성자 함수의 역할은 새로운 객체를 만드는 것이 아니라 이미 생성된 객체를 초기화하는 것임

</br>

모든 코드의 실행이 끝나면 자바스크립트 엔진은 마지막으로 `this` 를 암묵적으로 반환하기에 `return` 문을 작성하지 않아도 내부적으로 다음과 같은 동작이 수행됨

```tsx
function Circle(radius) {
  // 빈 객체 생성
  // this가 빈 객체를 참조하도록 바인딩

  this.radius = radius;

  // 암묵적으로 this 반환
  return this;
}
```

</br>

다만 생성자 함수에서 `return` 문을 명시적으로 작성하면 암묵적인 `this` 반환 규칙이 달라질 수 있음

객체를 반환하면 암묵적인 `this` 반환은 무시되고 명시적으로 반환한 객체가 반환됨

```tsx
function Circle(radius) {
	this.radius = radius;
	
	return {};
}

const circle = new Circle(5);

console.log(circle);  // {}
```

</br>

반대로 원시값을 반환하면 원시값은 무시되고 암묵적으로 `this` 가 반환됨

```tsx
function Circle(radius) {
  this.radius = radius;

  return 100;
}

const circle = new Circle(5);

console.log(circle); // Circle { radius: 5 }
```

</br>

여기서 중요한 점은 인스턴스를 생성하고 `this` 를 바인딩하는 암묵적인 처리는 생성자 함수가 아니라 `new` 연산자가 수행한다는 것임

즉, 일반 함수가 생성자 함수처럼 동작하는 이유도 함수 자체가 특별해서가 아니라 `new` 연산자와 함께 호출되었기 때문임

일반 함수 호출 → `[[Call]]` 이 실행, `new` 와 함께 호출 → `[[Construct]]` 이 실행됨

</br>

`new` 로 호출되었으므로 `[[Construct]]` 가 실행되어 자바스크립트 엔진이 암묵적으로 빈 인스턴스를 생성하고 `this` 를 바인딩한 뒤 함수를 실행하고 마지막에 `this` 를 반환함

```tsx
new foo();
```

이러한 이유 때문에 앞서 다룬 `Number` , `String` 등의 빌트인 함수도 `new` 없이 호출시 일반 함수로 실행되는 것임

예를 들어 `Number` 는 숫자로 형 변환한 원시값을 반환하도록 구현되어있는 함수이자 `[[Construct]]` 도 가지고 있는 constructor 함수임

</br>