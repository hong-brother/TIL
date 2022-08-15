# 타입스크립트란?

![](https://velog.velcdn.com/images/hong-brother/post/370fe79d-17d0-4b5b-bae9-35fcf8828728/image.png)
- 타입스크립트는 기존에 변수의 타입이 없는 자바스크립트에 정접타입을 부여한 언어이다. 자바스크립트의 확장된 언어라고 볼 수 있다. 타입스크립트는 자바스크립트와 달리 브라우저에서 실행하려면 컴파일 과정을 해야한다.
- 코드 작성 단계에서 타입을 체크해 사전에 오류를 확인 할 수 있고 미리 타입을 결정하기 때문에 실행 속도가 매우 빠르다는 장점이 있다.

[참고]동적 타입 언어와 정적 타입 언어

|동적 타입 언어|정적 타입 언어|
|----------|-----------|
|타입에 대한 고민을 하지 않아도 되므로 배우기 쉬움|변수를 선언 할 때 마다 타입을 고민해야 하므로 진입 장벽이 높다.|
|코드의 양이 적을 때 생샌성이 높다.|코드의 양이 많을 때 동적 타입 언어에 비해 생산성이 높다|
|타입 오류가 런타임 시 발견된다.|타입 오류가 컴파일 시 발견된다.|

# TypeScript의 특징
## 컴파일 언어, 정적인 타입(Static Type)
- 기존 타입이 없는 자바스크립트에 비해 타입스크립트는 컴파일 단계에서 타입을 체크하여 타입에 대한 오류를 사전에 줄여 줄 수 있다.
- 기존 자바스크립트의 타입에 대한 단점을 보안을 해주므로써 코드의 가독성을 높여준다.
### 비교
- javascript의 동적인 타입
```javascript
function sum(a, b) {
    return a + b;
}
```
- typescript의 정적인 타입
```javascript
function sum(a:number, b: number) {
	return a + b;
}
```

## 자바스크립트의 슈퍼셋(Superset)
- 슈퍼셋은 확장을 뜻하며 기존 자바스크립트의 모든 기능을 사용 가능하며 이외에 타입스크립트만이 지원하는 클래스, 인터페이스등 사용할 수 있는 확장판이다.


## 객체지향 프로그래밍 지원
- 타입스크립트는 es6에서 새롭게 사용된 문법을 포함하고 있으며 클래서, 인터페이스, 상속, 모듈 등 객체지향 프로그래밍을 지원한다.

- 타입스크립트의 클래스
```javascript
export class TestClass {
  private people: string;
  private car: string;
}                        
```

- 타입스크립트의 상속
```javascript
export class HomeClass extents SummerClass {
  private people: string;
  private car: string;
}                 
```


# 타입스크립트를 써야하는 이유
## 에러의 사전방지
```typescript
function sum(a: number, b: number) {
  return a + b;
}
```
- 타입을 지정하므로써 의도하지 않은 코드의 동작을 예방 할 수 있다.
- 기존 자바스크립트인 경우 타입을 지정하지 않고 sum(10, '20')입력을 하게 되면 '1020' 이라는 결과를 얻게 된다. 
>
자바를 하던 나로써는 nodejs로 넘어 오면서 명확하게 타입을 지정 할 수 있다는 typescript를 마음에 들어 하게 되었다. 하지만 아직은 자바가 더 좋은걸ㅠㅠㅠ

## 코드 가이드 및 자동완성
- 인텔리J에 익숙한 개발자는 쉽게 VSCode로 넘어가기 힘든것 같다. 인텔리J와 비슷한 웹스톰을 사용해서 typescript를 개발하는데 웹스톰에서도 코드 가이드 및 자동완성을 불폄함 없을 정도로 지원을 해준다.

>
참고 
[타입스크립트 핸드북](https://joshua1988.github.io/ts/why-ts.html#%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%9E%80)
