---
layout: post
title:  "[Stage 2] Centry from year"
date:   2018-01-23 10:15:00 +0900
categories: Algorithm
---

Given a year, return the century it is in. The first century spans from the year 1 up to and including the year 100, the second - from the year 101 up to and including the year 200, etc.

**Example**

- For year = 1905, the output should be centuryFromYear(year) = 20;

- For year = 1700, the output should be centuryFromYear(year) = 17.
- 

**Input/Output**

- [execution time limit] 4 seconds (py3)

- [input] integer year

A positive integer, designating the year.

Guaranteed constraints:
1 ≤ year ≤ 2005.

- [output] integer

The number of the century the year is in.

```python
def centuryFromYear(year):
    answer = 0
    if year % 100 == 0:
        answer = year // 100
    else:
        answer = year // 100 + 1
    return answer
```