## 다대일 [N:1]

![](https://velog.velcdn.com/images/hong-brother/post/e9a5715f-1130-4d5d-b8ee-8c0987277d6a/image.png)

```java
@Getter
@Setter
@Entity
public class Member {
		@Id @GeneratedValue
		@Column(name = "MEMBER_ID")
		private Long id;
		
		@Column(name = "USERNAME")
		private String username;
		
		@ManyToOne
		@JoinColumn(name = "TEAM_ID")
		private Team team;
}
```

```java
@Getter
@Setter
@Entity
public class Team {
		@Id @GeneratedValue
		@Column(name = "TEAM_ID")
		private Long id;
		
		@Column(name = "name")
		private String name;
}
```

---

![](https://velog.velcdn.com/images/hong-brother/post/2e015e14-5b8a-4ad6-93ec-d2961ddfc6f3/image.png)

```java
@Getter
@Setter
@Entity
public class Member {
		@Id @GeneratedValue
		@Column(name = "MEMBER_ID")
		private Long id;
		
		@Column(name = "USERNAME")
		private String username;
		
		@ManyToOne
		@JoinColumn(name = "TEAM_ID")
		private Team team;
}
```

```java
@Getter
@Setter
@Entity
public class Team {
		@Id @GeneratedValue
		@Column(name = "TEAM_ID")
		private Long id;
		
		@Column(name = "name")
		private String name;
		
		@OneToMany(mappedBy = "team")
		private List<Member> members = new ArrayList<>();
}
```

---

## 일대다 [1:N]

![](https://velog.velcdn.com/images/hong-brother/post/0fefdbe1-bb82-4db7-b28c-c77cbbee78aa/image.png)

- 실무에서 거의 권장하지 않음
    - 연관 관계 관리를 위해 추가적인 UPDATE SQL이 실행 된다.
- 객체와 테이블의 차이 때문에 반대편 테이블의 외래 키를 관리해야하는 구조
- Team 테이블에 JoinColumn을 반드시 사용해야한다. 사용하지 않으면 조인 테이블 방식(중간 Link 테이블)을 사용한다.
- 일대다 단방향 매핑보다는 **다대일 양방향** 매핑을 사용권장

```java
@Getter
@Setter
@Entity
public class Member {
		@Id @GeneratedValue
		@Column(name = "MEMBER_ID")
		private Long id;
		
		@Column(name = "USERNAME")
		private String username;
}
```

```java
@Getter
@Setter
@Entity
public class Team {
		@Id @GeneratedValue
		@Column(name = "TEAM_ID")
		private Long id;
		
		@Column(name = "name")
		private String name;
		
		@OneToMany(mappedBy = "team")
		@JoinColumn(name = "TEAM_ID")
		private List<Member> members = new ArrayList<>();
}
```

---

![](https://velog.velcdn.com/images/hong-brother/post/21cbca86-a108-4764-a4ea-a4ece6372c44/image.png)

- 공식적으로 지원하지 않는 연관관계
- **다대일 양방향을 사용**

```java
@Getter
@Setter
@Entity
public class Member {
		@Id @GeneratedValue
		@Column(name = "MEMBER_ID")
		private Long id;
		
		@Column(name = "USERNAME")
		private String username;

		@ManyToOne
		@JoinColumn(name = "TEAM_ID", insertable = false, updatable = false) 
		// Insert, Update 무효화 즉 읽기 전용
		private Team team;
}
```

```java
@Getter
@Setter
@Entity
public class Team {
		@Id @GeneratedValue
		@Column(name = "TEAM_ID")
		private Long id;
		
		@Column(name = "name")
		private String name;
		
		@OneToMany(mappedBy = "team")
		@JoinColumn(name = "TEAM_ID")
		private List<Member> members = new ArrayList<>();
}
```

---

## 일대일 [1:1]
![](https://velog.velcdn.com/images/hong-brother/post/cb03ad56-b93f-4218-835e-165921f95bd4/image.png)

- 일대일 관계는 그 반대도 일대일 관계
- 주 테이블이나 대상 테이블 중에 외래 키 선택 가능
- 외래 키에 데이터베이스 유니크(UNI) 제약 조건 추가
- 다대일(@ManyToOne) 단방향 매핑과 유사

```java
@Getter
@Setter
@Entity
public class Locker {
		@Id @GeneratedValue
		private Long id;

		@Column(name = "NAME")
		private String name;

}
```

```java
@Getter
@Setter
@Entity
public class Member {
		@Id @GeneratedValue
		@Column(name = "MEMBER_ID")
		private Long id;
		
		@Column(name = "USERNAME")
		private String username;
		
		@OneToOne
		@JoinColumn(name = "LOCKER_ID")
		private Locker;
}
```

![](https://velog.velcdn.com/images/hong-brother/post/4ff858e6-8ce5-40af-b072-9c110e9fd1fd/image.png)

```java
@Getter
@Setter
@Entity
public class Locker {
		@Id @GeneratedValue
		private Long id;

		@Column(name = "NAME")
		private String name;

		@OneToOne(mappedBy = "locker")
		private Member member;

}
```

```java
@Getter
@Setter
@Entity
public class Member {
		@Id @GeneratedValue
		@Column(name = "MEMBER_ID")
		private Long id;
		
		@Column(name = "USERNAME")
		private String username;
		
		@OneToOne
		@JoinColumn(name = "LOCKER_ID")
		private Locker;
}
```

## 일대일 관계에서 외래 키

### 주 테이블에 외래 키

- 주 객체가 대상 객체의 참조를 가지는 것 처럼 주 테이블에 외래 키를 두고 대상 테이블을 찾음
- 객체지향 개발자 선호
- JPA 매핑 편리
- 장점 : 주 테이블만 조회해도 대상 테이블에 데이터가 있는지 확인 가능
- 단점 : 값이 없으면 외래 키에 null 허용

### 대상 테이블에 외래 키

- 대상 테이블에 외래 키가 존재
- 전통적인 데이터베이스 개발자 선호
- 장점 : 주 테이블과 대상 테이블을 일대일에서 일대다 관계로 변경할 때 테이블 구조 유지
- 단점 : 프록시 기능의 한계로 지연 로딩으로 설정해도 항상 즉시 로딩됨

---

## 다대다 [N:M]

### 관계형 데이터 베이스에서 다대다란?
![](https://velog.velcdn.com/images/hong-brother/post/b6eb4c44-6573-44af-8185-37cd76d18619/image.png)
- 관계형 데이터베이스는 정규화된 테이블 2개로 다대다 관계를 표현할 수 없음
- 다대다 관계를 표현하기 위해서는 일대다, 다대일 관계로 표현 할 수 있음

### 객체에서 다대다란?
![](https://velog.velcdn.com/images/hong-brother/post/be133ec7-3190-4ac2-847e-86fc7ab4bba7/image.png)

- Member는 Collection으로 Product List를 가질 수 있고 Product도 Collection으로 Member List를 가질 수 있음

```java
@Getter
@Setter
@Entity
public class Product {
		@Id @GeneratedValue
		private Long id;

		@Column(name = "NAME")
		private String name;
		
		@ManyToMany(mappedBy = "products")
		private List<Member> members = new ArrayList<>();
}
```

```java
@Getter
@Setter
@Entity
public class Member {
		@Id @GeneratedValue
		@Column(name = "MEMBER_ID")
		private Long id;
		
		@Column(name = "USERNAME")
		private String username;
		
		@ManyToMany
		@JoinTable(name = "MEMBER_PRODUCT")
		private List<Product> products = new ArrayList<>();
}
```

### 다대다 한계 극복

- 연결 테이블을 엔티티로 승격하여 @ManyToMany보다는 @OneToMany, @ManyToOne으로 해결한다.
![](https://velog.velcdn.com/images/hong-brother/post/262782e8-fb0a-4b43-8d37-9bdd82085c2e/image.png)

```java
@Getter
@Setter
@Entity
public class Product {
		@Id @GeneratedValue
		private Long id;

		@Column(name = "NAME")
		private String name;
		
		@OneToMany(mappedBy = "products")
		private List<MemberProduct> memberProducts = new ArrayList<>();
}
```

```java
@Getter
@Setter
@Entity
public class Member {
		@Id @GeneratedValue
		@Column(name = "MEMBER_ID")
		private Long id;
		
		@Column(name = "USERNAME")
		private String username;
		
		@OneToMany(mappedBy = "member")
		private List<MemberProduct> memberProducts = new ArrayList<>();
}
```

```java
@Entity
public class MemberProduct {
		@Id @GeneratedValue
		private Long id;

		@ManyToOne
		@JoinCoumn(name = "MEMBER_ID")
		private Member member;

		@ManyToOne
		@JoinCoumn(name = "PRODUCT_ID")
		private Product product; 
}  
```
