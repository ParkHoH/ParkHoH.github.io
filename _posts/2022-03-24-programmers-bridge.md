---
title: "[프로그래머스] 다리를 지나는 연습 풀이"
excerpt: ""

categories: Algorithm
tags: [프로그래머스, Python]

date: 2022-03-24
last_modified_at: 2022-03-24
---

## 문제
🔗 [프로그래머스: 다리를 지나는 트럭](https://programmers.co.kr/learn/courses/30/lessons/42583)


## 아이디어
문제를 접하기도 전에 '스택/큐' 문제라고 설명이 되어 있어서 풀이방법을 스포당했다. 
물론 문제의 테이블을 봐도 '스택/큐'로 풀기 명확하게 되어 있어 '스택/큐' 문제로 정의했다.


## 풀이
먼저 추가로 선언이 필요한 자료구조는 아래와 같다.
```python
on_bridge = deque()                     # 다리를 건너고 있는 트럭
truck_weights = deque(truck_weights)    # truck_weights를 deque로 변환
seconds = 0                             # 경과된 시간(초)
on_bridge_weight = 0                    # 다리를 건너고 있는 트럭들의 무게 합
```
물론 `on_bridge_weight`는 `on_bridge`에서 `sum`으로 구할 수 있지만,  
조금이라도 연산 시간을 줄여주기 위해 `on_bridge_weight`가 필요했다.


여기서 on_bridge에는 트럭의 무게와 현재 초+다리의 길이의 배열,  
즉 `[트럭의 무게, seconds + bridge_length]` 을 하나씩 추가해준다.


다음으로 남아있는 트럭이 없을 때까지, 그리고 현재 다리에 남아있는 트럭이 모두 건널 때까지 반복문을 실행해준다.
이 때 1초씩 증가시켜준다.
```python
while truck_weights or on_bridge:
    seconds += 1
```


이후 해당 초에 완전히 지나간 트럭이 있으면 빼주고,
```python
if len(on_bridge) > 0 and on_bridge[0][1] == seconds:
    pop_weight, _ = on_bridge.popleft()
    on_bridge_weight -= pop_weight
```

대기하고 있는 트럭의 무게 + 현재 건너는 트럭의 무게가 weight 보다 작으면 새로운 트럭을 추가한다.
```python
if len(truck_weights) > 0 and truck_weights[0] + on_bridge_weight <= weight:
    pop_weight = truck_weights.popleft()
    on_bridge_weight += pop_weight
    on_bridge.append([pop_weight, seconds+bridge_length])
```

최종적으로 seconds를 리턴하면 답이 나온다.



## 전체 소스코드
```python
from collections import deque

def solution(bridge_length, weight, truck_weights):
    on_bridge = deque()
    truck_weights = deque(truck_weights)
    seconds = 0
    on_bridge_weight = 0
    
    # 대기 트럭이 빌 때까지 반복
    while truck_weights or on_bridge:
        seconds += 1
             
        # 해당 초에 완전히 지나간 트럭이 있으면 빼주기
        if len(on_bridge) > 0 and on_bridge[0][1] == seconds:
            pop_weight, _ = on_bridge.popleft()
            on_bridge_weight -= pop_weight
             
        # 대기하고 있는 트럭의 무게 + 현재 건너는 트럭의 무게가 weight 보다 작으면 새로운 트럭 추가하기
        if len(truck_weights) > 0 and truck_weights[0] + on_bridge_weight <= weight:
            pop_weight = truck_weights.popleft()
            on_bridge_weight += pop_weight
            on_bridge.append([pop_weight, seconds+bridge_length])
             
    return seconds
```


## 배운 부분
- 스택/큐에 하나의 데이터만 넣는 것이 아니라, 배열 형태로 넣게 되면 여러 조건에 대응할 수 있어 유용하다.
- 위 문제는 while문을 모두 돌아도 문제가 없었지만, 더 효율적인 시간 복잡도를 가지기 위해서는 새로 추가되는 트럭 없이 `on_bridge`에 있는 트럭이 지나가기만 할 때는 남아있는 다리의 길이를 계산해 불필요한 반복문을 없앨 수 있다.
