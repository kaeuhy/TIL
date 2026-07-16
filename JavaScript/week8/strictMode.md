---

### strict mode란?

strict mode는 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킴

클래스와 모듈은 기본적으로 strict mode가 적용됨

→ 레거시와는 다르게 새로운 문법이기에 처음부터 안전하게 만듦

</br>
</br>

### strict mode의 적용

전역의 선두 또는 함수 몸체의 선두에 `‘use strict’;` 를 추가하면됨

```tsx
'use strict';

function foo() {
	x = 10;  // ReferenceError: x is not defined
}

foo();
```

</br>

함수 몸체의 선두에 추가하면 해당 함수와 중첩 함수에 strict mode가 적용됨

```tsx
function foo() {
	'use strict';
	
  x = 10;  // ReferenceError: x is not defined
}

foo();
```

</br>
</br>

### strict mode가 발생시키는 에러

JS 엔진은 암묵적으로 전역 객체에 x 프로퍼티를 동적 생성함

이때 전역 객체의 x 프로퍼티는 마치 전역 변수처럼 사용할 수 있음

→ 암묵적 전역

</br>

strict mode를 사용하면 선언하지 않은 변수를 참조시 ReferenceError가 발생함

```tsx
(function() {
	'use strict';
	
	x = 1;
	console.log(x);  // ReferenceError: x is not defined
}());
```

</br>

`delete` 연산자로 변수, 함수, 매개변수를  삭제하면 SyntaxError가 발생함

→ 식별자를 제거하는 연산자가 아니라 객체의 프로퍼티를 제거하는 연산자이기에

```tsx
(function() {
  'use strict';
  
  let x = 1;
  delete x;  // SyntaxError: Deleting local variable in strict mode.
  
  function foo(a) {
    delete a;  // SyntaxError: Deleting local variable in strict mode.
  }
  delete foo;  // SyntaxError: Deleting local variable in strict mode.
}
```

</br>

중복된 매개변수 이름 사용에도 발생함

```tsx
(function() {
	'use strict';
	
	// SyntaxError: Argument name clash.
	function foo(x, x) {
		return x + x;
	}
	console.log(foo(1, 2));
}());
```

</br>
</br>

### strict mode 적용에 의한 변화

strict mode에서 함수를 일반 함수로서 호출하면 `this` 에 `undefined` 가 바인딩됨

```tsx
(function() {
	'use strict';
	
	function foo() {
		console.log(this);
	}
	foo();
	
	function Foo() {
		console.log(this);
	}
	new Foo();
}());
```

</br>

생성자 함수가 아닌 일반 함수 내부에서는 `this` 를 사용할 필요가 ㅇ없기 때문임

또 매개변수에 전달된 인수를 재할당하여 변경해도 `arguments` 객체에 반영되지 않음

```tsx
(function(a) {
	'use strict';
	
	a = 2;
	
	console.log(arguments);  // { 0: 1, length: 1, callee: [Getter] }
}(1));
```

</br>