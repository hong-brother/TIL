# JAVA Collection
>
컬렉션이란 JAVA에서 데이터를 저장하는 기본적인 자료구조들을 한 곳에 모아 관리하고 편하게 사용하기 위해 제공 JCF(Java Collection Framework)의 상속 구조 이며 크게 **Collection** 과 **Map**으로 나뉜다.

![](https://images.velog.io/images/hong-brother/post/c07e3d05-6adc-4d11-9d20-41126f31a15b/JCF%202.001.jpeg)


## 1. Set(집합) Collection
- 특징
	1. 요소의 **저장 순서**를 유지하지 않는다.
	2. 같은 요소의 **중복 저장**을 허용하지 않는다.
    
- 종류
	- HashSet<E>
  	- TreeSet<E>
  	- TreeSet<E>

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
  	- TreeSet도 HashSet과 같이 중복을 허용하지 않고, 순서를 가지지 않는다 하지만 다르게 **데이터를 정렬하여 저장**하고 있다는 특징이 있다.
  
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
