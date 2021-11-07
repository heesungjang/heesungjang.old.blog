---
emoji: ✏️
title: Big O 표기법 [파이썬]
date: '2021-06-19 00:00:00'
author: 장희성
tags: 알고리즘
categories: 알고리즘
---

### 시간복잡도 & 공간복잡도

- 시간 복잡도는 코드의 실행 시간을 예측해 코드의 효율성을 나타내는 개념.
- 공간 복잡도는 코드의 메모리 할당이 얼마나 효율적으로 이주어지고 있는지 예측하는 개념이다.
<hr>

# Big O 표기법

- 알고리즘의 성능을 수학적으로 표기하여 알고리즘의 “효율성”을 평가하는 방법.
- 실제 알고리즘의 정확한 러닝타임을 계산하기 보다는 알고리즘의 성능을 예측하는 것이 목표이기 때문에 **Big O 표기법에서 상수는 표기하지 않는다**.

`eg. O(2n) -> O(n)`

## O(1)

-> **Constant**

데이터 크기에 상관없이 일정한 시간이 걸리는 알고리즘.

```python
def is_zero(x):
    return (x==0)
```

## O(n)

-> **Linear**

선형 탐색 알고리즘과 같이 입력 테이터의 크기에 비례해 처리 시간이 증가하는 알고리즘

```python
def print_length(n: list):
    for x in n:
       print(len(n))
```

## O(n²)

-> **quadratic**

데이터가 증가하게되면 처리 시간이 급격하게 늘어난다.
대표적인 예로는 이중 for문 (n² matrix)가 있다.

```python
def f(n):
  for i in len(n):
      for j in len(n):
        print(i + j);
```

## O(nm)

quadratic과 비슷하지만 두번째 loop에서 m의 크기가 n보다 작을 경우 nm으로 표기해준다.

```python
def f(n,m):
    for i in len(n):
       for i in len(m):
         print(i+j)
```

## O(n³)

o(n²)이 가로 세로를 가진 matrix라면 o(n³)는 큐빅 모양을 가진다. n개의 데이터를 반복하는 3중 for문을 생각하면 되겠다.

```python
def f(n):
  for i in len(n):
      for j in len(n):
          for k in len(n):
              print(i+j+k)
```

## O(2ⁿ)

대표적인 예제: 피보나치 수열
데이터가 늘어나면서 증가하는 처리시간의 기울기가 O(n²)과 O(n³)보다 급격하게 증가한다.

```python
def f(n, r):
    if (n <= 0):
        reutrn 0;
    elif (n == 1):
        return r[n] =1
    return F(n - 1, r) + F(n - 2, r)
```

```toc

```
