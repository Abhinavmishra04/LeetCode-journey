# 2235. Add Two Integers

[LeetCode problem](https://leetcode.com/problems/add-two-integers/) · Easy · Java

## Problem

Given two integers `num1` and `num2`, return their sum. For example, `12 + 5 = 17`, and `-10 + 4 = -6`.

## Java solution

```java
class Solution {
    public int sum(int num1, int num2) {
        return num1 + num2;
    }
}
```

## Explanation

- `class Solution` is the class LeetCode expects.
- `public int sum(int num1, int num2)` defines a public method that accepts two integers and returns an integer.
- `return num1 + num2;` adds the inputs and returns their sum.

LeetCode calls this method with its test cases, so a separate `main()` method is not needed.

## Complexity

- Time: `O(1)`
- Extra space: `O(1)`
