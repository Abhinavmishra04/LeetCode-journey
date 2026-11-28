# 2235. Add Two Integers

[LeetCode problem](https://leetcode.com/problems/add-two-integers/) · Easy · C++

## What the problem asks

The function receives two integer values, `num1` and `num2`. It must return their sum. For example, if `num1` is 12 and `num2` is 5, the answer is 17. Negative numbers are allowed too, so `-10 + 4` gives `-6`.

## C++ solution

```cpp
class Solution {
public:
    int sum(int num1, int num2) {
        return num1 + num2;
    }
};
```

## Understand the code

- `class Solution` is the class name LeetCode expects.
- `public:` makes the method accessible to LeetCode's test runner.
- `int sum(int num1, int num2)` defines a method named `sum`. It takes two integers and returns an integer.
- `return num1 + num2;` adds the inputs and sends the result back.

We do not write `main()` for this LeetCode problem. LeetCode calls `sum` with test inputs and checks the returned answer.

## Complexity

- Time: `O(1)` — one addition, regardless of the input values.
- Extra space: `O(1)` — no additional data structure is needed.
