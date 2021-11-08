---
emoji: ✏️
title: 이진탐색
date: '2021-06-16 00:00:00'
author: 장희성
tags: 알고리즘
categories: 알고리즘
---

## 1. 이진 탐색

배열 구조의 자료에서 특정 값을 찾는 방법은 크게 두가지가 있을 수 있다.

- 순차 탐색
- **이진탐색**

순차 탐색에서는 배열의 시작부터 순차적으로 이동하며 찾고자 하는 특정 값을 모든 배열의 원소들과 비교하며 값을 찾을 때까지 반복한다.

> **이진 탐색**이란 데이터가 정렬돼 있는 배열에서 특정한 값을 찾아내는 알고리즘이다. 배열의 중간에 있는 임의의 값을 선택하여 찾고자 하는 값 X와 비교한다.

- X가 중간 값보다 작으면 중간 값을 기준으로 좌측의 데이터들을 대상으로,
- X가 중간값보다 크면 배열의 우측을 대상으로 다시 탐색한다.

##### 출처(https://cjh5414.github.io/binary-search/)

## 순차 탐색과 이진탐색을 비교해보자.

### 순차 탐색

```python
finding_target = 14
finding_numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16]

def is_existing_target_number_sequential(target, array):
    for number in array:
        if target == number:
            return True

    return False

result = is_existing_target_number_sequential(finding_target, finding_numbers)
print(result)  # True
```

> array 를 따라가면서 target 이 존재한다면 True 를 반환하고,
> 끝까지 없다면 False 를 반환한다.

### 이진 탐색

![](https://images.velog.io/images/heesungj7/post/e7c13b08-e919-41ad-94f2-b5fcc03bc9bf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-06-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.47.03.png)

```python
finding_target = 14
finding_numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16]


def is_existing_target_number_binary(target, array):
	current_min = 0
    current_max = len(array) - 1
    current_guess = (current_min + current_max) // 2

    while current_min <= current_max:
    	if array[current_guess] == target:
    	    return True
	elif array[current_guess] < target:
    	    current_min = current_guess + 1
	else:
    	    current_max = current_guess - 1
	current_guess = (current_min + current_max) // 2

    return False


result = is_existing_target_number_binary(finding_target, finding_numbers)
print(result)
```

```toc

```
