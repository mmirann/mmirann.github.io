---
layout: post
title: "[알고리즘] Two Pointers, Sliding Window"
subtitle: ""
categories: [Algorithm/Concept]
tags: [two-pointers, sliding-window]
comments: true
---

## 투 포인터(Two Pointers)

![image](https://user-images.githubusercontent.com/48276682/145563598-be595142-4b6d-4e9b-8702-982df10471bb.png){: width="70%" height="70%"}

- 어떤 특정 조건을 만족하는 연속 구간을 구할 때 `O(N)`으로 풀 수 있다.
- 2개의 포인터를 조작해가며 원하는 답을 얻는다.

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
 
## 슬라이딩 윈도우(Sliding Window)

- 투 포인터와 비슷한 유형이다.
- 투 포인터와는 달리 항상 구간의 길이가 같다는 차이점이 있다.
- 따라서 투 포인터는 구간 양쪽 끝이 되는 포인터가 2개 필요한 반면, 슬라이딩 윈도우는 구간의 길이가 고정되어 있으므로 포인터가 2개일 필요는 없다.

<br>

## Reference

- <https://leetcode.com/problems/longest-substring-without-repeating-characters/discuss/742926/Simple-Explanation-or-Concise-or-Thinking-Process-and-Example>
