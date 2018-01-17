---
layout: post
title:  "[Level1] 가운데 글자 가져오기"
date:   2018-01-16 11:30:00 +0900
categories: Algorythm
---

피보나치 수는 F(0) = 0, F(1) = 1일 때, 2 이상의 n에 대하여 F(n) = F(n-1) + F(n-2) 가 적용되는 점화식입니다. 2 이상의 n이 입력되었을 때, fibonacci 함수를 제작하여 n번째 피보나치 수를 반환해 주세요. 예를 들어 n = 3이라면 2를 반환해주면 됩니다.

```python
def fibonacci(num):
	list1 = [0, 1]
	if num == 1:
		print(1)
	else:	
		for i in range(2, num+1):
			list1.append(list1[i-1]+list1[i-2])
		return list1[num]

print(fibonacci(467))
```
