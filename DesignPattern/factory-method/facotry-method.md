</br>
</br>

### 팩토리 메서드를 들어가기전 팩토리 패턴이란?

일반적으로 코드에서 특정 객체의 인스턴스가 필요할 때 `new` 연산자를 직접 사용합니다. 하지만 이렇게 하면 코드가 구상 클래스에 직접 의존하게 됩니다.

만약 새로운 클래스를 추가하거나 기존 클래스를 변경해야 한다면, `new` 연산자를 사용한 모든 코드를 찾아 수정해야 합니다. 이는 수정에는 닫혀있고, 확장에는 열려있어야 한다는 OCP 원칙을 위배하며 유연성을 떨어뜨립니다.

</br>

팩토리 패턴은 이러한 객체 생성 로직을 별도의 공장인 팩토리로 분리하여, 클라이언트 코드가 구상 클래스가 아닌 인터페이스에만 의존하도록 만듭니다.

정리하자면 해당 패턴의 주된 목적은 클라이언트로부터 구상 클래스의 인스턴스를 만드는 코드를 분리하는 것입니다.

</br>
</br>

**Bad Pattern - Pizza**

`PizzaStore` 클래스가 피자를 주문받아 처리하는 `orderPizza` 메서드를 가지고 있다고 가정합니다.

새로운 피자가 추가될수록 `if/else` 문을 사용하여 상황에 맞는 객체를 생성합니다.

```tsx
class PizzaStore {
	// orderPizza 메서드
  public orderPizza(type: string): Pizza {
    let pizza: Pizza;

    if (type === "cheese") {
      pizza = new CheesePizza(); // 
    } else if (type === "pepperoni") {
      pizza = new PepperoniPizza();
    } else if (type === "clam") {
      pizza = new ClamPizza(); // 새로운 피자
    } else if (type === "veggie") {
      pizza = new VeggiePizza(); // 새로운 피자
    }

    pizza.prepare();
    pizza.bake();
    pizza.cut();
    pizza.box();

    return pizza;
  }
}
```

해당 코드는 다음과 같은 문제점이 발생합니다.

- **강한 결합**:
    - `PizzaStore` 클래스가 `CheesePizza`, `PepperoniPizza` 등 구상 클래스의 이름을 직접 알아야 합니다.
    - 이는 상위 모듈은 하위 모듈에 의존해서는 안 된다, 즉 추상화에 의존하라는 의존성 뒤집기 원칙에 위배됩니다.
- **OCP 위반**:
    - `VeggiePizza` 라는 신메뉴가 추가되면, `PizzaStore` 클래스의 `orderPizza` 메서드 내부 if 문을 직접 수정해야 합니다. 이는 확장에 열려 있어야 하고, 수정에는 닫혀 있어야 한다는 개방-폐쇠 원칙에 위배됩니다.

</br>
</br>

**객체 생성 캡슐화**

팩토리 패턴은 이러한 문제점을 캡슐화를 통해 해결합니다.

바뀌는 부분, 객체 생성 로직인 `orderPizza` 메서드 내부 `if` 문을 찾아내서 바뀌지 않는 부분과 분리합니다.

객체 생성 로직을 `PizzaStore` 에서 떼어내어, 객체 생성만을 전담하는 별도의 `Factory` 로 나눕니다.

</br>
</br>

**Good Pattern - Pizza**

바뀌는 부분을 찾아내서 바뀌지 않는 부분과 분리한다는 원칙에 따라, 객체를 생성하는 코드를 `SimplePizzaFactory`라는 별도의 클래스로 캡슐화합니다.

먼저 모든 피자가 따를 공통 인터페이스와 실제 구상 클래스를 정의합니다.

```tsx
// 모든 피자가 구현할 공통 인터페이스
export interface Pizza {
  name: string;
  prepare(): void;
  bake(): void;
  cut(): void;
  box(): void;
}
```
</br>

```tsx
// CheesePizza 구상 클래스
export class CheesePizza implements Pizza {
  name = "Cheese Pizza";
  
  prepare(): void { console.log("Preparing Cheese Pizza"); }
  bake(): void { console.log("Baking Cheese Pizza"); }
  cut(): void { console.log("Cutting Cheese Pizza"); }
  box(): void { console.log("Boxing Cheese Pizza"); }
}
```

</br>
</br>

객체 생성을 전담하는 팩토리 클래스를 정의합니다.

```tsx
import { Pizza, CheesePizza, PepperoniPizza } from "../products";

export class SimplePizzaFactory {
  public createPizza(type: string): Pizza | null {
    let pizza: Pizza | null = null;

    if (type === "cheese") {
      pizza = new CheesePizza();
    } else if (type === "pepperoni") {
      pizza = new PepperoniPizza();
    }

    return pizza;
  }
}
```

새로운 피자가 추가되면  해당 클래스만 수정하면 됩니다.

</br>

팩토리 클래스인 `SimplePizzaFactory.ts`를 사용하는 `PizzaStore` 클라이언트를 정의합니다.

```tsx
import { SimplePizzaFactory } from "./factory/SimplePizzaFactory";
import { Pizza } from "../products/Pizza";

export class PizzaStore {
	// 구상 팩토리 클래스에 의존합니다.
  private factory: SimplePizzaFactory;

  constructor(factory: SimplePizzaFactory) {
    this.factory = factory;
  }

  // 주문 메서드
  public orderPizza(type: string): Pizza | null {
    const pizza = this.factory.createPizza(type);

    if (!pizza) {
      console.log("Sorry, we don't have that pizza.");
      return null;
    }

    pizza.prepare();
    pizza.bake();
    pizza.cut();
    pizza.box();

    return pizza;
  }
}
```
</br>
</br>

**팩토리 패턴의 장점**

- **결합도 완화**:
    - 클라이언트인 `PizzaStore` 는 더 이상 `new CheesePizza()`를 호출하지 않고, `factory.createPizza(type)`을 호출하여 결합도를 완화합니다.
    - 클라이언트는 자신이 주문한 피자가 `Pizza` 인터페이스를 따른다는 것 외에는, 구체적으로 어떤 클래스인지 알 필요가 없어집니다.
- **OCP 준수**:
    - 신메뉴가 추가되어도, `PizzaStore` 의 `orderPizza` 코드는 수정할 필요가 없습니다.
    - 오직 팩토리 클래스 내부의 `createPizza` 메서드에 `else if` 문을 추가하면 됩니다.
    - 클라이언트인 `PizzaStore` 코드는 수정에 닫혀 있고, 팩토리는 확장에 열려있게 됩니다.
- **단일 책임 원칙**:
    - 클라이언트인 `PizzaStore` 는 피자를 주문받아 절차대로 처리하는 단일 책임만 맡습니다.
    - 어떤 피자를 만들지 결정하고 생성하는 책임은 `Factory`가 맡습니다.
    - 각 클래스가 단 하나의 책임을 갖게 되어 코드가 명확해집니다.

</br>
</br>

**팩토리 패턴의 3가지 주요 형태**

객체 생성 캡슐화라는 목표를 달성하기 위한 3가지 다른 접근 방식이 존재합니다.

- **간단한 팩토리**:
    - 객체 생성 로직을 별도의 팩토리 클래스로 분리합니다. 클라이언트는 팩토리의 인스턴스를 주입받아 사용합니다.
    - 클라이언트가 구상 팩토리에 의존하게 된다는 단점이 존재합니다.
- **팩토리 메서드 패턴**:
    - 클라이언트를 `abstract` 클래스로 만들고, 해당 클래스안에 팩토리 메서드를 선언합니다.
    - 상속을 통해 서브클래스가 객체 생성을 결정합니다.
- **추상 팩토리 패턴**:
    - 관련된 객체들의 묶음인 제품군을 생성하는 인터페이스를 정의합니다.
    - 클라이언트가 팩토리 인스턴스를 주입받아 사용합니다.
    - 제품군을 생성하는 데 특화되어 있습니다.

</br>
</br>

### 팩토리 메서드 패턴

간단한 팩토리는 유용하지만, `PizzaStore` 가 `SimplePizzaFacotry` 라는 구상 팩토리 클래스에 의존한다는 한계가 있습니다.

만약 새로운 여러 지점이 생기고, 각 지점마다 자신만의 스타일이 적용된 피자를 만들어야 한다면 문제점이 생깁니다.

하지만 `orderPizza` 의 핵심 절차를 모든 지점이 동일하게 유지해야합니다.

</br>

**팩토리 메서드 패턴의 핵심**

객체를 생성할 때 필요한 인터페이스를 만들고 어떤 클래스의 인스턴스를 만들지는 서브클래스에서 결정합니다.

팩토리 메서드 패턴을 사용하면 클래스 인스턴스 만드는 일을 서브클래스에게 맡기게 됩니다.

팩토리 메서드 패턴의 핵심은 상속을 사용한다는 점입니다.

</br>

**주요 구성 요소**

- `Product - Pizza`:
    - 팩토리 메서드가 생성할 객체의 공통 인터페이스 또는 추상 클래스입니다.
- `ConcreteProduct` **- 여러 스타일의 피자**:
    - `Product` 를 상속또는 구현하여 실제 객체를 정의한 클래스입니다.
- `Creator - PizzaStore`:
    - `Product` 를 생성하기 위한 팩토리 메서드 `createPizza` 를 `abstract` 로 선언한 추상 클래스입니다.
    - 공통 로직인 `orderPizza` 를 구현하며, 내부에서 팩토리 메서드를 호출해 구체 객체를 생성합니다.
- `ConcreteCreator` **- 각 지점 Store**:
    - `PizzaStore` 를 상속받아 팩토리 메서드 `createPizza` 를 구체적으로 구현하는 클래스입니다.
    - 각 지점의 피자 객체를 실제로 생성합니다.

</br>

`abstract` 키워드는 TypeScript에서 미완성 또는 뼈대만 있음을 의미하는 키워드를 말합니다.

- `abstract` 추상 클래스:
    - `new` 연산자를 사용하여 직접 인스턴스를 만들 수 없습니다.
    - 오직 다른 클래스가 추상 클래스를 상속하기 위한 부모 클래스로만 사용됩니다.
- `abstract` 추상 메서드:
    - `abstract` 클래스를 상속받는 자식 클래스는 부모가 가진 모든 `abstract` 메서드를 반드시 구현해야만 합니다.

</br>
</br>

**Factory Method Pattern - Pizza**

공통 로직을 슈퍼클래스인 추상 클래스에 구현합니다.

```tsx
// 제품의 인터페이스 역할을 하는 추상 클래스
export abstract class Pizza {

    protected constructor(protected name: string, protected dough: string, protected sauce: string, protected toppings: string[] = []) {
        this.name = name;
        this.dough = dough;
        this.sauce = sauce;
        this.toppings = toppings;
    }

    // 공통 로직인 피자 주문
    prepare(): void {
        console.log(`Preparing ${this.name}`);
        console.log("Tossing dough...");
        console.log("Adding sauce...");
        console.log("Adding toppings: ");
        this.toppings.forEach(topping => console.log(`  ${topping}`));
    }

    bake(): void {
        console.log("Bake for 25 minutes at 350");
    }

    cut(): void {
        console.log("Cutting the pizza into diagonal slices");
    }

    box(): void {
        console.log("Place pizza in official PizzaStore box");
    }

    public getName(): string {
        return this.name;
    }
}
```

</br>
</br>

각 지점에 맞는 피자들을 구현해줍니다.

```tsx
// 실제로 만들어지는 구체적인 뉴욕 스타일 피자
import {Pizza} from "./Pizza";

export class NYStyleCheesePizza extends Pizza {
    constructor() {
        // 뉴욕 스타일 속성을 인자로 넣어 부모 클래스에게 전달합니다.
        super(
            "NY Style Sauce and Cheese Pizza",
            "This Crust Dough",
            "Marinara Sauce",
        );

        this.toppings.push("Grated Reggiano Cheese");
    }
}
```

</br>

```tsx
// 실제로 만들어지는 구체적인 시카고 스타일 피자
import {Pizza} from "./Pizza";

export class ChicagoStyleCheesePizza extends Pizza {
    constructor() {
        // 시카고 스타일 속성을 인자로 넣어 부모 클래스에게 전달합니다.
        super(
            "Chicago Style Deep Dish Cheese Pizza",
            "Extra Thick Crust Dough",
            "Plum Tomato Sauce"
        );

        this.toppings.push("Shredded Mozzarella Cheese");
    }

    // 부모 클래스의 cut 메서드를 시카고 스타일의 방식으로 재정의합니다.
    cut(): void {
        console.log("Cutting the pizza into square slices");
    }
}
```

</br>
</br>

`PizzaStore`의 추상 클래스를 구현합니다.

```tsx
import {Pizza} from "../products/Pizza";

export abstract class PizzaStore {
    public orderPizza(type: string): Pizza {
        const pizza = this.createPizza(type);
        console.log(`--- Making a ${pizza.getName()} ---`);

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();

        return pizza;
    }

    // 아직 createPizza 구현 안함, 추상 메서드임
    protected abstract createPizza(type: string): Pizza;
}
```

</br>
</br>

팩토리 메서드를 실제로 구현하는 구상 생성자들을 구현합니다.

```tsx
// 어떤 피자를 만들지 구체적으로 결정
import {PizzaStore} from "./PizzaStore";
import {Pizza} from "../products/Pizza";
import {NYStyleCheesePizza} from "../products/NYStyleCheesePizza";

export class NYPizzaStore extends PizzaStore {
    protected createPizza(item: string): Pizza {
        if (item === "cheese") {
            return new NYStyleCheesePizza();
        } // else if 다른 스타일의 피자 ...
        else {
            throw new Error("Unknown pizza type");
        }
    }
}
```

</br>

```tsx
// 어떤 피자를 만들지 구체적으로 결정
import {ChicagoStyleCheesePizza} from "../products/ChicagoStyleCheesePizza";
import {Pizza} from "../products/Pizza";
import {PizzaStore} from "./PizzaStore";

export class ChicagoPizzaStore extends PizzaStore {
    protected createPizza(item: string): Pizza {
        if (item === "cheese") {
            return new ChicagoStyleCheesePizza();
        } // else if 다른 스타일의 피자 ...
        else {
            throw new Error("Unknown pizza type");
        }
    }
}
```

</br>
</br>

**Good Pattern - Guru**

`Product`의 공통 규격을 정의합니다.

```tsx
interface Produck {
	operation(): string;
}

class ConcreteProduct1 implements Product {
  public operation(): string {
    return '{ConcreteProduct1의 결과}';
  }
}

class ConcreteProduct2 implements Product {
  public operation(): string {
    return '{ConcreteProduct2의 결과}';
  }
 }
```

- `interface Produck`:
    - 공장이 만들어낼 제품의 공통 규격입니다. 여기서는 모든 제품이 `operation()` 메서드를 가져야 한다고 정의합니다.
- `class`:
    - `Produck` 인터페이스에 맞춰 실제로 만들어진 제품들입니다.

</br>
</br>

`Creator` 부분인 비즈니스 로직과 팩토리를 선언합니다.

`abstract` 키워드는 TypeScript에서 미완성 또는 뼈대만 있음을 의미하는 키워드를 말합니다.

- `abstract` 추상 클래스:
    - `new` 연산자를 사용하여 직접 인스턴스를 만들 수 없습니다.
    - 오직 다른 클래스가 추상 클래스를 상속하기 위한 부모 클래스로만 사용됩니다.
- `abstract` 추상 메서드:
    - `abstract` 클래스를 상속받는 자식 클래스는 부모가 가진 모든 `abstract` 메서드를 반드시 구현해야만 합니다.

</br>
</br>

```tsx
abstract class Creator {
	public abstract factoryMethod(): Product;
	
	public someOperation(): string {
		const product = this.factoryMethod();
		return `Creator: 동일한 생성자의 코드가 ${product.operation()}로 작동했습니다.`;
	}
}
```

- `factoryMethod()`:
    - 팩토리 메서드로 `abstract`로 선언하여 자식 클래스는 해당 메서드를 반드시 구현해야 한다고 강제합니다.
- `someOperation()`:
    - `new` 키워드를 사용하지않고 `this.factoryMethod()`를 호출하며 제품을 가져옵니다.
    - 어떤 제품이 생성되는지 알 필요 없이, `Product` 타입의 객체를 사용하여 자신의 로직을 수행합니다.

</br>
</br>

실제 생성 로직인 `ConcreteCreator`를 구현합니다.

```tsx
class ConcreteCreator1 extends Creator {
	public factoryMethod(): Product {
		return new ConcreateProduct1();
	}
}

class ConcreteCreator2 extends Creator {
	public factoryMethod(): Product {
		return new ConcreteProduct2();
	}
}
```

- `ConcreteCreator`:
    - 공장의 실제 구현체 자식 클래스들입니다.
- `facotoryMethod()`:
    - 부모인 `Creator` 가 `abstract` 로 선언했던 팩토리 메서드를 재정의 합니다.
    - 안좋은 패턴에 있었던 `if` 문 로직이 `Creator` 의 비즈니스 로직에서 완전히 분리되어, 각 자식 클래스로 나누어졌습니다.

</br>
</br>

`Client` 부분으로 생성자의 인터페이스만 사용합니다,

```tsx
function clientCode(creator: Creator) {
	console.log('클라이언트: 생성자의 클래스를 모르지만, 여전히 작동합니다.');
  console.log(creator.someOperation());
  // 다른 클라이언트 로직이 올 수 있습니다.
 }
 
 console.log('앱: ConcreteCreator1로 실행되었습니다.');
clientCode(new ConcreteCreator1());

console.log('');

console.log('앱: ConcreteCreator2로 실행되었습니다.');
clientCode(new ConcreteCreator2());
```

- `clientCode`:
    - 클라이언트 함수는 부모인 `Creator` 타입으로 받습니다.
- `new ConcreteCreator1()`이 전달:
    - `someOperation`은 `ConcreteCreator1`의 `factoryMethod`를 호출하여 `ConcreteProduct1`을 사용합니다.
- `new ConcreteCreator2()`가 전달:
    - `someOperation`은 `ConcreteCreator2`의 `factoryMethod`를 호출하여 `ConcreteProduct2`를 사용합니다.

</br>
</br>

**장점**

- **개방-폐쇄 원칙 준수**:
    - `PizzaStore` 의 핵심 공통 로직인 `orderPizza()` 메서드는 한 번 작성된 후 수정할 필요가 없습니다.
    - 또 다른 지점을 새로 추가하고 싶을 때, 기존 코드를 건드리지 않고, `PizzaStore` 를 상속받는 새로운 클래스를 추가하면 되어 확장에 열려있습니다.
- **의존성 뒤집기 원칙 준수**:
    - 모든 것이 추상화에 의존하게 되어, 구상 클래스 간의 강한 결합이 사라집니다.
- **생성 로직의 캡슐화 및 단일 책임 원칙**:
    - 객체 생성 책임을 분리합니다.
    - 각 클래스가 하나의 명확한 책임을 갖게 되어 코드를 이해하고 유지보수하기 쉬워집니다.
- **병렬 계층구조 지원**:
    - 생성자 계층구조와 제품 계층구조를 병렬로 가져갈 수 있게 해줍니다.
    - 생성자의 서브클래스와 제품의 서브클래스가 1:1로 매칭되는 구조를 깔끔하게 구현할 수 있습니다.

**단점**

- **클래스 복잡도 증가**:
    - 새로운 제품마다 새로운 클래스가 필요하여 클래스 복잡도가 증가합니다.
    - 제품군이 10개라면, `PizzaStore` 를 상속받는 10개의 구상 스토어 클래스가 필요합니다.
- **상속에 의존**:
    - 해당 패턴은 클래스 상속을 기반으로 하여 객체 구성에 비해 유연성이 떨어집니다.
- **슈퍼클래스와 서브클래스의 강한 연결**:
    - 슈퍼클래스가 공통 로직을 실행하지만, 해당 로직은 서브클래스가 반드시 구현해야만 완성됩니다.
    - 슈퍼클래스는 서브클래스 없이는 스스로 인스턴스화될 수 없습니다.

</br>
</br>