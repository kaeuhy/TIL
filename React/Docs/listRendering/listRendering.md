### 리스트 렌더링

데이터 모음에서 유사한 컴포넌트를 여러 개 표시하고 싶을 때가 종종 있습니다.

JavaScript 배열 메서드를 사용하여 데이터 배열을 조작할 수 있습니다.

</br>

내용이 있는 리스트가 있다고 가정합니다.

이러한 리스트 항목의 유일한 차이점은 콘텐츠, 즉 데이터입니다.

```tsx
<ul>
  <li>Creola Katherine Johnson: mathematician</li>
  <li>Mario José Molina-Pasquel Henríquez: chemist</li>
  <li>Mohammad Abdus Salam: physicist</li>
  <li>Percy Lavon Julian: chemist</li>
  <li>Subrahmanyan Chandrasekhar: astrophysicist</li>
</ul>
```

인터페이스를 구축할 때 서로 다른 데이터를 사용하여 동일한 컴포넌트의 여러 인스턴스를 표시해야 하는 경우가 종종 있습니다.

이러한 상황에서 해당 데이터를 JavaScript 객체와 배열에 저장하고 `map()` 과 `filter()` 같은 메서드를 사용하여 해당 객체에서 컴포넌트 리스트를 렌더링할 수 있습니다.

</br>

배열에서 항목 리스트를 생성하는 방법은 다음과 같습니다.

데이터를 배열로 이동합니다.

```tsx
const people = [
  'Creola Katherine Johnson: mathematician',
  'Mario José Molina-Pasquel Henríquez: chemist',
  'Mohammad Abdus Salam: physicist',
  'Percy Lavon Julian: chemist',
  'Subrahmanyan Chandrasekhar: astrophysicist'
];
```

</br>

`people` 의 요소를 새로운 JSX 노드 배열인 `listItems` 에 매핑합니다.

```tsx
const listItems = people.map(person => <li>{person}</li>);
```

</br>

`<ul>` 로 래핑된 컴포넌트의 `listItems` 를 반환합니다.

```tsx
return <ul>{listItems}</ul>;
```

</br>

결과는 다음과 같습니다.

```tsx
// App.js
const people = [
  'Creola Katherine Johnson: mathematician',
  'Mario José Molina-Pasquel Henríquez: chemist',
  'Mohammad Abdus Salam: physicist',
  'Percy Lavon Julian: chemist',
  'Subrahmanyan Chandrasekhar: astrophysicist'
];

export default function List() {
  const listItems = people.map(person =>
    <li>{person}</li>
  );
  return <ul>{listItems}</ul>;
}
```

</br>
</br>

### 배열의 항목들을 필터링하기

다음 아래 배열에서 직업이 `chemist` 인 사람들만 표시하고 싶다고 가정해 봅시다.

```tsx
const people = [{
  id: 0,
  name: 'Creola Katherine Johnson',
  profession: 'mathematician',
}, {
  id: 1,
  name: 'Mario José Molina-Pasquel Henríquez',
  profession: 'chemist',
}, {
  id: 2,
  name: 'Mohammad Abdus Salam',
  profession: 'physicist',
}, {
  id: 3,
  name: 'Percy Lavon Julian',
  profession: 'chemist',
}, {
  id: 4,
  name: 'Subrahmanyan Chandrasekhar',
  profession: 'astrophysicist',
}];
```

JavaScript의 `filter()` 메서드를 사용하여 해당하는 사람만 반환할 수 있습니다.

이 메서드는 항목 배열을 받아 `true` 혹은 `false` 를 반환하는 함수를 검증한 후 `true` 가 반환된 항목만 있는 새로운 배열을 반환합니다.

</br>

검증을 위한 `test` 함수는 `(person) ⇒ person.profession === ‘chemist’` 와 같습니다.

이를 적용하는 과정은 다음과 같습니다.

</br>

`people` 에서 `filter()` 를 호출해 `person.profession === ‘chemist’` 로 필터링해서 `chemist` 로만 구성된 새로운 배열 `chemists` 를 생성합니다.

```tsx
const chemists = people.filter(person =>
  person.profession === 'chemist'
);
```

</br>

이제 다음과 같이 `chemists` 를 매핑합니다.

```tsx
const listItems = chemists.map(person =>
  <li>
     <img
       src={getImageUrl(person)}
       alt={person.name}
     />
     <p>
       <b>{person.name}:</b>
       {' ' + person.profession + ' '}
       known for {person.accomplishment}
     </p>
  </li>
);
```

</br>

마지막으로 컴포넌트에서 `listItems` 를 반환합니다.

```tsx
return <ul>{listItems}</ul>;
```

</br>
</br>

화살표 함수는 암시적으로 `⇒` 바로 뒤에 식을 반환하기 때문에 `return` 문이 필요하지 않습니다.

```tsx
const listItems = chemists.map(person =>
  <li>...</li> // 암시적 반환
);
```

</br>

하지만 `⇒` 뒤에 `{` 중괄호가 오는 경우 `return` 을 명시적으로 작성해야 합니다.

```tsx
const listItems = chemists.map(person => {
  return <li>...</li>;
});
```

`⇒ {` 를 표현하는 화살표 함수를 **block body 를** 가지고 있다고 말합니다.

이 함수를 사용하면 한 줄 이상의 코드를 작성할 수 있지만 `return` 문을 반드시 작성해야 합니다.

그렇지 않으면 아무것도 반환되지 않습니다.

</br>
</br>

### key 를 사용해서 리스트 항목을 순서대로 유지하기

위에 코드들은 다음과 같이 콘솔에 에러가 표시되는 것을 확인할 수 있습니다.

```tsx
Warning: Each child in a list should have a unique “key” prop.
```

</br>

그 이유는 각 배열 항목에 다른 항목 중에서 고유하게 식별할 수 있는 문자열 또는 숫자를 `key` 로 지정해야 하기 때문입니다.

```tsx
<li key={person.id}>...</li>
```

`map()` 메서드 호출 내부의 JSX 엘리먼트에는 항상 `key` 가 필요합니다.

</br>

`Key` 는 각 컴포넌트가 어떤 배열 항목에 해당하는지 React에 알려주어 나중에 일치 시킬 수 있도록 합니다.

이는 배열 항목이 정렬 등으로 인해 이동하거나 삽입되거나 삭제될 수 있는 경우 중요해집니다.

`key` 를 잘 선택하면 React가 정확히 무슨 일이 일어났는지 추론하고 DOM 트리에 올바르게 업데이트 하는데 도움이 됩니다.

이를 리액트의 재조정 과정이라고 합니다.

</br>

다음과 같이 즉석에서 `key` 를 생성하는 대신 데이터 안에 `key` 를 포함해야 합니다.

```tsx
// data.js
export const people = [{
  id: 0, // JSX에서 key로 사용됩니다.
  name: 'Creola Katherine Johnson',
  profession: 'mathematician',
  accomplishment: 'spaceflight calculations',
  imageId: 'MK3eW3A'
}, {
  id: 1, // JSX에서 key로 사용됩니다.
  name: 'Mario José Molina-Pasquel Henríquez',
  profession: 'chemist',
  accomplishment: 'discovery of Arctic ozone hole',
  imageId: 'mynHUSa'
}, {
  id: 2, // JSX에서 key로 사용됩니다.
  name: 'Mohammad Abdus Salam',
  profession: 'physicist',
  accomplishment: 'electromagnetism theory',
  imageId: 'bE7W1ji'
}, {
  id: 3, // JSX에서 key로 사용됩니다.
  name: 'Percy Lavon Julian',
  profession: 'chemist',
  accomplishment: 'pioneering cortisone drugs, steroids and birth control pills',
  imageId: 'IOjWm71'
}, {
  id: 4, // JSX에서 key로 사용됩니다.
  name: 'Subrahmanyan Chandrasekhar',
  profession: 'astrophysicist',
  accomplishment: 'white dwarf star mass calculations',
  imageId: 'lrWQx8l'
}];

```

</br>


```tsx
// App.js
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const listItems = people.map(person =>
    <li key={person.id}>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}</b>
          {' ' + person.profession + ' '}
          known for {person.accomplishment}
      </p>
    </li>
  );
  return <ul>{listItems}</ul>;
}

```

</br>
</br>

### 각 리스트 항목에 대해 여러 DOM 노드 표시하기

각 항목이 하나가 아닌 여러 개의 DOM 노드를 렌더링해야 하는 경우에는 어떻게 해야 할까요?

</br>

짧은 `<> </>` fragment 구문으로는 `key` 를 전달할 수 없으므로 `key` 를 단일 `<div>` 로 그룹화하거나 약간 더 길고 명시적인 `<Fragment>` 문법을 사용해야 합니다.

```tsx
import { Fragment } from 'react';

// ...

const listItems = people.map(person =>
  <Fragment key={person.id}>
    <h1>{person.name}</h1>
    <p>{person.bio}</p>
  </Fragment>
);
```

Fragment는 DOM에서 사라지므로 `<h1>`, `<p>` 등의 평평한 리스트가 생성됩니다.

</br>
</br>

### key

**`Key` 를 가져오는 곳**

데이터 소스마다 다른 `key` 소스를 제공합니다.

- **데이터베이스의 데이터:**
    - 데이터베이스에서 데이터를 가져오는 경우 본질적으로 고유한 데이터베이스 `key`/`ID` 를 사용할 수 있습니다.
- **로컬에서 생성된 데이터:**
    - 데이터가 로컬에서 생성되고 유지되는 경우, 항목을 만들 때 증분 일련번호나 `crypto.randomUUID()` 또는 `uuid` 같은 패키지를 사용합니다.

</br>
</br>

**`key` 규칙**

`key` 는 형제 간에 고유해야 합니다.

하지만 같은 `key` 를 다른 배열의 JSX 노드에 동일한 `key` 로 사용해도 괜찮습니다.

`key` 는 변경되어서는 안 되며 그렇게 되면 `key` 는 목적에 어긋납니다.

렌더링 중에는 `key` 를 생성해선 안됩니다.

</br>
</br>

### React에 key가 필요한 이유

데스크탑의 파일에 이름이 없다면 첫 번째 파일, 두 번째 파일 등 순서대로 파일을 참조할 것입니다.

이때 파일을 삭제한다면 혼란스러워질 수도 있습니다.

두 번째 파일이 첫 번째 파일이 되고 세 번째 파일이 두 번째 파일이 되는 식으로 되어버립니다.

</br>

폴더의 파일 이름과 배열의 JSX `key` 는 비슷한 용도로 사용됩니다.

이를 통해 형제 항목 간에 항목을 고유하게 식별할 수 있습니다.

잘 선택된 `key` 는 배열 내 위치보다 더 많은 정보를 제공합니다.

재정렬로 인해 위치가 변경되더라도 `key` 는 React가 생명주기 내내 해당 항목을 식별할 수 있게 해줍니다.

</br>

배열에서 항목의 인덱스를 `key` 로 사용하고 싶을 수도 있습니다.

실제로 `key` 를 전혀 지정하지 않으면 React는 인덱스를 사용합니다.

하지만 항목이 삽입되거나 삭제하거나 배열의 순서가 바뀌면 시간이 지남에 따라 항목을 렌더링하는 순서가 변경됩니다.

인덱스를 `key` 로 사용하면 종종 미묘하고 혼란스러운 버그가 발생합니다.

</br>

마찬가지로 `key={Math.random()}` 처럼 즉석에서 `key` 를 생성하면 안됩니다.

이렇게 하면 렌더링 간에 `key` 가 일치하지 않아 모든 컴포넌트와 DOM이 매번 다시 생성될 수 있습니다.

속도가 느려질 뿐만 아니라 리스트 항목 내부의 모든 사용자의 입력도 손실됩니다.

</br>

컴포넌트는 `key` 를 `prop` 으로 받지 않습니다.

`key` 는 React 자체에서 힌트로만 사용됩니다.

컴포넌트에 ID가 필요하다면 `<Profile key={id} userId={id} />` 와 같이 별도의 `prop` 으로 전달해야 합니다.

</br>