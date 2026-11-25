# 1. Two Sum

[LeetCode problem](https://leetcode.com/problems/two-sum/) · Easy · C++

## Restate the task

Given an integer array `nums` and a target number, return the indices of two different numbers whose sum equals the target. The problem guarantees exactly one answer.

For example, with `nums = [2, 7, 11, 15]` and `target = 9`, the answer is `[0, 1]`, because `nums[0] + nums[1] = 2 + 7 = 9`.

## First idea: check every pair

A simple solution uses two loops. For each number, compare it with every number after it. This is easy to understand, but it can take about `n * n` checks.

## Faster idea: remember what we have seen

As we scan left to right, for each number `x`, ask: “What number would complete the target?” That missing number is `target - x`.

Store previously seen values and their indices in a hash map:

1. Calculate the needed partner: `target - nums[i]`.
2. If the partner is already in the map, return its index and `i`.
3. Otherwise, save `nums[i]` and its index for later.

We check the map before inserting the current value, so an element is never paired with itself.

## C++ solution

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> indexOf;

        for (int i = 0; i < nums.size(); ++i) {
            int needed = target - nums[i];

            if (indexOf.count(needed)) {
                return {indexOf[needed], i};
            }

            indexOf[nums[i]] = i;
        }

        return {}; // The problem guarantees that an answer exists.
    }
};
```

## Walkthrough

For `nums = [2, 7, 11, 15]` and `target = 9`:

- At index 0, the value is 2. We need `9 - 2 = 7`. We have not seen 7, so remember that 2 is at index 0.
- At index 1, the value is 7. We need `9 - 7 = 2`. The map has 2 at index 0, so return `[0, 1]`.

## Complexity

- Time: `O(n)` on average, because each hash-map lookup and insert is average constant time.
- Space: `O(n)` in the worst case, because we may store most of the array.

## What to remember

When a pair must add up to a target, try rewriting the condition as a lookup for the complement: `target - current value`. A hash map helps find that complement quickly.
