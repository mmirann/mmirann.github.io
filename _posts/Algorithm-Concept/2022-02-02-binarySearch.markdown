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

<br>

## 코딩테스트에서의 이진탐색

- **시간복잡도**: `O(logN)`
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

## 파라메트릭 서치

---

### 떡볶이 떡 자르기

오늘 동빈이는 여행 가신 부모님을 대신해서 떡집 일을 하기로 했다. 오늘은 떡볶이 떡을 만드는 날이다. 동빈이네 떡볶이 떡은 재밌게도 떡볶이 떡의 길이가 일정하지 않다. 대신에 한 봉지 안에 들어가는 떡의 총 길이는 절단기로 잘라서 맞춰준다.  
절단기에 높이(H)를 지정하면 줄지어진 떡을 한 번에 절단한다. 높이가 H보다 긴 떡은 H위의 부분이 잘릴 것이고, 낮은 떡은 잘리지 않는다.  
예를 들어 높이가 19, 14, 10, 17cm인 떡이 나란히 있고 절단기 높이를 15cm로 지정하면 자른 뒤 떡의 높이는 15, 14, 10, 15cm가 될 것이다. 잘린 떡의 길이는 차례대로 4, 0, 0, 2cm이다. 손님은 6cm만큼의 길이를 가져간다.  
손님이 왔을 때 요청한 총 길이가 M일 때 적어도 M만큼의 떡을 얻기 위해 절단기에 설정할 수 있는 높이의 최댓값을 구하는 프로그램을 작성하시오.

### 입력 조건

- 첫째 줄에 떡의 개수 N과 요청한 떡의 길이 M이 주어진다.
  (1 <= N <= 1,000,000, 1 <= M <= 2,000,000,000)
- 둘째 줄에는 떡의 개별 높이가 주어진다. 떡 높이의 총합은 항상 M 이상이므로, 손님은 필요한 양만큼 떡을 사갈 수 있다. 높이는 10억보다 작거나 같은 양의 정수 또는 0이다.

### 출력 조건

- 적어도 M만큼의 떡을 집에 가져가기 위해 절단기에 설정할 수 있는 높이의 최댓값을 출력한다.

<br>

## 풀이

**파라메트릭 서치 유형**의 문제이다. 파라메트릭 서치는 최적화 문제를 결정 문제(예, 아니오로 답하는 문제)로 바꾸어 해결하는 기법이다. 예를 들어 범위 내에서 조건을 만족하는 가장 큰 값을 찾으라는 최적화 문제라면 이진 탐색으로 결정 문제를 해결하면서 범위를 좁혀갈 수 있다.  
적절한 높이를 찾을 때까지 높이 H를 반복해서 조정하면서 문제를 해결한다. **이 높이로 자르면 조건을 만족할 수 있는가?를 확인** 한 뒤에 **조건의 만족 여부(예, 아니오)에 따라 탐색 범위를 좁혀서 해결**할 수 있다. 범위를 좁힐 때 이진 탐색을 활용한다.

```python

n, m = map(int, input().split())
arr = list(map(int, input().split()))

start = 0
end = max(array)

result = 0
while(start <= end):
    total = 0
    mid = (start+end)//2
    for x in array:
        # 잘랐을 때 떡의 양 계산
        if x > mid:
            total += x-mid
        # 떡의 양이 부족한 경우 더 많이 자르기(왼쪽 부분 탐색)
    if total < m:
        end = mid - 1
    # 떡의 양이 충분한 경우 덜 자르기(오른쪽 부분 탐색)
    else:
        result = mid  # 최대한 덜 잘렸을 때가 정답이므로 result 기록
        start = mid + 1

print(result)


```

<br>

## Reference

- 이것이 취업을 위한 코딩 테스트다 with 파이썬
