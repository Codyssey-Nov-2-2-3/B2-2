# 01. 배열과 연결 리스트

담당: 팀원 A (김동탁) / 실행 환경: Python 3.10 이상

## 1. 들어가며

배열과 연결 리스트는 데이터를 한 줄로 늘어놓는 **선형 자료구조**입니다. 둘 다 "순서가 있는 데이터 묶음"이라는 점은 같지만, 데이터를 **메모리에 어떻게 놓느냐**가 다르고, 이 차이가 접근·삽입·삭제 속도를 갈라 놓습니다.

## 2. 배열 (Array)

### 개념

같은 종류의 요소를 **연속된 메모리 공간**에 나란히 저장하고, 번호(인덱스)로 접근하는 구조입니다.

```text
index    0     1     2     3
       +-----+-----+-----+-----+
value  | 10  | 20  | 30  | 40  |   <- 메모리에 연속으로 배치
       +-----+-----+-----+-----+
```

### 핵심 원리

요소의 주소는 `시작 주소 + 인덱스 × 요소 크기`로 계산됩니다. 몇 번째 요소든 계산 한 번이면 바로 찾아가므로 **인덱스 접근은 O(1)**입니다.

### 삽입과 삭제

- 맨 뒤에 넣고 빼는 것은 빠릅니다.
- 앞이나 중간에 넣거나 빼면 뒤쪽 요소를 한 칸씩 밀거나 당겨야 해서 **O(n)**입니다.
- 전통적인 배열은 만들 때 크기가 정해지며, 가득 차면 더 큰 공간을 새로 잡아 옮겨야 합니다.

### 파이썬의 `list`

파이썬의 `list`는 **동적 배열**입니다. (CPython 기준)

- 요소 값 자체가 아니라 **객체에 대한 참조**를 연속된 공간에 저장합니다. 그래서 서로 다른 타입을 섞어 담을 수 있습니다.
- 공간이 가득 차면 여유 있게 더 큰 공간을 잡아 옮기므로, `append`는 평균적으로 **O(1)**입니다.
- `insert(0, x)`나 `pop(0)`처럼 앞쪽을 건드리는 연산은 **O(n)**입니다.

## 3. 연결 리스트 (Linked List)

### 개념

**노드(node)** 들이 참조(포인터)로 이어진 구조입니다. 노드 하나는 데이터와 **다음 노드에 대한 참조**를 가집니다. 첫 노드를 가리키는 `head`만 알면 나머지는 참조를 따라 갈 수 있습니다.

```text
head
 |
 v
[10 | next] --> [20 | next] --> [30 | next] --> None
```

### 핵심 원리

노드가 메모리에 **흩어져 있어도 됩니다.** 위치 정보를 `next` 참조가 대신 들고 있기 때문입니다. 대신 `i`번째 노드를 찾으려면 `head`부터 하나씩 따라가야 하므로 **탐색과 인덱스 접근은 O(n)**입니다.

### 삽입과 삭제

- 맨 앞 삽입은 새 노드의 `next`를 기존 `head`로 연결하고 `head`를 바꾸면 끝나서 **O(1)**입니다.
- 삽입·삭제할 위치의 **앞 노드를 이미 알고 있다면** 참조 한두 개만 바꾸면 되어 **O(1)**입니다. 요소를 옮길 필요가 없습니다.
- 다만 그 위치를 찾는 과정은 **O(n)**이라, 값을 기준으로 삭제하면 전체로는 O(n)입니다.

### 종류

| 종류 | 설명 |
|---|---|
| 단일 연결 리스트 | 노드가 다음 노드만 가리킵니다. 가장 단순한 형태입니다. |
| 이중 연결 리스트 | 이전 노드와 다음 노드를 모두 가리킵니다. 양방향 이동이 가능합니다. |
| 원형 연결 리스트 | 마지막 노드가 첫 노드를 가리켜 고리를 이룹니다. |

## 4. 배열과 연결 리스트 비교

| 비교 항목 | 배열 (파이썬 `list`) | 연결 리스트 |
|---|---|---|
| 메모리 배치 | 연속된 공간 | 노드가 흩어져 있고 참조로 연결 |
| 인덱스 접근 | 바로 접근 (빠름) | 처음부터 따라가야 함 (느림) |
| 앞쪽 삽입·삭제 | 뒤 요소를 모두 이동 (느림) | 참조만 변경 (빠름) |
| 뒤쪽 삽입·삭제 | 빠름 | `tail`을 따로 관리하지 않으면 끝까지 탐색 |
| 추가 메모리 | 거의 없음 | 노드마다 `next` 참조를 저장 |
| 크기 변경 | 가득 차면 더 큰 공간으로 이동 | 노드를 하나씩 붙이고 떼면 됨 |

## 5. 언제 무엇을 쓰나

- **배열(`list`)이 맞는 경우**: 인덱스로 자주 접근하거나, 끝에서 추가·삭제하는 일이 많거나, 처음부터 끝까지 순회하는 일이 많을 때
- **연결 리스트가 맞는 경우**: 앞쪽이나 중간에서 삽입·삭제가 잦고, 삽입할 위치의 노드를 이미 알고 있을 때
- **파이썬에서는**: 대부분 `list`로 충분합니다. 양쪽 끝에서 넣고 빼는 일이 많다면 `collections.deque`를 씁니다. 양 끝 삽입·삭제가 O(1)이며, 내부적으로 연결 구조를 사용합니다.

## 6. 파이썬 구현 예제

아래 코드는 Python 3.10 이상에서 그대로 실행됩니다. 코드 블록을 파일로 저장해 `python3 파일명.py`로 실행하면 바로 아래 실행 결과가 나옵니다.

### 6-1. 배열(`list`) 기본 연산

```python
numbers = [10, 20, 30, 40]

print(numbers[2])      # 인덱스 접근: 계산 한 번으로 바로 찾는다
numbers.append(50)     # 끝에 추가
numbers.insert(0, 5)   # 맨 앞에 삽입: 뒤 요소를 모두 한 칸씩 민다
print(numbers)
numbers.pop()          # 끝 삭제
numbers.pop(0)         # 맨 앞 삭제: 뒤 요소를 모두 한 칸씩 당긴다
print(numbers)
```

실행 결과:

```text
30
[5, 10, 20, 30, 40, 50]
[10, 20, 30, 40]
```

### 6-2. 단일 연결 리스트 구현

```python
class Node:
    """연결 리스트의 노드. 데이터와 다음 노드에 대한 참조를 가진다."""

    def __init__(self, data, next_node=None):
        self.data = data
        self.next = next_node


class SinglyLinkedList:
    """head 하나만 관리하는 단일 연결 리스트."""

    def __init__(self):
        self.head = None
        self._size = 0

    def __len__(self):
        return self._size

    def __iter__(self):
        current = self.head
        while current is not None:
            yield current.data
            current = current.next

    def prepend(self, data):
        """맨 앞에 추가한다. head만 바꾸면 되므로 O(1)."""
        self.head = Node(data, self.head)
        self._size += 1

    def append(self, data):
        """맨 뒤에 추가한다. 마지막 노드를 찾아가야 하므로 O(n)."""
        new_node = Node(data)
        if self.head is None:
            self.head = new_node
        else:
            current = self.head
            while current.next is not None:
                current = current.next
            current.next = new_node
        self._size += 1

    def find(self, data):
        """값이 같은 첫 노드를 돌려준다. 없으면 None. O(n)."""
        current = self.head
        while current is not None:
            if current.data == data:
                return current
            current = current.next
        return None

    def remove(self, data):
        """값이 같은 첫 노드를 삭제한다. 삭제했으면 True, 없으면 False.

        핵심은 앞 노드(prev)의 next가 삭제할 노드를 건너뛰게 바꾸는 것이다.
        """
        prev = None
        current = self.head
        while current is not None:
            if current.data == data:
                if prev is None:
                    self.head = current.next   # head 노드를 삭제하는 경우
                else:
                    prev.next = current.next   # 앞 노드가 다음 노드를 직접 가리킨다
                self._size -= 1
                return True
            prev = current
            current = current.next
        return False


if __name__ == "__main__":
    items = SinglyLinkedList()
    items.append(10)
    items.append(20)
    items.append(30)
    items.prepend(5)
    print(list(items), len(items))
    print(items.find(20) is not None)
    print(items.find(99) is not None)

    print(items.remove(10))   # 중간 노드 삭제
    print(list(items))
    print(items.remove(5))    # head 노드 삭제
    print(list(items))
    print(items.remove(99))   # 없는 값
    print(list(items), len(items))
```

실행 결과:

```text
[5, 10, 20, 30] 4
True
False
True
[5, 20, 30]
True
[20, 30]
False
[20, 30] 2
```

`remove`는 데이터를 지우는 것이 아니라 **앞 노드의 `next` 참조를 바꿔 해당 노드를 건너뛰게 만드는 것**입니다. 아무도 가리키지 않게 된 노드는 파이썬이 알아서 메모리에서 정리합니다.

## 7. 정리

- 배열은 **연속된 메모리**라서 접근이 빠르고, 중간 삽입·삭제가 느립니다.
- 연결 리스트는 **참조로 연결된 노드**라서 삽입·삭제 자체는 빠르지만, 원하는 위치를 찾는 데 시간이 듭니다.
- 두 구조의 차이는 결국 **"접근이 빠를 것인가, 삽입·삭제가 빠를 것인가"의 교환**입니다.
