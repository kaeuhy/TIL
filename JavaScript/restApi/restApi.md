### REST API란?

REST는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처로 REST API는 REST를 기반으로 서비스 API를 구현한 것을 의미함

REST API는 자원, 행위, 표현의 3가지 요소로 구성됨

- **자원**
    - URI 엔드포인트로 표현함
- **행위**
    - 자원에 대한 행위로 HTTP 요청 메서드로 표현함
- **표현**
    - 자원에 대한 행위의 구체적 내용으로 페이로드로 표현함

<br/>

REST에서 가장 중요하 기본적인 원칙은 두 가지임

먼저 URI는 리소스를 표현하는 데 중점을 두어야함

리소스를 식별할 수 있는 이름은 동사보다는 명사를 사용해야함

```bash
# bad pattern
GET /getTodos/1
GET /todos/show/1

# good pattern
GET /todos/1
```

위에서 처럼 이름에 get 같은 행위에 대한 표현이 들어가서는 안됨

<br/>

다음은 리소스에 대한 행위는 HTTP 요청 메서드로 표현해야한다는 것임

주로 5가지 요청 메서드를 사용하여 CRUD를 구현함

- `GET`
    - 모든/특정 리소스 취득
- `POST`
    - 리소스 생성
- `PUT`
    - 리소스의 전체 교체
- `PATCH`
    - 리소스의 일부 수정
- `DELETE`
    - 모든/특정 리소스 삭제

<br/>
<br/>

### GET 요청

`GET` 요청을 통해 `todos` 리소스에서 모든 `todo` 를 취득하는 코드를 먼저 살펴 볼 것임

```tsx
const xhr = new XMLHttpRequest();

xhr.open('GET', '/todos');

xhr.send();

xhr.onload = () => {
  if (xhr.status === 200) {
    document.querySelector('pre').textContent = xhr.response;
  } else {
    console.error('Error', xhr.status, xhr.statusText);
  }
}
```

<br/>

다음은 `GET` 요청을 통해 `todos` 리소스에서 `id` 를 사용하여 특정 `todo` 를 취득하는 코드임

```tsx
const xhr = new XMLHttpRequest();

// todos 리소스에서 id를 사용하여 특정 todo를 취득
xhr.open('GET', '/todos/1');

xhr.send();

xhr.onload = () => {
  if (xhr.status === 200) {
    document.querySelector('pre').textContent = xhr.response;
  } else {
    console.error('Error', xhr.status, xhr.statusText);
  }
}
```

<br/>
<br/>

### POST 요청

다음은 `todos` 리소스에 `POST` 메서드를 통해 새로운 `todo` 를 생성해 볼 것임

`POST` 요청 시에는 `setRequestHeader` 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야함

```tsx
const xhr = new XMLHttpRequest();

xhr.open('POST', '/todos');

xhr.setRequestHeader('content-type', 'application/json');

xhr.send(JSON.stringify({ id: 4, content: 'Angular', completed: false }));

xhr.onload = () => {
  if (xhr.status === 200 || xhr.status === 201) {
    document.querySelector('pre').textContent = xhr.response;
  } else {
    console.error('Error', xhr.status, xhr.statusText);
  }
}
```

<br/>
<br/>

### PUT 요청

`PUT` 은 특정 리소스 전체를 교체할 때 사용함

다음은 `todos` 리소스에서 `id` 로 `todo` 를 특정하여 `id` 를 제외한 리소스 전체를 교체하는 코드임

`PUT` 요청 시에는 `setRequestHeader` 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야함

```tsx
const xhr = new XMLHttpRequest();

xhr.open('PUT', '/todos/4');

xhr.setRequestHeader('content-type', 'application/json');

xhr.send(JSON.stringify({ id: 4, content: 'React', completed: true }));

xhr.onload = () => {
  if (xhr.status === 200) {
    document.querySelector('pre').textContent = xhr.response;
  } else {
    console.error('Error', xhr.status, xhr.statusText);
  }
}
```

<br/>
<br/>

### PATCH 요청

`PATCH` 는 특정 리소스의 일부를 수정할 때 사용함

다음은 `todos` 리소스의 `id` 로 `todo` 를 특정하여 `completed` 만 수정하는 코드임

`PATCH` 도 마찬가지로 `setRequestHeader` 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야함

```tsx
const xhr = new XMLHttpRequest();

xhr.open('PATCH', '/todos/4');

xhr.setRequestHeader('content-type', 'application/json');

xhr.send(JSON.stringify({ completed: false}));

xhr.onload = () => {
  if (xhr.status === 200) {
    document.querySelector('pre').textContent = xhr.response;
  } else {
    console.error('Error', xhr.status, xhr.statusText);
  }
}
```

<br/>
<br/>

### DELETE 요청

`DELETE` 는 리소스를 삭제할 때 사용함

다음은 `todos` 리소스에서 `id` 를 사용하여 `todo` 를 삭제하는 코드임

```tsx
const xhr = new XMLHttpRequest();

xhr.open('DELETE', '/todos/4');

xhr.send();

xhr.onload = () => {
  if (xhr.status === 200) {
    document.querySelector('pre').textContent = xhr.response;
  } else {
    console.error('Error', xhr.status, xhr.statusText);
  }
}
```

<br/>