# Sort

> 자료구조에 담긴 데이터를 어떻게 정렬할지에 대한 방법(알고리즘)은 굉장히 다양하다.
> 각각의 알고리즘마다 장점과 단점이 존재하고, 또 어떤 알고리즘은 데이터에 따라 적용이 되거나 안되기도 한다.
> 알고리즘의 기본중 하나인 정렬 알고리즘에 대해 살펴보자



***설명에서 사용되는 정렬기준은 오름차순임***

## Bubble Sort

가장 일반적인 알고리즘으로, 바로 다음 위치에 있는 데이터와 비교하며 자리를 바꾸는 방식이다.

```c
int BubbleSort(int arr[], int n) {
    for(int i = 0; i < n - 1; i++) {
        for(int j = 0; j < (n - i) - 1; j++) {
            if(arr[j] > arr[j + 1]) {
                int temp;
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
  return 0;
}
```

`3 - 2 - 4 - 1`이라는 배열을 정렬한다고 해보자.

위 동작방식에 따르면, 

1. 3과 2를 비교한다.
   - `3 > 2` 이므로, `temp`를 통해 두 수의 위치를 바꾼다 (`2 - 3 - 4 - 1`)
2. `2 - 3 - 4 - 1`에서 `3 > 4`는 참이 아니므로 넘어간다.
3. `4 > 1` 이므로 자리를 바꾼다. `(2 - 3 - 1 - 4)`

위 과정이 지나면 바깥쪽 for문이 한 번 다 돈 것이다.

다음 과정은  i = 1, 즉 3부터 시작하여 반복하는데, `j < (n - i) - 1`것에 주목하자.
버블정렬의 동작은 우선순위가 가장 낮은 데이터를 맨 뒤로 보내는 방식이다. 그리고 이 과정을 배열의 크기만큼 반복하는 것이다.

그렇기 때문에, 동작한 횟수만큼 맨 뒤의 데이터는 정렬 기준에 맞는 자리에 위치하게 되므로 범위를 설정하여 불필요한 연산을 조금 줄일 수 있다.

### 성능 평가

이중 for문이 돌고있다. 동작 범위를 보면 $n * (n-1)$ 정도로 생각할 수 있다. => $O(n^2)$



참고 이미지 (from https://wonjayk.tistory.com/219)

![정렬 - 버블정렬](https://t1.daumcdn.net/cfile/tistory/275F9A4A545095BD01)





## Selection Sort

정렬 순서에 맞게 하나씩 선택해서 옮기는 정렬.

```C
int SelSort(int arr[], int n) {
    for(int i = 0; i < n - 1; i++) {
        int maxIdx = i; //우선순위가 max (제일 낮은 숫자)
        for(int j = i + 1; j < n; j++) {
            if(arr[j] < arr[maxIdx])
                maxIdx = j;
        }
        int temp;
        temp = arr[maxIdx];
        arr[maxIdx] = arr[i];
        arr[i] = temp;
    }
  
  return 0;
}
```

버블정렬과 유사하게 `temp`를 활용하여 `maxIdx`를 찾아 교환
위치에 따라 우선순위에 맞는 값을 선택하서 넣음

`3 - 2 - 4 - 1`이라는 배열을 정렬한다고 해보자.

1. i번째 index에 들어갈 (우선순위가 0인) 수의 위치를 찾는다. => `maxIdx` = 3
2. `maxIdx`에 위치한 데이터와 현재 i 와 교환한다. => `(1 - 2 - 4 - 3)`
3. i를 증가하여 반복한다.

### 성능 평가

버블정렬과 다를 바 없이 $O(n^2)$이다.



참고 이미지 (from https://wonjayk.tistory.com/m/217)

![정렬 - 선택정렬](https://t1.daumcdn.net/cfile/tistory/256B9C34545081D835)



## Insertion Sort

삽입 정렬은 정렬이 된 부분과 아직 정렬되지 않은 부분으로 나뉜다.

다음 인덱스로 넘어가면서 정렬되지 않은 데이터를 정렬영역에 맞는 위치에 삽입하며 정렬을 구현한다.

```C
int InsertSort(int arr[], int n) {
    int i, j;
    int insData;

    for(i = 1; i < n; i++) {
        insData = arr[i];
        for(j = i - 1; j >= 0; j--) {
            if(arr[j] >	insData)
                arr[j + 1] = arr[j];
            else
                break;
        }

        arr[j + 1] = insData;
    }

    return 0;
}
```

1. 리스트의 처음부터 값을 비교하며 정렬된 부분의 어느 위치에 삽입되어야 하는가를 판단

2. 해당 위치에 이 숫자를 삽입하게 되면 정렬된 부분의 크기는 하나씩 커지며 자리이동

3. 정렬되지 않은 부분은 크기가 하나 줄어 듦



참고 이미지 (from https://wonjayk.tistory.com/218)

![img](https://t1.daumcdn.net/cfile/tistory/2569FD3854508BE811)