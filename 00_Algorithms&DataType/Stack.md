# Stack

- LIFO(Last-In-First-Out)구조
  
- Queue와 다르게 top에서만 자료의 추가나 삭제가 가능 (top은 stack의 가장 최근에 들어온 자료를 가리킴)
  
- `push` 메소드와 `pop`메소드를 통해 추가/삭제를 함

  

### Stack 구현해보기

```java
/**
 * Stack 구현해보기
 * @param <E>
 * 배열을 활용하여 구현.
 * pointer 개념을 통해 배열의 속도로 가변적으로 보이도록 함
 * 이때 pointer는 null인 원소 중 가장 앞 위치에 있음
 * ex) [1,1,1,null,null,null, ... , null] 인 경우, pointer = 3
 * 단, Stack의 최대 크기가 존재한다는 단점이 있음.
 */
class MyStack<E> {
    private final int STACK_MAX_SIZE;

    private int stackPointer;
    private Object[] stack;


  //Stack을 형성하면 최대 크기와 커서 할당
    public MyStack() {
        this.STACK_MAX_SIZE = 255;
        this.stackPointer = 0;
        stack = new Object[STACK_MAX_SIZE];
    }

  //맨 뒤에 추가. pointer는 null인 원소 중 가장 앞 위치에 있음
  //ex [1, null, ... ,null] 일 때, pointer는 1
  //add(3) -> [1, 3, null, ..., null] pointer +1 => 2
  //STACK_MAX_SIZE보다 큰 경우 실행하지 않음
    public void add(E element) {
        if (stackoverflow()) {
            System.out.println("stackoverflow");
            return;
        }

        stack[stackPointer] = element;
        stackPointer += 1;
    }
  
  //pointer위치 null, pointer -= 1
    public void pop() {
        stack[stackPointer] = null;
        stackPointer -= 1;
    }

  //마지막 원소 반환
    public E peek() {
        return (E) stack[stackPointer - 1];
    }

    public String stackToString() {
        if (isEmpty()) {
            return "null";
        }
      //문자열 연산에 유리한(Mutable) StringBuilder사용 
        StringBuilder sb = new StringBuilder();
        sb.append('[');
        for (int i = 0; ; i++) {
            sb.append(stack[i]);
            if (i == stackPointer - 1)
                return sb.append(']').toString();
            sb.append(", ");
        }
    }

    public boolean isEmpty() {
        if (stackPointer == 0) {
            return true;
        }
        return false;
    }

    public void clear() {
        for (int i = 0; stackPointer > 0 ; i--) {
            stack[i] = null;
        }
    }

    public int size() {
        return stackPointer - 1;
    }

    private boolean stackoverflow() {
        if (stackPointer >= STACK_MAX_SIZE) {
            return true;
        }
        return false;
    }
}

```

