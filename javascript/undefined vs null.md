# undefined VS null
- undfined와 null은 모두 자바스크립트에서 **값이 없음**을 나타낸다.

## undefind
- 정의되지 않은 값 또는 해당 값을 가진 변수
- **값이 할당외지 않은 상태**를 나타낼 때 사용

```javascript
let _undefind;
console.log(_undefind, typeof _undefind);  // undefined, "undefined"
```
## null
- 변수를 선언하고 **빈 값을 할당한 상태**(빈 객체)이다.
- primitive type 중 하나로, **어떤 값이 의도적으로 비어 있음을 표현**한다.
- 해당 변수가 어떤 객체도 가리키고 있지 않다는 것을 의미한다.

```javascript
let _null = null; 
console.log(_null, typeof _null); // null, "object"
```

## null 과 undefined의 차이점
- 변수에 값이 null이라면 변수가 선언되고 null이라는 값이 주어진 상태이다.
- 변수에 값이 undfined라면 변수가 선언되고 아무것도 하지 않은 상태이다.
- 즉 null은 직접적으로 값이 없어라고 말한 상태이지만 undfined는 아무것도 하지 않은 상태라고 말할 수 있다.
