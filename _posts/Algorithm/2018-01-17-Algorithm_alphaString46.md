---
layout: post
title:  "[Level1] 문자열 다루기 기본"
date:   2018-01-16 11:30:00 +0900
categories: Algorythm
---

alpha_string46함수는 문자열 s를 매개변수로 입력받습니다.
s의 길이가 4혹은 6이고, 숫자로만 구성되있는지 확인해주는 함수를 완성하세요.
예를들어 s가 a234이면 False를 리턴하고 1234라면 True를 리턴하면 됩니다

```python
def alpha_string46(s):
    #함수를 완성하세요
	if len(s) == 4 or len(s) == 6:
		for i in s:
			if i in ['1','2','3','4','5','6','7','8','9','0']:
				continue
			else:
				return False
		return True
	else:
		return False

# 아래는 테스트로 출력해 보기 위한 코드입니다.
print( alpha_string46("a234") )
print( alpha_string46("1234") )
```