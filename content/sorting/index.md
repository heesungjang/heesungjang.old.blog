---
emoji: ✏️
title: 정렬
date: '2021-06-14 00:00:00'
author: 장희성
tags: 알고리즘 정렬
categories: 알고리즘
---

알고리즘 공부를 시작하면서 점점 작아지는 내 모습이 안쓰러울 정도이다...처음 개발 공부를 시작할때 들었던 막막함과는 또 다른 느낌이다. 마치 끝이 보이지 않는 어두캄캄한 길을 걸어가는 느낌..다른 사람들은 당연하다고 생각하는 소스코드도 왜이렇게 이해하기가 힘든지..여튼 쉽지않은 하루를 보내고있다.

<hr>

## ✏️ 정렬

오늘은 정렬의 여러가지 방법들을 정리하면서 배운 내용들을 다시 한번 돌아보려고 한다.

## 1. bubble (버블정렬) - O(n²)

- 버블 정렬에서는 리스트의 원소들을 처음부터 순차적으로 이동하며 인접한 두 원소의 값을 검사하고 작은 값, 또는 큰 값의 위치를 교환하며 정렬해 나가는 방법이다.
- **1회차의 검사가 완료되면 가장큰 원소가 자료의 맨 마지막 위치로 이동하게 되므로** 2회차 검사에서는 마지막 위치의 온소는 제외하고 반목문을 실행한다.

```python
def bubble_sort(array):
    n = len(array)
    for i in range(n - 1):
        for j in range(n - i - 1):
            if array[j] > array[j + 1]:
                array[j], array[j + 1] = array[j + 1], array[j]
```

#### 버블 정렬의 장/단점

- 코드 구현이 쉽다.
- 최악의 경우 첫번째 위치의 원소가 마지막 위치까지 이동하기 위해서는 배열을 모든 원소들과 검사하고 교환되어야 한다.

## 2. selection - O(n²)

- 선택 정렬에서는 배열의 길이를 n-i 값으로 줄여가며 값을 비교하고 가장 작은 또는 가장 큰 값의 원소를 첫번째 두번째 세번째 식으로 위치 시키며 정렬하는 방법이다. (무슨 말이지..코드로 보면 이해가 빠르다.)

- **1회전을 수행하고 나면 가장 작은 값의 자료가 맨 앞에 오게 되므로** 그 다음 회전에서는 두 번째 자료를 가지고 비교한다. 마찬가지로 3회전에서는 세 번째 자료를 정렬한다.

```python
def selection_sort(array):
    n = len(input)
    for i in range(n - 1):
        min_index = i
        for j in range(n - i):
            if input[i + j] < input[min_index]:
                min_index = i + j
        array[i], array[min_index] = array[min_index], array[i]
```

#### 선택 정렬의 장/단점

- 이동 횟수가 미리 결정된다.
- 같은 원소값이 존재 할 경우 상대적인 위치가 변경될 수 있다.

## 3. insertion - O(n) or O(n²)

- 배열의 모든 원소를 앞에서부터 순차적으로 이미 정렬된 배열 부분과 비교하며, 자신의 위치를 찾아 삽입함으로써 정렬을 완성하는 알고리즘.
- **첫번째 원소를 이미 정렬되어 있다고 가정하고**, 두번째 원소부터 정렬을 시작한다.

```python
def insertion_sort(array):
    n = len(input)
    for i in range(1, n):
        for j in range(i):
            if array[i - j - 1] > array[i - j]:
                array[i - j - 1], array[i - j] = array[i - j], array[i - j - 1]
            else:
                break
    return
```

## 4. merge (divide and conquer)

- 문제를 작은 2개의 문제로 분리하고 각각을 해결한 다음, 결과를 모아서 원래의 문제를 해결하는 **분활 정복 알고리즘의 대표인 예이다.**

- 하나의 리스트를 두 개의 동일한 크기의 배열로 분할하고 분할된 부분 배열을 정렬한 다음, 두 개의 정렬된 부분 배열을 합하여 전체가 정렬된 배열이 되게 하는 방법이다.

```python
array = [5, 3, 2, 1, 6, 8, 7, 4]


def merge_sort(array):
    if len(array) <= 1:
        return array
    mid = (len(array)) // 2
    left_array = merge_sort(array[:mid])
    right_array = merge_sort(array[mid:])
    return merge(left_array, right_array)


def merge(array1, array2):
    result = []
    array1_index = 0
    array2_index = 0
    while array1_index < len(array1) and array2_index < len(array2):
        if array1[array1_index] < array2[array2_index]:
            result.append(array1[array1_index])
            array1_index += 1
        else:
            result.append(array2[array2_index])
            array2_index += 1

    if array1_index == len(array1):
        while array2_index < len(array2):
            result.append(array2[array2_index])
            array2_index += 1

    if array2_index == len(array2):
        while array1_index < len(array1):
            result.append(array1[array1_index])
            array1_index += 1

    return result
```

참고(https://gmlwjd9405.github.io/2018/05/08/algorithm-merge-sort.html) - 이미지를 이용해 정렬 방법들의 과정을 설명해놓은 블로그. 시각적으로 배열들의 위치 변화를 볼 수 있어 쉽게 이해할 수 있다.

```toc

```
