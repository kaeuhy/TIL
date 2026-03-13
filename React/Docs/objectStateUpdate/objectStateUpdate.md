### 객체 State 업데이트하기

`State` 는 객체를 포함한 모든 종류의 자바스크립트 값을 가질 수 있습니다.

하지만 React `state` 가 가진 객체를 직접 변경해서는 안 됩니다.

객체를 업데이트하고 싶을 때는 새로운 객체를 생성하여 또는 기존 객체의 복사본을 만들어, `state` 가 복사본을 사용하도록 해야합니다.

</br>
</br>

### 변경이란?

`State` 에는 모든 종류의 자바스크립트 값을 저장할 수 있습니다.

```jsx
const [x, setX] = useState(0);
```

</br>

지금까지 숫자, 문자열, 불리언을 다루었습니다.

이러한 자바스크립트 값들은 변경할 수 없거나 읽기 전용을 의미하는 불변성을 가집니다.

값을 교체하기 위해서는 리렌더링이 필요합니다.

</br>

```jsx
setX(5);
```

`x` `state` 는 `0` 에서 `5` 로 바뀌었지만, 숫자 `0` 자체는 바뀌지 않았습니다.

→ `0` 이라는 값 자체가 변경된 것이 아니라, `state` 가 새로운 값 `5` 를 가리키게 된 것

숫자, 문자열, 불리언과 같이 자바스크립트에 정의되어 있는 원시 값들은 불변(immutable) 즉, 값 자체를 수정할 수 없습니다.

</br>

다음과 같이 `state` 에 있는 객체를 생각해봅시다.

```jsx
const [position, setPosition] = useState({ x: 0, y: 0 });
```

</br>

기술적으로 객체 자체의 내용은 바꿀 수 있고 이것을 변경(mutation)이라고 합니다.

```jsx
position.x = 5;
```

하지만 React `state` 의 객체들이 기술적으로 변경 가능할지라도, 숫자, 불리언, 문자열과 같이 불변성을 가진 것처럼 다루어야 합니다.

객체를 변경하는 대신 교체해야 합니다.

</br>
</br>

### State를 읽기 전용인 것처럼 다루세요

다시 강조하자면, `state` 에 저장한 자바스크립트 객체는 어떤 것이라도 읽기 전용인 것처럼 다루어야 합니다.

아래 예시에서 `state` 의 object는 현재 포인터 위치를 나타냅니다.

프리뷰 영역을 누르거나 커서를 움직일 때 빨간 점이 이동해야 합니다.

```jsx
import { useState } from 'react';

export default function MovingDot() {
  const [position, setPosition] = useState({
    x: 0,
    y: 0
  });
  return (
    <div
      onPointerMove={e => {
        position.x = e.clientX;
        position.y = e.clientY;
      }}
      style={{
        position: 'relative',
        width: '100vw',
        height: '100vh',
      }}>
      <div style={{
        position: 'absolute',
        backgroundColor: 'red',
        borderRadius: '50%',
        transform: `translate(${position.x}px, ${position.y}px)`,
        left: -10,
        top: -10,
        width: 20,
        height: 20,
      }} />
    </div>
  )
}
```

하지만 점은 초기 위치에 머무릅니다.

</br>

문제는 이 코드입니다.

```jsx
onPointerMove={e => {
  position.x = e.clientX;
  position.y = e.clientY;
}}
```

해당 코드는 `position` 에 할당된 객체를 이전 렌더링에서 수정합니다.

그러나 React는 `state` `set` 함수가 없으면 객체가 변경되었는지 알 수 없기 때문에 React는 아무것도 하지 않습니다.

`state` 를 변경하는 것이 어떤 경우에는 동작할 수 있지만, 권장하지 않습니다.

렌더링 시에 접근하려는 `state` 값은 읽기 전용처럼 다루어야 합니다.

</br>

리렌더링을 발생시키려면, 새 객체를 생성하여 `state` 설정 함수로 전달 해야합니다.

```jsx
onPointerMove={e => {
  setPosition({
    x: e.clientX,
    y: e.clientY
  });
}}
```

`setPosition` 함수는 React에게 다음과 같이 요청합니다.

- `position` 을 새로운 객체를 교체하라
- 그리고 컴포넌트를 다시 렌더링하라

</br>
</br>

### 지역 변경

아래 코드는 `state` 에 존재하는 객체를 변경하기에 문제가 됩니다.

```jsx
position.x = e.clientX;
position.y = e.clientY;
```

</br>

하지만 아래 코드는 방금 생성한 새로운 객체를 변경하기 때문에 문제가 발생하지 않습니다.

```jsx
const nextPosition = {};
nextPosition.x = e.clientX;
nextPosition.y = e.clientY;
setPosition(nextPosition);
```

</br>

위 코드는 아래처럼 작성할 수 있습니다.

```jsx
setPosition({
  x: e.clientX,
  y: e.clientY
});
```

변경은 이미 `state` 에 존재하는 객체를 변경할 때만 문제가 됩니다.

방금 만든 객체를 수정하는 것은 아직 다른 코드가 해당 객체를 참조하지 않기 때문에 괜찮습니다.

이 객체를 변경하는 것은 해당 객체에 의존하는 무언가에 우연히 영향을 주지 않습니다.

→ 이것을 지역 변경이라고 부르고 렌더링하는 동안 지역변경이 가능함

</br>
</br>

### 스프레드 문법으로 객체 복사하기

이전 예시에서 `position` 객체는 현재 커서 위치에서 항상 새롭게 생성됩니다.

하지만 종종 새로 생성하는 객체에 존재하는 데이터를 포함하고 싶을 수 있습니다.

→ 폼에서 단 한 개의 필드만 수정하고, 나머지 모든 필드는 이전 값을 유지하고 싶을때

</br>

아래 코드에서 `input` 필드는 `onChange` 핸들러가 `state` 를 변경하기 때문에 React가 변경을 감지하지 못하고, 그 결과 `input` 값이 갱신되지 않습니다.

```jsx
import { useState } from 'react';

export default function Form() {
  const [person, setPerson] = useState({
    firstName: 'Barbara',
    lastName: 'Hepworth',
    email: 'bhepworth@sculpture.com'
  });

  function handleFirstNameChange(e) {
    person.firstName = e.target.value;
  }

  function handleLastNameChange(e) {
    person.lastName = e.target.value;
  }

  function handleEmailChange(e) {
    person.email = e.target.value;
  }

  return (
    <>
      <label>
        First name:
        <input
          value={person.firstName}
          onChange={handleFirstNameChange}
        />
      </label>
      <label>
        Last name:
        <input
          value={person.lastName}
          onChange={handleLastNameChange}
        />
      </label>
      <label>
        Email:
        <input
          value={person.email}
          onChange={handleEmailChange}
        />
      </label>
      <p>
        {person.firstName}{' '}
        {person.lastName}{' '}
        ({person.email})
      </p>
    </>
  );
}
```

</br>

아래 코드는 사용자가 입력시 값 자체는 바뀌지만 React는 `set` 함수가 호출되지 않으면 `state` 변경을 인지하지 않습니다.

렌더링이 발생하지 않기 때문에, `input` 은 React `state` 기준의 기존 `value` 로 유지됩니다.

```jsx
person.firstName = e.target.value;

```

</br>

원하는 동작을 정확히 얻기 위해서는 새로운 객체를 생성하여 `setPerson` 으로 전달해야 합니다.

```jsx
setPerson({
  firstName: e.target.value,
  lastName: person.lastName,
  email: person.email
});
```

하지만, 단 하나의 필드가 바뀌었기 때문에 기존에 존재하는 다른 데이터를 복사해야 합니다.

</br>

`…` 객체 스프레드 구문을 사용하면 모든 프로퍼티를 각각 복사하지 않아도 됩니다.

```jsx
setPerson({
  ...person,
  firstName: e.target.value
});
```

`…person` 을 통해 기존 객체 프로퍼티를 복사하고 `firstName` 으로 같은 `key` 를 새 값으로 덮어씁니다.

→ 기존 객체를 수정하는 것이 아닌 새로운 객체를 생성

스프레드 문법은 얕은 복사를 하여 객체의 첫 번째 레벨만 복사하고, 그 안에 있는 객체는 그대로 참조로 공유합니다.

그렇기에 중첩 객체를 업데이트하려면 스프레드 문법을 여러 번 사용해야 합니다.

</br>
</br>

### 여러 필드에 단일 이벤트 핸들러 사용하기

`[` 와 `]` 괄호를 객체 정의 안에 사용하여 동적 이름을 가진 프로퍼티를 명시할 수 있습니다.

→ 변수의 값을 `key` 로 사용하려면 `[]` 를 사용

아래에는 이전 예시와 같지만, 세 개의 다른 이벤트 핸들러 대신 하나의 이벤트 핸들러를 사용합니다.

```jsx
import { useState } from 'react';

export default function Form() {
  // input이 3개 존재
  const [person, setPerson] = useState({
    firstName: 'Barbara',
    lastName: 'Hepworth',
    email: 'bhepworth@sculpture.com'
  });

  function handleChange(e) {
    setPerson({
      ...person,
      [e.target.name]: e.target.value
    });
  }

  return (
    <>
      <label>
        First name:
        <input
          name="firstName"
          value={person.firstName}
          onChange={handleChange}
        />
      </label>
      <label>
        Last name:
        <input
          name="lastName"
          value={person.lastName}
          onChange={handleChange}
        />
      </label>
      <label>
        Email:
        <input
          name="email"
          value={person.email}
          onChange={handleChange}
        />
      </label>
      <p>
        {person.firstName}{' '}
        {person.lastName}{' '}
        ({person.email})
      </p>
    </>
  );
}
```

`e.target.name` 은 `<input>` DOM 엘리먼트의 `name` 프로퍼티를 나타냅니다.

</br>
</br>

### 중첩된 객체 갱신하기

아래와 같이 중첩된 객체 구조를 통해 알아봅시다.

```jsx
const [person, setPerson] = useState({
  name: 'Niki de Saint Phalle',
  artwork: {
    title: 'Blue Nana',
    city: 'Hamburg',
    image: 'https://i.imgur.com/Sd1AgUOm.jpg',
  }
});
```

</br>

`person.artwork.city` 를 업데이트하고 싶다면, 변경하는 방법은 다음과 같습니다.

```jsx
person.artwork.city = 'New Delhi';
```

</br>

하지만 React에서는 `state` 를 변경할 수 없는 것으로 다루어야 합니다.

`city` 를 바꾸기 위해서는 먼저 이전 객체의 데이터로 생성된 새로운 `artwork` 객체를 생성한 뒤, 그것을 가리키는 새로운 `person` 객체를 만들어야 합니다.

```jsx
const nextArtwork = { ...person.artwork, city: 'New Delhi' };
const nextPerson = { ...person, artwork: nextArtwork };
setPerson(nextPerson);
```

</br>

또는 단순하게 함수를 호출할 수 있습니다.

```jsx
setPerson({
  ...person, // 다른 필드 복사
  artwork: { // artwork 교체
    ...person.artwork, // 동일한 값 사용
    city: 'New Delhi' // 하지만 New Delhi!
  }
});
```

</br>
</br>

### 객체들은 사실 중첩되어 있지 않습니다.

이러한 객체는 코드에서 중첩되어 나타납니다.

```jsx
let obj = {
  name: 'Niki de Saint Phalle',
  artwork: {
    title: 'Blue Nana',
    city: 'Hamburg',
    image: 'https://i.imgur.com/Sd1AgUOm.jpg',
  }
};
```

</br>

중첩은 객체의 동작에 대해 생각하는 부정확한 방법입니다.

코드가 실행될 때, 중첩된 객체라는 것은 없으며 실제로 두 개의 다른 객체를 보는 것입니다.

```jsx
let obj1 = {
  title: 'Blue Nana',
  city: 'Hamburg',
  image: 'https://i.imgur.com/Sd1AgUOm.jpg',
};

let obj2 = {
  name: 'Niki de Saint Phalle',
  artwork: obj1
};
```

</br>

`obj1` 객체는 `obj2` 객체 안에 없고 `obj3` 또한 `obj1` 을 가리킬 수 있습니다.

즉, 객체 자체가 들어가는 것이 아니라 그 객체를 가리키는 참조가 들어갑니다.

```jsx
let obj1 = {
  title: 'Blue Nana',
  city: 'Hamburg',
  image: 'https://i.imgur.com/Sd1AgUOm.jpg',
};

let obj2 = {
  name: 'Niki de Saint Phalle',
  artwork: obj1
};

let obj3 = {
  name: 'Copycat',
  artwork: obj1
};
```

`obj3.artwork.city` 를 변경하려 했다면, `obj2.artwork.city` 와 `obj1.city` 둘 다에 영향을 미칠 것입니다.

→ `obj3.artwork` , `obj2.artwork` 와 `obj1` 이 같은 객체이기 때문

객체는 중첩된 것이 아니라 프로퍼티를 통해 서로를 가리키는 각각의 객체들입니다.

</br>
</br>

### Immer로 간결한 갱신 로직 작성하기

`state` 가 깊이 중첩되어 있다면 평탄화를 고려해야합니다.

만약 `state` 구조를 바꾸고 싶지 않다면, `Immer` 를 사용하면 됩니다.

`Immer` 는 편리하고, 변경 구문을 사용할 수 있게 해주며 복사본 생성을 도와주는 라이브러리입니다.

```jsx
updatePerson(draft => {
  draft.artwork.city = 'Lagos';
});
```

`Immer` 가 제공하는 `draft` 는 Proxy라고 하는 아주 특별한 객체 타입으로, 내부적으로 어느 부분이 변경되었는지 알아내어, 변경사항을 포함한 완전히 새로운 객체를 생성합니다.

</br>

`Immer` 를 사용하기 위해서는

- `package.json`에 `dependencies`로 `use-immer`를 추가합니다.
- `npm install`을 실행합니다.
- `import { useState } from 'react'`를 `import { useImmer } from 'use-immer'`로 교체합니다.

</br>

하나의 컴포넌트 안에서 원하는 만큼 `useState` 와 `useImmer` 를 섞어 사용할 수 있습니다.

`Immer` 는 업데이트 핸들러를 간결하게 관리할 수 있는 좋은 방법이며, 특히 `state` 가 중첩되어 있고 객체를 복사하는 것이 중복되는 코드를 만들 때 유용합니다.

</br>
</br>

### 왜 React에서 `state` 변경은 권장되지 않나요?

- **디버깅**
    - `console.log` 를 사용하고 `state` 를 변경하지 않는다면, 과거 로그들은 가장 최근 `state` 변경 사항들에 지워지지 않습니다.
    - 따라서 `state` 가 렌더링 사이에 어떻게 바뀌었는지 명확하게 알 수 있습니다.
- **최적화**
    - 보편적인 React 최적화 전략은 이전 `props` 또는 `state` 가 다음 것과 동일할 때 일을 건너뛰는 것에 의존합니다.
    - `state` 를 절대 변경하지 않는다면 변경사항이 있었는지 확인하는 작업이 매우 빨라집니다.
- **새로운 기능**
    - 새로운 React 기능들은 스냅샷처럼 다루어지는 것에 의존합니다.
- **요구사항 변화**
    - 취소/복원 구현, 변화 내역 조회, 사용자가 이전 값으로 폼을 재설정하기 등의 기능은 아무것도 변경되지 않았을 때 더 쉽습니다.
- **더 간단한 구현**
    - React는 상태 변화를 감지하기 위해 객체 내부의 변화를 추적하지 않습니다.
    - 그래서 Proxy 같은 특별한 반응형 객체가 필요 없습니다.

</br>