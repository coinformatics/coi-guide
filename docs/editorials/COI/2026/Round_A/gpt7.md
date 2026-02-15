---
tags:
  - Prefix Sum
  - Implementation
---

# GPT-7
<span class="author">
Author: ALeonidou
</span>

## Subtask 1 (5 points)
Constraints: $N = 1$, $M \leq 100$, $K = 1$

### Solution

Direct computation. Since there's only one sketch and $K=1$ (scalar values), simply compute the alternating sum of the $\text{Layer}[L..R]$ and then compute the error.

**Time Complexity:** $O(M)$

**Space Complexity:** $O(M)$

---

## Subtask 2 (5 points)
$N = 1$, $M = 1$, $K \leq 10$

### Solution
Direct computation. With only one layer, the output error is just `Error(InputMatrix + Layer[0])`.

**Time Complexity:** $O(K^2)$

**Space Complexity:** $O(K^2)$

---

## Subtask 3 (25 points)
Constraints: $N \leq 10^3$, $M \leq 10^3$, $K \leq 10$

### Solution
Brute force. For each sketch, iterate through the range $[L, R]$, apply layers with alternating signs and then compute the error for each `inputMatrix` and find the one with the smallest error.

### Sample Algorithm
```cpp
for i = 0 to N-1:
    outputMatrix = inputMatrix[i]
    for layer = L[i] to R[i]:
        if (layer - L[i]) is even:
            outputMatrix += Layer[layer]
        else:
            outputMatrix -= Layer[layer]
    compute error
```

**Time Complexity:** $O(N \times M \times K^2)$

**Space Complexity:** $O((N + M) \times K^2)$

---

## Subtask 4 (20 points)
Constraints: $N \leq 10^5$, $M \leq 10^5$, $K = 1$, $L = 0$ for all sketches

### Main idea
Since L=0 for all sketches, once we compute the alternating sum up to some index, we can reuse the range of the alternating sum we already computed, in order to compute the next sketch with a larger R.

### Solution
Sort and calculate the cumulative sum. Sort all the sketches by their $R$ parameter, in order to process the sketches in ascending order of their $R$ parameter. Processing the sketches in this order allows us to keep track of a cumulative alternating sum of `Layer[0..R]` and use that to directly determine the `outputMatrix` and the errors for the sketches. Note that in order to not lose track of the original indexes when sorting the sketches, we can use a map or sort the indexes together with the values in a tuple/pair.

**Time Complexity:** $O(N\log(N) + M)$

**Space Complexity:** $O(N + M)$

**Alternative Approach:** This subtask can also be solved with a single prefix sum array in $O(N + M)$ time (precompute $\text{prefix}_i$ for all $i$, then calculate each output in $O(1)$).

### Sample Algorithm
```cpp
sort(sketches by R, keeping track of original indices)
cumulative_sum = 0
p = 0  // pointer to current layer

for i = 0 to N-1:
    cur_R = R parameter of sketch[i]
    inputMatrix = input matrix of sketch[i]

    // Extend cumulative sum up to cur_R
    while p <= cur_R:
        if p is odd:
            cumulative_sum -= Layer[p]
        else:
            cumulative_sum += Layer[p]
        p++

    output = inputMatrix + cumulative_sum
    compute error
```

---

## Subtask 5 (30 points)
Constraints: $N \leq 10^5$, $M \leq 10^5$, $K = 1$

### Solution
Dual prefix sums. Precompute a prefix sum array for the alternating sum of the layers starting with $+$, and another prefix sum array for the alternating sum of the layers starting with $-$. Use the two prefix sums accordingly to directly compute the alternating sum of `Layer[L..R]` in $O(1)$. This way we can calculate each output matrix and its error in $O(1)$.

**Time Complexity:** $O(N + M)$
**Space Complexity:** $O(N + M)$

### Sample Algorithm
```cpp
prefix[0][0] = +Layer[0]
prefix[1][0] = -Layer[0]

for i = 1 to M-1:
    if i is odd:
        prefix[0][i] = prefix[0][i-1] - Layer[i]
        prefix[1][i] = prefix[1][i-1] + Layer[i]
    else:
        prefix[0][i] = prefix[0][i-1] + Layer[i]
        prefix[1][i] = prefix[1][i-1] - Layer[i]

for i = 0 to N-1:
    parity = L % 2
    range_sum = prefix[parity][R] - prefix[parity][L-1]
    outputMatrix = inputMatrix[i] + range_sum
    compute error
```

---

## Subtask 6 (15 points)
Constraints: $N \leq 10^5$, $M \leq 10^5$, $K \leq 10$

### Solution
Parallel Dual Prefix Sums. Extend the prefix sum technique from *Subtask 5*, for 2D $K\times K$ matrices, by precomputing two prefix sum arrays for each cell.

**Time Complexity:** $O((N + M) \times K^2)$

**Space Complexity:** $O((N + M) \times K^2)$

---

## Summary

| Subtask | Technique | Time Complexity | Space Complexity | Points |
|-|-|-|-|-|
| 1 | Direct Computation | O(M) | O(M) | 5 |
| 2 | Direct Computation | O(K^2) | O(K^2) | 5 |
| 3 | Brute Force | O(N\timesM\timesK^2) | O((N+M)\timesK^2) | 25 |
| 4 | Sort + Cumulative Sum **or** Prefix Sums | O(Nlog(N) + M) **or** O(N + M)  | O(N+M) | 20 |
| 5 | Dual Prefix Sums | O(N+M) | O(N+M) | 30 |
| 6 | Parallel Dual Prefix Sums | O((N+M)\timesK^2) | O((N+M)\timesK^2) | 15 |
