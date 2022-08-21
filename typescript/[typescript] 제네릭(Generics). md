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

# 제네릭을 사용하는 이유
## 
