---
layout: post
title:  "[Level1] 평균구하기"
date:   2018-01-16 11:30:00 +0900
categories: Algorithm
---

함수를 완성해서 매개변수 array의 평균값을 return하도록 만들어 보세요.
어떠한 크기의 array가 와도 평균값을 구할 수 있어야 합니다.

```python
def average(list):
	sum = 0
	for i in range(len(list)):
		sum += list[i]	
	return (sum / len(list))
# 아래는 테스트로 출력해 보기 위한 코드입니다.
list = [5,3,4] 
print("평균값 : {}".format(average(list)));
```
