---
layout: post
title:  "[Level1] 최대공약수와 최소공배수"
date:   2018-01-16 11:30:00 +0900
categories: Algorythm
---

두 수를 입력받아 두 수의 최대공약수와 최소공배수를 반환해주는 gcdlcm 함수를 완성해 보세요. 배열의 맨 앞에 최대공약수, 그 다음 최소공배수를 넣어 반환하면 됩니다. 예를 들어 gcdlcm(3,12) 가 입력되면, [3, 12]를 반환해주면 됩니다.

```python
def gcdlcm(a, b):
    answer = []
    x = a
    y = b
    while y != 0:
        (x, y) = (y, x % y)
    answer = [x, int(a*b/x)]
    return answer

# 아래는 테스트로 출력해 보기 위한 코드입니다.
print(gcdlcm(3,12))
```

