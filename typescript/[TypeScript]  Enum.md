# Enum
enum이란 enumerated type의 줄임말로 열거형이라고 부르며 멤버라 불리는 명명된 값의 집합을 이루는 자료형이다. 열거자 이름들은 일반적으로 해당 언어의 상수 역할을 하는 식별자이다.
**서로 연관된 상수들을 하나의 namespace로 묶어 응집도를 높이기 위함이다.**
# Enum 종류
## 숫자형 Enum
```typescript
enum Team {
  PL,   // 0
  PM,   // 1
  Developer, // 2
  Designer,  // 3
}

let hsnam:number = Team.Designer;
// Team.Designer = 3
```

## 문자형 Enum
```typescript
export enum LangagueTypeEnum {
    JAVA = 'java',
    TS = 'typescript',
    JS = 'javascript',
    PYTHON = 'python',
}
let developer1:string = LangagueTypeEnum.JAVA;
// develop1 = java

let developer2:string = LangagueTypeEnum.PYTHON;
// develop2 = python
```

## const Enum
const enum은 내부 상수값들이 전부 compile 단계에서 내부 필드를 전부 상수로 변경하기 때문에 런타임에 의존 모듈의 영향을 받지 않게 되며, 코드 크기가 더 적기 때문에 더 선호된다고 한다. 
아래 코드는 일반적은 Enum과 Const Enum에 대한 코드 이다.
```typescript
enum JustEnumNumber {
  zero,
  one,
}

console.log(JustEnumNumber.one) // 1
console.log(JustEnumNumber)



const enum EnumNumber {
  zero,
  one,
}

console.log(EnumNumber.one) // 1
console.log(EnumNumber) // 1
```
위의 코드를 javascript로 컴파일 하게 되면 아래와 같은 코드로 컴파일 된다.
```typescript
"use strict";
var JustEnumNumber;
(function (JustEnumNumber) {
    JustEnumNumber[JustEnumNumber["zero"] = 0] = "zero";
    JustEnumNumber[JustEnumNumber["one"] = 1] = "one";
})(JustEnumNumber || (JustEnumNumber = {}));
console.log(JustEnumNumber.one); // 1
console.log(JustEnumNumber);
console.log(1 /* EnumNumber.one */); // 1
console.log(EnumNumber); // 1
```
const enum은 컴파일 단계에서 내부 필드를 전부 상수로 변경하기 때문에 런타임에 의존 모듈의 영향을 받지 않게 되며 코드의 크기가 더 작아진다.
