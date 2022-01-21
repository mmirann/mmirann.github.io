---
layout: post
title: "[알고리즘] Two Pointers, Sliding Window"
subtitle: ""
categories: [Algorithm/Concept]
tags: [two-pointers, sliding-window]
comments: true
---

## 투 포인터(Two Pointers)
- 어떤 특정 조건을 만족하는 연속 구간을 구할 때 `O(N)`으로 풀 수 있다.
- 2개의 포인터를 조작해가며 원하는 답을 얻는다.

![image](https://user-images.githubusercontent.com/48276682/145563598-be595142-4b6d-4e9b-8702-982df10471bb.png){: width="70%" height="70%"}

<br>

## Python 코드

LeetCode의 [3번 문제](https://leetcode.com/problems/longest-substring-without-repeating-characters/)를 two-pointers로 푼 코드이다. l, r 두 개의 포인터 모두 0부터 시작한다. 딕셔너리에 인덱스와 문자를 같이 저장한다. 만약 문자가 딕셔너리에 있는 경우 l 포인터를 중복된 문자 다음 인덱스와 현재 위치 중 인덱스가 큰 곳으로 옮긴다. 이는 항상 오른쪽으로 가는 것을 보장하기 위함이다. 문자를 확인 할 때마다 현재 길이와 longest 값을 계속해서 비교하며 최대 길이를 찾는다. r 포인터는 일정하게 1씩 증가하며, l 포인터는 중복된 값이 발견될 때 움직인다.

```python

def lengthOfLongestSubstring(self, s):
    seen = {}
    l, r = 0, 0
    longest = 1

    while r < len(s):
        if s[r] in seen:
            l = max(l, seen[r] + 1)
        longest = max(longest, r - l + 1)
        seen[s[r]] = r
        r += 1

    return longest

```

<br>

그리고 LeetCode의 [11번문제](https://leetcode.com/problems/container-with-most-water/)도 투포인터 문제다. 그래프의 맨왼쪽과 맨오른쪽을 left, right 포인터로 둔다. left와 right 중에 짧은 포인터를 한칸씩 옮기면서 직사각형 면적의 최댓값을 구한다. left, right 포인터가 만났을 때 종료한다. Brute-force를 쓰면 시간초과가 뜨기 때문에 투포인터를 사용해서 O(N)으로 해결해야 한다.

```python

    def maxArea(self, height):
        l, r, maxArea = 0, len(height)-1, 0
        while l < r:
            if height[l] < height[r]:
                maxArea = max(maxArea, height[l]*(r-l))
                l += 1
            else:
                maxArea = max(maxArea, height[r]*(r-l))
                r -= 1
        return maxArea

```

<br>

## 슬라이딩 윈도우(Sliding Window)

- 투 포인터와 비슷한 유형이다.
- 투 포인터와는 달리 항상 구간의 길이가 같다는 차이점이 있다.
- 따라서 투 포인터는 구간 양쪽 끝이 되는 포인터가 2개 필요한 반면, 슬라이딩 윈도우는 구간의 길이가 고정되어 있으므로 포인터가 2개일 필요는 없다.

<br>

## Reference

- <https://leetcode.com/problems/longest-substring-without-repeating-characters/discuss/742926/Simple-Explanation-or-Concise-or-Thinking-Process-and-Example>
