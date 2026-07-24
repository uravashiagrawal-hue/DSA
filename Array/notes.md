# Arrays — Pattern Recognition Notes

This file is the master index for array patterns. Detailed deep-dives for Prefix Sum and Sliding Window live in their own files in this folder; everything else is covered here.

---

## How to Recognize Which Pattern to Use

| Signal in problem statement | Pattern | Complexity |
|---|---|---|
| "sorted array" + pair/triplet sum | Two Pointers | O(n) / O(n²) |
| "contiguous subarray/substring" | Sliding Window | O(n) |
| "range sum query", "subarray sum = k" | Prefix Sum | O(n) |
| "max subarray sum" | Kadane's Algorithm | O(n) |
| "sorted array, find element/boundary" | Binary Search | O(log n) |
| "minimize the maximum" / "min such that condition holds" | Binary Search on Answer | O(n log(range)) |
| "rearrange in place with 3 categories" | Dutch National Flag | O(n) |
| "cyclic sort / missing number 1 to n" | Cyclic Sort | O(n) |
| "next greater/smaller element" | Monotonic Stack | O(n) |
| "merge intervals / overlapping ranges" | Sort + Linear Scan | O(n log n) |
| "kth largest/smallest" | Heap or `nth_element` | O(n log k) |
| "duplicate number in array" | Floyd's Cycle Detection or HashSet | O(n) |
| "rotate array" | Reversal Algorithm | O(n) |
| "matrix traversal (spiral, rotate, search)" | Boundary/4-pointer simulation | O(n*m) |
| "product except self" | Prefix * Suffix arrays | O(n) |
| "majority element (> n/2 times)" | Moore's Voting Algorithm | O(n) |

---

## 1. Two Pointers

**Recognize when**: array is sorted (or sortable), and you need to find a pair/triplet/quadruplet satisfying a sum/difference condition, or compare from both ends.

```cpp
int l = 0, r = n - 1;
while (l < r) {
    int sum = a[l] + a[r];
    if (sum == target) { /* found pair */ break; }
    else if (sum < target) l++;
    else r--;
}
```

**Problems**: Two Sum II, 3Sum, 4Sum, Container With Most Water, Trapping Rain Water, Sort Colors (Dutch Flag variant).

---

## 2. Kadane's Algorithm (Max Subarray Sum)

**Recognize when**: "maximum sum contiguous subarray", can include negative numbers.

```cpp
int maxSoFar = a[0], maxEndingHere = a[0];
for (int i = 1; i < n; i++) {
    maxEndingHere = max(a[i], maxEndingHere + a[i]);
    maxSoFar = max(maxSoFar, maxEndingHere);
}
```

**Variants**: max product subarray (track both max and min ending here, since negative * negative = positive), max circular subarray sum (total - min subarray, handle all-negative edge case).

**Problems**: Maximum Subarray, Maximum Product Subarray, Maximum Sum Circular Subarray.

---

## 3. Dutch National Flag (3-way Partitioning)

**Recognize when**: need to sort/segregate an array into exactly 3 categories in a single pass, in place.

```cpp
int low = 0, mid = 0, high = n - 1;
while (mid <= high) {
    if (a[mid] == 0) swap(a[low++], a[mid++]);
    else if (a[mid] == 1) mid++;
    else swap(a[mid], a[high--]);
}
```

**Problems**: Sort Colors, Segregate 0s/1s/2s, partition around a pivot.

---

## 4. Cyclic Sort

**Recognize when**: array contains numbers in range `[1, n]` or `[0, n-1]`, and you need to find missing/duplicate/first-missing-positive in O(n) time O(1) space.

```cpp
int i = 0;
while (i < n) {
    int correct = a[i] - 1;   // if values are 1..n
    if (a[i] != a[correct]) swap(a[i], a[correct]);
    else i++;
}
// second pass: find index where a[i] != i+1 → that's the missing/misplaced number
```

**Problems**: Find Missing Number, Find All Duplicates, First Missing Positive, Find the Duplicate Number.

---

## 5. Moore's Voting Algorithm (Majority Element)

**Recognize when**: "find element appearing more than n/2 times" (or n/3 for the extended version).

```cpp
int count = 0, candidate = -1;
for (int x : a) {
    if (count == 0) candidate = x;
    count += (x == candidate) ? 1 : -1;
}
// verify candidate appears > n/2 times in a second pass
```

**Problems**: Majority Element I & II.

---

## 6. Rotate Array (Reversal Algorithm)

**Recognize when**: "rotate array by k positions" and in-place O(1) space is required.

```cpp
reverse(a.begin(), a.end());
reverse(a.begin(), a.begin() + k);
reverse(a.begin() + k, a.end());
```

**Problems**: Rotate Array, Rotate Matrix (transpose + reverse rows for 90°).

---

## 7. Merge Intervals (Sort + Scan)

**Recognize when**: "overlapping intervals", "merge ranges", "insert interval".

```cpp
sort(intervals.begin(), intervals.end()); // sort by start
vector<vector<int>> merged;
for (auto& iv : intervals) {
    if (!merged.empty() && merged.back()[1] >= iv[0])
        merged.back()[1] = max(merged.back()[1], iv[1]);
    else
        merged.push_back(iv);
}
```

**Problems**: Merge Intervals, Insert Interval, Non-overlapping Intervals, Meeting Rooms I/II.

---

## 8. Product of Array Except Self (Prefix * Suffix)

**Recognize when**: need product/sum of all elements except current, without division, in O(n).

```cpp
vector<int> res(n, 1);
int prefix = 1;
for (int i = 0; i < n; i++) { res[i] = prefix; prefix *= a[i]; }
int suffix = 1;
for (int i = n-1; i >= 0; i--) { res[i] *= suffix; suffix *= a[i]; }
```

---

## 9. Matrix Patterns

**Spiral traversal**: maintain 4 boundaries (top, bottom, left, right), shrink after each side traversed.

**Rotate matrix 90°**: transpose (swap `a[i][j]` with `a[j][i]`), then reverse each row (clockwise) or reverse each column (counter-clockwise).

**Set matrix zeroes**: use first row/column as markers instead of extra space.

**Search a 2D matrix**: if rows and columns are sorted, start from top-right or bottom-left corner and eliminate a row/column each step — O(n + m).

**Problems**: Spiral Matrix, Rotate Image, Set Matrix Zeroes, Search a 2D Matrix.

---

## 10. Binary Search on Arrays

Covered in depth in `Binary_Search/Notes.md`, but the array-specific triggers are:
- Sorted array, find element/first-last occurrence → standard binary search / `lower_bound`/`upper_bound`.
- Rotated sorted array → modified binary search (check which half is sorted).
- "Minimum in rotated sorted array", "search in rotated sorted array".
- Peak element (local maximum) → binary search using slope direction.

---

## 11. When It's NOT a "pure array" pattern

If the array problem mentions:
- **counting subarrays with a sum/XOR condition** → this is really **Prefix Sum + Hashmap** (see `Prefix_Sum.md`).
- **contiguous window with constraint (distinct chars, at most K, sum ≤ target)** → **Sliding Window** (see `Sliding_Window.md`).
- **next greater/smaller element** → this is a **Stack** pattern, not pure array (see `Stack/Notes.md`).
- **kth largest/smallest** → **Heap** pattern (see `Heaps/Notes.md`).

Cross-reference these rather than duplicating — arrays are the *data structure*, but the *technique* often belongs to another pattern family.
