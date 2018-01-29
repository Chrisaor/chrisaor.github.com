---
layout: post
title:  "[Stage 6] Almost Increasing Sequence"
date:   2018-01-29 10:05:00 +0900
categories: Algorithm
---

Given a sequence of integers as an array, determine whether it is possible to obtain a strictly increasing sequence by removing no more than one element from the array.

**Example**

For `sequence = [1, 3, 2, 1]`, the output should be
`almostIncreasingSequence(sequence) = false`;

There is no one element in this array that can be removed in order to get a strictly increasing sequence.

For `sequence = [1, 3, 2]`, the output should be
`almostIncreasingSequence(sequence) = true`.

You can remove `3` from the array to get the strictly increasing sequence `[1, 2]`. Alternately, you can remove `2` to get the strictly increasing sequence `[1, 3]`.

**Input/Output**

**[execution time limit] 4 seconds (py3)**

**[input] array.integer sequence**

Guaranteed constraints:
`2 ≤ sequence.length ≤ 105`,
`-105 ≤ sequence[i] ≤ 105`.

**[output] boolean**

Return `true` if it is possible to remove one element from the array in order to get a strictly increasing sequence, otherwise return `false`.


```python
def almostIncreasingSequence(sequence):
    j = 0 #1
    temp = list(sequence)
    for i in range(1,len(sequence)):
        if j == 1:
            for k in range(1,len(temp)):
                if temp[k] <= temp[k-1]:
                    return False
            return True
		#2
        elif sequence[0] >= sequence[i]:
            del temp[0]
            j += 1
		#3
        elif sequence[i] <= sequence[i-1]:

            if sequence[i] <= sequence[i-2]:
                del temp[i]
            else:
                del temp[i-1]

            j += 1

    return True
```

이번 문제는 풀긴했지만 정말 자신없게 풀었다.

문제의 요지는 오름차순으로 숫자가 배열되는 리스트를 만드는데 최대 한 번 방해되는 숫자를 제거할 수 있다. 최대 한 번 제거해서 리스트를 크기 순서대로 만들 수 있다면 True를 반환, 불가능하면 False를 반환하는 것

\#1 변수`j`를 1번 이상 리스트 원소를 삭제했을 때 1이 더해지는 체크를 위한 변수로 설정하며 `j`가 1일 경우 순서대로 잘 배치가 되어 있는지 확인하는 루프로 들어간다.

그 아래는 2가지 검사를 통해 오름 차순으로 만들기 위한 if문을 작성한 것이다.

\#2 첫 번째 원소가 두 번째 원소보다 클 경우 무조건 제거한다. 첫 번째 원소는 리스트에서 제일 작아야한다. 이 과정이 끝나면 j=1

\#3 현재 [i]위치의 원소 그 [i-1]위치의 원소보다 작거나 같을 때 [i] 또는 [i-1]위치의 원소를 제거해야한다.

[i-2]의 원소의 수와 다시 검사해서 어떤 원소를 제거할지 결정한다.

이 과정이 끝나도 j=1

원소 한 개를 최적의 방법으로 제거하였다.

주어진 찬스를 모두 사용했으므로 #1로 돌아가면 j==1이 적용되어 바뀐 리스트가 순차적으로 배열이 되었는지 검사를 한다. 

그렇다면 True 아니라면 False를 반환한다