# Queue(큐)

- FIFO(First In First Out)의 구조.
- FIFO의 원리로 동작하기 위해서 단순히 배열의 첫 index가 나가고 마지막 index에 추가하는 형태.
- 자료에 추가되는 위치와 삭제되는 곳의 위치가 다르기 때문에 두 개의 포인터(pointer)가 필요. (C언어의 메모리 포인터X, 그냥 curosr같은 순수히 가리키는 것을 말함)
  (head, tail)
- 선형 큐 보단 원형 큐를 많이 사용.
- 선형 큐의 경우, dequeue와 enqueue를  하면서 처음 할당된 메모리를 벗어난 위치에 head, tail 포인터가 지정 될 수 있기 때문에 잘 사용하지 않음.
- 원형 큐를 통해 처음 할당된 메모리를 연속적으로 활용하는 편이 나음.
- dequeue와 enqueue로 밀려서 비어진 메모리를 연속적으로 채우며 활용

원형 큐 대표문제

https://www.acmicpc.net/problem/1158