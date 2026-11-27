# 1281. Subtract the Product and Sum of Digits of an Integer

[LeetCode problem](https://leetcode.com/problems/subtract-the-product-and-sum-of-digits-of-an-integer/) · Easy · C++

## What the problem asks

For a positive integer `n`, multiply all its digits, add all its digits, then return:

`product of digits - sum of digits`

For `n = 234`, the product is `2 * 3 * 4 = 24`, the sum is `2 + 3 + 4 = 9`, and the answer is `24 - 9 = 15`.

## C++ solution

```cpp
class Solution {
public:
    int subtractProductAndSum(int n) {
        int digitProduct = 1;
        int digitSum = 0;

        while (n > 0) {
            int digit = n % 10;
            digitProduct *= digit;
            digitSum += digit;
            n /= 10;
        }

        return digitProduct - digitSum;
    }
};
```

## Understand the code

- `int digitProduct = 1;` stores the product so far. We start at 1 because multiplying by 1 does not change a value.
- `int digitSum = 0;` stores the sum so far. We start at 0 because adding 0 does not change a value.
- `while (n > 0)` repeats until we have processed every digit.
- `n % 10` gets the last digit. For example, `234 % 10` is `4`.
- `digitProduct *= digit` multiplies the current product by this digit.
- `digitSum += digit` adds this digit to the current sum.
- `n /= 10` removes the last digit. For example, integer division makes `234 / 10` become `23`.
- After all digits are processed, `return digitProduct - digitSum;` returns the requested difference.

## Dry run for n = 234

| Current `n` | Digit (`n % 10`) | Product so far | Sum so far | New `n` (`n / 10`) |
|---:|---:|---:|---:|---:|
| 234 | 4 | 1 * 4 = 4 | 0 + 4 = 4 | 23 |
| 23 | 3 | 4 * 3 = 12 | 4 + 3 = 7 | 2 |
| 2 | 2 | 12 * 2 = 24 | 7 + 2 = 9 | 0 |

The loop stops when `n` becomes 0. The function returns `24 - 9 = 15`.

## Complexity

If `d` is the number of digits in `n`, the time complexity is `O(d)`, because the loop handles one digit per iteration. Extra space is `O(1)`.
