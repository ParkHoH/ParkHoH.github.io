---
layout: single
title: 첫번째 포스팅입니다!

categories: Algorithm
tags: [프로그래머스, Python]

toc: true
toc_sticky: true

date: 2022-03-24
last_modified_at: 2022-03-24
---

# 🔎 문제
[🔗 백준 2729번 이진수 덧셈](https://www.acmicpc.net/problem/2729)

# 💡 풀이
파이썬에선 이진법으로 변환하려면 `int({변환할 수}, {진법})`을 이용하면 된다.

2진법으로 입력받고, 더한다. 이진법의 표시로 `0b`가 앖에 붙어있으니 3번째(인덱스로는 2) 부터 print 하면 된다.

# 📃 소스코드
```python
for _ in range(int(input())):
    A, B = map(lambda x: int(x, 2), input().split())
    print(bin(A + B)[2:])
```
<br>