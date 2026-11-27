# 191. Number of 1 Bits

[LeetCode problem](https://leetcode.com/problems/number-of-1-bits/) · Easy · C++

## What the problem asks

Given a positive integer, count how many `1` bits appear in its binary representation. For example, decimal 11 is binary `1011`, which contains three `1` bits, so the answer is 3.

## My accepted C++ solution

```cpp
class Solution {
public:
    int hammingWeight(int n) {
        int count = 0;

        while (n != 0) {
            if (n & 1) {
                count++;
            }
            n = n >> 1;
        }

        return count;
    }
};
```

## How the code works

- `int count = 0;` starts the number of set bits counted so far at zero.
- `while (n != 0)` repeats until shifting has moved every bit out of `n`.
- `n & 1` checks the rightmost bit. If it is 1, the result is nonzero and we increase `count`.
- `count++` adds one to the count.
- `n = n >> 1;` shifts all bits one place to the right. The rightmost bit is discarded, so the next loop checks the next bit.
- When `n` reaches zero, the loop ends and `return count;` gives the number of 1 bits.

## Dry run for n = 11

Decimal 11 in binary is `1011`.

| Binary `n` | Rightmost bit (`n & 1`) | Count after check | After shifting right |
|---|---:|---:|---|
| `1011` | 1 | 1 | `101` |
| `101` | 1 | 2 | `10` |
| `10` | 0 | 2 | `1` |
| `1` | 1 | 3 | `0` |

The loop stops at zero and returns 3.

## LeetCode result

[Accepted submission](https://leetcode.com/problems/number-of-1-bits/submissions/2204503373/): **0 ms** runtime and **8.24 MB** memory, as displayed for this run. Runtime measurements can vary between runs.

## Complexity

For an input with `b` binary digits, the loop runs `b` times: `O(b)` time and `O(1)` extra space.
