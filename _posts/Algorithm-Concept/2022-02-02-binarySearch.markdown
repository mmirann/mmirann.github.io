---
layout: post
title: "[알고리즘] 이진탐색"
subtitle: ""
categories: [Algorithm/Concept]
tags: [binary-search]
comments: true
---

## 이진탐색(Binary Search)

- 배열 내부의 데이터가 정렬되어 있을 때 사용할 수 있음
- 탐색 범위를 절반씩 좁혀가며 데이터를 탐색함
- **시작점, 끝점, 중간점**을 이용해 찾으려는 데이터와 중간점 위치에 있는 데이터 비교 반복
- `O(logN)`

<br>

## 코딩테스트에서의 이진탐색

- 탐색 범위가 큰 상황에서의 탐색을 가정하는 문제에서 사용
- 데이터의 개수가 1,000만 개를 넘어가거나 탐색 범위의 크기가 1,000억 이상일 때 사용

<br>

## 재귀함수) Python 코드

```python

def binary_search(arr, target, start, end):
    if start > end:
        return None
    mid = (start+end) // 2
    # 찾은 경우 중간점 인덱스 반환
    if arr[mid] == target:
        return mid
    # 중간점의 값보다 찾고자 하는 값이 작은 경우 왼쪽 확인
    elif arr[mid] > target:
        return binary_search(arr, target, start, mid-1)
    # 중간점의 값보다 찾고자 하는 값이 큰 경우 오른쪽 확인
    elif arr[mid] < target:
        return binary_search(arr, target, mid+1, end)

```

<br>

## 반복문) Python 코드

```python

def binary_search(arr, target, start, end):
    wheil start <= end:
    mid = (start+end)//2
    if arr[mid] == target:
        return mid
    # 중간점의 값보다 찾고자 하는 값이 작은 경우 왼쪽 확인
    elif arr[mid] < target:
        end = mid-1
    # 중간점의 값보다 찾고자 하는 값이 큰 경우 오른쪽 확인
    elif arr[mid] > target:
        start = mid+1

```

<br>

## Reference

- 이것이 취업을 위한 코딩 테스트다 with 파이썬
