### 중괄호가 있는 JSX 안에서 자바스크립트 사용하기

JSX를 사용하면 JavaScript 파일에 HTML과 비슷한 마크업을 작성하여 렌더링 로직과 콘텐츠를 같은 곳에 놓을 수 있습니다.

JavaScript 로직을 추가하거나 해당 마크업 내부의 동적인 프로퍼티를 참조하는 상황에서는 JSX에서 중괄호를 사용하여 JavaScript를 사용할 수 있습니다.

</br>
</br>

### 따옴표로 문자열 전달하기

문자열 어트리뷰트를 JSX에 전달하려면 작은따옴표나 큰따옴표로 묶어야 합니다.

```tsx
export default function Avatar() {
  return (
    <img
      className="avatar"
      src="https://i.imgur.com/7vQD0fPs.jpg"
      alt="Gregorio Y. Zara"
    />
  );
}
```

다음 코드는 `"https://i.imgur.com/7vQD0fPs.jpg"` 와 `"Gregorio Y. Zara"` 가 문자열로 전달되고 있습니다.

</br>

`src` 또는 `alt` 를 동적으로 지정하려면 `“` 와 `“` 를 `{` 와 `}` 로 바꿔 JavaScript의 값을 사용할 수 있습니다.

```tsx
export default function Avatar() {
  const avatar = 'https://i.imgur.com/7vQD0fPs.jpg';
  const description = 'Gregorio Y. Zara';
  return (
    <img
      className="avatar"
      src={avatar}
      alt={description}
    />
  );
}
```

다음 코드와 같이 중괄호를 사용하면 마크업에서 바로 JavaScript를 사용할 수 있습니다.

</br>
</br>

### 중괄호 사용하기: JavaScript 세계로 연결하는 창

JSX는 JavaScript를 작성하는 특별한 방법입니다.

중괄호 `{}` 사이에서 JavaScript를 사용할 수 있습니다.

```tsx
export default function TodoList() {
  const name = 'Gregorio Y. Zara';
  return (
    <h1>{name}'s To Do List</h1>
  );
}
```

다음과 같이 `name` 을 선언한 다음 `<h1>` 내부에 중괄호로 포함합니다.

</br>

`formatData()` 와 같은 함수 호출을 포함해 모든 JavaScript 표현식은 중괄호 사이에서 작동합니다.

```tsx
const today = new Date();

function formatDate(data) {
	return new Intl.DateTimeFormat(
		'en-US',
		{ weekday: 'long' }
	).format(date);
}

export default function TodoList() {
	return (
		<h1>To Do List for {formatDate(today)}</h1>
	);
}
```

</br>
</br>

### 중괄호를 사용하는 곳

JSX 안에서 중괄호는 두 가지 방법으로만 사용할 수 있습니다.

- **JSX 태그 안의 문자:**
    - `<h1>{name}'s To Do List</h1>` 는 작동하지만, `<{tag}>Gregorio Y. Zara's To Do List</{tag}>` 는 작동하지 않습니다.
- **`=` 바로 뒤에 오는 어트리뷰트:**
    - `src={avatar}` 는 `avatar` 변수를 읽지만 `src="{avatar}"` 는 `"{avatar}"` 문자열을 전달합니다.

</br>
</br>

### 이중 중괄호 사용하기: JSX의 CSS와 다른 객체

JSX에는 문자열, 숫자 및 기타 JavaScript 표현식뿐만 아니라 객체를 전달할 수도 있습니다.

또한 객체는 `{ name: “Hedy Lamarr”, inventions: 5 }` 처럼 중괄호로 표시됩니다.

따라서 JSX에서 객체를 전달하려면 `person={{ name: “Hedy Lamarr”, inventions: 5 }}` 와 같이 다른 중괄호 쌍으로 객체를 감싸야 합니다.

</br>

JSX의 인라인 CSS 스타일에서도 볼 수 있습니다.

React에서 CSS class는 대부분 잘 작동하기에 인라인 스타일을 사용할 필요가 없지만 인라인 스타일이 필요할 때 `style` 어트리뷰트에 객체를 전달해야 합니다.

```tsx
export default function TodoList() {
  return (
    <ul style={{
      backgroundColor: 'black',
      color: 'pink'
    }}>
      <li>Improve the videophone</li>
      <li>Prepare aeronautics lectures</li>
      <li>Work on the alcohol-fuelled engine</li>
    </ul>
  );
}
```

</br>

위 코드의 `<ul>` 태그를 보면 다음과 같은 JavaScript 객체를 볼 수 있습니다.

```tsx
<ul style={
  {
    backgroundColor: 'black',
    color: 'pink'
  }
}>
```

JSX에서 `{{` 와 `}}` 를 본다면 JSX 중괄호 안의 객체에 불과하다는 것을 알아야 합니다.

인라인 `style` 프로퍼티는 캐멀 케이스로 작성됩니다.

예를 들어 HTML에서의 `<ul style="background-color: black">` 은 컴포넌트에서 `<ul style={{ backgroundColor: 'black' }}>` 로 작성됩니다.

</br>
</br>

### JavaScript 객체와 중괄호에 대해서 더 알아보기

여러 표현식을 하나의 객체로 옮기고 중괄호 안의 JSX에서 참조할 수 있습니다.

아래 예시에서 `person` 객체는 `name` 문자열과 `theme` 객체를 포함합니다.

```tsx
const person = {
  name: 'Gregorio Y. Zara',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>{person.name}'s Todos</h1>
      <img
        className="avatar"
        src="https://i.imgur.com/7vQD0fPs.jpg"
        alt="Gregorio Y. Zara"
      />
      <ul>
        <li>Improve the videophone</li>
        <li>Prepare aeronautics lectures</li>
        <li>Work on the alcohol-fuelled engine</li>
      </ul>
    </div>
  );
}
```

</br>

컴포넌트는 `person` 값을 아래와 같이 사용할 수 있습니다.

```tsx
<div style={person.theme}>
  <h1>{person.name}'s Todos</h1>
```

이처럼 JSX는 JavaScript를 사용하여 데이터와 논리를 구성할 수 있는 매우 작은 템플릿 언어입니다.

</br>

```tsx
const person = {
  name: 'Gregorio Y. Zara',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  },
  image: "https://i.imgur.com/7vQD0fPs.jpg"
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>{person.name}'s Todos</h1>
      <img
        className="avatar"
        src={person.image}
        alt="Gregorio Y. Zara"
      />
      <ul>
        <li>Improve the videophone</li>
        <li>Prepare aeronautics lectures</li>
        <li>Work on the alcohol-fuelled engine</li>
      </ul>
    </div>
  );
}
```

</br>