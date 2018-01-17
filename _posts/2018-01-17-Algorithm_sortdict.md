---
layout: post
title:  "[Level1] 딕셔너리 정렬"
date:   2018-01-16 11:30:00 +0900
categories: Algorythm
---

딕셔너리는 들어있는 값에 순서가 없지만, 키를 기준으로 정렬하고 싶습니다. 그래서 키와 값을 튜플로 구성하고, 이를 순서대로 리스트에 넣으려고 합니다.
예를들어 {김철수:78, 이하나:97, 정진원:88}이 있다면 각각의 키와 값을

(김철수, 78)
(이하나, 97)
(정진원, 88)
과 같이 튜플로 분리하고 키를 기준으로 정렬해서 다음과 같은 리스트를 만들면 됩니다.
[ (김철수, 78), (이하나, 97), (정진원, 88) ]

다음 sort_dictionary 함수를 완성해 보세요.


```python
def sort_dictionary(dic):
    tup1 = tuple(dic.items())
    list1 = []
    for i in range(len(tup1)):
        list1.append(tup1[i])
        list1.sort()

    return list1

# 아래는 테스트로 출력해 보기 위한 코드입니다.
print( sort_dictionary( {"김철수":78, "이하나":97, "정진원":88} ))
```
