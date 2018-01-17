---
layout: post
title:  "[Level1] 서울에서 김서방 찾기"
date:   2018-01-16 11:30:00 +0900
categories: Algorithm
---

findKim 함수(메소드)는 String형 배열 seoul을 매개변수로 받습니다.

seoul의 element중 Kim의 위치 x를 찾아, 김서방은 x에 있다는 String을 반환하세요.
seoul에 Kim은 오직 한 번만 나타나며 잘못된 값이 입력되는 경우는 없습니다.

```python
    kimIdx = seoul.index("Kim")
    # 함수를 완성하세요

    return "김서방은 {}에 있다".format(kimIdx)


# 실행을 위한 테스트코드입니다.
print(findKim(["Queen", "Tod", "Kim"]))
```
