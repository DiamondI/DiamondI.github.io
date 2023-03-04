---
title: LeetCode Weekly Contest 129
categories: Algorithm
---

# LeetCode Weekly Contest 129

<!-- more -->

## [1020. Partition Array Into Three Parts With Equal Sum](https://leetcode.com/contest/weekly-contest-129/problems/partition-array-into-three-parts-with-equal-sum/)

## Description

Given an array `A` of integers, return `true` if and only if we can partition the array into three non-empty parts with equal sums.

Formally, we can partition the array if we can find indexes `i+1 < j` with `(A[0] + A[1] + ... + A[i] == A[i+1] + A[i+2] + ... + A[j-1] == A[j] + A[j-1] + ... + A[A.length - 1])`

**Example 1:**

>**Input:** [0,2,1,-6,6,-7,9,1,2,0,1]<br>
**Output:** true<br>
**Explanation:** 0 + 2 + 1 = -6 + 6 - 7 + 9 + 1 = 2 + 0 + 1<br>

**Example 2:**

>**Input:** [0,2,1,-6,6,7,9,-1,2,0,1]<br>
**Output:** false<br>

**Example 3:**

>**Input**: [3,3,6,5,-2,2,5,1,-9,4]<br>
**Output:** true<br>
**Explanation:** 3 + 3 = 6 = 5 - 2 + 2 + 5 + 1 - 9 + 4<br>

**Note:**

1. `3 <= A.length <= 50000`
2. `-10000 <= A[i] <= 10000`

## Idea

The idea is quite intuitive. **First,** calculate the total sum of all the elements and  get `1/3` of sum, here denoted as `average`. Then go through the vector by one pass. During the for-loop, maintain two numbers denoted as `curSum` and `cnt`. If `curSum == average`, then `++cnt` and let `curSum` be `0`. If `cnt == 2`, then we can return `true`, otherwisr, we should return `false`.

## Solution

```cpp
class Solution {
public:
    bool canThreePartsEqualSum(vector<int>& A) {
        int sum = 0;
        for (auto i: A) {
            sum += i;
        }
        if (sum % 3) {
            return false;
        }
        int average = sum / 3;
        int curSum = 0;
        int cnt = 0;
        for (auto i: A) {
            curSum += i;
            if (curSum == average) {
                cnt += 1;
                curSum = 0;
            }
            if (cnt == 2) {
                return true;
            }
        }
        return false;
    }
};
```

## [1022. Smallest Integer Divisible by K](https://leetcode.com/contest/weekly-contest-129/problems/smallest-integer-divisible-by-k/)

### Description

Given a positive integer `K`, you need find the smallest positive integer `N` such that `N` is divisible by `K`, and `N` only contains the digit **1**.

Return the length of `N`.  If there is no such `N`, return **-1**.

**Example 1:**

>**Input:** 1<br>
**Output:** 1<br>
**Explanation:** The smallest answer is N = 1, which has length 1.

**Example 2:**

>**Input:** 2<br>
**Output:** -1<br>
**Explanation:** There is no such positive integer N divisible by 2.

**Example 3:**

>**Input:** 3<br>
**Output:** 3<br>
**Explanation:** The smallest answer is N = 111, which has length 3.

### Idea

The basic idea is try the length one by one. However, the target number may be very long and the length can be even larger than `1000`. But, if you're familiar with long division, then it's quite easy to get the solution here. Take `K = 3` as an example:

<img src="https://ws4.sinaimg.cn/large/006tKfTcly1g1dzwcl9enj30c00e4mxs.jpg" width="200dip"/>

We should try `1`, `11` and `111` one by one and finally know the answer is `111`. But here's a trick that we don't really need to calculate `111 % 3`. As in the progress of long division, we first try to use `1` to do the division and get the remainder `1`, and apparently `1` can't be divided by `3` and then we need to try `11`. However, `11` can't be divided by `3`, either. The reminader here is `2`. Then we need to consider `111`, do we need to do calculation of `111 % 3`? No, here we just follow the long division progress and find whether `2 * 10 + 1 = 21` can be divided by `3`.

### Solution

```cpp
class Solution {
public:
    int smallestRepunitDivByK(int K) {
        int cur = 1, len = 1;
        while (len <= K && cur % K) {
            cur = (cur % K) * 10 + 1;
            ++len;
        }
        return len > K ? -1 : len;
    }
};
```

## [1021. Best Sightseeing Pair](https://leetcode.com/contest/weekly-contest-129/problems/best-sightseeing-pair/)

### Dscription

Given an array `A` of positive integers, `A[i]` represents the value of the `i`-th sightseeing spot, and two sightseeing spots `i` and `j` have distance `j - i` between them.

The score of a pair (`i < j`) of sightseeing spots is `(A[i] + A[j] + i - j)` : the sum of the values of the sightseeing spots, **minus** the distance between them.

Return the maximum score of a pair of sightseeing spots.

**Example 1:**

>**Input:** [8,1,5,2,6]<br>
**Output:** 11<br>
**Explanation:** i = 0, j = 2, A[i] + A[j] + i - j = 8 + 5 + 0 - 2 = 11

**Note:**

1. 2 <= A.length <= 50000
2. 1 <= A[i] <= 1000

### Idea

Notice that our target `A[i] + A[j] + i - j` can be divided into two parts, `A[i] + i` and `A[j] - j`. We first calculate two vectors from the origin vector and one of them consists of `A[i] + i` denoted as `ai` in my code while another consists of `A[j] - [j]` named `aj` in my code. Take **Example 1**:

- ai: [8, 2, 7, 5, 10]
- aj: [8, 0, 3, -1, 2]

Then the task becomes to find the largest sum of `ai[i] + aj[j]` where `i < j`. Any number less than `ai[i]` and occuring after it is no need to consider. Suppose we have an element `ai[k]` which is less than `ai[i]`, then for any `j`, `ai[k] + aj[j] < ai[i] + aj[j]` and `ai[i]` can even combine with `aj[k]` while `ai[k]` can not. Thus, we only need to consider the situations where `ai[k] > ai[i]`. And in such situation, we only need to compare the values `ai[i] + aj[m]` where `m` is in `(i, k]` and `ai[k] + aj[n]` where `n` is in `(k, q]` in which `q` is the index with `ai[q] > ai[k]`. Finally, we would get the correct answer.

### Solution

```cpp
class Solution {
public:
    int maxScoreSightseeingPair(vector<int>& A) {
        int n = A.size();
        vector<int> ai(n, 0), aj(n, 0);
        for (int i = 0; i < n; ++i) {
            ai[i] = A[i]+i;
            aj[i] = A[i]-i;
        }
        
        vector<int> cand(1, 0);
        for (int i = 0; i < n; ++i) {
            int pre = cand.back();
            if (ai[i] > ai[pre])
                cand.push_back(i);
        }
        cand.push_back(n);  // corner case
        
        int res = INT_MIN;
        for (int i = 0; i < cand.size()-1; ++i) {
            for (int j = cand[i]+1; j < n && j <= cand[i+1]; ++j) {
                res = max(res, ai[cand[i]]+aj[j]);
            }
        }
        return res;
    }
};
```

## [1023. Binary String With Substrings Representing 1 To N](https://leetcode.com/contest/weekly-contest-129/problems/binary-string-with-substrings-representing-1-to-n/)

### Description

Given a binary string `S` (a string consisting only of '0' and '1's) and a positive integer `N`, return true if and only if for every integer X from 1 to N, the binary representation of X is a substring of S.

**Example 1:**

>**Input:** S = "0110", N = 3<br>
**Output:** true<br>

**Example 2:**

>**Input:** S = "0110", N = 4<br>
**Output:** false

**Note:**

1. `1 <= S.length <= 1000`
2. `1 <= N <= 10^9`

### Idea

It surprises me that my brute-force solution using Python3 was accepted and it cost only 48ms. For more solutions, you can go to discussion [here](https://leetcode.com/problems/binary-string-with-substrings-representing-1-to-n/discuss/?currentPage=1&orderBy=most_posts&query=).

### Solution

```python
class Solution:
    def queryString(self, S: str, N: int) -> bool:
        for i in range(1, N + 1):
            x = bin(i)[2:]
            if x not in S:
                return False
        return True
```
