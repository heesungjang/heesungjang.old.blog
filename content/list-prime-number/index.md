---
emoji: ✏️
title: [알고리즘] 소수 나열하기
date: '2021-06-14 00:00:00'
author: 장희성
tags: 알고리즘 소수 나열
categories: 알고리즘
---

# 1.문제

정수를 입력 했을 때, **그 정수 이하의 소수를 모두 반환하시오**. 소수는 자신보다 작은 두 개의 자연수를 곱하여 만들 수 없는 1보다 큰 자연수이다.

> Input = 20

```
>>>[2, 3, 5, 7, 11, 13, 17, 19]
```

## 접근법:

1. **소수는 자기와 1외에는 나눌 수 없다**.

2. **소수는 2부터 n - 1까지 어떤 소수로도 나누어 떨어지지 않는다**.
   (소수인지 확인하고자 하는 수가 n 일때, n이 2와3으로 나누어 떨어지지 않는다면 2x2인 4와 2x3인 6으로도 나누어 떨어지지 않는다.)
3. **n의 제좁근 이하의 어떤 소수로도 나누어 떨어지지 않는다**.

## 풀이법:

### python code:

```python
input = 20

def find_prime_list_under_number(number):
    prime_list = []

    for n in range(2, number + 1):
        for i in prime_list:
            if n % i == 0 and i * i <= n:
                break
        else:
            prime_list.append(n)

    return prime_list

```

```toc

```
