---
layout: post
title:  "[Stage 31] Deposit Profit"
date:   2018-06-04 12:20:00 +0900
categories: Algorithm
---

You have deposited a specific amount of dollars into your bank account. Each year your balance increases at the same growth `rate`. Find out how long it would take for your balance to pass a specific `threshold` with the assumption that you don't make any additional deposits.

**Example**

For `deposit = 100`, `rate = 20` and `threshold = 170`, the output should be
`depositProfit(deposit, rate, threshold) = 3`.

Each year the amount of money on your account increases by `20%`. It means that throughout the years your balance would be:

- year 0: `100`;
- year 1: `120`;
- year 2: `144`;
- year 3: `172,8`.
- 
Thus, it will take `3` years for your balance to pass the `threshold`, which is the answer.

**Input/Output**

- **[execution time limit] 4 seconds (py3)**

- **[input] integer deposit**

The initial deposit as a positive integer.

_Guaranteed constraints_:
`1 ≤ deposit ≤ 100`.

- **[input] integer rate**

The rate of increase. Each year the balance increases by the `rate` percent of the current sum.

_Guaranteed constraints_:
`1 ≤ rate ≤ 100`.

- **[input] integer threshold**

The target balance.

_Guaranteed constraints_:
`deposit < threshold ≤ 200`.

- **[output] integer**

The number of years it would take to hit the `threshold`.

```python
def depositProfit(deposit, rate, threshold):
    year = 0
    while deposit < threshold:
        deposit *= (1+rate/100)
        year += 1
    return year
```