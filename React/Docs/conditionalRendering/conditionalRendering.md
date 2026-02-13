### 조건부 렌더링

컴포넌트는 조건에 따라 다른 항목을 표시해야 하는 경우가 많습니다.

React는 `if` 문, `&&` 및 `?` `:` 연산자와 같이 자바스크립트 문법을 사용하여 조건부로 JSX를 렌더링할 수 있습니다.

</br>

짐챙김 여부에 따라 여러 개의 `Item` 을 렌더링하는 `PackingList` 컴포넌트가 있다고 가정해봅시다.

```tsx
function Item({ name, isPacked }) {
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item
          isPacked={true}
          name="Space suit"
        />
        <Item
          isPacked={true}
          name="Helmet with a golden leaf"
        />
        <Item
          isPacked={false}
          name="Photo of Tam"
        />
      </ul>
    </section>
  );
}
```

`Item` 컴포넌트 중 일부는 `isPacked` `prop` 이 `false` 가 아닌 `true` 로 설정되어 있습니다.

</br>

`isPacked={true}` 인 경우 김을 챙긴 항목에 체크 표시를 추가하려면 다음과 같이 `if`/`else` 문으로 작성할 수 있습니다.

```tsx
if (isPacked) {
	return <li className="item">{name} ✅</li>;
}
return <li className="item">{name}</li>;
```

`isPacked` `prop` 이 `true` 이면 위 코드는 다른 JSX 트리를 반환합니다.

이처럼 JavaScript의 `if` 와 `return` 문으로 분기 로직을 만들어서 React에서 제어 흐름을 JavaScript로 처리할 수 있습니다.

</br>
</br>

### 조건부로 null을 사용하여 아무것도 반환하지 앟기

어떤 경우에는 아무것도 렌더링하고 싶지 않을 수 있습니다.

다음과 같이 짐을 챙긴 항목을 전혀 보여주지 않는다고 가정할때 컴포넌트는 반드시 무언가를 반환해야 하는데 이 경우에 `null` 을 반환할 수 있습니다.

```tsx
is (isPacked) {
	return null;
}
return <li className="item">{nane}</li>;
```

`isPacked` 가 `true` 라면 컴포넌트는 아무것도 반환하지 않지만, `false` 라면 JSX가 반환될 것입니다.

</br>
</br>

### 조건부로 JSX 포함시키기

이전 예시에서는 어떤 항목을 제어하여 컴포넌트에 의해 JSX 트리가 반환되었습니다.

하지만 아래와 같이 일부 중복이 발생합니다.

```tsx
<li className="item">{name} ✅</li>

<li className="item">{name}</li>
```

두 조건부 분기가 모두 `<li className=”item”>…</li>` 를 반환합니다.

해당 중복코드가 나쁘지는 않지만, 코드를 유지 보수하기 더 어렵게 만들 수 있습니다.

`className` 을 바꾸고 싶다면 코드상 두 군데를 수정해야 합니다.

이러한 상황에서 조건부로 약간의 JSX를 포함해 코드를 덜 반복적이게 만들 수 있습니다.

</br>
</br>

### 삼항 조건 연산자 ? :

JavaScript는 조건 연산자 또는 삼항 조건 연산자라는 조건식을 작성하기 위한 간단한 문법이 있습니다.

```tsx
if (isPacked) {
  return <li className="item">{name} ✅</li>;
}
return <li className="item">{name}</li>;
```

해당 코드 대신 아래와 같이 작성할 수 있습니다.

</br>

```tsx
return (
	<li className="item">
		{isPacked ? name + ' ✅' : name}
	</li>
); 
```

다음 코드는 `isPacked` 가 참이면 `*name + '* ✅'` 을 렌더링하고, 그렇지 않으면 `name` 을 렌더링합니다.

</br>
</br>

> `<li>` 의 두 가지 다른 인스턴스를 만들 수 있기 때문에 객체 지향 프로그래밍에서는 위의 두 예가 미묘하게 다르다고 생각할 수 있습니다. 그러나 JSX 엘리먼트는 내부 상태를 보유하지 않으며 실제 DOM 노드가 아니기 때문에 인스턴스가 아닙니다.
따라서 위의 두 가지 예시 코드는 실제로 완전히 동일합니다.
> 

</br>
</br>


완료한 항목은 `<del>` 태그를 사용하여 줄 긋기를 해주고 더 많은 JSX를 중첩하기 쉽도록 새로운 줄과 괄호를 추가할 수 있습니다.

```tsx
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {isPacked ? (
        <del>
          {name + ' ✅'}
        </del>
      ) : (
        name
      )}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item
          isPacked={true}
          name="Space suit"
        />
        <Item
          isPacked={true}
          name="Helmet with a golden leaf"
        />
        <Item
          isPacked={false}
          name="Photo of Tam"
        />
      </ul>
    </section>
  );
}

```

삼항 조건 연산자는 간단한 조건에 잘 어울리지만, 적당히 사용하는 게 좋습니다.

중첩된 조건부 마크업이 너무 많아 컴포넌트가 지저분해질 경우 자식 컴포넌트를 추출하여 정리하는게 좋습니다.

React에서 마크업은 코드의 일부이므로 변수 및 함수와 같은 도구를 사용하여 복잡한 식을 정리할 수 있습니다.

</br>
</br>

### 논리 AND 연산자 &&

또 다른 일반적인 손쉬운 방법은 JavaScript 논리 AND `&&` 연산자입니다.

React 컴포넌트에서는 조건이 참일 때 일부 JSX를 렌더링하거나 그렇지 않으면 아무것도 렌더링하지 않을 때를 나타내는 경우가 많습니다.

다음과 같이 `&&` 를 사용하면 `isPacked` 가 `true` 인 경우에만 조건부로 체크 표시를 렌더링할 수 있습니다.

```tsx
return (
  <li className="item">
    {name} {isPacked && '✅'}
  </li>
);
```

위 코드는 `isPacked` 가 `true` 이면 체크 표시를 렌더링하고, 그렇지 않으면 아무것도 렌더링하지 않는다라고 읽을 수 있습니다.

</br>

JavaScript `&&` 표현식은 조건이 `true` 이면 오른쪽 값을 반환합니다.

그러나 조건이 `false` 이면 전체 표현식이 `false` 가 됩니다.

React는 `false` 를 `null` 또는 `undefined` 처럼 JSX 트리의 구멍으로 간주하고 그 자리에 아무것도 렌더링하지 않습니다.

</br>
</br>

> `&&` 의 왼쪽에 숫자를 두면 안됩니다.
조건식을 테스트하기 위해 JavaScript는 자동으로 왼쪽을 부울로 변환합니다.
그러나 왼쪽이 `0`이면 전체식이 `0`을 얻게 되고, React는 아무것도 아닌 `0`을 렌더링할 것입니다.
예를 들어, 흔하게 하는 실수로 `messageCount && <p>New messages</p>` 와 같은 코드를 작성하는 것입니다.
메시지 카운트가 `0` 일 때 아무것도 렌더링하지 않는다고 쉽게 추측할 수 있지만, 실제로는 `0` 자체를 렌더링합니다.
해당 문제를 해결하려면 `messageCount > 0 && <p>New messages</p>` 처럼 왼쪽 부울로 만들어야 합니다.
> 

</br>
</br>

### 변수에 조건부로 JSX를 할당하기

위와 같은 방법이 일반 코드를 작성하는 데 방해가 되면 `if` 문과 변수를 사용하면 됩니다.

`let` 으로 정의된 변수는 재할당할 수 있으므로 표시할 기본 내용인 이름을 먼저 대입합니다.

```tsx
let itemContent = name;
```

</br>

`if` 문을 사용하여 `isPacked` 가 `true` 인 경우 JSX 표현식을 `itemContent` 에 다시 할당합니다.

```tsx
if (isPacked) {
	itemContent = name + " ✅";
}
```

</br>

반환된 JSX 트리에 중괄호를 사용하고 이전에 계산된 식을 JSX 내부에 중첩하여 변수를 포함합니다.

```tsx
<li className="item">
	{itemContent}
</li>
```

</br>

해당 스타일은 가장 장황하면서도 가장 유연합니다.

```tsx
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = name + " ✅";
  }
  return (
    <li className="item">
      {itemContent}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item
          isPacked={true}
          name="Space suit"
        />
        <Item
          isPacked={true}
          name="Helmet with a golden leaf"
        />
        <Item
          isPacked={false}
          name="Photo of Tam"
        />
      </ul>
    </section>
  );
}
```

</br>