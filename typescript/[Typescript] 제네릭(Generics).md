# Generics이란?
제네릭은 `<>`을 가지는 클래스와 인터페이스를 말하며, 데이터와 타입을 일반화 한다는 것을 뜻한다.
제네릭은 자료형을 정하지 않고 생성 시점에 타입을 명시하여 하나의 타입만을 사용하는것이 아니라 다양한 타입을 사용 할 수 있도록하는 것을 뜻한다.
**즉, 특정 타입에 대해서 함수의 파라미터 처럼 사용 할 수 있도록 하는 것이다.**
한번의 선언으로 다양한 타입을 `재사용`이 가능하다는 이유로 실무에서 많이 쓰인다.


# Generics 사용법
## 함수에서의 제네릭
```typescript
function getLogs<T>(message: T): T {
	console.log(T);
}

getLogs<string>('LOGGER Test');
getLogs<number>(123123);
getLogs<boolean>(true);
```
함수에서의 제네릭을 사용하는 기본 문법형태이다. `getLogs<T>`와 같이 `T`에 제네릭을 선언하고 함수 사용시 타입을 넘겨 줄 수 있다.
입력 값의 타입이 `string`이면서 반환값도 `string`이어야 한다.
>
제네릭 함수를 호출할때 `<>`안에 타입을 명시하는 것은 필수가 아니다. `getLogs('test')`처럼 `<>`를 생략할 수 있는데 `<>`을 생략할 경우 컴파일러가 인수의 타입을 보고 타입을 경정한다.

## 클래스에서의 제네릭
```typescript
class Mart<T> {
  private items: T[] = [];
  private price: number;
  
  push(goods: T): void {
    items.push(goods);
  }
  
  getList(): T {
    return this.items;
  }
}

const meal = new Mart<string>();
meal.push('a');

const numberMeal = new Mart<number>();
meal.push(1);
```
클래스 선언부에 `<T>`라는 제네릭을 사용하겠다고 선언한다. 그리고 클래스 멤버 변수에 사용할 제네릭을 `T`와 같이 명시해 특정한 타입이 된다.

> 제네릭에 `<T>`는 타입 매개변수(type argument)의 의미 이며 관용적으로 `T`를 사용할뿐 `T`대신에서 다른 문자를 사용해도 상관없다.

# 제네릭을 사용하는 이유
- 재사용성을 위한 제네릭 사용
	제네릭을 사용하면 함수에서 타입에 매개변수를 여러타입으로 처리 할 수 있으므로 중복되는 코드를 줄일 수 있으므로 코드의 가독성도 함께 향상된다.
- 컴파일 단계에서 에러 케치
	- 만약 제네릭을 `any`로 구현한다면 컴파일 단계에서 타입에대한 오류를 찾을 수 없다. 사실 `any`로 제네릭 대신 사용해도 상관없지만 우리가 왜 javascript를 두고 typescript를 쓰는지에 대한 의미를 한번 더 생각해보자!
    ![](https://velog.velcdn.com/images/hong-brother/post/53b41392-122f-468c-a68c-254d70973234/image.png)

# 다양한 제네릭 사용
## 클래스에서의 멀티 제네릭
```typescript
class CustomClss <T, V> {
	genericData1: T;
  	genericData2: V;
  
  constructor(t: T, v: V) {
    this.t = t;
    this.v = v;
  }
  
}
```
해당 클래스는 제네릭을 2개의 값을 받을 수 있다. 기존 클래스에 제네릭을 사용하는 것과 동일 하다.

## 인터페이스에서의 제네릭
```typescript
interface ICustomInterface<T> {
  value: T;
}
```
- 클래스와 같이 인터페이스에도 제네릭을 선언하여 사용 할 수 있다.


## 정의된 타입으로 제네릭 타입 제한
```typescript
interface LengthType {
  length: number;
}

function logMessageLength<T extents LengthType> (message: T): T {
  console.log(message.length);
  return message;
}

logMessageLength('error message');
```
- 제네릭을 사용시에는 원하지 않는 속성에 접근하는 것을 막기 위해 제약 조건을 사용 할 수 있다.
`extends` 키워드로 제약 조건을 걸어준다.
- 특정 타입들로만 동작하는 제네릭 함수를 만들때 유용하다.(제약 조건에 벗어나는 타입을 선언하면 에러가 발생한다.)

## keyof로 제네릭의 타입 제한하기
```typescript
interface ShoppingItem {
  name: string;
  price: number;
  stock: number;
}

type Test = keyof ShoppingItem; // ("name", "price", "stock")
function getShoppingItemOption<T extends keyof ShoppingItem>(itemOption: T): T {
  return itemOption;
}

getShoppingItemOption('name')
```
