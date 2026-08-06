### Static 메서드

`static` 키워드를 붙인 메서드는 인스턴스가 아닌 클래스 자체에 속하는 메서드임

```tsx
class User {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  static fromBirthYear(name, birthYear) {
    const age = new Date().getFullYear() - birthYear;
    return new User(name, age);
  }
}

const user = User.fromBirthYear("Kim", 2000);
```

그렇기에 `static` 메서드는 인스턴스가 아닌 클래스에서 호출함

<br/>
<br/>

### Static Factory Method

객체를 생성하는 책임을 `constructor` 대신 `static` 메서드에게 맡기는 패턴임

기존 방식은 생성자를 직접 호출했음

```tsx
const user = new User("Kim", 26);
```

<br/>

Static Factory Method 패턴을 사용하면 다음과 같이 사용함

```tsx
const user = User.fromBirthYear("Kim", 2000);
```

즉, `new` 를 사용하는 책임을 클래스 내부로 숨기는 것임

실제로는 `fromBirthYear()` 내부에서 `new User(…)` 를 호출하지만, 외부에서는 이를 알 필요가 없음

<br/>
<br/>

### 왜 사용하는가?

생성자는 이름을 붙일수가 없지만 Static Factory Method는 이름을 가질 수 있음

```tsx
User.fromBirthYear("Kim", 2000);
User.fromJson(json);
User.create(data);
```

메서드 이름만 봐도 어떤 방식으로 객체를 생성하는지 알 수 있음

<br/>

다음은 생성 로직을 클래스 내부로 숨길 수 있음

`new` 만 사용하는 경우에는 호출하는 쪽이 생성 과정을 알아야함

```tsx
const age = new Date().getFullYear() - birthYear;
const user = new User("Kim", age);
```

즉, 객체를 생성하는 곳마다 나이를 계산하고 `new User()` 를 호출하는 동일한 로직이 반복될 수 있음

<br/>

반면 Static Factory Method를 사용하면 다음과 같이 할 수 있음

```tsx
class User {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  static fromBirthYear(name, birthYear) {
    const age = new Date().getFullYear() - birthYear;
    return new User(name, age);
  }
}

const user = User.fromBirthYear("Kim", 2000);
```

호출하는 쪽은 출생연도만 전달하면 되고, 나이를 계산하는 로직은 모두 `User` 클래스 내부에서 처리됨

즉, 객체 생성과 관련된 로직을 한 곳에서 관리할 수 있음

<br/>

그다음은 객체 생성 책임을 한 곳으로 모을 수 있음

Static Factory Method를 사용하면 다음과 같이 `User` 를 생성하는 모든 방법이 `User` 클래스 안에 모임

```tsx
class User {
	static fromBirthYear(...) {}
	static fromJson(...) {}
	static fromDatabase(...) {}
}
```

즉, 객체 생성 책임을 클래스 내부로 캡슐화할 수 있음

<br/>

마지막으로 생성 방식이 변경되어도 사용하는 코드는 변경되지 않음

```tsx
static create(data) {
	if (data.role === "admin") {
		return new AdminUser(data);
	}
	
	return new User(data);
}

// 메서드 내부 로직이 바뀌어도 사용하는 코드는 그대로 유지됨
const user = User.create(data);
```

즉, 객체 생성 방식이 변경되더라도 사용하는 코드는 영향을 받지 않음

<br/>