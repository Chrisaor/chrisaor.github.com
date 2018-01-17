---
layout: post
title:  "[Level1] 제일 작은 수 제거하기"
date:   2018-01-16 11:30:00 +0900
categories: Algorythm
---



```python
def rm_small(mylist):
    return [x for x in mylist if not x==min(mylist)]


# 아래는 테스트로 출력해 보기 위한 코드입니다.
my_list = [4, 3, 2, 1]
print("결과 {} ".format(rm_small(my_list)))
```
