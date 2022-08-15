
# TypeScript의 변수

## 변수 선언
- TS 문자열 선언
```javascript
let str: string = 'hello';
```
- TS 숫자 선언
```javascript
let num: number = 10;
```
- TS 배열 선언
```javascript
let arr: Array<number> = [1,2,3];
let heroes: Array<string> = ['Cept', 'Thor', 'Hulk'];
let items: number[] = [1,2,3];

```

## 튜플
- 배열에 인덱스에 타입을 세부적으로 지정할 수 있다.
```javascript
let address: [string, number] = ['gangnam', 1000];
```

## 객체
```javascript
let person: { name: string, age: number} = {
  name: 'thor',
  age: 1000
}
```

## 진위값
```javascript
let show: boolean = true;
```

# 함수
- 함수의 파라미터에 타입을 정의하는 방식
```javascript
function sum(a: number, b:number) {
	return a + b;  
}
sum(10, 20);
```

- 함수의 반환 값에 타입을  정의하는 방식
```javascript
function add(): number {
  return 10;
}
add();
```

- 함수에 타입을 정의하는 방식
```javascript
function sum(a: number, b: number): number {
  return a + b;
}
sum(10, 20);
```

- 함수의 파라미터를 제한 하는 특성(유현한 javascript를 제약함)
```javascript
function sum(a, b) {
  return a + b;
}

sum(10, 20, 30, 40, 50);
```

- 함수의 옵셔널 파라미터
```javascript
function log(a: string, b?: string) {
  console.log(a + b);
}

log('hello world');
log('hello ts', 'abc');
```
