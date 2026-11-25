# 3568. Minimum Moves to Clean the Classroom

[LeetCode problem](https://leetcode.com/problems/minimum-moves-to-clean-the-classroom/) · Medium · C++

## Restate the task

Start at `S` in a grid. Move up, down, left, or right to collect every `L`. Each move costs one energy. Walls (`X`) cannot be crossed, and stepping on a reset cell (`R`) refills energy. Return the fewest moves, or `-1` if all litter cannot be collected.

## Key idea

A normal shortest-path search remembers which grid cell we are in. Here, that is not enough: being at the same cell with more energy, or with different litter already collected, can lead to different outcomes.

So one search state remembers:

- the current cell,
- remaining energy,
- which litter has been collected,
- how many moves it took to get there.

Breadth-first search (BFS) checks states in increasing number of moves. The first state that has collected all litter therefore gives the minimum.

There are at most 10 litter cells. Give each litter an ID and use an integer bitmask to record which ones are collected. For example, with three pieces of litter, mask `101` means litter 0 and 2 are collected.

## Step-by-step example

For `["S.", "XL"]` with energy `2`:

1. Move right from `S` (energy becomes 1).
2. Move down onto `L` (energy becomes 0, and all litter is collected).

That is 2 moves. The cell below the start is a wall, so the path cannot go down first.

## C++ solution

```cpp
class Solution {
public:
    int minMoves(vector<string>& classroom, int energy) {
        int m = classroom.size();
        int n = classroom[0].size();

        int start = -1;
        int litterCount = 0;
        vector<vector<int>> litterId(m, vector<int>(n, -1));

        // Assign each litter cell a bit position.
        for (int r = 0; r < m; ++r) {
            for (int c = 0; c < n; ++c) {
                if (classroom[r][c] == 'S') {
                    start = r * n + c;
                } else if (classroom[r][c] == 'L') {
                    litterId[r][c] = litterCount++;
                }
            }
        }

        int allCollected = (1 << litterCount) - 1;

        struct State {
            int position;
            int remainingEnergy;
            int mask;
            int moves;
        };

        queue<State> q;
        q.push({start, energy, 0, 0});

        // visited[position][energy][mask] stops us revisiting the same state.
        vector<vector<vector<bool>>> visited(
            m * n,
            vector<vector<bool>>(energy + 1, vector<bool>(1 << litterCount, false))
        );
        visited[start][energy][0] = true;

        int dr[4] = {-1, 1, 0, 0};
        int dc[4] = {0, 0, -1, 1};

        while (!q.empty()) {
            State current = q.front();
            q.pop();

            if (current.mask == allCollected) {
                return current.moves;
            }

            // With no energy left, we cannot make another move.
            if (current.remainingEnergy == 0) {
                continue;
            }

            int r = current.position / n;
            int c = current.position % n;

            for (int direction = 0; direction < 4; ++direction) {
                int nr = r + dr[direction];
                int nc = c + dc[direction];

                if (nr < 0 || nr >= m || nc < 0 || nc >= n) {
                    continue;
                }
                if (classroom[nr][nc] == 'X') {
                    continue;
                }

                int nextEnergy = current.remainingEnergy - 1;
                if (classroom[nr][nc] == 'R') {
                    nextEnergy = energy;
                }

                int nextMask = current.mask;
                if (litterId[nr][nc] != -1) {
                    nextMask |= (1 << litterId[nr][nc]);
                }

                int nextPosition = nr * n + nc;

                if (!visited[nextPosition][nextEnergy][nextMask]) {
                    visited[nextPosition][nextEnergy][nextMask] = true;
                    q.push({
                        nextPosition,
                        nextEnergy,
                        nextMask,
                        current.moves + 1
                    });
                }
            }
        }

        return -1;
    }
};
```

## Why use a bitmask?

Each litter gets one bit. If litter ID 2 is collected, we turn on bit 2 using:

```cpp
nextMask |= (1 << 2);
```

When all litter is collected, every litter bit is on. This is exactly `(1 << litterCount) - 1`. If there are no litter cells, this expression is 0, so the starting state already satisfies the finish condition.

## Complexity

Let `m * n` be the number of grid cells, `E` the energy capacity, and `L` the number of litter cells. There are at most `m * n * (E + 1) * 2^L` different states. Each state checks four neighbors, so the time and memory are both bounded by `O(m * n * E * 2^L)`.

## What to remember

For shortest paths with extra constraints, ask: “What information changes whether a future move is possible?” Here, that is energy and already-collected litter, so both belong in the BFS state.
