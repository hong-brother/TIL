# JAVA Collection
>
컬렉션이란 JAVA에서 데이터를 저장하는 기본적인 자료구조들을 한 곳에 모아 관리하고 편하게 사용하기 위해 제공 JCF(Java Collection Framework)의 상속 구조 이며 크게 **Collection** 과 **Map**으로 나뉜다.

![](https://images.velog.io/images/hong-brother/post/c07e3d05-6adc-4d11-9d20-41126f31a15b/JCF%202.001.jpeg)

## Collection의 특징
 - 제네릭이라는 기법으로 구현 되어 있다. 컬렉션 클래스나 인터페이스의 이름에 < E >, < K >, < V >이 포함 되어 있고 이들은 **타입 매개 변수**로 붚리며, 컬렉션의 요소(element)를 인반화시킨 타입니다. 특정 타입만 다루지 않고, 여러 종류의 타입으로 변신할 수 있도록 컬렉션을 일반화시키기 위해 < E > 를 사용 하는것 이다. 그러므로 E를 일반화시킨 타입 혹은 **제네릭 타입(generic type)** 이라고도 부른다.
 - 컬렉션의 요소는 객체들만 가능하다. int, char, double 등의 기본 타입의 데이터는 기본적으로 컬렉션의 요소로 불가능하지만, 기본 타입의 값이 삽입되면 자동으로 박싱(auto boxing)에 의해 Wrapper 클래스로 변환되어 객체 형태로 저장한다.

## 1. Set(집합) Collection
- 특징
	1. 요소의 **저장 순서**를 유지하지 않는다.
	2. 같은 요소의 **중복 저장**을 허용하지 않는다.
    
- 종류

|   클래스   |특징|
|----------|----|
|  HashSet | 순서에 상관없이 저장하고 중복값을 허용하지 않는다. Set 중에 가장 성능이 좋다.|
|  TreeSet | 중복값을 허용하지 않고 데이터를 정렬하여 저장한다.|
| LinkedHashSet | 중복을 허용하지 않고 저장된 순서에 따라 값이 정렬 |

  ### 1.1 HashSet
  - 특징
  	- HashSet은 Set 인터페이스를 구현하므로 요소를 **순서에 상관없이 저장**하고 **중복값을 허용하지** 않는다.
  	- 만약 요소의 저장 순서를 유지해야 한다면 LinkedHashSet 클래스를 사용.
  
```java
    @Test
    @DisplayName("HashSet Test")
    public void hashSetTest() {
        HashSet<Integer> hashSet = new HashSet<Integer>();
        hashSet.add(21);
        hashSet.add(12);
        hashSet.add(1);
        hashSet.add(3);
        hashSet.add(15);
        hashSet.add(3);
        hashSet.add(15);
        for (int i : hashSet) {
            System.out.println("result = " + i);
        }
		assertEquals(hashSet.toString(), "[21, 12, 1, 3, 15]");
    }
```
  결과   
```
  result = 1
  result = 3
  result = 21
  result = 12
  result = 15

  expected: <[1, 3, 21, 12, 15]> but was: <[1, 3, 15, 12, 21]>
  Expected :[1, 3, 21, 12, 15]
  Actual   :[1, 3, 15, 12, 21]
```
  
  ### 1.2 LinkedHashSet
  - 특징
  	- LinkedHashSet은 중복을 허용하지 않고 HashSet과 다르게 순서를 가진다는 특징이 있다.
  
```java
    @Test
    @DisplayName("LinkedHashSet Test")
    public void linkedHashSetTest() {
        LinkedHashSet<Integer> hashSet = new LinkedHashSet<>();
        hashSet.add(21);
        hashSet.add(12);
        hashSet.add(1);
        hashSet.add(3);
        hashSet.add(15);
        hashSet.add(3);
        hashSet.add(15);
        for (int i : hashSet) {
            System.out.println("result = " + i);
        }
        assertEquals(hashSet.toString(), "[21, 12, 1, 3, 15]");
    }
```
  
```
  result = 21
  result = 12
  result = 1
  result = 3
  result = 15
```
  
  ### 1.3 TreeSet
  - 특징
  	- TreeSet도 HashSet과 같이 중복을 허용하지 않고, 순서를 가지지 않는다 하지만 다르게 **오름차순으로 데이터를 정렬하여 저장**하고 있다는 특징이 있다.
  
```java
    @Test
    @DisplayName("treeSetTest")
    public void treeSetTest() {
        TreeSet<Integer> treeSet = new TreeSet<>();
        treeSet.add(21);
        treeSet.add(12);
        treeSet.add(1);
        treeSet.add(3);
        treeSet.add(15);
        treeSet.add(3);
        for (int i: treeSet) {
            System.out.println("result = " + i);
        }
        assertEquals(treeSet.toString(), "[1, 3, 12, 15, 21]");
    }
```
  
```
  result = 1
  result = 3
  result = 12
  result = 15
  result = 21
```
  
 
## 2. List(집합) Collection
- 특징 
	- 순서를 가지고 가지고 있으며 원소의 중복이 허용된다는 특징이 있다.
- 종류

|  클래스   |  특징  |
|---------|-------|
|ArrayList|~~~|
| Stack| |
|LinkedList||
|Vector| |

### 2-1 ArrayList
- 특징 :
**가변 크기의 배열을 구현한 컬렉션 클래스로서 Vector 클래스와 거의 동일하다.** ArrayList는 스레드 간에 **동기화를 지원하지 않기 때문에** 다수의 스레드가 동시에 ArrayList에 요소를 삽입하거나 삭제할 때 충돌이 발생할 소지가 많다. ArrayList를 이용하려면 멀티스레드의 동기화를 사용자가 직접 구현해야 한다.
요소를 맨 뒤에 추가하거나 중간에 삽입 가능하며, 삽입이나 삭제 시 중간에 빈 공간이 생기지 않도록 요소들의 위치를 앞뒤로 하나씩 자동으로 이동시킨다.

```java
    @Test
    @DisplayName("Test ArrayList")
    public void arrayListTest(){
        List<String> list = new ArrayList<>();
        list.add("Hello");
        list.add("word");
        list.add(null);
        list.add(1, "temp"); // list 중간에 삽입
    }
```

### 2-2 Vector

- 특징 : 
List 인터페이스를 구현한 클래스로서 가변 개수의 배열이 필요할 때 적합하다. 백터에 삽입되는 요소의 수가 많아지면 자동으로 크기가 조절 된다. 요소는 백터의 맨 마지막이나 중간에 삽입될 수 있다.
**Vector는 자동 동기화를 보장하며 멀티 스레드 환경에서 안정적으로 사용이 가능하다.** 하지만 단일 스레드에서는 **ArrayList가 성능이 더 좋다.**

```java
    @Test
    @DisplayName("Test Vector")
    public void VectorTest() {
        List<Integer> vector = new Vector<>();
        vector.add(2);
        vector.add(5);
        vector.add(1);
        vector.add(6);
    }
```

### 2-3 LinkedList

- 특징 : 
**LinkedList는 요소들을 양방향(next, prev)으로 연결하여 관리한다는 점**을 제외하고 Vector, ArrayList와 매우 유사하다.
**포인터로 가각의 노드들을 연결하고 있어서, 삽입/삭제가 빠르다는 장점이 있다.**
(단순히 기존 포인터를 끊고 새로운 노드에 연결하면 되기 때문)
하지만 특정 원소를 조회 하기 위해서는 첫 노드부터 순회해야 하기 때문에 ArrayList에 비해 느리다는 단점이 있다.

```java
    @Test
    @DisplayName("Test LinkedList")
    public void linkedList() {
        LinkedList<Integer> linkedList = new LinkedList<>();
        linkedList.add(3);
        linkedList.add(4);
        linkedList.addFirst(1); // 맨앞 삽입
        linkedList.addLast(2); // 맨뒤 삽입
        linkedList.removeFirst(); // 맨앞 삭제
        linkedList.removeLast(); // 맨뒤 삭제
    }
```
