### class의 get, set

`class`에서 프로퍼티를 정의할 때 클래스 내부에서만 관리해야 하는 상태라면 `#private` 프로퍼티로 숨기고, 외부에서 필요한 경우 `get`과 `set`을 통해 접근하도록 만들 수 있음.

일반적인 프로퍼티는 다음과 같이 외부에서 직접 값을 변경할 수 있음

```tsx
class Person {
  constructor(name) {
    this.name = name;
  }
}

const person = new Person('Lee');

console.log(person.name); // Lee

person.name = 'Kim';

console.log(person.name); // Kim
```

`person.name` 에 접근하면 값을 바로 읽을 수 있고, `person.name = “Kim”` 사용시 값을 변경할 수 있음

<br/>

하지만 `#` 을 사용해 클래스 내부에서만 관리해야 하는 상태라면 외부에서 직접 접근할 수 없음

이때 외부에서 값을 읽거나 변경할 필요가 있다면 `get` , `set` 을 함께 사용할 수 있음

```tsx
class Person {
  #name;
  
  constructor(name) {
    this.#name = name;
  }
  
  get name() {
    return this.#name;
  }
  
  set name(value) {
    this.#name = value;
  }
}
```

<br/>

`#name` 은 실제 값을 저장하는 private 프로퍼티이고, `name` 은 외부에서 접근할 때 사용하는 프로퍼티임

```tsx
const person = new Person('Lee');

console.log(person.name); // Lee

person.name = 'Kang';

console.log(person.name); // Kang
```

여기서 `get name()` 은 `person.name` 에 접근할 때 실행되고, `set name(value)` 는 `person.name = ‘Kang’` 처럼 값을 할당할 때 실행됨

<br/>

`set` 을 처음 보면 `value` 라는 매개변수가 있기 때문에 다음처럼 사용해야 할 것처럼 보일 수 있음

```tsx
// 잘 못된 방식
person.name('Kang');

// 옳은 방식
person.name = 'Kang';
```

하지만 `set` 은 일반 메서드가 아니라 프로퍼티에 값을 할당할 때 실행되는 `setter` 이므로 함수 호출 방식으로 사용하지 않음

즉, `=` 오른쪽의 `‘Kang’` 이 `set name(value)` 의 `value` 에 전달됨

<br/>

반면 일반 메서드는 직접 함수를 호출해야 함

```tsx
name(value) {
  this.#name = value;
}

person.name('Kang');
```

<br/>

`person.name = ‘Kang’` 만으로도 값을 변경할 수 있는데 `set` 왜 정의해야하는지 의문이 들 수 있음

`set` 의 핵심적인 목적은 값을 넣는 문법을 바꾸는 것이 아니라, 값을 할당하는 순간에 추가적인 로직을 실행할 수 있도록 하는 것임

```tsx
class Person {
  #name;
  
  constructor(name) {
    this.#name = name;
  }
  
  get name() {
    return this.#name;
  }
  
  set name(value) {
    if (typeof value !== 'string') {
      throw new TypeError('이름은 문자열이어야 합니다.');
    }
    
    this.#name = value;
  }
}

const person = new Person('Lee');

person.name = 'Kang';
```

따라서 `set` 을 사용하면 외부에서는 일반 프로퍼티처럼 값을 할당하면서도, 내부에서는 검증이나 가공 등의 로직을 적용할 수 있음

특히 `#private` 과 함께 사용하면 외부에서 실제 상태인 `#name` 을 직접 변경할 수 있고, setter를  통해서만 변경하도록 만들 수 있다는 장점이 있음

<br/>