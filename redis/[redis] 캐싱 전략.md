# Redis

## Redis란?
- Redis는 Remote Dictionary Server의 약자로서 `키-값` 구조의 비정형 데이터를 저장하고 관리하기 위한 오픈소스 이다.
- 비관계형 데이터 베이스 이며 모든 데이터를 메모리로 불러와 처리하는 메모리 기반 데이터베이스(In-Memory) 이다.
- 영속성(Persistence) 보장을 위해 보관과 백업 기능이 있다.

## Key-Value 모델
![](https://velog.velcdn.com/images/hong-brother/post/85938d3e-f7d8-41da-8271-28cd78fc28d4/image.png)
### 주요 모델
- String
	- String 자료형은 단순히 `Key-Value` 형태로 값을 저장한다.
- Set
	- Set은 집합형 Data Type으로 정렬되지 않고 중복되지 않는 데이터를 저장한다.
- Sorted Set
	- Set 구조와 다르게 Value값을 score 기준으로 정렬하여 저장한다.
- List
	- Linked List 형태로 lpush, rpush와 같이 List에 왼쪽 오른쪽에 데이터를 저장 할 수 있으며 Set과 다르게 중복된 데이터를 저장 할 수 있다.
- Hash
	- Hash구조형태로 `field-value` 쌍의 hash 형태로 저장한다.
    
## Expire
- 아무리 서버 메모리가 테라단위로 증설이 가능한다고 하지만 물리적인 하드웨어 비용은 한정적이기에 redis에서는 데이터에 대해서 expire를 설정 할 수 있다.
- 대부분 key에 Expire를 설정할 것을 권장 한다.


# Redis 캐시 전략
## Lazy Loading
![](https://velog.velcdn.com/images/hong-brother/post/c56e5902-73f3-4e86-b8d3-970851752386/image.png)
- 클라이언트에게서 데이터가 필요로 해줄때 캐싱하는 로딩 전략
- 클라이언트 요청시 캐시 데이터가 없다면 캐시를 DB로 부터 로딩한다.


## Write-Through
![](https://velog.velcdn.com/images/hong-brother/post/bc7b78e5-24a0-47f8-88dc-dff2d9849fb7/image.png)
- 데이터를 입력하거나 수정할때는 캐시 데이터 및 DB를 모두 함께 반영한다.
![](https://velog.velcdn.com/images/hong-brother/post/a75a5c39-3088-4fa8-aa6d-0ba11af7921f/image.png)
- 데이터를 조회 할때는 캐시 데이터만 조회 한다.

# 적용
![](https://velog.velcdn.com/images/hong-brother/post/1a5b48ab-34cd-4025-b93b-7a799d283a88/image.png)



# Ref
>
[위키백과](https://ko.wikipedia.org/wiki/%EB%A0%88%EB%94%94%EC%8A%A4)
[redis](https://sabarada.tistory.com/142)
