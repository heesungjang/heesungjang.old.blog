---
emoji: ✏️
title: 스택(stack)& 큐(queue)
date: '2021-06-20 00:00:00'
author: 장희성
tags: 알고리즘 스택 큐 stack queue
categories: 자료구조
---

![](https://images.velog.io/images/heesungj7/post/3c68fa92-99f0-4de9-9d08-6b795583f212/1_r4Bfo3rrFprzFM2zbgzZXA.jpeg)

## ✏️ 스택 (stack)

스택은 **후입선출 구조 (LIFO, Last In First Out) 구조**라고 해서 한쪽 끝으로만 자료를 넣고 뺄 수 있는 자료 구조이다.

### 스택이 사용되는 예시

- **컴퓨터의 되돌리기(Ctrl + Z) 기능**: 직전에 했던 행동을 되돌고 싶을 때 사용하는 기능으로, 이를 위해서는 내가 했던 행동들을 순서대로 기억해야 하므로 스택을 사용한다.

## 📌 특징

- 스택은 위의 사진처럼 같은 구조와 크기의 자료를 정해진 방향으로만 쌓을수 있으며 데이터 접근 또한 데이터가 삽입된 top을 통해서만 접근이 가능하다.

- 스택의 가장 상위에 위치하는 자료는 가장 최근에 들어온 자료이며, 새로 삽입되는 자료는 top이 가리키는 자료의 위에 쌓이게 된다.

- 스택에서 자료를 삭제할 때도 top을 통해서만 가능하다.

> 스택에서 top을 통해 삽입하는 연산을 **'push' , top을 통한 삭제하는 연산을 'pop'**이라고 한다.

### 스택 자료구조의 기능들을 코드로 직접 구현해보자.

### 1. Push: 맨 앞에 데이터 넣기

```python
    def push(self, value):
        new_head = Node(value)
        new_head.next = self.head
        self.head = new_head

```

### 2. pop: 맨 앞의 데이터 뽑기

```python
    def pop(self):
        if self.is_empty():
            return "Stack is empty"
        deleted_stack = self.head
        self.head = self.head.next
        return deleted_stack
```

### 3. peek: 맨 앞의 데이터 보기

```python
    def peek(self):
        if self.is_empty():
            return "Stack is empty"
        return self.head.data
```

### 4. is_empty: 스택이 비었는지 안 비었는지 여부 반환해주기

```python
    def is_empty(self):
        return self.head is None
```

<hr>

## ✏️ 큐 (queue)

한쪽 끝으로 자료를 넣고, 반대쪽에서는 자료를 뺄 수 있는 **선형구조**.

### 🌟 왜 이런 자료구조가 필요할까?

> 순서대로 처리되어야 하는 일에 필요하기 때문이다!

## 📌 특징

> 가장 먼저 들어온 프론트 원소가 가장 먼저 삭제.
> 접근방법은 가장 첫 원소와 끝 원소로만 가능.

- 데이터의 삽입과 삭제가 한쪽(top)으로만 이루어지는 스택 구조와는 달리, 큐는 한쪽 끝에서 삽입 작업이, 다른 쪽 끝에서 삭제 작업이 선형적으로 이루어진다.

- **front**: 삭제 연산이 이루어지는 곳. 즉, 가장 먼저 들어온 데이터가 큐를 나가는 곳이다.
- **rear**: 삽입 연산이 이루어지는 곳. 새로 추가되는 데이터는 rear통해 큐에 삽입된다.

- 이때, 큐의 리어에서 이루어지는 삽입연산을 **인큐(enQueue)** 라고하며 프론트에서 이루어지는 삭제연산을 **디큐(dnQueue)** 라고 부른다.

### 큐 자료구조의 기능들을 코드로 직접 구현해보자.

### 1. enqueue(data) : 맨 뒤에 데이터 추가하기

```python
    def enqueue(self, value):
        new_node = Node(value)
        if self.is_empty():
            self.head = new_node
            self.tail = new_node
            return
        self.tail.next = new_node
        self.tail = new_node
        return
```

### 2. dequeue() : 맨 앞의 데이터 뽑기

```python
    def dequeue(self):
        if self.is_empty():
            print("Queue is empty")
        removed_queue = self.head
        self.head = self.head.next
        return removed_queue.data
```

### 3. peek() : 맨 앞의 데이터 보기

```python
 def peek(self):
        if self.is_empty():
            print("Queue is empty")

        return self.head.data
```

### 4. isEmpty(): 큐가 비었는지 안 비었는지 여부 반환해주기

```python
    def is_empty(self):
        return self.head is None
```

<hr>

참고 (https://devuna.tistory.com/22)

```toc

```
