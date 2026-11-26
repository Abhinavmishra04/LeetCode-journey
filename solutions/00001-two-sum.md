# 1. Two Sum

[LeetCode problem](https://leetcode.com/problems/two-sum/) · Easy · C++

## What the question asks

You receive an integer array `nums` and an integer `target`. Find two different array positions whose values add up to `target`. Return the positions (indices), not the values.

Example: in `[3, 2, 4]`, the values `2` and `4` are at indices `1` and `2`. They add to `6`, so for target `6` we return `[1, 2]`.

Array indices start at zero. You cannot use the same index twice. The problem guarantees one answer.

## Approach 1: Check every pair

This is the simplest idea: pick one number, then check each number after it. If their sum equals the target, return their indices.

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        for (int i = 0; i < nums.size(); ++i) {
            for (int j = i + 1; j < nums.size(); ++j) {
                if (nums[i] + nums[j] == target) {
                    return {i, j};
                }
            }
        }

        return {}; // The problem guarantees that an answer exists.
    }
};
```

### How the loops work

- The outer loop chooses the first index, `i`.
- The inner loop checks later indices, `j`.
- Starting `j` at `i + 1` prevents using the same array element twice and avoids checking the same pair again in reverse.
- When the two values add to the target, `{i, j}` is the answer.

For `nums = [3, 2, 4]` and `target = 6`, the pairs are:

| Indices | Values | Sum |
|---|---|---:|
| `(0, 1)` | `3 + 2` | 5 |
| `(0, 2)` | `3 + 4` | 7 |
| `(1, 2)` | `2 + 4` | 6 |

The last pair works, so return `[1, 2]`.

This approach is useful for learning nested loops. It takes `O(n²)` time in the worst case, so we can improve it.

## Approach 2: Use a hash map for a faster search

For each value `x`, the partner we need is `target - x`. For example, if the target is `6` and `x` is `2`, we need `4`.

An `unordered_map` can remember values we have already passed and the indices where we found them. At each position:

1. Calculate the needed partner.
2. Look for that partner among the values already seen.
3. If found, return the saved index and the current index.
4. Otherwise, save the current value and index for later.

We look in the map before saving the current value. This ensures we do not pair an element with itself.

```cpp
#include <vector>
#include <unordered_map>
using namespace std;

class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> indexOf;

        for (int i = 0; i < nums.size(); ++i) {
            int needed = target - nums[i];

            if (indexOf.count(needed) > 0) {
                return {indexOf[needed], i};
            }

            indexOf[nums[i]] = i;
        }

        return {}; // The problem guarantees that an answer exists.
    }
};
```

### Dry run of the faster approach

Input: `nums = [3, 2, 4]`, `target = 6`

| Index `i` | Current value | Needed value (`target - current`) | Values remembered before this step | Action |
|---:|---:|---:|---|---|
| 0 | 3 | 3 | none | 3 is not remembered; save `3 -> 0` |
| 1 | 2 | 4 | `3 -> 0` | 4 is not remembered; save `2 -> 1` |
| 2 | 4 | 2 | `3 -> 0, 2 -> 1` | 2 is at index 1; return `[1, 2]` |

### What the map stores

For this problem, `unordered_map<int, int>` means:

- The first `int` is an array value.
- The second `int` is the index where that value appeared.

For example, `indexOf[2] = 1` means “the value 2 was found at index 1.”

## Complexity comparison

Let `n` be the number of values.

| Approach | Time | Extra space |
|---|---:|---:|
| Check every pair | `O(n²)` | `O(1)` |
| Hash map | `O(n)` average | `O(n)` |

The hash-map version is faster on average because it avoids scanning the rest of the array for every value.

## Key idea to remember

When you need a pair that adds to a target, calculate the missing partner with `target - currentValue`. A hash map lets you quickly check whether that partner has already appeared.
