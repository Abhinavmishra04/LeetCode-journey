# 412. Fizz Buzz

[LeetCode problem](https://leetcode.com/problems/fizz-buzz/) · Easy · C++

## What the problem asks

For every number from 1 through `n`, add one string to the answer:

- Use `"FizzBuzz"` if the number is divisible by both 3 and 5.
- Use `"Fizz"` if it is divisible by 3.
- Use `"Buzz"` if it is divisible by 5.
- Otherwise, write the number as text, such as `"7"`.

For `n = 5`, the result is `["1", "2", "Fizz", "4", "Buzz"]`.

## C++ solution

```cpp
#include <string>
#include <vector>
using namespace std;

class Solution {
public:
    vector<string> fizzBuzz(int n) {
        vector<string> answer;

        for (int number = 1; number <= n; ++number) {
            if (number % 3 == 0 && number % 5 == 0) {
                answer.push_back("FizzBuzz");
            } else if (number % 3 == 0) {
                answer.push_back("Fizz");
            } else if (number % 5 == 0) {
                answer.push_back("Buzz");
            } else {
                answer.push_back(to_string(number));
            }
        }

        return answer;
    }
};
```

## Understand the code

- `vector<string> answer` creates a list that will hold the output strings.
- The `for` loop starts at 1 and continues through `n`, including `n`.
- `number % 3 == 0` means the remainder after division by 3 is zero, so the number is divisible by 3. We use the same check for 5.
- `&&` means “and”, so both divisibility checks must be true for `"FizzBuzz"`.
- We check the combined condition first. For example, 15 is divisible by both 3 and 5.
- `push_back(...)` adds a string to the end of the answer list.
- `to_string(number)` converts an ordinary number, such as 7, into the string `"7"`.
- At the end, `return answer` gives the completed list back to LeetCode.

## Small walkthrough

For `n = 5`:

| Current number | Rule that matches | String added |
|---:|---|---|
| 1 | Neither | `"1"` |
| 2 | Neither | `"2"` |
| 3 | Divisible by 3 | `"Fizz"` |
| 4 | Neither | `"4"` |
| 5 | Divisible by 5 | `"Buzz"` |

For 15 or 30, both divisible-by-3 and divisible-by-5 checks are true, so the first branch adds `"FizzBuzz"`.

## Complexity

- Time: `O(n)`, because the loop processes each number once.
- Extra space: `O(n)` for the list of output strings.
