# Union Type
## Union Type 이란
유니온타입이란 자바스크립트의 OR 연산자와 같이 타입을 'A' 또는 'B'로 타입을 지정 할 수 있다.
```typescript
function infoText(data: string | number) {
  console.log(data);
}
```
해당 `infoText`의 파라미터의 타입을 `string` 또는 `number` 타입이 모두 입력 될 수 있다.
이처럼 `|` 연산자를 이용하여 타입을 여러 개 연결하는 방식을 Union Type 방식이라고 한다.

함수나 변수에 타입을 지정하기에는 애매한 상황에서 유리 할 수 있다. `any`라는 타입을 사용하여 애매한 상황을 `Untion Type` 대신 해결 할 수 있지만 `any`타입을 마구 쓰다보면 우리가 타입스크립트를 사용하는 목적을 상실 할 수 있다./
`any`는 최대한 **변수 타입체크 해제(?)** 기능 말고는 사용을 자제 하길 바란다.

## Union Type의 활용
```typescript
type direction = 'left' | 'right' | 'up' | 'down';
function move(action: direction) {
  console.log(action)
}
```
위와 같이 사용 할 경우 action은 `left`, `right`, `up`, `down`에 대항하는 문자열만 사용 할 수 있다.

# Intersection Type
## Intersection Type 이란
Untion Type과는 다르게 Intersection Type은 여러 가지 타입을 결합(교집합)해서 사용 할수 있다.
Intersction Type은 `|` 대신 `&` 연산자를 사용하여 표현한다.

## Intersection Type 활용
```typescript
interface Person {
  name: string;
  age: number;
}

interface Developer {
  name: string;
  skill: string
}

function askSomeone(someone: Developer & Person) {
  someone.name;
  someone.age;
  someone.skill;
}
```
`Person`, `Developer`의 인터페이스 속성을 모두 포함하여 새로운 타입을 만들게 된다.


# Ref
[타입스크립트 GitBook](https://typescript-kr.github.io/pages/unions-and-intersections.html)
