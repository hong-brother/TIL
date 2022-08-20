# Class란 
- 클래스란 객체를 정의하는 틀 또는 설계도와 같은 의미로 사용된다.
- 클래스는 객체의 상태를 나타내는 `field`와 객체의 행동을 나타내는 `method`로 구성된다.
- `field`는 클래스에 포함된 변수를 의미하며 `method`는 어떠한 특정 작업을 수행하기 위한 명령문의 집합이라고 말 할 수 있다. 
> ES6에서 새롭게 도입된 클래스는 기존 프로토타입 기반 객체지향 언어보다 클래스 기반 언어에 익숙한 개발자가 보다 빠르게 학습할 수 있는 새로운 문법을 제시하고 있다. 

## TypeScript Clss
- ES6부터 추가된 Clss 
```javascript
class Coffe {
  constructor(name, price) {
    this.name = name;
    this.price = price;
  }
  
  take() {
    return this.price;
  }
}
```
ES6에 추가된 Class에는 클래스 몸체에 `method`만 포함 할 수 있다. 

- typescript class
```typescript
class Coffe {
  	name: string;
  	prise: number
  constructor(name: string, price: number) {
    this.name = name;
    this.price = price;
  }
  
  take() {
    return this.price;
  }
}
```

## 접근 제한자
### 멤버 변수에 대한 접근 제한자
- TypeScript 클래스는 타언어가 지원하는 접근 제한자(Access modifier)-`public`, `private`, `protected`를 지원하며 의미 또한 타언어와 동일하다.

|접근 가능성|public|protected|private|
|--------|------|---------|-------|
|클래스 내부|O|O|O|
|자식 클래스 내부|O|O|X|
|클래스 인스턴스|O|X|X|

- 클래스 내부 멤버 변수에 접근 제한자를 명시 하지 않는 경우 다른 클래스 기반언어는 암묵적으로 protected로 지정되어 상속을 받아야만 공개가 되지만 TypeScript의 경우는 암묵적으로 `public`으로 선언된다.

```typescript
class Paper {
  public x: string;
  protected y: string;
  private z: string;

  // public, protected, private 접근 제한자 모두 클래스 내부에서 참조 가능하다.
  constructor(x: string, y: string, z: string) {  
    this.x = x;
    this.y = y;
    this.z = z;
  }
}

const paper = new Paper('x', 'y', 'z');

console.log(paper.x); // public 접근 제한자는 생성된 인스턴스를 통해 접근 할 수 있다.

console.log(paper.y); // Property 'y' is protected and only accessible within class 'Paper' and its subclasses.

console.log(paper.z); // Property 'z' is private and only accessible within class 'Paper'.

class draw extends Paper {
  constructor(x: string, y: string, z: string) {
    super(x, y, z);

    console.log(this.y); // protected 접근 제한자는 자식 클래스 내부만 접근이 가능하다
  }
}
```

### 생성자 파라미터의 접근 제한자
- 접근 제한자를 클래스 멤버 변수에만 선언 할 수있는거 뿐만 아니라 생성자 파라미터에도 선언 할 수 있다.
- constructor에 선언된 생성자 파라미터 X는 멤버 변수로 자동으로 선언 되며 클래스 내부에서만 참조 가능하다.
```typescript
class Paper {
  constructor(private x: string) { }
}

const Paper = new Paper('MAN~');

console.log(bar); // Bar { x: 'MAN~' }

console.log(bar.x);  // private이 선언된 Paper.x는 클래스 내부에서만 참조 가능하다

```

## readonly 
- typescript에서는 타언어에서 지원하는 readonly를 사용 할 수 있다.
- readonly는 클래스 멤버 변수 선언 시 또는 생성자 내부에서만 값을 할당할 수 있다. 그 외의 경우에는 값을 절대 할당 하거나 변경 할 수는 없고 오직 읽기 가능한 상태가 된다.

```typescript
class Paper {
  private readonly VALUE: number = 12;
  private readonly X: string;
  
  constructor() { 
  	this.X = 'just readonly';  
  }
}

````

## static
- ES6에서 추가된 기능 이며 클래스에서 static 메소드를 정의 한다.
정적 메서드는 클래스 인스턴스가 아닌 클래스 이름으로 호출하기 때문에 인스턴스를 생성하지 않아도 된다.
```typescript
class Paper {
  static staticPaper() {
    return 'static'
  }
  
  constructor() { 
  	this.X = 'just readonly';  
  }
}

console.log(Paper.staticPaper()); //return 'static'
```
