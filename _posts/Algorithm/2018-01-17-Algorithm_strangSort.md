---
layout: post
title:  "[Level1] 문자열 내 마음대로 정렬하기"
date:   2018-01-16 11:30:00 +0900
categories: Algorithm
---

strange_sort함수는 strings와 n이라는 매개변수를 받아들입니다.
strings는 문자열로 구성된 리스트인데, 각 문자열을 인덱스 n인 글자를 기준으로 정렬하면 됩니다.

예를들어 strings가 [sun, bed, car]이고 n이 1이면 각 단어의 인덱스 1인 문자 u, e ,a를 기준으로 정렬해야 하므로 결과는 [car, bed, sun]이 됩니다.
strange_sort함수를 완성해 보세요.

```python
def strange_sort(strings, n):
    temp = []
    result = []
    for i in range(len(strings)):
        temp.append(strings[i][n:]+str(i))
        temp.sort()
    for j in range(len(strings)):
        result.append(strings[int(temp[j][-1])])
    return result


# 아래는 테스트로 출력해 보기 위한 코드입니다.
print( strange_sort(["sun", "bed", "car"], 1) )
```
