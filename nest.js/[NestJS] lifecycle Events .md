# LifeCycle Events
Nest 어플리케이션에서도 관리되는 lifecycle이 있다. 마치 아마두..? 싱글톤 컨테이너(스프링 컨테이너) 생명주기와 비슷하다.

>스프링 컨테이너 생명주기
스프 컨테이너 생성 - 스프링 빈 생성 - 의존관계 주입 - 초기화 콜백 - 사용 - 소멸전 콜백 - 스프링 종료

LifeCycle 이벤트에 대한 가시성을 제공하는 lifecycle hook과 발생 시 조치(모듈, 주입 가능한 또는 컨트롤러에 등록된 코드를 실행)할 수 있는 기능을 제공한다.


## LifeCycle Sequeue
- 아래 다이어그램은 애플리케이션이 부트스트랩된 시간 부터 노드 프로세서가 종료될 때까지의 Nest 어플리케이션 생명 주기 이벤터의 순서를 보여준다.
- 크게 초기화, 실행, 종료의 세 단계로 나눌 수 있다.
