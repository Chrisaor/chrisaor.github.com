---
layout: post
title:  "[Level2] 이상한 문자만들기"
date:   2018-01-16 11:30:00 +0900
categories: Algorithm
---

toWeirdCase함수는 문자열 s를 매개변수로 입력받습니다.
문자열 s에 각 단어의 짝수번째 인덱스 문자는 대문자로, 홀수번째 인덱스 문자는 소문자로 바꾼 문자열을 리턴하도록 함수를 완성하세요.
예를 들어 s가 try hello world라면 첫 번째 단어는 TrY, 두 번째 단어는 HeLlO, 세 번째 단어는 WoRlD로 바꿔 TrY HeLlO WoRlD를 리턴하면 됩니다.

주의 문자열 전체의 짝/홀수 인덱스가 아니라, 단어(공백을 기준)별로 짝/홀수 인덱스를 판단합니다.

```python
def toWeirdCase(s):
    answer = []
    list1 =s.lower().split(" ")
    for i in range(len(list1)):
        str1 = ''
        for j in range(len(list1[i])):
            if j % 2 ==0:
                str1 += list1[i][j].upper()
            else:
                str1 += list1[i][j]
        answer.append(str1)
    return " ".join(answer)

print("결과 : {}".format(toWeirdCase("try hello world")));
```