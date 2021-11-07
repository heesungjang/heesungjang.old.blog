---
emoji: ✏️
title: 재귀함수 [파이썬]
date: '2021-06-16 00:00:00'
author: 장희성
tags: 알고리즘
categories: 알고리즘
---

## 1. 재귀함수

> 📘 재귀(Recursion)은 어떠한 것을 정의할 때 자기 자신을 참조하는 것을 뜻한다. [위키백과]

**어느 한 컴퓨터공학과 학생이 유명한 교수님을 찾아가 물었다**.
"재귀함수가 뭔가요?"
"잘 들어보게. 옛날옛날 한 산 꼭대기에 이세상 모든 지식을 통달한 선인이 있었어. 마을 사람들은 모두 그 선인에게 수많은 질문을 했고, 모두 지혜롭게 대답해 주었지. 그의 답은 대부분 옳았다고 하네.
그런데 어느날, 그 선인에게 한 선비가 찾아와서 물었어.
"재귀함수가 뭔가요?"
"잘 들어보게. 옛날옛날 한 산 꼭대기에 이세상 모든 지식을...

## 2. 코드 구현

### 숫자 세기

> 간단하게 60에서 0까지 숫자를 카운트하는 기능을 재귀 함수로 구현해보자.

```python
def count_down(number):
    if number < 0:         # 만약 숫자가 0보다 작다면, 빠져나가자!
        return

    print(number)          # number를 출력하고
    count_down(number - 1) # count_down 함수를 number - 1 인자를 주고 다시 호출한다!

count_down(60)
```

### 팩토이얼

> 재귀함수와 관련되어 나오는 대표적인 문제인 팩토리얼 문제를 코드로 구현해보자.

- 팩토리얼은 1부터 어떤 양의 정수 n까지의 정수를 모두 곱한 것을 의미한다.

예제)
3! 은 3 _ 2 _ 1 = 6,
4! 는 4 _ 3 _ 2 _ 1 = 4 _ 3! = 24

```python
def factorial(n):
    if n == 1:
        return 1
    return n * factorial(n - 1)

print(factorial(60))

```

### 회문 검사

> 회문은 똑바로 읽으나 거꾸로 읽으나 똑같은 단어나 문장을 의미합니다.

- 토마토
- 오디오
- 아시아
- 일요일

```python
input = "abcba"

def is_palindrome(string):
    if len(string) <= 1:
        return True
    if string[0] != string[-1]:
        return False
    return is_palindrome(string[1:-1])

print(is_palindrome(input))
```

```toc

```
