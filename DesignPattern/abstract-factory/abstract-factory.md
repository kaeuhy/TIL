</br>
</br>

### 추상 팩토리 패턴

팩토리 메서드 패턴으로 지점별 피자 스타일은 해결했지만, 지점마다 사용하는 원재료를 통제하고 싶다고 가정합니다.

즉, 연관된 객체들의 묶음을 한 번에 관리하고 교체할 필요가 생겼습니다.

</br>
</br>

**추상 팩토리  패턴의 핵심**

추상 팩토리 패턴은 구상 클래스에 의존하지 않고도 서로 연관되거나 의존적인 객체로 이루어진 제품군을 생성하는 인터페이스를 제공합니다.

정리하자면 관련된 제품군을 생성하는 팩토리를 만드는 방법을 말합니다.

</br>
</br>

**제품군 - Product Family**

팩토리 메서드 패턴이 단일 객체인 `Pizza`를 만드는 방법을 서브클래스에 위임했다면, 추상 팩토리 패턴은 여러 객체의 묶음(`Dough`, `Sauce`, `Cheese`)을 만드는 방법을 다룹니다.

</br>

피자 예제에서 제품군은 각 지역 스타일을 구성하는 원재료 묶음입니다.

- **뉴욕 지점**:
    - `ThinCrustDough`, `MarinaraSauce`, `ReggianoCheese`
- **시카고 지점**:
    - `ThickCrustDough`, `PlumTomatoSauce`, `MozzarellaCheese`

추상 팩토리 패턴의 목표는, 클라이언트가 각 지점 스타일의 재료 팩토리를 주입받으면 해당 재료만 사용하도록 보장하는 것입니다.

</br>
</br>

**주요 구성 요소**

- **추상 팩토리 -** `AbstractFactory`:
    - 제품군을 생성하기 위한 인터페이스로 제품군의 모든 부품을 생성하는 추상 메서드를 선언합니다.
    - `createDough()`, `createSauce()`, `createCheese()`
- **구상 팩토리 -** `ConcreteFactory`:
    - 추상 팩토리 인터페이스를 상속받아 구현하여 특정한 제품군을 생성하는 실제 팩토리입니다.
- **추상 제품 -** `AbstractProduct`:
    - 제품군에 속하는 개별 부품들의 공통 인터페이스 입니다.
    - `Dough`, `Sauce`, `Cheese`
- **구상 제품 -** `ConcreteProduct`:
    - 추상 제품 인터페이스를 상속받아 구현한 부품 객체입니다.
    - `ThinCrustDough`, `MarinaraSauce`, `ThickCrustDough`, `PlumTomatoSauce`

</br>
</br>

재료에 대한 인터페이스를 정의합니다.

```tsx
// 팩토리가 생성할 각 재료의 공통 인터페이스
export interface Dough {
    toString(): string;
}

export interface Sauce {
    toString(): string;
}

export interface Cheese {
    toString(): string;
}

// 필요시 추가적인 재료 인터페이스 구현
```

</br>
</br>

정의한 실제 재료에 대한 인터페이스를 구현합니다.

```tsx
// 뉴욕 지점에 실제 재료들
import {Cheese, Dough, Sauce} from "./Interfaces";

export class ThinCrustDough implements Dough {
    toString = () => "Thin Crust Dough";
}

export class MarinaraSauce implements Sauce {
    toString = () => "Marinara Sauce";
}

export class ReggianoCheese implements Cheese {
    toString = () => "Reggiano Cheese";
}

```

</br>

```tsx
// 시카고 지점에 실제 재료들
import {Cheese, Dough, Sauce} from "./Interfaces";

export class ThickCrustDough implements Dough {
    toString = () => "Extra Thick Crust Dough";
}

export class PlumTomatoSauce implements Sauce {
    toString = () => "Plum Tomato Sauce";
}

export class MozzarellaCheese implements Cheese {
    toString = () => "Shredded Mozzarella Cheese";
}
```

</br>
</br>

연관된 재료, 객체군을 생성하는 인터페이스를 정의합니다.

```tsx
// 연관된 재료 묶음을 생성하기 위한 인터페이스
import {Cheese, Dough, Sauce} from "../products/ingredients/Interfaces";

export interface PizzaIngredientFactory {
    createDough(): Dough;

    createSauce(): Sauce;

    createCheese(): Cheese;
}
```

</br>
</br>

각 지점 제품군에 맞는 재료들을 생성하여 반환하는 구상 팩토리를 구현합니다.

```tsx
// 뉴욕 지점 제품 묶음에 맞는 재료를 생성하는 실제 공장
import {PizzaIngredientFactory} from "./PizzaIngredientFactory";
import {Cheese, Dough, Sauce} from "../products/ingredients/Interfaces";
import {MarinaraSauce, ReggianoCheese, ThinCrustDough} from "../products/ingredients/NYIngredients";

export class NYPizzaIngredientFactory implements PizzaIngredientFactory {
    public createDough(): Dough {
        return new ThinCrustDough();
    }

    public createSauce(): Sauce {
        return new MarinaraSauce();
    }

    public createCheese(): Cheese {
        return new ReggianoCheese();
    }
}
```

</br>

```tsx
// 시카고 지점 제품 묶음에 맞는 재료를 생성하는 실제 공장
import {PizzaIngredientFactory} from "./PizzaIngredientFactory";
import {Cheese, Dough, Sauce} from "../products/ingredients/Interfaces";
import {MozzarellaCheese, PlumTomatoSauce, ThickCrustDough} from "../products/ingredients/ChicagoIngredients";

export class ChicagoPizzaIngredientFactory implements PizzaIngredientFactory {
    public createDough(): Dough {
        return new ThickCrustDough();
    }

    public createSauce(): Sauce {
        return new PlumTomatoSauce();
    }

    public createCheese(): Cheese {
        return new MozzarellaCheese();
    }
}
```

</br>
</br>

기존에 `Pizza` 클래스는 추상 팩토리의 클라이언트가 됩니다.

```tsx
// 팩토리 메서드 패턴의 제품이자 추상 팩토리 패턴의 클라이언트
import {Cheese, Dough, Sauce} from "./ingredients/Interfaces";

export abstract class Pizza {

    // 추상 인터페이스에 의존
    protected name!: string;
    protected dough!: Dough;
    protected sauce!: Sauce;
    protected cheese!: Cheese;

    // 재료 준비 추상 메서드
    public abstract prepare(): void;

    bake(): void {
        console.log("Bake for 25 minutes at 350");
    }

    cut(): void {
        console.log("Cutting the pizza into diagonal slices");
    }

    box(): void {
        console.log("Place pizza in official PizzaStore box");
    }

    getName(): string {
        return this.name;
    }

    setName(name: string): void {
        this.name = name;
    }
}
```

</br>
</br>

`Pizza` 를 상속받는 구상 제품이자, `PizzaIngredientFactory` 를 사용하는 추상 팩토리의 클라이언트를 구현합니다.

```tsx
import {PizzaIngredientFactory} from "../factory/PizzaIngredientFactory";
import {Pizza} from "./Pizza";

export class CheesePizza extends Pizza {
    private ingredientFactory: PizzaIngredientFactory;

    public constructor(factory: PizzaIngredientFactory) {
        super();
        this.ingredientFactory = factory;
    }

    public prepare(): void {
        console.log(`Preparing ${this.name}`);
        this.dough = this.ingredientFactory.createDough(); //
        this.sauce = this.ingredientFactory.createSauce(); //
        this.cheese = this.ingredientFactory.createCheese(); //

        console.log(`  ... using ${this.dough.toString()}`);
        console.log(`  ... using ${this.sauce.toString()}`);
        console.log(`  ... using ${this.cheese.toString()}`);
    }
}
```

</br>
</br>

팩토리 메서드에 대한 인터페이스를 정의합니다.

```tsx
import {Pizza} from "../products/Pizza";

export abstract class PizzaStore {
    public orderPizza(type: string): Pizza {
        const pizza = this.createPizza(type);
        console.log(` Making a ${pizza.getName()} `);

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();

        return pizza;
    }

    protected abstract createPizza(type: string): Pizza;
}
```

</br>
</br>

정의한 팩토리 메서드에 대한 인터페이스를 구현합니다.

```tsx
import {PizzaStore} from "./PizzaStore";
import {Pizza} from "../products/Pizza";
import {PizzaIngredientFactory} from "../factory/PizzaIngredientFactory";
import {NYPizzaIngredientFactory} from "../factory/NYPizzaIngredientFactory";
import {CheesePizza} from "../products/CheesePizza";

export class NYPizzaStore extends PizzaStore {
    protected createPizza(type: string): Pizza {
        let pizza: Pizza;

        // 뉴욕 지점에 맞는 재료를 생성
        const ingredientFactory: PizzaIngredientFactory =
            new NYPizzaIngredientFactory(); // [cite: 1249]

        if (type === "cheese") {
            pizza = new CheesePizza(ingredientFactory);
            pizza.setName("New York Style Cheese Pizza"); // [cite: 1249]
        } // else if 또다른 피자
        else {
            throw new Error("Unknown pizza type");
        }

        return pizza;
    }
}
```

</br>

```tsx
import {PizzaStore} from "./PizzaStore";
import {Pizza} from "../products/Pizza";
import {ChicagoPizzaIngredientFactory} from "../factory/ChicagoPizzaIngredientFactory";
import {PizzaIngredientFactory} from "../factory/PizzaIngredientFactory";
import {CheesePizza} from "../products/CheesePizza";

export class ChicagoPizzaStore extends PizzaStore {
    protected createPizza(type: string): Pizza {
        let pizza: Pizza;

        const ingredientFactory: PizzaIngredientFactory = new ChicagoPizzaIngredientFactory();

        if (type === "cheese") {
            pizza = new CheesePizza(ingredientFactory);
            pizza.setName("Chicago Style Deep Dish Cheese Pizza");
        } // else if 다른 피자
        else {
            throw new Error("Unknown pizza type");
        }
        return pizza;

    }
}
```

</br>
</br>

**장점**

- **클라이언트와 구상 제품의 완벽한 분리:**
    - 클라이언트는 `PizzaIngredientFactory` 인터페이스에만 의존합니다.
    - `NYIngredients.ts`나 `ChicagoIngredients.ts` 파일이 존재 하는지조차 알 필요가 없습니다. 의존성 뒤집기 원칙을 준수합니다.
- **제품군 교체의 용이성:**
    - 클라이언트 코드 수정 없이 구상 팩토리만 교체하여 주입하면 됩니다.
- **제품군 간의 일관성 보장:**
    - 팩토리가 한 제품군을 책임지므로, 클라이언트가 실수로 다른 제품군의 부품을 섞어 쓰는 것을 원천적으로 방지합니다.

**단점**

- **새로운 종류의 제품 추가의 어려움:**
    - 만약 모든 피자에 다른 재료를 추가한다면 추상 팩토리에 추셍 메서서드를 추가해야 합니다.

</br>
</br>