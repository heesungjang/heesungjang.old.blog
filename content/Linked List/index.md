---
emoji: ✏️
title: [자료구조] Linked List
date: '2021-06-15 00:00:00'
author: 장희성
tags: 알고리즘 자료구조 링크드리스트 linked list
categories: 알고리즘
---

## 링크드 리스트란?

```python
array_train_compartments = ["기관실", "시멘트", "자갈", "밀가루", "우편"]

linked_train_compartments = ["기관실"] -> ["시멘트"] -> ["자갈"] -> ["밀가루"] -> ["우편"]
```

![](https://images.velog.io/images/heesungj7/post/845f334d-ad5f-4e5b-b6ad-7e6ce84b5dee/0_b6gu1MSbusnn0SHQ.png)

> 링크드 리스트는 기존 배열 구조와 마찬가지로 선형 데이터 자료구조를 가진다. 배열에서는 담고있는 데이터 자체가 연속적으로 저장되어 있다면, **링크드리스트는 노드들이 포인터를 통해 연결 관계를 유지하는 리스트이며, 노드는 데이터를 저장하는 부분, 다음 노드를 가르키는 부분으로 구성되어있다**.

### 링크리스트 장/단점:

#### 장점

- 삽입과 삭제가 O(1)에 이루어진다.
- 삽입과 삭제를 할 때마다 동적으로 링크드 리스트의 크기가 결정되므로 전통적인 배열(Array)에 비해 처음부터 큰 공간을 할당할 필요가 없어진다.
- 메모리 관리가 용이하다.

#### 단점

- Random Access, 즉 배열처럼 index를 통해 탐색이 불가능하다.
- 탐색이 O(N)이 걸린다. (Head부터 Tail까지 모두 탐색 시)
- 사실상 삽입과 삭제가 왼쪽에서(Head에서) 이루어지지 않을 경우, 결국 탐색을 먼저 해야 하기 때문에 삽입과 삭제 모두 적게는 O(k)부터 최악의 경우 O(N)까지 걸릴 가능성이 있다.

(출처: https://underflow101.tistory.com/3)

## 링크드 리스트 구현 🧑🏻‍💻

#### 1. 데이터를 담을 노드를 클래스를 통해 생성한다.

- init을 통해 데이터와 다음 노드를 포인트할 next를 생성하고 None으로 초기화 해준다.

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
```

#### 2. 파이썬 코드를 롱해 링크드 리스트 구현해보자.

- **constructor**:
  - 처음 인스턴스를 생성할때 주어진 노드로 리스트의 시작 부분인 **head**를 초기화 해준다.
- **append**:
  - cur 변수에 현재 head node를 담아주고 next값을 None으로 가지는 해드가 나올때까지 whilea문으로 이동하여 cur.next를 새로운 노드로 설정해준다.
- **pirnt_all**:
  - 반복문을 통해 cur에 node를 이동하며 node의 데이터를 출력한다.
- **get_node**:
  - index만큼 node를 이동하여 while문이 끝나는 시점에 노드를 반환한다.
- **add_node**

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None


class LinkedList:
    def __init__(self, value):
        self.head = Node(value)

    def append(self, value):
        cur = self.head
        while cur.next is not None:
            cur = cur.next
        cur.next = Node(value)

    def print_all(self):
        cur = self.head
        while cur is not None:
            print(cur.data)
            cur = cur.next

    def get_node(self, index):
        node = self.head
        count = 0
        while count < index:
            node = node.next
            count += 1
        return node

    def add_node(self, index, value):
        new_node = Node(value)
        if index == 0:
            new_node.next = self.head
            self.head = new_node
            return
        new_node = Node(value)
        node = self.get_node(index-1)
        next_node = node.next
        node.next = new_node
        new_node.next = next_node

```

```toc

```
