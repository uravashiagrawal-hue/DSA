# Complete C++ STL Notes for DSA Placements

---

## 1. `vector` — Dynamic Array

### Declaration
```cpp
vector<int> v;
vector<int> v(n);           // size n, value 0
vector<int> v(n, val);      // size n, all = val
vector<vector<int>> v(n, vector<int>(m, 0)); // 2D
```

### Functions
| Function | Description | Complexity |
|---|---|---|
| `push_back(x)` | insert at end | O(1) amortized |
| `pop_back()` | remove last | O(1) |
| `size()` | number of elements | O(1) |
| `empty()` | true if size==0 | O(1) |
| `clear()` | remove all elements | O(n) |
| `front()`, `back()` | first/last element | O(1) |
| `v[i]`, `v.at(i)` | access (at() bounds-checked) | O(1) |
| `insert(it, x)` | insert x before iterator it | O(n) |
| `erase(it)` / `erase(l,r)` | remove element/range | O(n) |
| `begin()`, `end()` | iterators | O(1) |
| `rbegin()`, `rend()` | reverse iterators | O(1) |
| `resize(n)` | change size | O(n) |
| `assign(n, val)` | reassign n copies of val | O(n) |
| `swap(v2)` | swap contents | O(1) |
| `capacity()` | allocated storage size | O(1) |

### Pattern recognition
- **Use when**: need dynamic-size sequential storage, random access by index.
- Sliding window, two-pointer, prefix sums → vector<int>.
- Graph adjacency list → `vector<vector<int>> adj(n)`.
- DP tables → `vector<vector<int>> dp`.

---

## 2. `pair`

```cpp
pair<int,int> p = {1,2};
p.first, p.second
make_pair(a,b)
```
- **Pattern**: store (value,index), (weight,node) in Dijkstra, coordinates, intervals.
- Comparators sort by `.first` then `.second` by default → useful for interval/greedy problems.

## `tuple` (3+ elements)
```cpp
tuple<int,int,int> t = {1,2,3};
get<0>(t), get<1>(t)
tie(a,b,c) = t;
```
- **Pattern**: Dijkstra with (dist, node, parent), 3D DP states.

---

## 3. `string`

| Function | Description |
|---|---|
| `s.length()`/`s.size()` | length |
| `s.substr(pos, len)` | substring |
| `s.find(str)` | first occurrence index or `npos` |
| `s.rfind(str)` | last occurrence |
| `s.append(str)` / `s += str` | concatenate |
| `s.insert(pos, str)` | insert |
| `s.erase(pos, len)` | remove |
| `s.replace(pos, len, str)` | replace |
| `s.compare(s2)` | lexicographic compare |
| `reverse(s.begin(), s.end())` | reverse |
| `sort(s.begin(), s.end())` | sort chars |
| `to_string(x)` | number → string |
| `stoi(s)`, `stol(s)`, `stod(s)` | string → number |
| `s.c_str()` | C-string conversion |
| `isalpha`, `isdigit`, `isupper`, `islower`, `toupper`, `tolower` | `<cctype>` char utils |

- **Pattern**: anagram check → sort/frequency array; palindrome check → two pointers; substring search → sliding window / KMP; string hashing for pattern matching.

---

## 4. `set` — Sorted Unique Elements (Red-Black Tree)

| Function | Description | Complexity |
|---|---|---|
| `insert(x)` | insert x | O(log n) |
| `erase(x)` | erase by value | O(log n) |
| `erase(it)` | erase by iterator | O(log n) |
| `find(x)` | iterator to x or end() | O(log n) |
| `count(x)` | 0 or 1 | O(log n) |
| `lower_bound(x)` | first element >= x | O(log n) |
| `upper_bound(x)` | first element > x | O(log n) |
| `begin()`, `end()`, `rbegin()`, `rend()` | iterators (sorted order) | O(1) |
| `size()`, `empty()`, `clear()` | | |

- **multiset**: same as set but allows duplicates. `erase(x)` removes ALL occurrences; use `erase(find(x))` to remove one.
- **Pattern**:
  - Need sorted unique elements with fast insert/delete/search → `set`.
  - Need to find next greater/smaller element dynamically → `lower_bound`/`upper_bound` on set.
  - Sliding window with distinct elements + order → set/multiset.
  - Kth smallest with insertions/deletions → `set`/`multiset` (or `PBDS` for indexed access).

---

## 5. `unordered_set` — Hash Set

| Function | Description |
|---|---|
| `insert(x)`, `erase(x)`, `find(x)`, `count(x)` | O(1) average, O(n) worst |
| `size()`, `empty()`, `clear()` | |

- **Pattern**: need O(1) average lookup, don't care about order → duplicate detection, "have I seen this before", frequency existence checks.
- Use `set` instead of `unordered_set` when you need sorted order or `lower_bound`.

---

## 6. `map` — Sorted Key-Value (Red-Black Tree)

| Function | Description |
|---|---|
| `m[key] = val` | insert/update (creates key with default val if absent — careful in checks) |
| `m.insert({k,v})` | insert only if not present |
| `m.find(k)` | iterator or end() |
| `m.count(k)` | 0 or 1 |
| `m.erase(k)` | remove by key |
| `m.at(k)` | access, throws if absent |
| `lower_bound(k)`, `upper_bound(k)` | sorted search |
| iterate in **sorted key order** with `for(auto&[k,v]:m)` | |

- **multimap**: allows duplicate keys.
- **Pattern**: frequency count needing sorted keys, prefix-sum-count with order, ordered grouping.

## `unordered_map` — Hash Map
- Same interface, O(1) average, no ordering.
- **Pattern**: THE most common tool for frequency counting, seen-before checks, two-sum style (`value → index`), grouping (anagrams: sorted-string → list).

---

## 7. `stack` (LIFO)

| Function | Description |
|---|---|
| `push(x)` | insert top |
| `pop()` | remove top (no return) |
| `top()` | access top |
| `empty()`, `size()` | |

- **Pattern**:
  - Next Greater/Smaller Element (monotonic stack).
  - Balanced parentheses / matching brackets.
  - Expression evaluation (infix→postfix, calculator problems).
  - Backtracking simulation (undo operations), histogram largest rectangle.

---

## 8. `queue` (FIFO)

| Function | Description |
|---|---|
| `push(x)` | insert at back |
| `pop()` | remove front |
| `front()`, `back()` | access ends |
| `empty()`, `size()` | |

- **Pattern**: BFS traversal (graphs, trees, grids), level-order processing.

## `deque` — Double-Ended Queue
| Function | Description |
|---|---|
| `push_back`, `push_front`, `pop_back`, `pop_front` | O(1) both ends |
| `front()`, `back()`, `[i]` | O(1) random access |

- **Pattern**: Sliding Window Maximum/Minimum (monotonic deque), palindrome checks, 0-1 BFS.

## `priority_queue` (Max-Heap by default)

```cpp
priority_queue<int> pq;                          // max-heap
priority_queue<int, vector<int>, greater<int>> pq; // min-heap
priority_queue<pair<int,int>> pq;                 // compares .first, then .second
```

| Function | Description |
|---|---|
| `push(x)` | O(log n) |
| `pop()` | remove top | O(log n) |
| `top()` | max (or min) | O(1) |
| `empty()`, `size()` | |

- **Pattern**:
  - Kth largest/smallest element → heap of size k.
  - Dijkstra's shortest path → min-heap of (dist, node).
  - Merge K sorted lists → min-heap.
  - Top-K frequent elements → heap.
  - Median in data stream → two heaps (max-heap + min-heap).
  - Greedy scheduling (meeting rooms, task scheduler).

---

## 9. `bitset`

```cpp
bitset<32> b(num);
b.set(i); b.reset(i); b.flip(i);
b.count();      // number of set bits
b.any(); b.none(); b.all();
b.to_string(); b.to_ulong();
```
- **Pattern**: bitmask DP, subset enumeration, fast bit counting, space-optimized boolean arrays (sieve of Eratosthenes).

---

## 10. `<algorithm>` Header — Key Functions

| Function | Description | Complexity |
|---|---|---|
| `sort(begin, end)` | ascending sort | O(n log n) |
| `sort(begin, end, greater<int>())` | descending | O(n log n) |
| `sort(begin, end, cmp)` | custom comparator | O(n log n) |
| `stable_sort(begin,end)` | preserves relative order of equal elems | O(n log n) |
| `reverse(begin, end)` | reverse range | O(n) |
| `max_element(begin,end)`, `min_element(...)` | returns iterator | O(n) |
| `accumulate(begin,end,init)` | sum (needs `<numeric>`) | O(n) |
| `count(begin,end,x)` | count occurrences of x | O(n) |
| `find(begin,end,x)` | iterator to x or end() | O(n) |
| `binary_search(begin,end,x)` | true/false, needs sorted range | O(log n) |
| `lower_bound(begin,end,x)` | iterator to first >= x | O(log n) |
| `upper_bound(begin,end,x)` | iterator to first > x | O(log n) |
| `next_permutation(begin,end)` | next lexicographic permutation | O(n) |
| `prev_permutation(begin,end)` | previous permutation | O(n) |
| `unique(begin,end)` | removes consecutive duplicates (use after sort) | O(n) |
| `distance(it1, it2)` | number of elements between iterators | O(1)/O(n) |
| `fill(begin,end,val)` | fill range with val | O(n) |
| `swap(a,b)` | swap two values | O(1) |
| `__gcd(a,b)` | GCD (built-in gnu extension) | O(log min(a,b)) |
| `min(a,b)`, `max(a,b)` | | O(1) |
| `abs(x)` | absolute value | O(1) |
| `is_sorted(begin,end)` | check sorted | O(n) |
| `rotate(begin, mid, end)` | rotate range | O(n) |
| `set_union`, `set_intersection`, `set_difference` | merge sorted ranges | O(n+m) |
| `merge(...)` | merge two sorted ranges | O(n+m) |
| `nth_element(begin, nth, end)` | partial sort, nth element in place | O(n) avg |
| `partial_sort(begin, mid, end)` | sorts only first part | O(n log k) |
| `iota(begin,end,start)` | fill increasing sequence (`<numeric>`) | O(n) |

- **Pattern**:
  - `sort` + two pointers → pair-sum problems, merging intervals.
  - `lower_bound`/`upper_bound` on sorted array → LIS (O(n log n)), search-based binary search problems, counting elements in range.
  - `next_permutation` → generate all permutations without recursion.
  - `nth_element` → Kth largest/smallest without full sort (O(n) avg).
  - `unique` + `erase` → remove duplicates: `v.erase(unique(v.begin(),v.end()), v.end())`.

---

## 11. Iterators — Quick Reference
```cpp
it->first / *it       // dereference
next(it), prev(it)     // move iterator without changing original
advance(it, k)          // move iterator by k
```

---

## 12. Custom Comparator Patterns

```cpp
// Sort pairs by second element ascending
sort(v.begin(), v.end(), [](pair<int,int>&a, pair<int,int>&b){
    return a.second < b.second;
});

// Min-heap using priority_queue
priority_queue<int, vector<int>, greater<int>> minHeap;

// Custom struct comparator for priority_queue
struct cmp {
    bool operator()(pair<int,int>&a, pair<int,int>&b) {
        return a.first > b.first; // min-heap on first
    }
};
priority_queue<pair<int,int>, vector<pair<int,int>>, cmp> pq;
```

---

## 13. Master Pattern-Recognition Table

| Keyword / Signal in Problem | STL Tool to Reach For |
|---|---|
| "find frequency", "count occurrences" | `unordered_map<int,int>` |
| "two sum", "seen before" | `unordered_map` (val→index) |
| "next greater/smaller element" | monotonic `stack` |
| "sliding window maximum/minimum" | monotonic `deque` |
| "kth largest/smallest" | `priority_queue` (heap of size k) or `nth_element` |
| "top K frequent" | `unordered_map` + `priority_queue` |
| "median of stream" | two heaps (max-heap + min-heap) |
| "shortest path weighted graph" | `priority_queue` (Dijkstra) |
| "level order traversal / BFS" | `queue` |
| "DFS with explicit stack" | `stack` |
| "balanced parentheses / valid expression" | `stack` |
| "sorted unique dynamic elements", "range queries lower/upper bound" | `set` |
| "duplicates allowed but sorted", "count of value dynamically" | `multiset` |
| "group anagrams" | `unordered_map<string, vector<string>>` (key = sorted string) |
| "generate all permutations" | `next_permutation` or backtracking |
| "binary search on sorted array/answer" | `lower_bound`/`upper_bound`/`binary_search` |
| "longest increasing subsequence" (optimized) | `lower_bound` in O(n log n) approach |
| "merge intervals / overlapping intervals" | `sort` by start, then linear scan |
| "remove duplicates from sorted array" | `unique` + `erase` |
| "union/intersection of two sorted arrays" | `set_union` / `set_intersection` |
| "graph adjacency representation" | `vector<vector<int>>` or `vector<vector<pair<int,int>>>` (weighted) |
| "subset generation / bitmask DP" | `bitset` or integer bitmasking |
| "LRU cache" | `unordered_map` + `list` (doubly linked list) |
| "check if element exists in O(1)" | `unordered_set` |
| "need sorted iteration + O(log n) insert/delete" | `set`/`map` |
| "prefix sums with counting subarrays" | `unordered_map<prefixSum, count>` |
| "sliding window with distinct-count constraint" | `unordered_map` + two pointers |
| "GCD / number theory quick calc" | `__gcd`, `accumulate` |

---

## 14. Time Complexity Cheat Sheet

| Container | Insert | Delete | Search | Sorted? |
|---|---|---|---|---|
| vector | O(1) amortized (end) | O(n) | O(n) | No |
| set/map | O(log n) | O(log n) | O(log n) | Yes |
| unordered_set/map | O(1) avg | O(1) avg | O(1) avg | No |
| stack/queue | O(1) | O(1) | O(n) | No |
| priority_queue | O(log n) | O(log n) (top) | — | Heap order |
| deque | O(1) both ends | O(1) both ends | O(n) | No |

---

## 15. Quick Tips for Interviews
1. Prefer `unordered_map`/`unordered_set` for average O(1), but mention worst-case O(n) risk (hash collisions) if asked.
2. Use `set`/`map` when order matters or you need `lower_bound`/`upper_bound`.
3. Always `reserve()` vectors if size is known in advance to avoid reallocation overhead.
4. Remember `erase(x)` on `multiset` removes **all** copies — use `erase(find(x))` for one.
5. `priority_queue` default is max-heap — remember `greater<int>` flips it to min-heap.
6. For pairs in heaps, default comparison is lexicographic (`first`, then `second`).
7. `map`'s `operator[]` inserts a default value if key doesn't exist — use `count()` or `find()` for read-only checks.

