# 1281. Subtract the Product and Sum of Digits of an Integer

[LeetCode problem](https://leetcode.com/problems/subtract-the-product-and-sum-of-digits-of-an-integer/) · Easy · C++

## What the problem asks

For a positive integer `n`, multiply all its digits, add all its digits, then return:

`product of digits - sum of digits`

For `n = 234`, the product is `2 * 3 * 4 = 24`, the sum is `2 + 3 + 4 = 9`, and the answer is `24 - 9 = 15`.

## My accepted C++ solution

```cpp
class Solution {
public:
    int subtractProductAndSum(int n) {
        int prod = 1;
        int sum = 0;

        while (n != 0) {
            int digit = n % 10;
            prod = prod * digit;
            sum = sum + digit;
            n = n / 10;
        }

        int result = prod - sum;
        return result;
    }
};
```

## How the code works

- `int prod = 1;` stores the product of the digits processed so far. Start at 1 because multiplying by 1 leaves a number unchanged.
- `int sum = 0;` stores the sum of the digits processed so far. Start at 0 because adding 0 leaves a number unchanged.
- `while (n != 0)` repeats while there are still digits to process.
- `int digit = n % 10;` gets the last digit. For example, `4567 % 10` is `7`.
- `prod = prod * digit;` multiplies that digit into the product.
- `sum = sum + digit;` adds that digit to the sum.
- `n = n / 10;` removes the last digit. Integer division changes `4567` to `456`, so the next loop processes `6`.
- When `n` becomes 0, the loop condition is false and the loop ends.
- `int result = prod - sum;` calculates the requested difference.
- `return result;` sends the answer back to LeetCode.

### Important difference between `%` and `/`

Use `n % 10` to **get** the last digit. Use `n / 10` to **remove** it.

If we wrote `n = n % 10`, then `4567` would become `7`, and on every later loop `n` would stay `7`. The loop would never finish.

## Dry run for n = 234

| Current `n` | Digit (`n % 10`) | Product so far | Sum so far | New `n` (`n / 10`) |
|---:|---:|---:|---:|---:|
| 234 | 4 | 1 * 4 = 4 | 0 + 4 = 4 | 23 |
| 23 | 3 | 4 * 3 = 12 | 4 + 3 = 7 | 2 |
| 2 | 2 | 12 * 2 = 24 | 7 + 2 = 9 | 0 |

The function returns `24 - 9 = 15`.

## LeetCode result

Accepted submission 2204484159: **0 ms** runtime and **7.71 MB** memory. The displayed runtime is a measurement for this submission and may vary between runs.

## Complexity

If `d` is the number of digits in `n`, time complexity is `O(d)`, because the loop processes one digit per iteration. Extra space is `O(1)`.
