### 컴포넌트

컴포넌트는 사용자 인터페이스(UI)를 구축하는 기반으로React의 핵심 개념 중 하나입니다.

</br>

웹에서는 HTML을 통해 태그를 사용하여 풍부한 구조의 문서를 만들 수 있습니다.

```html
<article>
  <h1>My First Component</h1>
  <ol>
    <li>Components: UI Building Blocks</li>
    <li>Defining a Component</li>
    <li>Using a Component</li>
  </ol>
</article>
```

이와같은 마크업은 스타일을 위한 CSS, 상호작용을 위한 JavaScript와 결합하여 웹이서 볼 수 있는 모든 사이드바, 아바타, 모달, 드롭다운 등 모든 UI의 기반이 됩니다.

</br>

React를 사용하면 마크업, CSS, JavaScript를 앱의 재사용 가능한 UI 요소인 사용자 정의 컴포넌트로 결합할 수 있습니다.

위 코드는 모든 페이지에 렌더링할 수 있는 `<TableOfContents />` 컴포넌트로 전환될 수 있고, 내부적으로는 여전히 동일한 HTML 태그를 사용합니다.

</br>

HTML 태그와 마찬가지로 컴포넌트를 작성, 순서 지정 및 중첩하여 전체 페이지를 디자인할 수 있습니다.

```html
<PageLayout>
  <NavigationHeader>
    <SearchBar />
    <Link to="/docs">Docs</Link>
  </NavigationHeader>
  <Sidebar />
  <PageContent>
    <TableOfContents />
    <DocumentationText />
  </PageContent>
</PageLayout>
```

프로젝트가 성장함에 따라 이미 작성한 컴포넌트를 재사용하여 많은 디자인을 구성할 수 있습니다.

</br>
</br>

### 컴포넌트 정의

기존에는 웹 페이지를 만들 때 개발자가 컨텐츠를 마크업한 다음 JavaScript를 뿌려 상호작용을 추가했습니다.

React는 동일한 기술을 사용하면서도 상호작용을 우선시합니다.

</br>

React 컴포넌트는 마크업으로 뿌릴 수 있는 JavaScript 함수입니다.

그 모습은 다음과 같습니다.

```tsx
export default function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3Am.jpg"
      alt="Katherine Johnson"
    />
  )
}
```

</br>

컴포넌트를 빌드하는 방법은 다음과 같습니다.

</br>

**컴포넌트 내보내기**

`export default` 접두사는 표준 JavaScript 구문으로 React에만 해당되지 않습니다.

해당 접두사를 사용하면 나중에 다른 파일에서 가져올 수 있도록 파일에 주요 기능을 표시할 수 있습니다.

</br>

**함수 정의하기**

`function Profile() {}` 을 사용하면 `Profile` 이라는 이름의 JavaScript 함수를 정의할 수 있습니다.

</br>

> **React 컴포넌트는 일반 JavaScript 함수이지만, 이름은 대문자로 시작해야 하며 그렇지 않으면 작동하지 않습니다.**
>

</br>

**마크업 추가하기**

```tsx
export default function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3Am.jpg"
      alt="Katherine Johnson"
    />
  )
}
```

다음 컴포넌트는 `src` 및 `alt` 속성을 가진 `<img />` 태그를 반환합니다.

`<img />` 는 HTML처럼 작성되었지만 실제로는 JavaScript로 해당 구문을 **JSX**라고 합니다.

JSX 구문을 통해 JavaScript 안에 마크업을 삽입할 수 있습니다.

</br>

반환문은 다음과 같이 한 라인에 모두 작성할 수 있습니다.

```tsx
return <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />;
```

</br>

그러나 마크업이 모두 `return` 키워드와 같은 라인에 있지 않은 경우에는 다음과 같이 괄호로 묶어야 합니다.

```tsx
return (
  <div>
    <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  </div>
);
```

괄호가 없으면 `return` 뒷 라인에 있는 모든 코드는 무시됩니다.

</br>
</br>

### 컴포넌트 사용하기

`Profile` 컴포넌트를 정의했으므로 다른 컴포넌트 안에 중첩할 수 있습니다.

다음과 같이 여러 `Profile` 컴포넌트를 사용하는 `Gallery` 컴포넌트를 내보낼 수 있습니다.

```tsx
function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3As.jpg"
      alt="Katherine Johnson"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

`<section>` 태그는 소문자이므로 React는 HTML 태그를 가리킨다고 이해합니다.

`<Profile />` 은 대문자로 시작하므로 React는 컴포넌트를 사용하고자 한다고 이해합니다.

</br>

브라우저에 표시되는 내용은 다음과 같습니다.

```tsx
<section>
  <h1>Amazing scientists</h1>
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
</section>
```

</br>
</br>

### 컴포넌트 중첩 및 구성

컴포넌트는 일반 JavaScript 함수이므로 같은 파일에 여러 컴포넌트를 포함할 수 있습니다.

컴포넌트가 상대적으로 작거나 서로 밀접하게 관련되어 있을 때 편리합니다.

</br>

컴포넌트를 한 번 정의한 다음 원하는 곳에서 원하는 만큼 여러 번 사용할 수 있습니다.

```tsx
function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3As.jpg"
      alt="Katherine Johnson"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

`Profile` 컴포넌트는 `Gallery` 안에서 렌더링되기 때문에 `Gallery` 컴포넌트는 각 `Profile` 컴포넌트를 자식으로 렌더링하는 부모 컴포넌트라고 말할 수 있습니다.

</br>

컴포넌트는 다른 컴포넌트를 렌더링할 수 있지만, 그 정의를 중첩해서는 안 됩니다.

```tsx
export default function Gallery() {
  function Profile() {
    // ...
  }
  // ...
}
```

위 코드는 매우 느리고 버그를 촉발합니다.

</br>

다음과 같이 최상위 레벨에서 컴포넌트를 정의해야합니다.

```tsx
export default function Gallery() {
  // ...
}

function Profile() {
  // ...
}
```

자식 컴포넌트에 부모 컴포넌트의 일부 데이터가 필요한 경우, 정의를 중첩하는 대신 `props`를 사용하여 전달합니다.

</br>
</br>

### 컴포넌트의 모든 것

React 애플리케이션은 `“root”` 컴포넌트에서 시작됩니다.

보통 새 프로젝트를 시작할 때 자동으로 생성됩니다.

</br>

대부분의 React 앱은 모든 부분에서 컴포넌트를 사용합니다.

즉, 버튼과 같이 재사용 가능한 부분뿐만 아니라 사이드바, 목록, 그리고 궁극적으로 전체 페이지와 같은 큰 부분에도 컴포넌트를 사용하게 됩니다.

</br>
</br>

### React 기반 프레임워크

기존 React 애플리케이션은 초기 로딩 시 내용이 비어 있는 HTML 파일만을 브라우저에 제공합니다.

이후 JavaScript가 로드되고 실행되어야만 비로소 React가 DOM을 조작하여 화면을 그리기 시작하므로, JavaScript 실행 전까진 사용자는 빈 화면을 보게 됩니다.

</br>

반면, 최신 React 기반 프레임워크는 서버 환경에서 React 컴포넌트를 미리 렌더링하여 콘텐츠가 포함된 HTML을 자동으로 생성합니다.

브라우저는 완성된 HTML을 수신하자마자 즉시 화면에 표시할 수 있으며, 덕분에 사용자는 JavaScript 실행 전이라도 앱의 초기 콘텐츠와 레이아웃을 확인할 수 있습니다.

</br>