---
layout: post
title:  "[Level3] 다음 큰 숫자"
date:   2018-01-16 11:30:00 +0900
categories: Algorithm
---

어떤 수 N(1≤N≤1,000,000) 이 주어졌을 때, N의 다음 큰 숫자는 다음과 같습니다.

- N의 다음 큰 숫자는 N을 2진수로 바꾸었을 때의 1의 개수와 같은 개수로 이루어진 수입니다.
- 1번째 조건을 만족하는 숫자들 중 N보다 큰 수 중에서 가장 작은 숫자를 찾아야 합니다.

예를 들어, 78을 2진수로 바꾸면 1001110 이며, 78의 다음 큰 숫자는 83으로 2진수는 1010011 입니다.
N이 주어질 때, N의 다음 큰 숫자를 찾는 nextBigNumber 함수를 완성하세요.

```python
def nextBigNumber(n):
    numOne = 0
    for i in str(bin(n))[2:]:
        if i == "1":
            numOne += 1
    numBiggerOne = 0
    while numBiggerOne != numOne:
        n += 1
        numBiggerOne = 0
        for j in str(bin(n))[2:]:
            if j == "1":
                numBiggerOne += 1
    return n

#아래 코드는 테스트를 위한 출력 코드입니다.
print(nextBigNumber(78))
```