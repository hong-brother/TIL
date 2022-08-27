# tsconfig.json란?
타입스크립트 ts파일들을 js파일로 컴파일 할때 어떻게 변환할 것인지 세부 항목을 설정하는 json파일이다.
타입스크립트의 세부 설정 항목을 한번 살펴보겠다.
먼저 타이스크립트를 설정파일 을 만들기 위해 아래 typescript를 적용시켜보자

## typescript 적용
- package.json 파일 생성
```bash
npm init -y
```

- install typescript
```bash
npm install -D typescript
```

- tsconfig.json 생성
기본으로 타입 스크립트 설정 파일이 설정 될 것이다.
```bash
npx tsc --init
```

# tsconfig.json 
- 아래 tsconfig파일은 nestjs에서 기본으로 프로젝트 생성 시 기본으로 설정되어 있는 항목이다.
기본으로 설정되어 있는 항목을 살펴보면 아래와 같다.
```
{
  "compilerOptions": {
    "module": "commonjs",
    "declaration": true,
    "removeComments": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "allowSyntheticDefaultImports": true,
    "target": "es2017",
    "sourceMap": true,
    "outDir": "./dist",
    "baseUrl": "./",
    "incremental": true,
    "skipLibCheck": true,
    "strictNullChecks": false,
    "noImplicitAny": false,
    "strictBindCallApply": false,
    "forceConsistentCasingInFileNames": false,
    "noFallthroughCasesInSwitch": false
  }
}
```
- `module`은 TS 에서 컴파일된 JS가 모듈이 어떤 모듈을 사용할지 설정하는 항목이다.
기본으로 CommonJS로 설정 되어 있으며 각각 설정 항목을 확인하자면 아래와 같다.
- Commonjs
- AMD
- UMD
- Syem
- ES6, ES2015, ES2020, ESNext

- `target`은 타입스크립트 파일을 어떤 버전의 자바스크립트로 버전으로 컴파일 할 지 `target`을 설정하는 부분이다. 자바스크립트 버전(ECMA Script)은 es5(es2009), es6(es2015), es7(es2016), es8(es2017), es9(es2018) 등으로 원하는 버전을 지정하면 해당 타입스크립트가 선택한 자바스크립트 버전으로 컴파일 된다.
대표적으로 `commonjs`와 `amd`, `esnext` 가 있다.
- `noImplicitAny`은 any라는 타입이 의도치 않게 발생할 경우 에러를 발생하는 설정이다.

```json
{
 "compilerOptions": {
  "target": "es5", // 'es3', 'es5', 'es2015', 'es2016', 'es2017','es2018', 'esnext' 가능
  "module": "commonjs", //무슨 import 문법 쓸건지 'commonjs', 'amd', 'es2015', 'esnext'
  "allowJs": true, // js 파일들 ts에서 import해서 쓸 수 있는지 
  "checkJs": true, // 일반 js 파일에서도 에러체크 여부 
  "jsx": "preserve", // tsx 파일을 jsx로 어떻게 컴파일할 것인지 'preserve', 'react-native', 'react'
  "declaration": true, //컴파일시 .d.ts 파일도 자동으로 함께생성 (현재쓰는 모든 타입이 정의된 파일)
  "outFile": "./", //모든 ts파일을 js파일 하나로 컴파일해줌 (module이 none, amd, system일 때만 가능)
  "outDir": "./", //js파일 아웃풋 경로바꾸기
  "rootDir": "./", //루트경로 바꾸기 (js 파일 아웃풋 경로에 영향줌)
  "removeComments": true, //컴파일시 주석제거 

  "strict": true, //strict 관련, noimplicit 어쩌구 관련 모드 전부 켜기
  "noImplicitAny": true, //any타입 금지 여부
  "strictNullChecks": true, //null, undefined 타입에 이상한 짓 할시 에러내기 
  "strictFunctionTypes": true, //함수파라미터 타입체크 강하게 
  "strictPropertyInitialization": true, //class constructor 작성시 타입체크 강하게
  "noImplicitThis": true, //this 키워드가 any 타입일 경우 에러내기
  "alwaysStrict": true, //자바스크립트 "use strict" 모드 켜기

  "noUnusedLocals": true, //쓰지않는 지역변수 있으면 에러내기
  "noUnusedParameters": true, //쓰지않는 파라미터 있으면 에러내기
  "noImplicitReturns": true, //함수에서 return 빼먹으면 에러내기 
  "noFallthroughCasesInSwitch": true, //switch문 이상하면 에러내기 
 }
}
```







