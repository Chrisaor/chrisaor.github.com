---
layout: post
title:  "[Level2] 2016년"
date:   2018-01-16 11:30:00 +0900
categories: Algorithm
---

2016년 1월 1일은 금요일입니다. 2016년 A월 B일은 무슨 요일일까요? 두 수 A,B를 입력받아 A월 B일이 무슨 요일인지 출력하는 getDayName 함수를 완성하세요. 요일의 이름은 일요일부터 토요일까지 각각

`SUN,MON,TUE,WED,THU,FRI,SAT`

를 출력해주면 됩니다. 예를 들어 A=5, B=24가 입력된다면 5월 24일은 화요일이므로 `TUE`를 반환하면 됩니다.

```python
def getDayName(a,b):
    dict1 = {
    1:0,
    2:31,
    3:60,
    4:91,
    5:121,
    6:152,
    7:182,
    8:213,
    9:245,
    10:275,
    11:305,
    12:335,
    }
    dict2 = {
    0:"THU",
    1:"FRI",
    2:"SAT",
    3:"SUN",
    4:"MON",
    5:"TUE",
    6:"WED",
    }
    numDay = dict1[a]+b
    c = numDay % 7
    answer = dict2[c]
    return answer

#아래 코드는 테스트를 위한 출력 코드입니다.
print(getDayName(12,31))
```
