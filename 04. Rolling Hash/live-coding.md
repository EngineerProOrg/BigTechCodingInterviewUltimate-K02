# Rolling Hash - Live Coding Solutions

Speaker: Hiệp
## [1. Longest Chunked Palindrome Decomposition](https://leetcode.com/problems/longest-chunked-palindrome-decomposition/description/)

```python
class Solution:
    def longestDecomposition(self, text: str) -> int:
        mod = 10**9 + 7
        base = 29

        n = len(text)
        res = 0

        power = [1] * (n + 1)
        for i in range(1, n + 1):
            power[i] = (power[i - 1] * base) % mod

        i, j = 0, n - 1
        l, r = 0, 0
        m = 0
        while i <= j:
            l = (l * base + ord(text[i]) - 96) % mod
            r = ((ord(text[j]) - 96) * power[m] + r) % mod
            m += 1
            i += 1
            j -= 1
            if l == r:
                if i - 1 < j + 1:
                    res += 2
                else:
                    res += 1
                l, r, m = 0, 0, 0
        
        if m > 0:
            res += 1
        
        return res
```

Complexity:

- Time: O(N).
- Space: O(N).

## [2. Distinct Echo Substrings](https://leetcode.com/problems/distinct-echo-substrings/description/)

```python
class Solution:
    def distinctEchoSubstrings(self, text: str) -> int:
        mod = 10**9 + 7
        base = 29

        n = len(text)
        hash = [0] * (n + 1)
        power = [1] * (n + 1)
        for i in range(1, n + 1):
            hash[i] = (hash[i - 1] * base + ord(text[i - 1]) - 96) % mod
            power[i] = (power[i - 1] * base) % mod         

        def get_hash(i: int, j: int) -> int:
            return (hash[j + 1] - (hash[i] * power[j - i + 1]) % mod + mod) % mod
        
        found = set()
        for i in range(n):
            for j in range(i, n):
                hij = get_hash(i, j)
                # i...j, j+1...2 * j - i + 1
                if 2 * j - i + 1 < n:
                    hij2 = get_hash(j + 1, 2 * j - i + 1)
                    if hij == hij2:
                        found.add(hij)
        return len(found)
```

Complexity:

- Time: O(N^2).
- Space: O(N).

## [3. Longest Happy Prefix](https://leetcode.com/problems/longest-happy-prefix/description/)

```python
class Solution:
    def longestPrefix(self, s: str) -> str:
        mod = 10**9 + 7
        base = 29

        n = len(s)
        hash = [0] * (n + 1)
        power = [1] * (n + 1)
        for i in range(1, n + 1):
            hash[i] = (hash[i - 1] * base + ord(s[i - 1]) - 96) % mod
            power[i] = (power[i - 1] * base) % mod

        def get_hash(i: int, j: int) -> int:
            return (hash[j + 1] - (hash[i] * power[j - i + 1]) % mod + mod) % mod
        
        for i in range(n - 1, 0, -1):
            if hash[i] == get_hash(n - i, n - 1):
                return s[:i]
        
        return ""
```

Complexity:

- Time: O(N).
- Space: O(N).
