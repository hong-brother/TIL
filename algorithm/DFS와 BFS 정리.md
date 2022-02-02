## DFS(깊이 우선 탐색) - 스택,재귀
- DFS는 깊이 우선 탐색이라고 부르며 그래프에서 깊은 부분을 우선적으로 탐색하는 알고리즘
- DFS는 스택 자료구조 또는 재귀 함수를 이용하여 구체적인 동작 과정은 아래와 같다.
    - 탐색 시작 노드를 스택에 삽입하고 방문 처리한다.
    - 스택의 최상단 노드에 방문하지 않은 인접한 노드가 하나라도 있으면 그 노드를 스택에 넣고 방문 처리한다. 방문하지 않은 인접 노드가 없으면 스택에서 최상단 노드를 꺼낸다.
    - 더이상 2번의 과정을 수행 할 수 없을때 가지 반복.
```java
    import java.util.*;
    
    public static boolean[] visited = new boolean[9];
    public static ArrayList<ArrayList<Integer>> graph = new ArrayList<ArrayList<Integer>>();

    @Test
    public void dfsTest() {
        for (int i = 0; i < 9; i++) {
            graph.add(new ArrayList<Integer>());
        }

        // 노드 1에 연결된 노드 정보 저장
        graph.get(1).add(2);
        graph.get(1).add(3);
        graph.get(1).add(8);

        // 노드 2에 연결된 노드 정보 저장
        graph.get(2).add(1);
        graph.get(2).add(7);

        // 노드 3에 연결된 노드 정보 저장
        graph.get(3).add(1);
        graph.get(3).add(4);
        graph.get(3).add(5);

        // 노드 4에 연결된 노드 정보 저장
        graph.get(4).add(3);
        graph.get(4).add(5);

        // 노드 5에 연결된 노드 정보 저장
        graph.get(5).add(3);
        graph.get(5).add(4);

        // 노드 6에 연결된 노드 정보 저장
        graph.get(6).add(7);

        // 노드 7에 연결된 노드 정보 저장
        graph.get(7).add(2);
        graph.get(7).add(6);
        graph.get(7).add(8);

        // 노드 8에 연결된 노드 정보 저장
        graph.get(8).add(1);
        graph.get(8).add(7);

        dfs(1);
    }

    public void dfs(int x) {
        //현재 노드를 방문 처리
        visited[x]= true;
        System.out.println(x + " ");
        // 현재 노드와 연결된 다른 노드를 재귀적으로 처리
        for (int i=0; i<graph.get(x).size(); i++) {
            int y= graph.get(x).get(i);
            if(!visited[y])
                dfs(y);
        }
    }
```

## BFS(너비 우선 탐색) - 큐
- BFS는 너비 우선 탐색이라고 부르며, 그래프에서 가가운 노드부터 우선적으로 탐색하는 알고리즘이다.
- BFS는 큐 자료구조를 이용하며, 구체적인 동작 과정은 다음과 같다.
	- 탐색 시작 노드를 큐에 삽입하고 방문 처리를 한다.
    - 큐에서 노드를 꺼낸 뒤에 해당 노드의 인접 노드 중에서 방문하지 않은 노드를 모드 큐에 삽입하고 방문처리한다.
    - 더이상 위의 과정을 수행 할 수 없을때까지 반복한다.
    
```java
    public static boolean[] visited = new boolean[9];
    public static ArrayList<ArrayList<Integer>> graph = new ArrayList<ArrayList<Integer>>();

    @Test
    public void dfsTest() {
        for (int i = 0; i < 9; i++) {
            graph.add(new ArrayList<Integer>());
        }

        // 노드 1에 연결된 노드 정보 저장
        graph.get(1).add(2);
        graph.get(1).add(3);
        graph.get(1).add(8);

        // 노드 2에 연결된 노드 정보 저장
        graph.get(2).add(1);
        graph.get(2).add(7);

        // 노드 3에 연결된 노드 정보 저장
        graph.get(3).add(1);
        graph.get(3).add(4);
        graph.get(3).add(5);

        // 노드 4에 연결된 노드 정보 저장
        graph.get(4).add(3);
        graph.get(4).add(5);

        // 노드 5에 연결된 노드 정보 저장
        graph.get(5).add(3);
        graph.get(5).add(4);

        // 노드 6에 연결된 노드 정보 저장
        graph.get(6).add(7);

        // 노드 7에 연결된 노드 정보 저장
        graph.get(7).add(2);
        graph.get(7).add(6);
        graph.get(7).add(8);

        // 노드 8에 연결된 노드 정보 저장
        graph.get(8).add(1);
        graph.get(8).add(7);

        System.out.println("Start bfs~");
        bfs(1);
        System.out.println(" ");
    }

    public void dfs(int x) {
        // 현재 노드 방문 처리
        visited[x] = true;
        System.out.print(x + " ");
        for(int i=0; i<graph.get(x).size(); i++) {
            int y = graph.get(x).get(i);
            if(!visited[y])
                dfs(y);
        }
    }

    public static void bfs(int start) {
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(start);
        visited[start] = true; // 현재 노드 방문처리

        while (!queue.isEmpty()) {
            int x = queue.poll();
            System.out.print(x + " ");
            for(int i=0; i<graph.get(x).size(); i++) {
                int y = graph.get(x).get(i);
                if(!visited[y]) {
                    queue.offer(y);
                    visited[y] = true;
                }
            }
        }
    }
```

## BFS VS DFS 언제 사용해야할까?
- 모든 노드를 방문하고자할 경우에는 DFS선택
- 깊이 우선 탐색(DFS)이 너비 우선 탐색(BFS)보다 좀 더 간단함
- 검색 속도 자체는 너비 우선 탐색(BFS)에 비해서 느림

- 검색 대상 그래프가 정말 크다면 DFS(깊이 우선)
- 검색 대상의 규모가 크지 않고, 검색 시작 지점으로부터 원하는 대상이 별로 멀지 않다면 BFS을 선택.
