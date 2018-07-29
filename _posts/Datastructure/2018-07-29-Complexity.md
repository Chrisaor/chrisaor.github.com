---
layout: post
title:  "[Complexity] 알고리즘의 복잡도"
date:   2018-07-29 12:22:00 +0900
categories: datastructure
---

1. 시간 복잡도(Time Complexity)

- 문제의 크기와 이를 해결하는 데 걸리는 시간 사이의 관계


2. 공간 복잡도(Space Complexity)

- 문제의 크기와 이를 이해결하는 데 필요한 메모리 공간 사이의 관계

시간 복잡도에 대해서 좀 더 알아보자.


- 평균 시간 복잡도(Average Time Complexity)

임의의 입력 패턴을 가정했을 때 소용되는 시간의 평균

- 최악 시간 복잡도(Worst-case Time Complexity)

가장 긴 시간을 소요하게 만드는 입력에 따라 소요되는 시간

**Big-O Notation**

점근 표기법(asymptotic notation)의 하나

어떤 함수의 증가 양상을 다른 함수와의 비교로 표현(알고리즘의 복잡도를 표현할 때 흔히 쓰임)

O(log*n*), O(n), O(n<sup>2</sup>), O(2<sup>n</sup>) 등으로 표기



1. 선형 시간 알고리즘 - O(n)

예) n 개의 무작위로 나열된 수에서 최댓값을 찾기 위해 선형 탐색 알고리즘을 적용

--> 끝까지 다 살펴보기 전까지는 최댓값을 알 수 없다

2. 로그 시간 알고리즘 - O(log*n*)

예) n 개의 **크기 순으로 정렬된 수**에서 특정 값을 찾기 위해 이진 탐색 알고리즘을 적용

3. 이차 시간 알고리즘 - O(n<sup>2</sup>)

예) 삽입 정렬(insertion sort)

![ok6qz8oaecrovodd27vpgc5r](https://user-images.githubusercontent.com/33015649/43363511-5f7255f6-9341-11e8-99b5-098d52669793.png)

Best case : O(n) (이미 정렬이 되어있는 경우)  
Worst case : O(n<sup>2</sup>) (역순으로 정렬되어 있는 경우)

4. 보다 낮은 복잡도를 가지는 정렬 알고리즘

예) 병합 정렬(merge sort)

정렬할 데이터를 반씩 나누어 각각을 정렬하고 정렬된 데이터를 두 묶음씩 한데 합친다.(divide and conqure방법)

![merge_sort](https://user-images.githubusercontent.com/33015649/43363531-d06d1b42-9341-11e8-849e-59d4260a0a47.png)

5. 꽤나 복잡한 문제

예) 배낭 문제 (Knapsack problem) - dynamic programming을 이용하면 그나마 효율적으로 풀 수 있지만 여전히 복잡함

![default](https://user-images.githubusercontent.com/33015649/43363539-f8734800-9341-11e8-94ff-4a82a1d6d35c.png)




