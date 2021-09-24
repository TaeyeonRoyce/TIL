# BFS(Breadth First Search)

너비 우선 탐색



- Queue를 활용하여 구현한다.

- 자식의 정점을 방문하는 것이 아닌, 인접한 정점들을 차례로 방문하는 방식.

  `[A, B, C, D, E, F, G]`구조의 이진트리에서

  A - B - C - D - E - F - G 순서로 방문

  (DFS였다면 A - B - D - E - C - F - G)

- BFS는 방문한 노드의 인접한 노드를 큐에 저장하고, 방문한 노드에 visited했다는 표시를 해야한다.

- 레벨이 달라질때 마다 부모의 노드를 큐에서 dequeue하고 현재 레벨의 인접 노드들을 enqueue하며 탐색.

- 주로 두 노드 사이의 최단 경로 혹은 임의의 경로를 찾고 싶을 때 이 방법을 사용함.

- BFS는 너비를 우선 탐색한다는 특성을 활용하여 다른 알고리즘에 적용을 하는것이 핵심.



상세이미지 및 참조

https://chanhuiseok.github.io/posts/algo-27/

대표문제 

https://www.acmicpc.net/problem/2178