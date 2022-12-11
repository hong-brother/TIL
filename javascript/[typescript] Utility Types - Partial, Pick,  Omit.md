# What is Typescript Partial
- 특정 타입의 부분 집합을 만족하는 타입을 정의 할 수 있다. 즉 모든 요소를 Optional 지정한 타입을 새로 생성할 수 있다.
```typescript
type userInfoType = {
    id: string;
    email: string;
    password: string;
}

const hsnam: Partial<userInfoType> = {
    id: 'hsnam',
    email: 'robbins.hs@gmail.com'
}
```
- Partial 타입을 활용하면 모든 요소를 Optional로 지정하여 새로운 타입을 생성 할 수 있다.

# What is Typescript Pick
- 특정 타입 요소만 포함된 타입을 새로 생성한다.

```typescript
type userInfoType = {
    id: string;
    email: string;
    password: string;
}

const newUserInfoType: Pick<userInfoType, "id"|"password"> = {
    id: 'hsnam',
    password: 'password',
}
```
- Pick은 첫 번째 인자로 받은 타입에서 주어진 속성을 포함하여 새로운 타입을 리턴한다.


# What is Typescript Omit
- 주어진 타입에서 특정 속성만 제거한 타입을 정의힌다.
```typescript
type userInfoType = {
    id: string;
    email: string;
    password: string;
}


const omitType: Omit<userInfoType, 'id'>  = {
    email: "robbins.hs@gmail.com",
    password: "123456789a"
}
```
- omit은 첫 번째 인자로 받은 타입에서 주어진 속성을 제외한 새로운 타입을 리턴한다.

