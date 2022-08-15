# interface
- 자바에서의 인터페이스는 클래스를 구현하기 전에 필요한 메서드를 정의하는 용도로 쓰이지만,타입스크립트에서는 인터페이스로 정의할 수 있는 타입의 종류와 인터페이스로 인터페이스 타입을 정의 하는 방벙으로 사용한다.

## 변수를 정의하는 인터페이스
- User라는 인터페이스를 정의 하고 **hsnam** 이라는 변수에 사전에 정의된 인터페이스를 정의 할 수 있다. 정의된 인터페이스에 맞는 속성을 명시를 해야 한다.
```typescript
interface User {
  age: number;
  name: string;
}

var hsnam: User = {
  name: 'hsname',
  age: 34
}
```

## 함수의 인자를 정의하는 인터페이스
- 해당 함수(getUser)의 인자는 정의된 인터페이스 형식을 준수하는 데이터만 받을 수 있다. 
```typescript
interface User {
  age: number;
  name: string;
}

function getUser(user: User) {
	console.log(user);
}

const hsnam = {
  name: 'hsnam',
  age: 100
}
getUser(hsnam);
```

## 함수 구조를 정의하는 인터페이스
- 자바와는 다르게 함수의 구조적인 설계도 타입스크립트는 정의하여 활용 할 수 있다.
- 해당 방법은 라이브러리를 직접 만들거나 협업할때 유용하게 사용 할 것 같다.
```typescript
interface SumFunction {
 (a: number, b: number): number; 
}

var sum: SumFunction;
sum = function(a: number, b: number): number {
  return a + b;
}
```

## 인덱싱 방식을 정의하는 인터페이스
- 배열 안에 들어가는 index와 값을 인터페이스로 정의하여 활용 할 수 있다.
```typescript
interface StringArray {
  [index: number]: string;
}

var arr: StringArray = ['a', 'b', 'c'];
arr[0]; // 'a'
```

## 인터페이스 딕셔너리 패턴
- 딕셔너리 패턴
```typescript
interface StringRegexDictionary {
  [key: string]: RegExp //정규표현식 생성자
}

var obj: StringRegexDictionary = {
  // sth: /abc/,
  cssFile: /\.css$/ ,
  jsFile: /\.js$/ ,
  
}

```

## 인터페이스 상속
- 인터페이스는 extents 키워드를 사용하여 인터페이스 또는 클래스를 상속받아 확장 할 수 있다.
```typescript
interface Persion {
  name: string;
  age: number;  
}

interface Developer extends Person {
  language: string;
}

var hsnam: Developer = {
  language: 'ts',
  age: 100,
  name: '홍식쓰~'
}
```

# type
## 타입 별칭(type aliases)
- 타입 별칭은 새로운 타입 값을 하나 생성하는 것이 아니라 정의한 타입에 대한 쉽게 참고할 수 있게 이름을 부여하는 것과 같다.
```typescript

type Person {
  name: string;
  age: number;
}

var hsnam: Person = {
  name: '홍식쓰'
  age: 12
}
```

# interface VS type
- 인터페이스와 타입별칭과 가장 큰 차이점은 타입의 확장 가능 여부 입니다. 인터페이스는 확장이 가능한데 반해 타입 별칭은 확장이 불가능 합니다. 
- 따라서 간단한 타입 정의에서는 타입 별칭을 추천하지만 확장성을 고려해야하는 상황이라면 interface를 사용하는것을 추천합니다.
