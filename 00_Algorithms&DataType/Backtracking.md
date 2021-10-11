# BackTracking(퇴각검색)

###  

- 해를 찾아나아 갈때, 특정한 조건만 만족하는 경우만 살펴보는 방법.

- DFS랑 비슷하지만, 모든 경우를 탐색하는 것이 아니기 때문에 경우의 수가 줄어듬.

- 보통 DFS과정에서 현재 노드에 조건을 걸어 절대 답이 될수 없는 노드(가지)는 바로 바져나와 부모노드로 다시 돌아가는 방법.

- 유망한 노드인지 아닌지 구별하는 조건 설정이 핵심.

  

대표적인 문제로 N-Queen 문제가 있다.

https://www.acmicpc.net/problem/9663
