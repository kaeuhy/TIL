### JavaScript XML를 공부해야 하는 이유

JSX는 자바스크립트 코드에서 HTML과 유사한 구문을 사용하여 UI를 선언적으로 설계하는 문법 확장을 말함

JSX는 HTML과 거의 흡사한 문법 덕분에 개발자들은 복잡한 UI 구조도 쉽게 구성할 수 있음

→ 상태 관리 로직과 비즈니스 로직에 더 집중할 수 있게 함

</br>

하지만 JSX의 동작 원리에 대한 이해 없다면 다음과 같은 한계에 부딪힘

- 예기치 않은 렌더링 오류가 발생했을 때 디버깅이 어려워짐
- 불필요한 리렌더링을 방지하는 성능 최적화 전략을 세우기 힘들어짐

그렇기에 JSX가 어떻게 UI의 근본적인 구조를 만들고 리액트 런타임과 상호작용하는지 깊이 있게 파고들어야 함

</br>
</br>

### DSL과 JSX 알아보기

소프트웨어 개발에는 특정 문제 영역에 최적화된 도구를 사용하는 것이 효율적인 떄가 많음

이런 도구를 특정 도메인의 문제를 해결할 목적으로 설계된, 간결하고 직관적인 문법을 가진 DSL(Domain specific language)라고 부름

예) CSS는 스타일링이라는 UI 표현 도메인에 특화된 언어, SQL은 데이터 질의/조작이라는 데이터베이스 도메인에 특화된 언어

DSL은 크게 외부 DSL(external DSL)과 내부 DSL(internal DSL)로 나뉨

</br>
</br>

#### 외부 DSL

독자적인 문법과 파서를 가지며, 별개의 파일에서 독립적으로 실행되는 언어

→ 파서는 텍스트 형태의 코드를 문법적으로 분해하여 컴퓨터가 즉시 처리할 수 있는 구조적 데이터로 바꿔주는 도구임

- **YAML**
    - YAML은 설정 및 구성이라는 인프라/운영 도메인에 특화된 외부 DSL
- **SQL**
    - SQL은 데이터 질의/조작이라는 데이터베이스 도메인에 특화된 외부 DSL
- **CSS**
    - CSS는 스타일링이라는 UI 표현 도메인에 특화된 외부 DSL

</br>

HTML은 그 자체로 외부 DSL이며 브라우저가 직접 해석할 수 있음

하지만 HTML만으로는 정적인 구조만 표현할 수 있고, 동적인 UI를 만들려면 자바스크립트에서 DOM을 명령형으로 조작해야함

→ 이 두 언어의 경계는 개발 과정을 번거롭게 만듦

JSX를 통해 이러한 문제점을 해결

</br>
</br>

#### 내부 DSL

범용적인 프로그래밍 언어의 문법과 기능을 확장하여, 새로운 언어처럼 보이게 만들며, 특정 기능을 수행하는 역할을 하는 언어

- **RSpec**
    - 루비의 테스트 프레임워크
- **LINQ**
    - 데이터를 쉽게 찾을 목적으로 만든 LINQ
- **JSX**
    - 자바스크립트에서 파생된 내부 DSL

</br>
</br>

### JSX를 구성하는 요소

JSX는 자바스크립트 표준이 아니기 때문에 브라우저는 JSX 문법을 직접 해석하지 못함

바벨과 같은 트랜스파일러를 사용해 JSX를 자바스크립트로 변경해야 함

이번 섹션에서는 JSX를 구성하는 핵심 요소와 주요 문법 규칙을 살펴볼 것임

</br>
</br>

### JSX에서의 요소들

JSX에서 UI를 구성할 때 사용하는 기본 단위들을 이 글에서는 JSX 요소라고 부름

HTML이나 XML에서 하나의 태그가 화면 구조를 만드는 벽돌 역할을 하듯, JSX에서도 이런 요소들이 모여 UI를 선언적으로 쌓아 올리게 됨

JSX 코드가 파싱될 때 이 JSX 요소들은 크게 JSXElement, JSXFragment, JSXElementName 세 가지 형태로 나눌 수 있음

</br>
</br>

#### **JSXElement**

JSX에서 작성된 개별 UI 구성 요소로, 리액트 컴포넌트로 변환되어 렌더링되는 기본 단위임

하나의 JSXElement는 보통 다음 세 가지로 구성된 그룹으로 볼 수 있음.

- JSXOpeningElement
- JSXChildren
- JSXClosingElement

이 세 가지가 합쳐져서 하나의 JSXElement를 이루고, 경우에 따라 JSXSelfClosingElement 형태로도 나타남

</br>
</br>

#### **JSXElement의 기본 구조**

일반적인 JSXElement는 다음과 같이 생김

```jsx
function Button2() {
  return (
    <CustomButton>
      Golden Rabbit
    </CustomButton>
  );
}
```

- JSXOpeningElement
    - `‎<CustomButton>`
- JSXChildren
    - `Golden Rabbit`
    - 단순 문자열뿐 아니라 다른 JSXElement, 표현식, 중첩된 컴포넌트 등이 올 수 있음
- JSXClosingElement
    - `</CustomButton>`

이렇게 세 부분으로 이루어진 하나의 JSXElement가 됨

</br>
</br>

#### **JSXSelfClosingElement**

일반적인 JSXElement 구조말고도 자식 없이 자기 자신으로 닫히는 형태도 있음

```jsx
// 1번
<div className="container">
  <h1>Hello, JSX!</h1>
  // 2번
  <MyComponent prop="value" />
</div>
// 3번
<img src="logo.png" alt="Logo" />
```

- **1번 형태**
    - JSXOpeningElement
        - `<div className="container">`
    - JSXChildren
        - `<h1>Hello, JSX!</h1>` , ‎`<MyComponent prop="value" />`
    - JSXClosingElement
        - `‎</div>`
- **2번 형태**
    - 커스텀 컴포넌트이지만, 태그가 하나로 닫혀 있기 때문에 JSXSelfClosingElement
- **3번 형태**
    - HTML의 img 태그를 그대로 사용한 JSXSelfClosingElement이며, 자식을 가질 수 없음

</br>

여기서 중요한 규칙은 리액트에서 커스텀 컴포넌트는 반드시 대문자로 시작해야 한다는 것

```jsx
<camelCase />
```

다음과 같이 잘못 작성하면

</br>

콘솔에는 다음 경고가 출력

```jsx
Warning: The tag <camelCase> is unrecognized in this browser. 
If you meant to render a React component, start its name with an uppercase letter.
```

- **소문자 시작 태그**
    - 브라우저에 실제로 존재하는 HTML 태그로 인식
- **대문자 시작 태그**
    - 리액트 컴포넌트로 인식

</br>
</br>

#### **JSXChildren과 props.children**

JSXChildren 태그 안에 들어가는 내용은 트랜스파일 과정에서 ‎`children` 이라는 프로퍼티로 묶여서 컴포넌트에 전달됨

→ 리액트 컴포넌트 내부에서는 ‎`props.children` 으로 이 JSXChildren에 접근할 수 있음

```jsx
function Button(props) {
  return (
    <button
      className={props.className}
      onClick={props.onClick}
    >
      {props.children ?? "Golden Rabbit"}
    </button>
  );
}

function Page() {
  return (
    <Button
      className="primary"
      onClick={console.log}
    />
  );
}
```

- `‎Button` 컴포넌트는 JSXChildren을 ‎`props.children` 으로 받음
    - 만약 부모가 children을 넘겨주지 않았다면 ‎Golden Rabbit을 렌더링
    - `props.children`의 유무에 따라 유연하게 처리할 수 있음
- ‎`Page` 에서 `‎Button` 컴포넌트를 Self Closing 형태로 사용
    - JSXChildren이 없고, 따라서 ‎`props.children` 도 없음
    - 그 결과 ‎Golden Rabbit이 버튼 내부에 표시됨

</br>

#### JSXElement 구조를 실제 코드에서 보는 방법 - 트랜스파일 결과

지금까지 개념적인 구조를 봤으니, 실제 자바스크립트 코드로 어떻게 바뀌는지 확인해 보면 더 잘 이해할 수 있음

JSX는 브라우저가 바로 이해할 수 있는 문법이 아니기에, 빌드 과정에서 `jsx()` 함수 호출 형태로 트랜스파일됨

</br>

기본 HTML 태그를 사용한 JSX 예시를 보면

```jsx
// 트랜스파일링 전
function Button() {
  return (
    <div>
      Golden Rabbit
    </div>
  );
}

// 트랜스파일링 후
function Button(e) {
  return nn.jsx("div", {
    className: e.className,
    onClick: e.onClick,
    children: e.children,
  });
}
```

- `‎nn.jsx("div", { ... })`
    - 첫 번째 인자 `‎div`
        - JSXOpeningElement가 ‎`<div>` 처럼 기본 태그일 때, 내부적으로는 ‎`div`, ‎`p`, ‎`h1` 같은 문자열 태그 이름으로 표현됨
    - 두 번째 인자 객체
        - JSXOpeningElement에 적었던 속성들(‎`className` , ‎`onClick` 등)이 그대로 들어감
        - JSXChildren에 해당하는 내용은 ‎`children` 프로퍼티로 들어감

즉, JSXOpeningElement / JSXChildren이 트랜스파일시 첫 번째 인자와 두 번째 인자의 props/children 객체로 대응된다고 볼 수 있음

</br>

커스텀 컴포넌트의 트랜스파일 결과 예시를 보면

```jsx
// 트랜스파일링 전
function Button2() {
  return (
    <Button>
      Golden Rabbit
    </Button>
  );
}

// 트랜스파일링 후
function Button2() {
  return En.jsx(Button, { children: "Golden Rabbit" });
}

```

- `nn.jsx("div", { ... })`
    - 첫 번째 인자가 문자열 ‎`div`
- `En.jsx(Button, { ... })`
    - 첫 번째 인자가 문자열이 아니라 컴포넌트 식별자 ‎`Button`

즉, JSXOpeningElement가 `‎<div>` 같은 HTML 태그라면 트랜스파일 시 ‎`div` 같은 문자열로, JSXOpeningElement가 ‎`<Button>` 같은 커스텀 컴포넌트라면 트랜스파일 시 ‎`Button` 같은 식별자로 표현된다고 볼 수 있음. 

</br>
</br>

### JSXFragment

JSX에서 부모 요소 없이 여러 가지 요소를 그룹화하에 사용되는 특별한 문법으로, 리액트에서는 `<>` `</>` 와 같이 빈 태그로 표현함

```jsx
// 잘 못된 형태
export const JSEFragmentNotValid = () => {
	return (
		<li>1</li>
		<li>2</li>
	)
}
```

다음과 같이 JSX를 작성할 떄 각 컴포넌트의 최상위 레벨의 엘리먼트는 여러 개의 엘리먼트로 작성할 수 없음

</br>

두 개 이상의 태그를 최상위 레벨에 위치하고 싶다면 JSXFragment를 사용해야 함

```jsx
// 단축 형태
<>
	<li>First item</li>
	<li>Second item</li>
</>

// 명시적 Fragment 사용
<React.Fragment>
	<li>First item</li>
	<li>Second item</li>
</React.Fragment>
```

`<>` `</>` 는 `<Fragment>` `</Fragment>` 를 단축한 형태

단, ‎`<>` `</>` 는 단축 문법이라서 ‎props를 줄 수 없기에 props를 사용하고 싶다면 이름이 있는 형태인 `<Fragment>` 를 사용해야함

</br>
</br>

### JSXElementName

JSX에서 태그 이름 부분에 들어가는 것을 통칭해서 JSXElementName이라고 부름

`<div>` , `<span>` 같은 태그 이름이 전부 해당함

JSXElementName은 세 가지 형태로 나눌 수 있음

- JSXIdentifier
- JSXMemberExpression
- JSXNamespacedName

</br>
</br>

#### JSXIdentifier

가장 기본적인 형태의 JSXElementName으로, 단일 식별자로 이루어진 태그 이름을 의미함

```jsx
<div>내용</div>
<span>텍스트</span>
<GoldenRabbit>콘텐츠</GoldenRabbit>
```

다음 코드에서 `<div>` , `<span>` , `<GoldenRabbit>` 이 해당됨

</br>

JSXIdentifier는 일반 자바스크립트 식별자가 가질 수 있는 문자들을 대부분 허용하지만, JSX 문법과 섞여 있을 때 주로 다음 규칙들을 지킴

- 알파벳, 숫자, ‎`_`, ‎`$` 등
- 숫자로 시작할 수는 없음
- 하이픈은 식별자 이름으로는 사용할 수 없으므로 ‎`Button-Special` 같은 이름은 함수/컴포넌트 이름으로는 불가

예시를 보면

```jsx
function $SpecialButton() {
  return <button>특수 버튼</button>;
}

function 한국컴포넌트() {
  return <div>한글이름을 가진 컴포넌트</div>;
}

function π지수컴포넌트() {
  return <div>π = 3.14159...</div>;
}
```

위 함수 이름들은 모두 자바스크립트에서 유효한 식별자이기 때문에 JSXIdentifier로도 사용 가능함

하지만 실무 코드 스타일, 협업, 가독성 측면에서는 영어 알파벳과 의미 있는 네이밍을 쓰는 것이 일반적임

</br>

반대로 다음과 같은 이름은 문제가 됨

```jsx
function Button-Special() {
	return <button>...</button>;
}
```

하이픈은 자바스크립트 식별자로 유효하지 않기 때문임

즉, JSXElementName에 들어가는 값은 자바스크립트 식별자 규칙을 따르는 JSXIdentifier여야 한다는 점이 핵심임

</br>
</br>

#### JSXIdentifier와 유니코드 이스케이프

JSXIdentifier는 일반 문자뿐 아니라 유니코드 이스케이프를 활용한 식별자도 혀용함

다음 예시를 보면

```jsx
const \u03A9mega = "Omega";

function Omega() {
  return (<div>\u03A9mega</div>);
}
```

JSX 안에서 ‎`<div>\u03A9mega</div>` 처럼 문자열 내에 유니코드 이스케이프를 사용하는 것도 가능함

이처럼 JSXIdentifier는 자바스크립트 식별자 규칙과 동일하게 유니코드 기반 식별자도 인정하지만, 가독성과 협업을 생각하면 최소한으로 사용하는 것이 좋음

</br>
</br>

#### JSXMemberExpression

JSXMemberExpression은 점을 이용해서 컴포넌트를 네임스페이스처럼 계층 구조로 표현하는 형태의 JSXElementName 

예를 들면

```jsx
<GoldenRabbit />
<GoldenRabbit.Ear />
<GoldenRabbit.Ear.Ball />
```

- `<GoldenRabbit />`
    - JSXIdentifier
- `‎<GoldenRabbit.Ear />`
    - `GoldenRabbit.Ear` 라는 JSXMemberExpression
- `‎<GoldenRabbit.Ear.Ball />`
    - `GoldenRabbit.Ear.Ball` 이라는 JSXMemberExpression

이 모든 것이 최종적으로는 JSXElementName으로 인정됨

</br>

JSXMemberExpression은 보통 다음과 같은 용도로 쓰임

먼저 알아 볼 것은 컴포넌트 묶음을 표현하는 패턴임

```jsx
const GoldenRabbit = () => <div>A golden rabbit</div>;
// 서브 컴포넌트
GoldenRabbit.Ear = () => <div>Ear</div>;
GoldenRabbit.Ear.Ball = () => <div>Ear Ball</div>;
```

위와 같이 하나의 컴포넌트에 서브 컴포넌트를 프로퍼티로 매달아 둠

점 표기법으로 접근하는 이런 패턴은 디자인 시스템, 컴포넌트 라이브러리에서 자주 사용됨

</br>

다음은 시각적, 구조적 구분임

컴포넌트 이름을 계층적으로 표현함으로써, 해당 컴포넌트가 어떤 부모 개념 아래에 속하는지 명확하게 드러낼 수 있음.

→ 네임스페이스 + 서브 컴포넌트처럼 구조화된 이름으로 표현하는 역할을 함

</br>
</br>

#### JSXNamespacedName

JSXNamespacedName은 콜론을 사용한 네임스페이스 형태의 `JSXElementName` 

XML이나 SVG에서 사용하는 네임스페이스 문법과 비슷한 구조를 JSX에서 표현할때 사용함

예를 들어, XML 스타일 코드를 보면

```jsx
var xml = <ns:root xmlns:ns="http://example.com/ns">
  <ns:child>Content</ns:child>
</ns:root>;
console.log(xml.ns:child);
```

여기서 ‎`<ns:root>`, ‎`<ns:child>` 같은 태그 이름이 네임스페이스를 가진 요소들로, JSX 문법 측면에서 보면 ‎`ns:root`, ‎`ns:child`는 JSXNamespacedName 형태의 JSXElementName이 됨

</br>

SVG 예시로 보면

```jsx
<svg viewBox="0 0 160 40" xmlns="http://www.w3.org/2000/svg">
  <xlink:href="https://goldenrabbit.co.kr/">
    <text x="10" y="25">Golden Rabbit</text>
  </xlink:href>
</svg>
```

위의 ‎`<xlink:href>`처럼 ‎`xlink:...` 형태로 네임스페이스를 나타내는 패턴 역시 JSXNamespacedName에 해당함

단, 리액트의 일반적인 JSX 사용에서는 JSXNamespacedName을 거의 사용하지 않음

</br>

실제 리액트 코드에서는 네임스페이스 대신 JSX 속성/컴포넌트 조합을 사용함

```jsx
<svg viewBox="0 0 160 40" xmlns="http://www.w3.org/2000/svg">
  <a xlinkHref="https://goldenrabbit.co.kr/">
    <text x="10" y="25">Golden Rabbit</text>
  </a>
</svg>
```

</br>
</br>

### JSXAttributes

JSX에서 Attributes는 요소나 컴포넌트에 추가 정보를 전달하는 수단이고, HTML의 속성과 리액트 컴포넌트의 `props` 를 관통하는 개념으로 볼 수 있음

다음과 같은 형태로 나타남
→ `<div className=”box”>` , `<Button disabled />` , `<Icon name=”add” />` , `<button {…baseProps} />` 

</br>

문법적으로는 대략 다음과 같이 나눌 수 있음

- JSXAttribute
- JSXAttributeName
- JSXAttributeInitializer
- JSXAttributeValue
- JSXSpreadAttribute

</br>
</br>

#### JSXAttribute 기본 개념

가장 기본적인 JSX 속성은 이름-값 형태임

```jsx
<input type="text" placeholder="입력하세요" className="input-field" />
<button className="btn" id="submit-btn">제출</button>
```

`type="text"`, ‎`placeholder="입력하세요"`, ‎`className="input-field"`, `className="btn"`, `‎id="submit-btn”` 이 해당함

</br>

JSXAttribute는 다시 이렇게 나뉨

- JSXAttributeName
    - `type` , `placeholder` , `className` , `id`
- JSXAttributeInitializer + JSXAttributeValue
    - `=”text”` , `=”입력하세요”` , `=”input-field”` , `=”btn”` , `=”submit-btn”`

JSX 입장에서는 이름 + 초기화 + 값 구조라고 이해하면 됨

</br>
</br>

#### String Literal Attributes

가장 자주 쓰는 형태는 문자열 리터럴을 값으로 가지는 `JSXAttribute` 임

```jsx
function StringLiteralAttributes() {
	return (
		<div>
			<input
				type="text"
				placeholder="입력하세요"
				className="input-field"
			/>
			<button
				className="btn"
				id="submit-btn"
			>
				제출
			</button>
		</div>
	);
}
```

`“…”` 로 감싸진 상태가 문자열 리터럴이 해당함

</br>

이 패턴의 특징은 다음과 같음

- HTML에서 쓰던 속성들을 거의 그대로 가져와도 동작함
- `class` 대신 `className` , `for` 대신 `htmlFor` 처럼 리액트에서 바뀌는 몇몇 속성 이름만 주의하면 됨
- 문자열이기 때문에 정적인 값 설정에 적함함

</br>
</br>

#### Boolean Attributes

JSX에서도 불리언 값도 속성으로 많이 사용됨

```jsx
function BooleanAttributes() {
  return (
    <div>
      <input
        type="checkbox"
        checked={true}
        disabled={false}
        readOnly={true}
      />
    </div>
  );
}
```

`checked={true}` , `disabled={false}` , `readOnly={true}` 이 해당함

</br>

특징을 정리하면 다음과 같음

- JSX에서 ‎`isActive={true}` 라고 쓰면 JS 입장에서 ‎`props.isActive === true` 인 것과 동일
- 단축형으로 ‎`isActive` 만 써도 ‎`isActive={true}` 와 동일하게 동작하지만, 코드 스타일에 따라 명시적으로 ‎`={true}` 를 쓰기도 함
- HTML의 `disabled` , ‎`checked` 같은 속성이 JSX에서는 실제로는 ‎`true` / `false` 값으로 다뤄진다고 보면 됨

</br>
</br>

#### JSXAttributeValue로서 JSX

JSXAttributeValue는 단순 문자열이나 불리언뿐 아니라 다른 JSX 요소나 Fragment도 값으로 가질 수 있음

→ 속성에 `JSXElement` 를 통째로 넘길 수도 있다는 말

```jsx
function JSXAttributeValue() {
  const Icon = ({ name }) => <span className={`icon-${name}`} />;

  return (
    <div>
      <button
        type="button"
        // JSX 요소를 속성 값으로 전달
        icon={<Icon name="add" />}
      >
        추가하기
      </button>

      <Tooltip
        header={<h3>도움말</h3>}
        content={
          <>
            <p>이 버튼을 클릭하면 새로운 항목이 추가됩니다.</p>
            <p>추가 설명이 여기에 들어갑니다.</p>
          </>
        }
      />
    </div>
  );
}
```

- `icon={<Icon name="add" />}`
    - JSXAttributeValue가 ‎`<Icon name="add" />` 라는 JSXElement
- `‎header={<h3>도움말</h3>}`
    - JSXAttributeValue가 ‎`<h3>도움말</h3>`
- `‎content={<> ... </>}`
    - JSXAttributeValue가 Fragment `(‎<>...</>)`

</br>

이처럼 JSXAttributeValue는 문자열 리터럴, 불리언, 숫자, 객체, 함수 등 JS 값, JSXElement, JSXFragment 까지 폭 넓게 담을 수 있음

컴포넌트는 이를 `props.icon` , `props.header` , `props.content` 등으로 그대로 받아서 렌더링에 활용할 수 있음

</br>
</br>

#### JSXSpreadAttribute

JSX에서 `…` 문법은 JSXSpreadAttribute라고 부르며, 객체에 들어 있는 여러 속성을 한 번에 펼쳐서 JSXAttributes로 전달하는 문법임

```jsx
function SpreadAttributeExample() {
  const buttonProps = {
    type: 'button',
    className: 'btn',
    onClick: () => alert('버튼 클릭됨'),
  };

  return (
    <div>
      <button {...buttonProps}>스프레드 버튼</button>

      <button>기본 버튼</button>

      <button
        {...buttonProps}
        id="special-btn"
      >
        특수 버튼
      </button>
    </div>
  );
}
```

- `‎<button {...buttonProps}>`
    - ‎`buttonProps` 객체 안의 ‎`type` , ‎`className` , ‎`onClick` 이 각각 JSXAttribute로 스프레드되어 전달
- `‎<button {...buttonProps} id="special-btn">`
    - ‎`buttonProps` 에 더해 ‎`id="special-btn"` 이라는 추가 속성을 함께 사용

</br>

JSXSpreadAttribute는 다음 특징이 있음

- 공통 속성을 객체에 모아 두고 여러 요소, 컴포넌트에 재사용할 수 있음
- 스프레드 위치에 따라 우선순위가 달라짐

```jsx
function MixedAttributesExample() {
  const baseProps = {
    className: 'base-class',
    style: { color: 'blue' },
    onClick: () => console.log('baseProps click'),
  };

  return (
    <div>
      {/* 1. 스프레드 후 개별 속성으로 덮어쓰기 */}
      <button
        {...baseProps}
        className="override-class"
        onClick={() => alert('새로 클릭 이벤트')}
      >
        버튼 1
      </button>

      {/* 2. 먼저 개별 속성을 쓰고 뒤에서 스프레드로 덮어쓰기 */}
      <button
        className="will-be-overridden"
        {...baseProps}
      >
        버튼 2
      </button>

      {/* 3. 중간에 스프레드를 끼워넣은 경우 */}
      <button
        {...baseProps}
        className="middle-class"
        {...{ className: 'final-class' }}
      >
        버튼 3
      </button>
    </div>
  );
}
```

- **버튼 1**
    - `…baseProps` 적용 후 `className` , `onClick` 을 별도 JSXAttribute로 다시 지정
    - 나중에 지정한 속성이 이전 스프레드 값을 덮어씀
    - 최종적으로 `className=”override-class”` , `onClick=새 이벤트`
- **버튼 2**
    - 먼저 `className=”overrider-class”` 을 지정하고 그 뒤에 `…baseProps` 를 적용
    - `baseProps.className` 이 뒤에서 덮어쓰면서 최종적으로 `className=”base-class”`
- **버튼 3**
    - 가장 마지막 스프레드의 값이 적용되기에 최종적으로 `className=”final-class”`

즉, JSXAttributes + JSXSpreadAttribute는 왼쪽에서 오른쪽으로 읽히는 순ㄴ서대로 적용되며, 같은 이름의 속성이 있으면 나중에 나온 속성이 적용됨

</br>
</br>

### JSXChildren

JSX에서 JSXChildren은 컴포넌트나 요소 태그 안에 들어가는 모든 내용을 말함

`<Parent>여기 들어가는 것 전부</Parent>` 에서 ‎`여기 들어가는 것 전부` 가 JSXChildren에 해당함

JSXChildren은 다음과 같은 형태들로 이뤄질 수 있음

→ JSXCHildren = JSXText | JSXElement | JSXFragment | JSXChildExpression, 여러 개가 연속해서 섞여 나올 수 있음

조합 방식에 따라 다양한 UI를 만들 수 있고, 리액트 컴포넌트 트리를 안에서부터 구성하는 핵심 개념이라고 볼 수있음

</br>
</br>

#### JSXChildExpression

JSXChildren 중에서도 중괄호 안에 들어가는 자바스크립트 표현식을 의미함

```jsx
export const JSXExpressionExample = () => {
  const name = '홍길동';
  const age = 25;
  const hobbies = ['코딩', '독서', '운동'];

  return (
    <div className="example">
      <h2>JSXChildExpression 예제</h2>

      {/* 단순 변수 사용 */}
      <p>이름: {name}</p>
      <p>나이: {age}</p>

      {/* 배열 렌더링 */}
      <h3>취미 목록</h3>
      <ul>
        {hobbies.map((hobby) => (
          <li key={hobby}>{hobby}</li>
        ))}
      </ul>
    </div>
  );
};
```

여기서 JSXChildren 안에 들어 있는 ‎`{name}` , ‎`{age}` , ‎`{hobbies.map(...)}` 가 전부 JSXChildExpression

</br>

JSXChildExpression에서는 단순 변수, 연산 결과, 함수 호출 결과 등 임의의 JS 표현식을 쓸 수 있음

배열을 `map` 메서드를 사용하여 JSXElement 리스트로 바꿔 렌더링할 수 있고, 렌더링 시 문자열, 숫자는 그대로 화면에 출력됨

→ `true` , `false` , `null` , `undefined` 는 자리만 차지하고 화면에는 보이지 않음

이 덕분에 JSXChildren은 단순한 고정 텍스트가 아니라, 자바스크립트 값과 로직에 따라 동적으로 결정되는 UI 조각이 됨

</br>
</br>

#### JSXChildren에서의 조건부 렌더링

대표적으로 `&&` , 삼항 연산자, `if` 분기 + 조기 반환 패턴 등으로 많이 사용함

```jsx
<div className="notification-example">
  <h4>알림 메시지: {count}</h4>

  <div className="incorrect">
    {/* 잘못된 예: count가 0이면 0이 그대로 렌더링됨 */}
    {!count && <p>새로운 알림이 없습니다.</p>}
  </div>

  <div className="correct">
    {/* 올바른 예: count > 0일 때만 메시지를 렌더링 */}
    {count > 0 && <p>{count}개의 새로운 알림이 있습니다.</p>}
  </div>

  <button onClick={() => setCount((prev) => prev + 1)}>
    {count === 0 ? '알림 추가' : '알림 더 추가'}
  </button>
</div>

```

리액트 JSX 렌더링 규칙에서는 0은 비어 있지 않은 문자열이기에 그대로 렌더링 됨

그래서 `!count` 을 사용할때 `count` 가 0이면 `!count` 는 `true` 가 되지만, 동시에 0 자체는 화면에 찍힐 수 있음

더 안전한 방법은 올바른 예처럼 명시적으로 조건을 비교하는 것임

</br>
</br>

#### JSXText

JSXChildren 중에서 가장 단순한 형태로, 태그 안에 직접 적힌 텍스트를 의미함

```jsx
<div className="example">
  <h1>JSXText 예제</h1>
  <div>안녕하세요, 코드를 배웁니다.</div>

  {/* 여러 줄의 텍스트도 가능하지만 들여쓰기와 개행은 대부분 무시됩니다 */}
  <div>
    여러 줄의
    텍스트는
    컴포넌트 하나로 렌더링됩니다.
  </div>
</div>
```

`JSXText 예제` , `안녕하세요, 코드를 배웁니다` , `여러 줄의 텍스트는 컴포넌트 하나로 렌더링됩니다` 가 JSXText에 해당

</br>

특징은 다음과 같음

- JSXText는 HTML과 비슷하게 태그 안에 있는 순수 텍스트로 취급됨
- 여러 줄로 나눠서 적어도, 렌더링 시에는 공백, 개행이 정리되어 하나의 텍스트 노드로 처리됨
- 복잡한 동적 값이 아니라, 고정된 설명 문구, 레이블 등을 표시할 때 적합함

</br>
</br>

#### JSXChildren으로서의 JSXElement

JSXChildren 안에는 텍스트뿐 아니라 다른 JSXElement도 들어갈 수 있음

이때 부모-자식 컴포넌트 구조가 자연스럽게 만들어짐

```jsx
<div className="example">
  <h2>JSXElement 예제</h2>
  <Parent>
    <Child text="첫 번째 자식" />
    <Child text="두 번째 자식" />
  </Parent>
</div>
```

`<Parent>` 의 JSXChildren은 두 개의 JSXElement로 구성되어 있음

</br>

위의 구조는 컴포넌트 입장에서 보면 다음처럼 받아들여짐

```jsx
function Parent({ children }) {
	return (
		<div className="parent">
			{children}
		</div>
	);
}
```

JSXChildren은 결국 컴포넌트 입장에서는 `props.children` 으로 전달되는 하위 요소들의 집합

안에는 JSXText, JSXElement, JSXFragment, JSXChildExpression이 섞여 있을 수 있음

</br>
</br>

#### JSXChildren으로서의 JSXFragment

JSXFragment는 여러 JSXChildren을 한 번에 묶어 주지만, 실제 DOM에는 별도의 래퍼 요소를 만들지 않는 특수한 JSXElement 형태임

```jsx
<div className="example">
  <h2>JSXFragment 예제</h2>
  <div className="container">
    <>
      <p>첫 번째 문단</p>
      <p>두 번째 문단</p>
    </>
  </div>
</div>
```

위 코드에서 `<div className=”container”>` 의 JSXChildren은 `<>…</>` 이고, Fragment 내부에는 다시 두 개의 `<p>` 요소가 JSXChildren으로 들어 있음

</br>

렌더링 결과를 보면

```jsx
<div class="container">
  <p>첫 번째 문단</p>
  <p>두 번째 문단</p>
</div>
```

Fragment 자체는 DOM에 나타나지 않는 것을 알 수 있음

</br>
</br>

### JSX Strings

JSX에서 Strings는 화면에 보이는 텍스트를 표현하는 가장 기본 단위이면서, 동시에 HTML 엔티티, 이스케이프 시퀀스, 유니코드(이모지 포함)까지 모두 아우르는 개념으로 볼 수 있음

JSXString은 단순히 ‎문자열만 의미하는 것이 아니라 JSXText, JSXChildren 안의 문자열, 속성 값으로서의 문자열, 그리고 HTML 엔티티/이스케이프 처리 방식까지를 함께 이해해야 활용이 편해짐

</br>
</br>

#### JSX에서의 텍스트와 이스케이프

JSX는 기본적으로 텍스트를 HTML처럼 이스케이프해서 안전하게 렌더링함

즉, JSX 노드 안에 작성된 텍스트는 브라우저에 렌더링될 때 HTML로 실행되지 않고 문자 그대로 표시됨

```jsx
const jsString = '첫 번째 줄\n두 번째 줄';

function EscapeSequenceExample() {
  const isJsTabString = (str) => str.includes('\t');

  return (
    <div className="example">
      <h2>이스케이프 예제</h2>

      <div className="escape-in-jsx">
        <h3>1. JSX 텍스트는 노드에서의 이스케이프 시퀀스</h3>
        <p>&lt;p&gt;태그 안의 &amp;와 &quot;문자&quot;를 안전하게 표시합니다.&lt;/p&gt;</p>
      </div>

      <div className="escape-in-js-vars">
        <h3>2. 자바스크립트 변수에서의 이스케이프 시퀀스</h3>
        <pre>{jsString}</pre>
        <p>위 jsString 안의 \n은 자바스크립트 문자열에서 줄바꿈을 의미하지만, HTML 렌더링에서는 그대로 보이지 않을 수 있습니다.</p>
        <strong>
          변수 안의 이스케이프는 JS 단계에서 처리되고, JSX는 그 결과 문자열만 받습니다.
        </strong>
        <p>탭 문자를 포함하는지 여부: {isJsTabString(jsString) ? '포함됨' : '포함되지 않음'}</p>
      </div>
    </div>
  );
}
```

JSX 텍스트에서의 이스케이프

- `‎<p>` 안에 쓰는 텍스트는 HTML처럼 보이지만, 실제로는 React가 이스케이프 처리해서 안전하게 렌더링
- ‎`&` , ‎`<` , ‎`>`  같은 문자를 직접 쓰면 HTML 파서 때문에 문제가 될 수 있으므로, 필요할 때는 HTML 엔티티사용해 문자 그대로 표시할 수 있음

자바스크립트 변수에서의 이스케이프

- `‎const jsString = "첫 번째 줄\n두 번째 줄";` 처럼 JS 코드에서 문자열 리터럴을 만들 때는 자바스크립트의 이스케이프 규칙을 따름
- JSX에 `‎{jsString}` 으로 넣으면 ‎`\n` 은 실제 줄바꿈으로 렌더링되지 않고, HTML에서는 공백/줄바꿈 정리 규칙에 따라 한 줄처럼 보일 수 있음

위 코드에서 중요한 포인트는 이스케이프 시퀀스 처리는 JS 단계에서 끝나고, JSX는 최종 문자열을 그대로 DOM 텍스트 노드로 출력함

</br>
</br>

#### 줄바꿈을 올바르게 처리하는 세 가지 방법

문자열 안의 `\n` 을 사용했을 때, 브라우저가 이를 우리가 기대하는 것처럼 줄바꿈으로 보여주지 않는 경우가 많음

그래서 JSX Strings에서 줄바꿈을 표시하려면 별도의 처리가 필요함

</br>

첫 번째는 `<br />` 태그 사용임

```jsx
<p>
  첫 번째 줄<br />
  두 번째 줄<br />
  세 번째 줄
</p>
```

텍스트 사이에 `<br />` 을 넣어 줄바꿈을 명시적으로 만들어줌

단, `\n` 과 같이 동적 문자열을 `<br />` 로 바꾸고 싶다면 JS에서 `split('\n’)` 후 `map` 으로 끼워 넣는 식으로 처리해야 함

</br>

두 번째는 CSS `white-space` 속성을 사용하는 것임

```jsx
// JSX
<p className="using-whitespace">
  {jsString}
</p>

// CSS
.using-whitespace {
  white-space: pre-line;
}
```

`white-space: pre-line` 을 사용하면 문자열 안의 `\n` 이 실제 줄바꿈처럼 보임

이 방식은 DOM 구조는 그대로 두고 스타일만으로 줄바꿈 표현을 제어하고 싶을 때 유용함

</br>

마지막 방법은 `dangerouslySetInnerHTML` 을 사용하는 것임

`‎dangerouslySetInnerHTML` 는 문자열 안에 들어 있는 HTML 태그들을 실제 HTML로 해석해서, 그 구조 그대로 DOM에 집어넣고 싶을 때 사용하는 방법임

```jsx
<div
  className="using-innerhtml"
  dangerouslySetInnerHTML={htmlWithLineBreaks}
/>
```

하지만 신뢰할 수 없는 입력에 이 방식을 사용하면 XSS 공격에 취약하기에 정말 필요한 상황에서만 사용해야함

→ 외부에서 들어오는 문자열에 `<script>` , `onerror` 속성, 인라인 이벤트 핸들러 등이 포함되면 그대로 실행될 위험이 있음

</br>
</br>

#### JSXStrings와 HTML 엔티티

JSX 텍스트나 속성 값 안에서 `<` , `>` , `&` , `“` 같은 문자를 그대로 보여 주고 싶을때는 HTML 엔티티를 사용함

```jsx
function HtmlEntityExample() {
  return (
    <div className="example">
      <h2>HTML 엔티티 예제</h2>

      <h3>HTML 특수 문자</h3>
      <p>
        &lt;div&gt;는 HTML의 블록 요소이고, &amp;lt;, &amp;gt;를 사용하면
        꺾쇠 괄호를 문자 그대로 표시할 수 있습니다.
      </p>

      <p>Copyright © 2023 콘텐츠 제작팀</p>
    </div>
  );
}
```

실제 화면에는 엔티티가 아니라 다음과 같이 `‎&lt;` → `‎<` , `&gt;` → ‎`>` , ‎`&amp;` → ‎`&` , `‎&quot;` → `‎"` 로 렌더링됨

</br>
</br>

#### 이모지와 다양한 언어 문자열

JSX는 내부적으로 UTF-16 기반 문자열을 사용하므로, 이모지(서로게이트 쌍 포함), 한글, 라틴 문자, 키릴 문자 등 대부분의 유니코드 문자를 문제 없이 처리함

```jsx
<div className="example">
  <h2>이모지와 다양한 언어들의 렌더링</h2>

  {/* 이모지 사용 */}
  <div className="emoji-examples">
    <h3>1. 이모지 사용</h3>
    <p>추가적인 비트가 시작됩니다.</p>
    <p>❤️ 기능을 선택합니다.</p>
    <p>리프레시합니다.</p>
  </div>

  {/* 다양한 언어의 문자 */}
  <div className="unicode-examples">
    <h3>2. 다양한 언어의 지원</h3>
    <p>안녕하세요</p>
    <p>¿Qué tal?</p>
    <p>Привет</p>
  </div>
</div>
```

</br>