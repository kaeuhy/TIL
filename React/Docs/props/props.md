### 컴포넌트에 props 전달하기

React 컴포넌트는 `props` 를 이용해 서로 통신합니다.

모든 부모 컴포넌트는 `props` 를 줌으로써 몇몇의 정보를 자식 컴포넌트에게 전달할 수 있습니다.

`props` 는 HTML 어트리뷰트를 생각나게할 수도 있지만, 객체, 배열, 함수를 포함한 모든 JavaScript 값을 전달할 수 있습니다.

</br>
</br>

### 친숙한 props

`props` 는 JSX 태그에 전달하는 정보입니다.

예를 들어, `className`, `src`, `alt`, `width`, `height` 는 `<img>` 태그에 전달할 수 있습니다.

```tsx
function Avatar() {
  return (
    <img
      className="avatar"
      src="https://i.imgur.com/1bX5QH6.jpg"
      alt="Lin Lanying"
      width={100}
      height={100}
    />
  );
}
```

`<img>` 태그에 전달할 수 있는 `props` 는 미리 정의되어 있습니다. (ReactDOM은 HTML 표준을 준수합니다.)

</br>
</br>

### 컴포넌트에 props 전달하기

자신이 생성한 `<Avatar>` 와 같은 어떤 컴포넌트든 `props` 를 전달할 수 있습니다.

아래 코드에서 `Profile` 컴포넌트는 자식 컴포넌트인 `Avatar` 에 어떠한 `props` 도 전달하지 않습니다.

```tsx
export default function Profile() {
	return (
		<Avatar />
	);
}
```

다음 두 단계에 걸쳐 `Avatar` 에 `props` 를 전달할 수 있습니다.

</br>
</br>

**자식 컴포넌트에 `props` 전달하기**

먼저, `Avatar` 에 몇몇 `props` 를 전달합니다.

예를 들어 다음 아래에 예시처럼 `person` 과 `size` 를 전달해 보겠습니다.

```tsx
export default function Profile() {
  return (
    <Avatar
      person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
      size={100}
    />
  );
}
```

`person=` 뒤에 있는 이중 괄호는 앞선 단계에서 다룬 JSX 중괄호 안의 객체입니다.

이제 `Avatar` 컴포넌트 내 `props` 를 읽을 수 있습니다.

</br>
</br>

**자식 컴포넌트 내부에서 `props` 읽기**

이러한 `props` 는 `function Avatar` 바로 뒤에 있는 `({` 와 `})` 안에 그들의 이름인 `person`, `size` 등을 쉼표로 구분함으로써 읽을 수 있습니다.

```tsx
function Avatar({ person, size }) {
	// person과 size는 이곳에서 사용가능합니다.
}
```

이렇게 하면 `Avatar` 코드 내에서 변수를 사용하는 것처럼 사용할 수 있습니다.

</br>

`Avatar` 에 렌더링을 위해 `person` 과 `size` `props` 를 사용하는 로직을 추가하면 완료됩니다.

이제 `Avatar` 를 다른 `props` 를 이용해 다양한 방식으로 렌더링하도록 구성할 수 있습니다.

```tsx
// utils.js
export function getImageUrl(person, size = 's') {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

</br>

```tsx
// App.js
function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

export default function Profile() {
  return (
    <div>
      <Avatar
        size={100}
        person={{
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
      />
      <Avatar
        size={80}
        person={{
          name: 'Aklilu Lemma',
          imageId: 'OKS67lh'
        }}
      />
      <Avatar
        size={50}
        person={{
          name: 'Lin Lanying',
          imageId: '1bX5QH6'
        }}
      />
    </div>
  );
}
```

`props` 를 사용하면 부모 컴포넌트와 자식 컴포넌트를 독립적으로 생각할 수 있습니다.

`props` 를 어떻게 사용하는지 생각할 필요 없이 `Profile` 의 `person` 또는 `size` `props` 를  수정할 수 있습니다.

마찬가지로 `Profile` 을 보지 않고도 `Avatar` 가 `Props` 를 사용하는 방식을 바꿀 수 있습니다.

</br>

`props` 는 함수의 인수와 동일한 역할로 컴포넌트에 대한 유일한 인자입니다.

React 컴포넌트 함수는 하나의 인자, 즉 `props` 객체를 받습니다.

```tsx
function Avatar(props) {
  let person = props.person;
  let size = props.size;
  // ...
}
```

</br>

`props` 를 선언할 때 `(` 및 `)` 안에 `{` 및 `}` 쌍을 놓치지 마세요

```tsx
function Avatar({ person, size }) {
	// ...
}
```

해당 문법을 구조 분해 할당이라고 부르며 함수 매개변수의 속성과 동등합니다.

보통은 전체 `props` 자페를 필요로 하지는 않기에, 개별 `props` 로 구조 분해 할당합니다.

</br>
</br>

### prop의 기본값 지정하기

값이 지정되지 않았을 때, `prop` 에 기본값을 주길 원한다면, 변수 바로 뒤에 `=` 과 함께 기본값을 넣어 구조 분해 할당을 해줄 수 있습니다.

```tsx
function Avatar({ person, size = 100 }) {
	// ...
}
```

이제 `Avatar` 컴포넌트가 `size prop` 이 없이 렌더링 된다면, `size` 는 `100` 으로 설정됩니다.

해당 기본 값은 `size prop`이 없거나 `size={undefined}` 로 전달될 때 사용됩니다.

그러나 `size={null}` 또는 `size={0}` 으로 전달된다면, 기본값은 사용되지 않습니다.

</br>
</br>

### JSX spread 문법으로 props 전달하기

때때로 전달되는 `props` 는 반복적입니다.

반복적인 코드는 가독성을 높일 수 있다는 점에서 잘못된 것은 아니지만, 때로는 간결함이 중요할 때도 있습니다.

```tsx
function Profile({ person, size, isSepia, thickBorder }) {
  return (
    <div className="card">
      <Avatar
        person={person}
        size={size}
        isSepia={isSepia}
        thickBorder={thickBorder}
      />
    </div>
  );
}
```

`Profile` 이 `Avatar` 에서 하는 것처럼, 일부 컴포넌트는 그들의 모든 `props` 를 자식 컴포넌트에 전달합니다.

</br>

`Profile` 컴포넌트에서 `props` 를 직접 사용하지 않기 때문에 보다 간결한 `spread` 문법을 사용하는 것이 합리적일 수 있습니다.

```tsx
function Profile(props) {
  return (
    <div className="card">
      <Avatar {...props} />
    </div>
  );
}
```

이렇게 하면 `Profile` 의 모든 `props` 를 각각의 이름을 나열하지 않고 `Avatar` 로 전달합니다.

하지만 다른 모든 컴포넌트에 해당 구문을 사용한다면 문제가 있는 것으로 `spread` 문법은 제한적으로 사용해야합니다.

이는 종종 컴포넌트들을 분할하여 자식을 JSX로 전달해야 함을 나타냅니다.

</br>
</br>

### 자식을 JSX로 전달하기

내장된 브라우저 태그는 중첩하는 것이 일반적입니다.

```tsx
<div>
	<img />
</div>
```

</br>

때로는 같은 방식으로 자체 컴포넌트를 중첩하고 싶을 때가 있습니다.

```tsx
<Card>
	<Avatar />
</Card>
```

</br>

JSX 태그 내에 콘텐츠를 중첩하면, 부모 컴포넌트는 해당 콘텐츠를 `children` 이라는 `prop` 으로 받을 것입니

다.

예를 들어, 아래의 `Card` 컴포넌트는 `<Avatar />` 로 설정된 `children prop` 을 받아 이를 래퍼 `div` 에 렌더링 할 것입니다.

```tsx
// Avatar.js
import { getImageUrl } from './utils.js';

export default function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}
```

가독성상 현재 내용에 `utils.js`가 메인이 아니기에 코드를 따로 정리하지는 않습니다.

</br>

```tsx
// App.js
import Avatar from './Avatar.js';

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

export default function Profile() {
  return (
    <Card>
      <Avatar
        size={100}
        person={{
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
      />
    </Card>
  );
}
```

`<Card>` 내부의 `<Avatar>` 를 텍스트로 바꾸어 `<Card>` 컴포넌트가 중첩된 콘텐츠를 다음과 같이 감싸게됩니다.

`<Card>` 컴포넌트는 내부에서 무엇이 렌더링 되는지 알 필요는 없습니다.

</br>

`children prop` 을 가지고 있는 컴포넌트는 부모 컴포넌트가 임의의 JSX로 채울 수 있는 구멍이 있는 것으로 생각하면 이해가 쉽습니다.

패널, 그리드 등의 시각적 래퍼에 종종 `children prop` 를 사용합니다.

즉 `children prop` 을 사용하면 컴포넌트 뿐만 아니라 컴포넌트의 재사용성을 높이기위해 외부에서 주입받는 모든 콘텐츠들을 사용할 수 있습니다,

</br>
</br>

### 시간에 따라 props가 변하는 방식

아래의 `Clock` 컴포넌트는 부모 컴포넌트로부터 `color` 와 `time` 이라는 두 가지 `props` 를 받습니다.

```tsx
export default function Clock({ color, time }) {
	return (
		<h1 style={{ color: color }}>
			{time}
		</h1>
	);
}
```

해당 예시는 `props` 가 항상 고정되어 있지 않고, 컴포넌트가 시간에 따라 다른 `props` 를 받을 수 있음을 보여줍니다.

`time prop` 은 매초 변경되고, `color prop` 은 다른 색상을 선택하면 변경됩니다.

`props` 는 컴포넌트의 데이터를 처음에만 반영하는 것이 아니라 모든 시점에 반영합니다.

</br>

`props` 는 컴퓨터 과학에서 변경할 수 없다라는 의미의 불변성을 가지고 있습니다.

사용자의 상호작용이나 새로운 데이터에 대한 응답으로 컴포넌트가 `props` 를 변경해야 하는 경우, 부모 컴포넌트에 다른 `props`, 즉 새로운 객체를 전달하도록 요청해야 합니다.

그러면 이전의 `props` 는 버려지고, 결국 자바스크립트 엔진은 기존 `props` 가 차지했던 메모리를 회수하게 됩니다.

</br>

`props` 변경을 시도하면 안됩니다.

선택한 색을 변경하는 등 사용자 입력에 반응해야 하는 경우에는 `set state`를 사용해야합니다.

</br>