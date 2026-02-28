### State: 컴포넌트의 기억 저장소

컴포넌트는 상호 작용의 결과로 화면의 내용을 변경해야 하는 경우가 많습니다.

폼에 입력하면 입력 필드가 업데이트되어야 하고, 이미지 캐러셀에서 다음을 클릭할 때 표시되는 이미지가 변경되어야 하고, 구매를 클릭하면 상품이 장바구니에 담겨야 합니다.

컴포넌트는 현재 입력 값, 현재 이미지, 장바구니와 같은 것들을 기억해야 합니다.

React는 이런 종류의 컴포넌트별 메모리를 `state` 라고 부릅니다.

</br>
</br>

### 일반 변수로 충분하지 않은 경우

다음은 조각상 이미지를  렌더링하는 컴포넌트입니다.

Next 버튼을 클릭하면 `index` 를 `1`, `2` 로 변경하여 다음 조각상을 표시해야 합니다.

그러나 다음 코드는 작동하지 않습니다.

```tsx
// App.js
import { sculptureList } from './data.js';

export default function Gallery() {
  let index = 0;

  function handleClick() {
    index = index + 1;
  }

  let sculpture = sculptureList[index];
  return (
    <>
      <button onClick={handleClick}>
        Next
      </button>
      <h2>
        <i>{sculpture.name} </i>
        by {sculpture.artist}
      </h2>
      <h3>
        ({index + 1} of {sculptureList.length})
      </h3>
      <img
        src={sculpture.url}
        alt={sculpture.alt}
      />
      <p>
        {sculpture.description}
      </p>
    </>
  );
}
```

`handleClick` 이벤트 핸들러는 지역 변수 `index` 를 업데이트하고 있습니다.

하지만 이러한 변화를 보이지 않게 하는 두 가지 이유가 있습니다.

- **지역 변수는 렌더링 간에 유지되지 않습니다.**
    - React는 이 컴포넌트를 두 번째로 렌더링할 때 지역 변수에 대한 변경 사항은 고려하지 않고 처음부터 렌더링 합니다.
- **지역 변수를 변경해도 렌더링을 일으키지 않습니다.**
    - React는 새로운 데이터로 컴포넌트를 다시 렌더링해야 한다는 것을 인식하지 못합니다.

이때, 전역 변수는 렌더링 간에 유지되지만 리렌더링은 일으키지 않습니다.

</br>

컴포넌트를 새로운 데이터로 업데이트하기 위해선 다음 두 가지가 필요합니다.

- 렌더링 사이에 데이터를 유지합니다.
- React가 새로운 데이터로 컴포넌트를 렌더링하도록 유발합니다.

</br>

`useState` 훅은 다음 두 가지를 제공합니다.

- 렌더링 간에 데이터를 유지하기 위한 `state` 변수
- 변수를 업데이트하고 React가 컴포넌트를 다시 렌더링하도록 유발하는 `state setter` 함수

</br>
</br>

### state 변수 추가하기

`state` 변수를 추가하려면 파일 상단의 React에서 `useState` 를 가져옵니다.

```tsx
import { useState } from 'react';
```

</br>

그런 다음 다음 아래 코드를

```tsx
let index = 0;
```

</br>

다음과 같이 바꿉니다.

```tsx
const [index, setIndex] = useState(0);
```

`index` 는 `state` 변수이고 `setIndex` 는 `setter` 함수입니다.

여기서 `[` 와 `]` 문법을 배열 구조 분해라고 하며, 배열로부터 값을 읽을 수 있게 해줍니다.

`useState` 가 반환하는 배열에는 항상 두 개의 항목이 있습니다.

</br>

이것이 `handleClick` 에서 함께 작동하는 방식입니다.

```tsx
function handleClick() {
  setIndex(index + 1);
}
```

</br>
</br>

### 첫 번째 훅 만나기

React에서 `useState` 와 같이 `use` 로 시작하는 다른 모든 함수를 훅이라고 합니다.

훅은 React에서 오직 렌더링 중일 때만 사용할 수 있는 특별한 함수입니다.

이를 통해 다양한 React 기능을 연결 할 수 있습니다.

</br>

훅은 컴포넌트의 최상위 수준 또는 커스텀 훅에서만 호출할 수 있습니다.

조건문, 반복문 또는 기타 중첩 함수 내부에서는 훅을 호출할 수 없습니다.

훅은 함수이지만 컴포넌트의 필요에 대한 무조건적인 선언으로 생각하면 도움이 됩니다.

`import` 하는 것과 유사하게 컴포넌트 상단에서 React 기능을 사용합니다.

</br>
</br>

### useState 해부하기

`useState` 를 호출하는 것은, React에 이 컴포넌트가 무언가를 기억하기를 원한다고 말하는 것입니다.

```tsx
const [index, setIndex] = uesState(0);
```

이 경우 React가 `index` 를 기억하기를 원합니다.

</br>

이 쌍의 이름은 `const [something, setSomething]` 과 같이 지정하는 것이 규칙입니다.

원하는 대로 이름을 지을 수 있지만, 규칙을 사용하면 프로젝트 전반에 걸쳐 상황을 더 쉽게 이해할 수 있습니다.

</br>

`useState` 의 유일한 인수는 `state` 변수의 초깃값입니다.

위 예시에서 `index` 의 초깃값은 `useState(0)` 에 의해 `0` 으로 설정됩니다.

</br>

컴포넌트가 렌더링될 때마다, `useState` 는 다음 두 개의 값을 포함하는 배열을 제공합니다.

즉, 튜플처럼 쓰는 값 배열을 반환하고 그걸 배열 구조 분해 할당으로 꺼내 사용합니다.

- 저장한 값을 가진 `state` 변수
    - 위 예시에선 `index`
- `state` 변수를 업데이트하고 React에 컴포넌트를 다시 렌더링하도록 유발하는 `state setter` 함수
    - 위 예시에선 `setIndex`

</br>

실제 작동 방식은 다음과 같습니다.

```tsx
const [index, setIndex] = uesState(0);
```

- **컴포넌트가 처음 렌더링 됩니다.**
    - `index` 의 초깃값으로 `useState` 를 사용해 `0` 을 전달했으므로 `[0, setIndex` 를 반환합니다.
    - React는 `0` 을 최신 `state` 값으로 기억합니다.
- **`state` 를 업데이트합니다.**
    - 사용자가 버튼을 클릭하면 `setIndex(index + 1)` 를 호출합니다.
    - `index` 는 `0` 이므로 `setIndex(1)` 입니다.
    - 이는 React에 `index` 는 `1` 임을 기억하게 하고 또 다른 렌더링을 유발합니다.
- **컴포넌트가 두 번째로 렌더링 됩니다.**
    - React는 여전히 `useState(0)` 를 보지만, `index` 를 `1` 로 설정한 것을 기억하고 있기 때문에, 이번에는 `[1, setIndex]` 를 반환합니다.
- **이런 식으로 반복됩니다.**

</br>
</br>

### 컴포넌트에 여러 state 변수 지정하기

하나의 컴포넌트에 원하는 만큼 많은 타입의 `state` 변수를 가질 수 있습니다.

아래의 컴포넌트는 숫자 타입 `index` 와 Show details를 클릭했을 때 토글 되는 불리언 타입인 `showMore` 라는 두 개의 `state` 변수를 가지고 있습니다.

```tsx
// App.js
import { useState } from 'react';
import { sculptureList } from './data.js';

export default function Gallery() {
  const [index, setIndex] = useState(0);
  const [showMore, setShowMore] = useState(false);

  function handleNextClick() {
    setIndex(index + 1);
  }

  function handleMoreClick() {
    setShowMore(!showMore);
  }

  let sculpture = sculptureList[index];
  return (
    <>
      <button onClick={handleNextClick}>
        Next
      </button>
      <h2>
        <i>{sculpture.name} </i>
        by {sculpture.artist}
      </h2>
      <h3>
        ({index + 1} of {sculptureList.length})
      </h3>
      <button onClick={handleMoreClick}>
        {showMore ? 'Hide' : 'Show'} details
      </button>
      {showMore && <p>{sculpture.description}</p>}
      <img
        src={sculpture.url}
        alt={sculpture.alt}
      />
    </>
  );
}
```

서로 연관이 없는 경우 여러 개의 `state` 변수를 가지는 것이 좋습니다.

하지만 두 `state` 변수를 자주 함께 변경하는 경우에는 두 변수를 하나로 합치는 것이 더 좋을 수 있습니다.

예를 들어, 필드가 많은 폼의 경우 필드별로 `state` 변수를 사용하는 것보다 하나의 객체 `state` 변수를 사용하는 것이 더 편리합니다.

</br>
</br>

### React는 어떤 state를 반환할지 어떻게 알 수 있을까요?

`useState(index)` 처럼 `index` 상태 같은 식별자를 React에 넘기지 않는데도, React가 매 렌더마다 첫 번째 `useState` 는 `index`, 두 번째 `useState` 는 `showMore` 를 어떻게 매칭하는걸까요?

```tsx
const [index, setIndex] = useState(0);
const [showMore, setShowMore] = useState(false);
```

위 코드를 보면 `useState` 호출이 어떤 `state` 변수를 참조하는지에 대한 정보를 받지 못한다는 것을 알 수 있습니다.

</br>

간결한 구문을 구현하기 위해 훅은 동일한 컴포넌트의 모든 렌더링에서 안정적인 호출 순서에 의존합니다.

**“최상위 수준에서만 훅 호출”** 규칙을 따르면, 훅은 항상 같은 순서로 호출되기 때문에 실제로 문제가 발생하지 않습니다.

만약 조건문, 반복문, 중첩 함수 내부에서 훅을 호출하면 렌더링마다 호출 횟수나 순서가 달라질 수 있습니다.

이런 경우 React가 저장해 둔 `state` 배열의 인덱스와 실제 훅 호출 순서가 어긋나게 되어, 서로 다른 `state` 가 잘못 매칭되는 문제가 발생합니다.

</br>

내부적으로 React는 모든 컴포넌트에 대해 한 쌍의 `state` 배열을 가집니다.

또한 렌더링 전에 `0` 으로 설정된 현재 인덱스 쌍을 유지합니다.

`useState` 를 호출할 때마다, React는 다음 `state` 쌍을 제공하고 인덱스를 증가시킵니다.

즉, React는 변수 이름이나 코드 위치를 기준으로 `state` 를 구분하지 않고, 오직 몇 번째로 호출되었는지를 기준으로 구분합니다.

그렇기에 `state` 변수명을 개발자가 마음대로 바꿀 수 있습니다.

</br>
</br>

### State는 격리되고 비공개로 유지됩니다.

`State` 는 화면에서 컴포넌트 인스턴스에 지역적입니다.

다시 말해, 동일한 컴포넌트를 두 번렌더링한다면 각 복사본은 완전히 격리된 `state` 를 가집니다.

그중 하나를 변경해도 다른 하나에는 영향을 미치지 않습니다.

</br>

다음 코드는 `Gallery` 컴포넌트가 로직 변경 없이 두 번 렌더링되었습니다.

```tsx
// App.js
import Gallery from './Gallery.js';

export default function Page() {
  return (
    <div className="Page">
      <Gallery />
      <Gallery />
    </div>
  );
}

// Gallery.js
import { useState } from 'react';
import { sculptureList } from './data.js';

export default function Gallery() {
  const [index, setIndex] = useState(0);
  const [showMore, setShowMore] = useState(false);

  function handleNextClick() {
    setIndex(index + 1);
  }

  function handleMoreClick() {
    setShowMore(!showMore);
  }

  let sculpture = sculptureList[index];
  return (
    <section>
      <button onClick={handleNextClick}>
        Next
      </button>
      <h2>
        <i>{sculpture.name} </i>
        by {sculpture.artist}
      </h2>
      <h3>
        ({index + 1} of {sculptureList.length})
      </h3>
      <button onClick={handleMoreClick}>
        {showMore ? 'Hide' : 'Show'} details
      </button>
      {showMore && <p>{sculpture.description}</p>}
      <img
        src={sculpture.url}
        alt={sculpture.alt}
      />
    </section>
  );
}
```

각각의 갤러리 내부 버튼을 클릭해도 그들의 `state` 가 서로 독립적임을 알 수 있습니다.

이것이 `state` 를 일반적인 모듈 상단에 선언할 수 있는 보통의 변수와 구별하는 요소입니다.

`State` 는 특정 함수 호출이나 코드 내의 특정 위치와 관련이 없습니다.

대신, 화면의 특정 위치에 지역적입니다.

`<Gallery />` 컴포넌트를 두 번 렌더링했으므로 그들의 `state` 는 별도로 저장됩니다.

</br>

또한 `Page` 컴포넌트가 `Gallery` 의 `state` 에 대해 아무것도 알지 않는다는 점과 심지어 그것이 있는지도 모른다는 것도 주목해야합니다.

`Props` 와 달리, `state` 는 선언한 컴포넌트에 완전히 비공개이기에 부모 컴포넌트는 이를 변경할 수 없습니다.

이로써 다른 컴포넌트에 영향을 미치지 않고 어떤 컴포넌트에든 `state` 를 추가하거나 제더할 수 있게 됩니다.

</br>

만약 두 개의 갤러리가 `state` 를 동기화하길 원한다면, React에서 올바른 방법은 자식 컴포넌트에서 `state` 를 제거하고 가장 가까운 공통 부모 컴포넌트에 추가하는 것입니다.

</br>
