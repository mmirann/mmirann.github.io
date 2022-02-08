---
layout: post
title: "[알고리즘] 최단 경로"
subtitle: ""
categories: [Algorithm/Concept]
tags: [Shortest-path]
use_math: true
comments: true
---

## 다익스트라 최단 경로 알고리즘 (Dijkstra Algorithm)

다익스트라 최단 경로 알고리즘은 그래프에서 여러 개의 노드가 있을 때, **특정한 노드에서 출발하여 다른 노드로 가는 각각의 최단 경로**를 구해주는 알고리즘이다. 이 알고리즘은 음의 간선이 없을 때 정상적으로 동작하는데, 현실 세계의 길은 음의 간선으로 표현되지 않는다. 이 알고리즘은 매번 **가장 비용이 적은 노드**를 선택해서 임의의 과정을 반복하기 때문에 그리디 알고리즘이라고 할 수 있다. 알고리즘은 다음과 같이 동작한다.

1. 출발 노드를 설정한다.
2. 최단 거리 테이블을 초기화한다.
3. 방문하지 않은 노드 중에서 최단 거리가 가장 짧은 노드를 선택한다.
4. 해당 노드를 거쳐 다른 노드로 가는 비용을 계산하여 최단 거리 테이블을 갱신한다.
5. `3`과 `4`를 반복한다.

다익스트라 알고리즘은 최단 경로를 구하는 과정에서 각 노드에 대한 현재까지의 최단 거리 정보를 항상 1차원 리스트에 저장하며 리스트를 계속 갱신한다는 특징이 있다.

<br>

## 파이썬 코드 $O(ElogV)$

힙 자료구조를 사용하여 특정노드까지의 최단 거리에 대한 정보를 힙에 담아 처리하므로 출발 노드로부터 가장 거리가 짧은 노드를 더 빠르게 찾을 수 있다.

### `deque` vs `heapq`

- `deque`: duble-ended queue로, 큐의 처음과 끝에 아이템을 삽입, 삭제할 때 `O(1)`의 시간복잡도를 가진다. FIFO 큐를 만들 때 사용한다.
- `heapq`: 우선순위 큐를 지원하며, `PriorityQueue`보다 빠르게 동작한다. 우선순위 큐를 구현할 때 내부적으로 최소힙, 최대힙을 이용하는데 최소힙인 경우 값이 낮은 데이터가 먼저 삭제되고, 최대 힙을 이용하는 경우 값이 큰 데이터가 먼저 삭제된다. 튜플을 넣는 경우 첫 번째 원소를 기준으로 우선순위 큐를 구성한다.

다익스트라 알고리즘에서는 비용이 적은 노드를 우선 방문하므로 최소 힙 구조를 기본적으로 사용하는 `deque`를 그대로 사용하면 된다. **현재 가장 가까운 노드를 저장하기 위한 목적**으로만 우선순위 큐를 이용한다.

```python
import heapq
import sys
input = sys.stdin.readline
INF = int(1e9)  # 무한을 의미하는 값으로 10억을 설정

n, m = map(int, input().split())  # 노드 개수, 간선
start = int(input())  # 시작 노드 번호
graph = [[] for i in range(n+1)]  # 각 노드에 연결된 노드에 대한 정보 담은 리스트
distance = [INF]*(n+1)  # 최단 거리 테이블 무한으로 초기화

for _ in range(m):
    a, b, c = map(int, input(), split())
    grpah[a].append((b, c))  # a -> b (cost c)


def dijkstra(start):
    q = []
    # 시작 노드로 가기 위한 최단 경로는 0으로 설정 후 큐에 삽입
    heapq.heappush(q, (0, start))
    distance[start] = 0
    while q:  # 큐가 비어있지 않다면
        # 가장 최단 거리가 짧은 노드 꺼냄
    dist, now = heapq.heappop(q)
    # 현재 노드가 이미 처리된 적이 있다면 무시
    if distance[now] < dist:
        continue
    # 현재 노드와 연결된 다른 인접한 노드 확인
    for i in graph[now]:
        cost = dist + i[1]
        # 현재 노드를 거쳐서, 다른 노드로 이동하는 거리가 더 짧은 경우
        if cost < distance[i[0]]:
            distance[i[0]] = cost # 비용 업데이트
            heapq.heappush(q, (cost, i[0]))


dijkstrat(start)

# 모든 노드로 가기 위한 최단 거리 출력
for i in range(1, n+1):
    # 도달할 수 없으면 무한 출력
    if distance[i] == INF:
        print("INFINITY")
    else:
        print(distance[i])

```

<br>

## 플로이드 워셜 알고리즘(Floyd-Warshall Algorithm)

다익스트라 알고리즘은 한 지점에서 다른 특정 지점까지의 최단 경로를 구해야 하는 경우에 사용하는 알고리즘이라면, 플로이드 워셜 알고리즘은 **모든 지점에서 다른 모든 지점까지의 최단 경로**를 모두 구해야 하는 경우에 사용한다.

플로이드 워셜 알고리즘은 다익스트라 알고리즘과 같이 단계마다 거쳐가는 노드를 기준으로 알고리즘을 수행하지만 매번 방문하지 않은 노드 중에서 최단 거리를 갖는 노드를 찾을 필요가 없다는 점이 다르다. 2차원 리스트에 모든 노드에 대해 다른 모든 노드로 가는 최단 거리 정보를 담아야 하므로 2차원 리스트에 최단 거리 정보를 저장한다. N번의 단계에서 매번 $O(N^2)$ 시간이 소요된다. 또한 노드의 개수가 N이라고 할 때 N번 만큼의 단계를 반복해 점화식에 맞게 2차원 리스트를 갱신하기 때문에 다이나믹 프로그래밍으로 볼 수 있다. 알고리즘은 다음과 같이 동작한다.

1. 현재 확인하고 있는 노드를 제외하고 N-1개의 노드 중에서 서로 다른 노드 (A,B)쌍을 선택
2. A->1번노드->B로 가는 비용을 확인한 뒤 최단 거리 갱신 -> $_{N-1}\rm P_2$개의 쌍을 단계마다 반복해서 확인

K번의 단계에 대한 점화식은 다음과 같다.

$D_{ab} = min(D_{ab}, D_{ak} + D_{kb})$

위의 점화식은 A에서 B로 가는 최소 비용과 A에서 K를 거쳐 B로 가는 비용을 비교해 더 작은 값으로 갱신한다는 의미이다.

<br>

## 파이썬 코드 $O(V^3)$

```python
INF = int(1e9)  # 무한을 의미하는 값으로 10억을 설정

n = int(input())
m = int(input())

graph = [[INF] * (n+1) for _ in range(n+1)]

# 자기 자신에서 자기 자신으로 가는 비용은 0으로 초기화
for a in range(1, n+1):
    for b in range(1, n+1):
        if a == b:
            graph[a][b] = 0

# 각 간선에 대한 정보를 입력받아, 그 값으로 초기화
for _ in range(m):
    # a->b cost:c
    a, b, c = map(int, input().split())
    graph[a][b] = c

# 점화식에 따라 플로이드 워셜 알고리즘 수행
for k in range(1, n+1):
    for a in range(1, n+1):
        for b in range(1, n+1):
            graph[a][b] = min(graph[a][b], graph[a][k]+graph[k][b])

# 수행된 결과 출력
for a in range(1, n+1):
    for b in range(1, n+1):
        if graph[a][b] == INF:
            print("INFINITY", end=" ")
        else:
            print(graph[a][b], end=" ")


```

<br>

## Reference

- 이것이 취업을 위한 코딩 테스트다 with 파이썬
