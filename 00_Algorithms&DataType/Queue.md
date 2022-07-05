# Queue

> **FIFO(First In First Out)의 구조를 가진 자료구조**

Queue에서 데이터가 나가는 위치와 추가되는 위치가 다르다.

- **Enqueue : 데이터 추가**

- **Dequeue : 데이터 추출**



## 연결리스트 기반 구현

우선, 데이터가 들어오고 나가는 위치가 다르므로 이를 가리킬 포인터가 2개 필요하다.

### ListQueue.h

```c
#ifndef DATASTRUCTURE_LINKEDLISTQUEUE_H
#define DATASTRUCTURE_LINKEDLISTQUEUE_H

#define TRUE 1
#define FALSE 0

typedef int Data;

typedef struct _node {
    Data data;
    struct _node *next;
} Node;

typedef struct _lQueue {
    Node *front;
    Node *rear;
} LQueue;

typedef LQueue Queue;

void QueueInit(Queue *pq);

int QIsEmpty(Queue *pq);

void Enqueue(Queue *pq, Data data);

Data Dequeue(Queue *pq);

Data QPeek(Queue *pq);

#endif //DATASTRUCTURE_LINKEDLISTQUEUE_H

```

- `QueueInit` : Queue 초기화
- `Enqueue` : 데이터 추가
- `Dequeue` : 데이터 추출
- `QPeek` : 데이터 peek (선택)

#### **`QueueInit`**

```C
void QueueInit(Queue *pq) {
    pq->front = NULL;
    pq->rear = NULL;
}
```

- 처음 `QueueInit`으로 Queue를 생성하면, `Front`, `Rear` 모두 NULL을 가리키도록 한다.

### `Enqueue`

```C
void Enqueue(Queue *pq, Data data) {
    Node *newNode = (Node *)malloc(sizeof(Node));
    newNode->next = NULL;
    newNode->data = data;

    if(QIsEmpty(pq)) {
        pq->front = newNode;
        pq->rear = newNode;
    }
    else {
        pq->rear->next = newNode;
        pq->rear = newNode;
    }
}
```

- 추가된 데이터를 기반으로 새로운 노드를 생성한다
- 만약 Queue가 비어있다면, Front와 Rear 모두 가리키도록 하고,
- 일반적인 상황에선 Rear가 가리키는 노드 뒤로 추가한 뒤, Rear가 새로운 노드를 가리키도록 한다(꼬리를 유지)

### `Dequeue`

```C
Data Dequeue(Queue *pq) {
    Node *delNode;
    Data retData;

    if(QIsEmpty(pq)) {
        printf("Queue Memory Error!\n");
        exit(-1);
    }

    delNode = pq->front;
    retData = delNode->data;

    pq->front = pq->front->next;

    free(delNode);
    return retData;
}
```

- Queue가 비어있지 않는 경우에만 수행
- Dequeue는 앞(Front)에서 이루어 지므로 Front 포인터를 지워질 노드 다음으로 옮긴 뒤 지운다.

### 전체코드 (ListBaseQueue.c)

```C
#include <stdio.h>
#include <stdlib.h>
#include "LinkedListQueue.h"

void QueueInit(Queue *pq) {
    pq->front = NULL;
    pq->rear = NULL;
}

int QIsEmpty(Queue *pq) {
    if(pq->front == NULL)
        return TRUE;
    else
        return FALSE;
}

void Enqueue(Queue *pq, Data data) {
    Node *newNode = (Node *)malloc(sizeof(Node));
    newNode->next = NULL;
    newNode->data = data;

    if(QIsEmpty(pq)) {
        pq->front = newNode;
        pq->rear = newNode;
    }
    else {
        pq->rear->next = newNode;
        pq->rear = newNode;
    }
}

Data Dequeue(Queue *pq) {
    Node *delNode;
    Data retData;

    if(QIsEmpty(pq)) {
        printf("Queue Memory Error!\n");
        exit(-1);
    }

    delNode = pq->front;
    retData = delNode->data;

    pq->front = pq->front->next;

    free(delNode);
    return retData;
}

Data QPeek(Queue *pq) {
    if(QIsEmpty(pq)) {
        printf("Queue Memory Error!\n");
        exit(-1);
    }

    return pq->front->data;
}
```

Queue의 구조와 동작은 비교적 쉽게 이해가 된다.

### 배열 기반 구현

배열을 기반으로 하면, 고려해야 할 것이 있다.

앞선 LinkedList를 활용해서 구현했던 것 처럼, Enqueue와 Dequeue를 구현할 때 Front와 Rear 개념을 활용해서 가리키는 위치를 변경시키는 아이디어를 활용한다면, 다음과 같이 동작할 것이다.

|        | Front, Rear |      |      |      |
| ------ | ----------- | ---- | ---- | ---- |
| 인덱스 | 0           |      |      |      |
| 데이터 | A           |      |      |      |

-> `Enqueue(B)`, `Enqueue(C)`, `Enqueue(D)`

|        | Front |      |      | Rear |
| ------ | ----- | ---- | ---- | ---- |
| 인덱스 | 0     | 1    | 2    | 3    |
| 데이터 | A     | B    | C    | D    |

이처럼, `Rear`가 이동하며 꼬리 부분에 추가하는 `Enqueue`가 구현될 것이고,

-> `Dequeue()`, `Dequeue()`

|        |      |      | Front | Rear |
| ------ | ---- | ---- | ----- | ---- |
| 인덱스 |      |      | 2     | 3    |
| 데이터 |      |      | C     | D    |

`Front`가 이동하며 `Dequeue`가 구현될 것이다.

위 방식에서 다음과 같은 문제점이 발생한다.
`Queue`의 `MaxSize`가 4라고 하면, 현재 `size = 2`이다. 논리적으로 데이터를 추가하기엔 공간이 충분하지만 `Rear`가 더 이상 이동할 위치가 없다.

배열로 구현한 Queue의 경우 이러한 문제가 발생할 수 있기에, 원형큐라는 개념을 활용한다. 위와 같은 상황에서 데이터가 추가되면, `Rear`를 맨 앞으로 옮기는 것이다.

-> `Enqueue(E)`

|        | Rear |      | Front |      |
| ------ | ---- | ---- | ----- | ---- |
| 인덱스 | 0    |      | 2     | 3    |
| 데이터 | E    |      | C     | D    |



추가적으로

- Enqueue 연산 시, Rear를 먼저 이동 시킨 후 데이터를 저장하고,
- Dequeue 연산 시, Front를 먼저 이동 시킨뒤 데이터를 반환 & 소멸

하도록 구현한다면 다음과 같은 결과를 얻을 수 있다.

- 원형 큐가 비어있는 경우 => `Front`와 `Rear`가 동일한 위치를 가리킨다.
- 원형 큐가 꽉 찬 경우 => `Rear` 다음의 위치가 `Front`이다



### 구현 ( CircularQueue.h)

```C
#define TRUE 1
#define FALSE 0

#define QUE_LEN 100
typedef int Data;

typedef struct _cQueue {
    int front;
    int rear;
    Data queArr[QUE_LEN]; //배열 기반
} CQueue;

typedef CQueue Queue;

void QueueInit(Queue *pq);

int QIsEmpty(Queue *pq);

void Enqueue(Queue *pq, Data data);

Data Dequeue(Queue *pq);

Data QPeek(Queue *pq);
```

### CircularQueue.c

```C
#include <stdio.h>
#include <stdlib.h>
#include "CircularQueue.h"

void QueueInit(Queue *pq) {
    pq->front = 0;
    pq->rear = 0;
}

int	QIsEmpty(Queue *pq) {
    if(pq->front == pq->rear) //원형 큐가 비어있는 경우 특징
        return TRUE;
    else
        return FALSE;
}

int NextPosIdx(int pos) {
    if(pos == QUE_LEN - 1)
        return 0;
    else
        return pos + 1;
}

//rear 한칸 이동 후 Data 추가
void Enqueue(Queue *pq, Data data) {
    if(NextPosIdx(pq->rear) == pq->front) {
        printf("Queue Memory Error!\n");
        exit(-1);
    }

    pq->rear = NextPosIdx(pq->rear);
    pq->queArr[pq->rear] = data;
}

//Front 한 칸 이동 후 Data 추가
Data Dequeue(Queue *pq) {
    if(QIsEmpty(pq)) {
        printf("Queue Memory Error!\n");
        exit(-1);
    }

    pq->front = NextPosIdx(pq->front);

    return pq->queArr[pq->front];
}

Data QPeek(Queue *pq) {
    if(QIsEmpty(pq)) {
        printf("Queue Memory Error!\n");
        exit(-1);
    }

    return pq->queArr[NextPosIdx(pq->front)];
}
```

배열 기반의 큐는 대부분이 원형 큐이다. 원형 큐라고 해서 새롭거나 특별한 큐의 종류는 아니다. 단지, 큐를 더 원활하게 구현하기 위해 고안된 아이디어 임을 잊지 말자.

# Deque

`Head`와 `Tail`에서 데이터의 삽입&삭제가 모두 가능한 구조

-> 앞에서 삽입 & 삭제
-> 뒤에서 삽입 & 삭제

기능을 가짐

### 구현 파일 : `Deque.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include "Deque.h"

void DequeInit(Deque *pdeq) {
    pdeq->head = NULL;
    pdeq->tail = NULL;
}

int DQIsEmpty(Deque *pdeq) {
    if(pdeq->head == NULL)
        return TRUE;
    else
        return FALSE;
}

void DQAddFirst(Deque *pdeq, Data data) {
    Node *newNode =(Node * )malloc(sizeof(Node));
    newNode->data = data;
    newNode->prev = NULL;

    newNode->next = pdeq->head;

    if(DQIsEmpty(pdeq))
        pdeq->tail = newNode;
    else
        pdeq->head->prev = newNode;

    pdeq->head = newNode;
}

void DQAddLast(Deque *pdeq, Data data) {
    Node *newNode = (Node *)malloc(sizeof(Node));
    newNode->data = data;
    newNode->next = NULL;

    newNode->prev = pdeq->tail;

    if(DQIsEmpty(pdeq))
        pdeq->head = newNode;
    else
        pdeq->tail->next = newNode;

    pdeq->tail = newNode;
}

Data DQRemoveFirst(Deque *pdeq) {
    Node *delNode;
    Data retData;

    if(DQIsEmpty(pdeq)) {
        printf("Deque Memory Error!\n");
        exit(-1);
    }

    delNode = pdeq->head;
    retData = pdeq->head->data;

    pdeq->head = pdeq->head->next;

    free(delNode);

    if(pdeq->head == NULL)
        pdeq->tail = NULL;
    else
        pdeq->head->prev = NULL;

    return retData;
}

Data DQRemoveLast(Deque *pdeq) {
    Node *delNode;
    Data retData;

    if(DQIsEmpty(pdeq)) {
        printf("Deque Memory Error!\n");
        exit(-1);
    }

    delNode = pdeq->tail;
    retData = pdeq->tail->data;

    pdeq->tail = pdeq->tail->prev;

    free(delNode);

    if(pdeq->tail == NULL)
        pdeq->head = NULL;
    else
        pdeq->tail->next = NULL;

    return retData;
}

Data DQPeekFirst(Deque *pdeq) {
    if(DQIsEmpty(pdeq)) {
        printf("Deque Memory Error!\n");
        exit(-1);
    }

    return pdeq->head->data;
}

Data DQPeekLast(Deque *pdeq) {
    if(DQIsEmpty(pdeq)) {
        printf("Deque Memory Error!\n");
        exit(-1);
    }

    return pdeq->tail->data;
}
```

