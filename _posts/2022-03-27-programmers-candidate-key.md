---
title: "[프로그래머스] 후보키 - 2019 카카오 공채 풀이"
excerpt: ""

categories: Algorithm
tags: [프로그래머스, Python]

date: 2022-03-27
last_modified_at: 2022-03-27
---

## 문제
🔗 [프로그래머스: 후보키](https://programmers.co.kr/learn/courses/30/lessons/42890)


## 아이디어
이 문제의 핵심은 DB 후보 키(Candidate Key)의 특징인 유일성과 최소성을 제대로 이해하고 이를 구현할줄 아는 것이다.
여기서 유일성과 최소성은,
- 유일성: 릴레이션에 있는 모든 튜플에 대해 유일하게 식별되어야 한다. 즉, DB의 모든 튜플을 서로 다르게 구분할 수 있는 특성을 말한다.
- 최소성: 각각의 조합을 구성하는 속성들 중에서 하나의 속성이라도 빠질 경우, 유일성이 깨진다는 것을 의미한다.

참고로 보통 DB의 후보 키에서 하나를 선택해 기본 키(Primary Key, PK)로 결정한다.


## 풀이
- 먼저 `1`부터 `len(relation)+1`까지 조합(combinations)를 통해 모든 인덱스의 조합을 구했다.  
- 이후 각각의 조합들을 순차적으로 보면서, 해당 조합에 해당 하는 인덱스의 칼럼의 요소들을 선택해 리스트를 만들었다. 예를 들면, `[학번], ... , [학번, 이름], [이름, 전공], ...` 등을 의미한다.
    ```python
    temp = [tuple([relation[i][j] for j in comb]) for i in range(len(relation))]
    ```
- 이후 temp의 중복을 제거한 길이가 `len(relation)` 보다 작게 되면 유일성을 충족하지 못하는 것이다. 중복을 제거하기 위해 set으로 변환한다.
- 유일성의 기준을 충족한 뒤, 해당 조합을 구성하는 속성이 이전에 후보키로 확인되어진 적이 있으면, 최소성을 충족하지 못하는 것이다.
- 최소성의 기준을 충족하면, 결과 리스트에 해당 후보키를 추가한 후, 위의 과정을 차례대로 반복한다.


## 전체 소스코드
```python
from itertools import combinations

def solution(relation):
    result = []
    for num in range(1, len(relation[0])+1):
        combination = list(combinations(range(len(relation[0])), num))
        for comb in combination:
            temp = [tuple([relation[i][j] for j in comb]) for i in range(len(relation))]

            # 유일성 확인
            if len(set(temp)) == len(relation):
                notSubset = True
                for i in result:
                    # 최소성 확인
                    if set(i).issubset(comb):
                        notSubset = False
                        break
                if notSubset:
                    result.append(comb)
                    
    return len(result)
```


## 배운 부분(TIL)
- 처음에는 속성들의 조합이 이전에 사용됐는지 확인하는 방법을 떠올리지 못했었다. 다행히 `issubset`을 사용해 해당 조합이 이전에 사용됐는지 확인이 가능했다.
- python의 set에서 1개의 요소를 추가할 때는 `add`를, 여러 개의 요소를 추가할 때는 `update`를 사용한다.


```python
a = set()

# add
a.add(1)
print(a) # {1}

# update
a.update(2, 3, 4, (1, 2))
print(a) # {1, 2, 3, 4, (1, 2)}
```
- combinations 자체는 객체로 반환된다. 리스트로 사용하기 위해서는 `list(combinations(range(n), m))`으로 써줘야 한다.