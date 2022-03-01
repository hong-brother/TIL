# 스프링 핵심 원리 -기본편 (객체지향 설계와 스프링) - 2편

## 좋은 객체 지향 설계의 5가지 원칙(SOLID)
>클린코드로 유명한 로버트 마틴이 좋은 객체 지향 설계의 5가지 원칙을 정리

### SRP: 단일 책임 원칙(Single Responsibility Principle)
- 한 클래스는 하나의 책임만 가져야한다.
- 하나의 책임이라는 것은 모호하다 책임이라는 것은 클 수도 있고, 작을 수도 있다 또한 문맥과 상황에 따라 다르다
- 중요한 기준은 변경이다. 변경이 있을때 파급 효과가 적으면 단일 책임 원칙을 잘 따른 것이다.

### OCP: 개방-폐쇄 원칙(Open/Closed Principle)
- 소프트웨어 요소는 확장에는 여려 있으나 변경에는 닫혀 있어야 한다.
### LSP: 리스코프 치환 원칙(Liskov Substitution Principle)

### ISP: 인터페이스 분리 원칙(Interface Segregation Principle)

### DIP: 의존관계 역전 원칙(Dependency Inversion Principle)
