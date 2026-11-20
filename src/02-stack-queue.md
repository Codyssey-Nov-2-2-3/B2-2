# 스택과 큐

## 1. 스택(Stack)

**LIFO(Last In, First Out)**: 가장 나중에 들어간 데이터가 가장 먼저 나온다. 접시를 쌓고 맨 위부터 꺼내는 것과 같다.

| 연산 | 설명 | 시간복잡도 |
|---|---|---|
| `push` | 맨 위에 데이터를 넣는다 | O(1) |
| `pop` | 맨 위 데이터를 꺼내고 제거한다 | O(1) |
| `peek` | 맨 위 데이터를 제거하지 않고 확인한다 | O(1) |
| `is_empty` | 비어 있는지 확인한다 | O(1) |

활용 예: 함수 호출 스택, 괄호 짝 검사, 실행 취소(Undo), 깊이 우선 탐색(DFS)

## 2. 큐(Queue)

**FIFO(First In, First Out)**: 가장 먼저 들어간 데이터가 가장 먼저 나온다. 줄 서서 순서대로 처리하는 것과 같다.

| 연산 | 설명 | 시간복잡도 |
|---|---|---|
| `enqueue` | 맨 뒤에 데이터를 넣는다 | O(1) |
| `dequeue` | 맨 앞 데이터를 꺼내고 제거한다 | O(1) |
| `front` | 맨 앞 데이터를 제거하지 않고 확인한다 | O(1) |
| `is_empty` | 비어 있는지 확인한다 | O(1) |

활용 예: 작업 대기열, 프린터 출력 순서, 너비 우선 탐색(BFS)

## 3. 스택과 큐 비교

| 구분 | 스택 | 큐 |
|---|---|---|
| 처리 순서 | LIFO | FIFO |
| 삽입 위치 | 맨 위 | 맨 뒤 |
| 삭제 위치 | 맨 위 | 맨 앞 |

## 4. Python 3.10+ 구현 예제

### 스택: `list` 사용

파이썬 `list`의 `append`와 `pop()`은 모두 맨 뒤에서 동작하므로 O(1)이다. 리스트의 끝을 스택의 맨 위로 쓴다.

```python
class Stack:
    def __init__(self) -> None:
        # 예제를 단순하게 유지하기 위해 원소 타입을 int와 str로 제한한다.
        self._items: list[int | str] = []

    def push(self, item: int | str) -> None:
        self._items.append(item)

    def pop(self) -> int | str:
        if self.is_empty():
            raise IndexError("빈 스택에서는 pop할 수 없다")
        return self._items.pop()

    def peek(self) -> int | str:
        if self.is_empty():
            raise IndexError("빈 스택에서는 peek할 수 없다")
        return self._items[-1]

    def is_empty(self) -> bool:
        return not self._items


if __name__ == "__main__":
    stack = Stack()
    stack.push(1)
    stack.push(2)
    stack.push(3)
    print(stack.pop())  # 가장 나중에 넣은 값이 먼저 나온다.
    print(stack.peek())
    print(stack.is_empty())

    try:
        Stack().pop()
    except IndexError as error:
        print(error)
```

**실행 결과**

```text
3
2
False
빈 스택에서는 pop할 수 없다
```

### 큐: `collections.deque` 사용

`list.pop(0)`은 나머지 원소를 한 칸씩 당기므로 O(n)이다. 큐에는 양 끝 삽입/삭제가 모두 O(1)인 `deque`를 쓴다.

```python
from collections import deque


class Queue:
    def __init__(self) -> None:
        # 예제를 단순하게 유지하기 위해 원소 타입을 int와 str로 제한한다.
        self._items: deque[int | str] = deque()

    def enqueue(self, item: int | str) -> None:
        self._items.append(item)

    def dequeue(self) -> int | str:
        if self.is_empty():
            raise IndexError("빈 큐에서는 dequeue할 수 없다")
        return self._items.popleft()

    def front(self) -> int | str:
        if self.is_empty():
            raise IndexError("빈 큐에서는 front를 볼 수 없다")
        return self._items[0]

    def is_empty(self) -> bool:
        return not self._items


if __name__ == "__main__":
    queue = Queue()
    queue.enqueue("a")
    queue.enqueue("b")
    queue.enqueue("c")
    print(queue.dequeue())  # 가장 먼저 넣은 값이 먼저 나온다.
    print(queue.front())
    print(queue.is_empty())

    try:
        Queue().dequeue()
    except IndexError as error:
        print(error)
```

**실행 결과**

```text
a
b
False
빈 큐에서는 dequeue할 수 없다
```
