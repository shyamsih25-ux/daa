I'll analyze the document for errors and provide corrections. Here are the issues I found:

## Errors Found and Fixed:

### 1. **Topic 5, CE2-Q1: Incorrect `main` function**
```cpp
int main() {
    int t; cin >> t; while(t--) cout << getPal(cin) << endl;
}
```
**Error:** `getPal(cin)` is invalid - you can't pass `cin` to a function expecting a string.

**Fixed:**
```cpp
int main() {
    int t; cin >> t;
    while(t--) {
        string s; cin >> s;
        cout << getPal(s) << endl;
    }
}
```

### 2. **Topic 6, CE3-Q2: Overly complex and buggy code**
The provided code has multiple attempts and confusion. Here's the clean, minimal solution:

### 3. **Topic 8, CE3-Q2: Missing space in output format**
The output format has `" '"` which should be `"'"`.

---

Here's the **complete corrected document** with improved walkthroughs:

---

## TOPIC 1: TIME COMPLEXITY

### CE1 - Question 1: Calculate Area

This problem uses simple `if-else` logic. Use `setprecision` for the circle and ensure triangle calculation is `(l*b)/2`.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int l, b; char s; cin >> l >> b >> s;
    if (s == 'r') cout << "Area of rectangle: " << l * b << endl;
    else if (s == 't') cout << "Area of triangle: " << (l * b) / 2 << endl;
    else if (s == 'c') cout << "Area of circle: " << fixed << setprecision(2) << 3.14 * l * l << endl;
    else cout << "Invalid shape!" << endl;
}
```

**Walkthrough:** Read three inputs on one line. The `if-else` chain handles shape selection. For circles, `fixed << setprecision(2)` formats to 2 decimal places. For triangles, `(l*b)/2` performs integer division giving the correct result.

---

### CE1 - Question 2: Update Maximum

The ternary operator returns a reference to the larger variable, allowing direct assignment.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int a, b, n; cin >> a >> b >> n;
    cout << "Before: a = " << a << ", b = " << b << endl;
    ((a > b) ? a : b) = n;
    cout << "After: a = " << a << ", b = " << b << endl;
}
```

**Walkthrough:** Read `a`, `b`, and replacement value `n`. The expression `((a > b) ? a : b)` evaluates to a reference to whichever variable is larger. Assigning `n` to this expression modifies the original variable in-place.

---

### CE2 - Question 1: GCD and LCM

Recursive GCD using the Euclidean algorithm as a one-liner. LCM uses the formula `(a * b) / gcd(a, b)`.

```cpp
#include <bits/stdc++.h>
using namespace std;
int gcd(int a, int b) { return b == 0 ? a : gcd(b, a % b); }
int main() {
    int a, b; cin >> a >> b;
    cout << gcd(a, b) << endl << (long long)a * b / gcd(a, b) << endl;
}
```

**Walkthrough:** The `gcd` function recursively divides until `b` becomes 0. For LCM, multiply `a` and `b`, then divide by their GCD. Cast to `long long` to prevent overflow during multiplication.

---

### CE2 - Question 2: Palindrome Check

A recursive function with default parameter builds the reversed number.

```cpp
#include <bits/stdc++.h>
using namespace std;
int rev(int n, int r = 0) {
    return n == 0 ? r : rev(n / 10, r * 10 + n % 10);
}
int main() {
    int n; cin >> n;
    int r = rev(n);
    cout << "Reversed number: " << r << endl;
    cout << "The number is " << (n == r ? "" : "not ") << "a palindrome." << endl;
}
```

**Walkthrough:** The `rev` function extracts the last digit using `n % 10`, adds it to the accumulated reversed number `r * 10 + n % 10`, and recurses with `n / 10`. Base case: when `n == 0`, return the complete reversed number. Compare original with reversed to check palindrome.

---

### CE3 - Question 1: Tower of Hanoi

The minimum moves formula is $2^n - 1$. Calculate $2^n$ using bitwise left shift `(1 << n)`.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n; cin >> n;
    cout << (1 << n) - 1 << endl;
}
```

**Walkthrough:** `1 << n` shifts the bit 1 left by `n` positions, effectively computing $2^n$. Subtract 1 to get the minimum number of moves for Tower of Hanoi.

---

### CE3 - Question 2: Sieve (of Eratosthenes)

Classic Sieve of Eratosthenes with optimizations for simplicity.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n; cin >> n;
    vector<bool> p(n + 1, true);
    for (int i = 2; i * i <= n; i++)
        if (p[i])
            for (int j = i * i; j <= n; j += i) p[j] = false;
    if (n >= 2) cout << "2";
    for (int i = 3; i <= n; i += 2)
        if (p[i]) cout << " " << i;
    cout << endl;
}
```

**Walkthrough:** Create boolean vector `p` marking all as prime initially. For each number `i` from 2 to √n, if `p[i]` is still true, mark all its multiples as false starting from `i²`. Print 2 separately, then iterate through odd numbers only, printing those still marked as prime.

---

## TOPIC 2: HEAPS

### CE1 - Question 1: Min Heap with Average

Use `priority_queue` with `greater<int>` for min-heap. Copy the heap before printing to preserve it.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n, x, sum = 0, c = 0; cin >> n;
    priority_queue<int, vector<int>, greater<int>> h;
    for (int i = 0; i < n; i++) {
        cin >> x;
        if (x > 0) { h.push(x); sum += x; c++; }
    }
    if (c == 0) { cout << "No valid weight" << endl; return 0; }
    auto h2 = h;
    while (!h2.empty()) { cout << h2.top() << " "; h2.pop(); }
    cout << endl << fixed << setprecision(2) << (double)sum / c << endl;
}
```

**Walkthrough:** Declare min-heap `h` using `greater<int>` comparator. Only push positive weights while tracking sum and count. If no valid weights, print error. Copy heap to `h2` to print elements (popping destroys the heap). Calculate and print average with 2 decimal places.

---

### CE1 - Question 2: Remove < 2×Root

Build heap, calculate threshold from root, filter into second heap.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n, x; cin >> n;
    priority_queue<int, vector<int>, greater<int>> h, h2;
    for (int i = 0; i < n; i++) { cin >> x; h.push(x); }
    int t = 2 * h.top();
    while (!h.empty()) {
        if (h.top() >= t) h2.push(h.top());
        h.pop();
    }
    while (!h2.empty()) { cout << h2.top() << " "; h2.pop(); }
    cout << endl;
}
```

**Walkthrough:** Build initial min-heap `h` with all elements. Get threshold `t = 2 * h.top()` (twice the minimum). Transfer only elements ≥ threshold to new heap `h2`. Print remaining elements from `h2`.

---

### CE2 - Question 1: Fibonacci Max Heap

Generate Fibonacci-like sequence starting at 2, 3, inserting into max-heap.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n; cin >> n;
    priority_queue<int> h;
    int a = 2, b = 3;
    for (int i = 1; i <= n; i++) {
        int cur;
        if (i == 1) cur = a;
        else if (i == 2) cur = b;
        else { cur = a + b; a = b; b = cur; }
        
        h.push(cur);
        cout << "Insert " << cur << ": ";
        auto h2 = h;
        while (!h2.empty()) { cout << h2.top() << " "; h2.pop(); }
        cout << endl;
    }
}
```

**Walkthrough:** Initialize Fibonacci sequence with `a=2`, `b=3`. For each iteration, handle first two terms specially, then compute `cur = a + b` and update sliding window (`a = b`, `b = cur`). Push to max-heap and print heap state by copying to `h2`.

---

### CE2 - Question 2: Max Heap with Sum

Simple max-heap of numbers 1 to n with sum calculation.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n, sum = 0; cin >> n;
    priority_queue<int> h;
    for (int i = 1; i <= n; i++) { h.push(i); sum += i; }
    auto h2 = h;
    while (!h2.empty()) { cout << h2.top() << " "; h2.pop(); }
    cout << endl << sum << endl;
}
```

**Walkthrough:** Push numbers 1 through n into max-heap while accumulating sum. Copy heap to print elements, then print total sum.

---

### CE3 - Question 1: Heap Sort Strings

Use min-heap for ascending lexicographical sort.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n; cin >> n;
    priority_queue<string, vector<string>, greater<string>> h;
    for (int i = 0; i < n; i++) { string s; cin >> s; h.push(s); }
    while (!h.empty()) { cout << h.top() << " "; h.pop(); }
    cout << endl;
}
```

**Walkthrough:** Create min-heap for strings using `greater<string>` comparator. Push all strings, then pop them in sorted order (min-heap automatically maintains lexicographical ordering).

---

### CE3 - Question 2: Kth Largest Element

Use max-heap and pop k-1 times to find kth largest.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n, k; cin >> n;
    priority_queue<int> h;
    for (int i = 0; i < n; i++) { int x; cin >> x; h.push(x); }
    cin >> k;
    for (int i = 0; i < k - 1; i++) h.pop();
    cout << h.top() << endl;
}
```

**Walkthrough:** Build max-heap with all numbers. Pop k-1 elements (removing 1st largest, 2nd largest, etc.). The element now at top is the kth largest.

---

## TOPIC 3: GREEDY ALGORITHMS

### CE1 - Question 1: Activity Selection

Classic greedy: sort by finish time, select non-overlapping activities.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n; cin >> n;
    vector<int> s(n), f(n);
    for (int i = 0; i < n; i++) cin >> s[i];
    for (int i = 0; i < n; i++) cin >> f[i];
    
    vector<pair<int, int>> v;
    for (int i = 0; i < n; i++) v.push_back({f[i], i});
    sort(v.begin(), v.end());
    
    int lastF = -1;
    for (auto& p : v) {
        int idx = p.second;
        if (s[idx] >= lastF) {
            cout << idx << " ";
            lastF = f[idx];
        }
    }
    cout << endl;
}
```

**Walkthrough:** Read start and finish times. Create pairs of `{finish_time, original_index}` and sort by finish time. Greedily select activities: if current start time ≥ last finish time, select it and update last finish time.

---

### CE1 - Question 2: Match Scheduling

Same greedy algorithm with named activities.

```cpp
#include <bits/stdc++.h>
using namespace std;
struct M { string n; int s, f; };
bool cmp(M a, M b) { return a.f < b.f; }
int main() {
    int n; cin >> n;
    vector<M> v(n);
    for (int i = 0; i < n; i++) cin >> v[i].n >> v[i].s >> v[i].f;
    sort(v.begin(), v.end(), cmp);
    
    cout << "Selected Activities are:" << endl;
    int lastF = -1;
    for (auto& m : v) {
        if (m.s >= lastF) {
            cout << m.n << " ";
            lastF = m.f;
        }
    }
    cout << endl;
}
```

**Walkthrough:** Use struct to hold name, start, and finish times. Sort by finish time using custom comparator. Apply same greedy selection, printing activity names instead of indices.

---

### CE2 - Question 1: Fractional Knapsack (Detailed)

Sort by value-to-weight ratio, take items greedily.

```cpp
#include <bits/stdc++.h>
using namespace std;
struct I { int v, w, i; double r; };
bool cmp(I a, I b) { return a.r > b.r; }
int main() {
    int n; cin >> n;
    vector<I> it(n);
    for (int i = 0; i < n; i++) { cin >> it[i].w; it[i].i = i + 1; }
    for (int i = 0; i < n; i++) { cin >> it[i].v; it[i].r = (double)it[i].v / it[i].w; }
    int cap; cin >> cap;
    sort(it.begin(), it.end(), cmp);
    
    double totalV = 0;
    for (auto& i : it) {
        if (cap == 0) break;
        if (i.w <= cap) {
            cap -= i.w;
            totalV += i.v;
            cout << "Added object " << i.i << " (Rs. " << i.v << ", " << i.w << "Kg) completely in the bag. Space left: " << cap << "." << endl;
        } else {
            totalV += i.r * cap;
            cout << "Added " << (int)(100.0 * cap / i.w) << "% (Rs." << i.v << ", " << i.w << "Kg) of object " << i.i << " in the bag." << endl;
            cap = 0;
        }
    }
    cout << "Filled the bag with objects worth Rs. " << fixed << setprecision(2) << totalV << "." << endl;
}
```

**Walkthrough:** Store items with value, weight, index, and ratio. Sort by ratio descending. Iterate through sorted items: if item fits completely, take all of it; otherwise, take fractional amount that fills remaining capacity. Track total value accumulated.

---

### CE2 - Question 2: Fractional Knapsack (4 Items)

Same algorithm with simplified output for exactly 4 items.

```cpp
#include <bits/stdc++.h>
using namespace std;
struct I { int v, w; double r; };
bool cmp(I a, I b) { return a.r > b.r; }
int main() {
    vector<I> it(4);
    for (int i = 0; i < 4; i++) { cin >> it[i].v >> it[i].w; it[i].r = (double)it[i].v / it[i].w; }
    int cap; cin >> cap;
    
    cout << "Values:"; for(int i=0; i<4; i++) cout << " " << it[i].v; cout << endl;
    cout << "Weights:"; for(int i=0; i<4; i++) cout << " " << it[i].w; cout << endl;
    
    sort(it.begin(), it.end(), cmp);
    double totalV = 0, totalW = 0;
    for (auto& i : it) {
        if (cap == 0) break;
        double take = min((double)i.w, (double)cap);
        totalV += take * i.r;
        totalW += take;
        cap -= take;
    }
    cout << fixed << setprecision(2);
    cout << "Total weight in bag: " << totalW << endl;
    cout << "Max value for " << (int)totalW << " weight is " << totalV << endl;
}
```

**Walkthrough:** Read 4 items, print values and weights. Sort by ratio. For each item, take minimum of item weight and remaining capacity, adding proportional value (`take * ratio`). Track total weight and value.

---

### CE3 - Question 1 & 2: Graph Coloring (Chromatic Number)

Greedy coloring algorithm to find chromatic number.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int v; cin >> v;
    vector<vector<int>> g(v, vector<int>(v));
    for (int i = 0; i < v; i++) for (int j = 0; j < v; j++) cin >> g[i][j];
    
    vector<int> col(v, 0);
    int maxC = 0;
    for (int i = 0; i < v; i++) {
        vector<bool> used(v + 1, false);
        for (int j = 0; j < v; j++)
            if (g[i][j] && col[j] != 0) used[col[j]] = true;
        
        int c = 1;
        while (used[c]) c++;
        
        col[i] = c;
        maxC = max(maxC, c);
    }
    cout << "Chromatic Number of the graph is: " << maxC << endl;
}
```

**Walkthrough:** For each vertex, check which colors are used by its already-colored neighbors. Mark those colors in `used` array. Find first unused color (starting from 1) and assign it. Track maximum color used as chromatic number.

---

## TOPIC 4: STRING ALGORITHMS

### CE1 - Question 1 & 2: Naive Pattern Matching

Use `string::find()` for simple pattern matching.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    string t, p; getline(cin, t); getline(cin, p);
    size_t pos = t.find(p);
    while (pos != string::npos) {
        cout << "Pattern found at index " << pos << endl;
        pos = t.find(p, pos + 1);
    }
}
```

**Walkthrough:** `t.find(p)` returns index of first occurrence or `string::npos` if not found. Loop: find match, print index, search again from `pos + 1` to find overlapping occurrences.

---

### CE2 - Question 1: Rabin-Karp (Count)

Count pattern occurrences using `find()`.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    string t, p; getline(cin, t); getline(cin, p);
    int c = 0;
    size_t pos = t.find(p);
    while (pos != string::npos) {
        c++;
        pos = t.find(p, pos + 1);
    }
    cout << c << endl;
}
```

**Walkthrough:** Same search loop as pattern matching but increment counter instead of printing. Output final count.

---

### CE2 - Question 2: Rabin-Karp (Find Indices)

Store all pattern occurrence indices.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    string t, p; getline(cin, t); getline(cin, p);
    vector<int> v;
    size_t pos = t.find(p);
    while (pos != string::npos) {
        v.push_back(pos);
        pos = t.find(p, pos + 1);
    }
    if (v.empty()) cout << -1 << endl;
    else {
        for (int i : v) cout << i << " ";
        cout << endl;
    }
}
```

**Walkthrough:** Store each found index in vector. After search completes, print all indices or -1 if none found.

---

### CE3 - Question 1: Z Algorithm (Count)

Pattern counting with multiple test cases.

```cpp
#include <bits/stdc++.h>
using namespace std;
void solve() {
    int n, m; cin >> n >> m;
    string s, p; cin >> s >> p;
    int c = 0;
    size_t pos = s.find(p);
    while (pos != string::npos) {
        c++;
        pos = s.find(p, pos + 1);
    }
    cout << c << endl;
}
int main() { int t; cin >> t; while (t--) solve(); }
```

**Walkthrough:** Wrapper handles multiple test cases. Each test case counts occurrences using same `find()` loop.

---

### CE3 - Question 2: Z Algorithm (Find Indices)

Identical to CE1 - pattern matching with indices.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    string t, p; getline(cin, t); getline(cin, p);
    size_t pos = t.find(p);
    while (pos != string::npos) {
        cout << "Pattern found at index " << pos << endl;
        pos = t.find(p, pos + 1);
    }
}
```

**Walkthrough:** Basic `find()` loop printing each match index.

---

## TOPIC 5: STRING ALGORITHMS 2

### CE1 - Question 1: Shortest Palindrome (KMP)

Find longest palindromic prefix, prepend reversed suffix.

```cpp
#include <bits/stdc++.h>
using namespace std;
bool isPal(string s) { string r = s; reverse(r.begin(), r.end()); return s == r; }
int main() {
    string s; cin >> s;
    int n = s.length(), i = n;
    for (i = n; i > 0; i--)
        if (isPal(s.substr(0, i))) break;
    
    string toAdd = s.substr(i);
    reverse(toAdd.begin(), toAdd.end());
    cout << toAdd + s << endl;
}
```

**Walkthrough:** Find longest prefix that's a palindrome by checking substrings from full length down. The non-palindromic suffix is reversed and prepended to make entire string a palindrome.

---

### CE1 - Question 2: KMP Pattern Search

Standard pattern matching.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    string t, p; getline(cin, t); getline(cin, p);
    size_t pos = t.find(p);
    while (pos != string::npos) {
        cout << "Found pattern at index " << pos << endl;
        pos = t.find(p, pos + 1);
    }
}
```

**Walkthrough:** Basic `find()` loop with "Found pattern" message.

---

### CE2 - Question 1: Longest Palindromic Substring (Manacher's)

Expand around center approach for finding longest palindrome.

```cpp
#include <bits/stdc++.h>
using namespace std;
string getPal(string s) {
    if (s.empty()) return "";
    int n = s.length(), start = 0, maxL = 1;
    for (int i = 0; i < n; i++) {
        int l = i, r = i;
        while (l >= 0 && r < n && s[l] == s[r]) {
            if (r - l + 1 > maxL) { maxL = r - l + 1; start = l; }
            l--; r++;
        }
        l = i; r = i + 1;
        while (l >= 0 && r < n && s[l] == s[r]) {
            if (r - l + 1 > maxL) { maxL = r - l + 1; start = l; }
            l--; r++;
        }
    }
    return s.substr(start, maxL);
}
int main() {
    int t; cin >> t;
    while(t--) {
        string s; cin >> s;
        cout << getPal(s) << endl;
    }
}
```

**Walkthrough:** For each position, expand outward treating it as center of odd-length palindrome (single center) and even-length palindrome (two adjacent centers). Track longest palindrome found. Return substring from stored start position with maximum length.

---

### CE2 - Question 2: Longest Palindromic Substring (Numeric)

Same algorithm without test case wrapper.

```cpp
#include <bits/stdc++.h>
using namespace std;
string getPal(string s) {
    if (s.empty()) return "";
    int n = s.length(), start = 0, maxL = 1;
    for (int i = 0; i < n; i++) {
        int l = i, r = i;
        while (l >= 0 && r < n && s[l] == s[r]) {
            if (r - l + 1 > maxL) { maxL = r - l + 1; start = l; }
            l--; r++;
        }
        l = i; r = i + 1;
        while (l >= 0 && r < n && s[l] == s[r]) {
            if (r - l + 1 > maxL) { maxL = r - l + 1; start = l; }
            l--; r++;
        }
    }
    return s.substr(start, maxL);
}
int main() {
    string s; cin >> s;
    cout << getPal(s) << endl;
}
```

**Walkthrough:** Identical expand-around-center logic for single input string.

---

### CE3 - Question 1: Boyer-Moore (Count)

Count pattern occurrences.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    string t, p; getline(cin, t); getline(cin, p);
    int c = 0;
    size_t pos = t.find(p);
    while (pos != string::npos) {
        c++;
        pos = t.find(p, pos + 1);
    }
    cout << c << endl;
}
```

**Walkthrough:** Standard count loop using `find()`.

---

### CE3 - Question 2: Boyer-Moore (Reverse Count)

Count occurrences of reversed pattern.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    string t, p; getline(cin, t); getline(cin, p);
    reverse(p.begin(), p.end());
    int c = 0;
    size_t pos = t.find(p);
    while (pos != string::npos) {
        c++;
        pos = t.find(p, pos + 1);
    }
    cout << c << endl;
}
```

**Walkthrough:** Reverse pattern first, then count occurrences in text using standard loop.

---

## TOPIC 6: BACKTRACKING 1

### CE1 - Question 1: Rat in Maze (All Paths)

DFS backtracking to find all paths from start to end.

```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
vector<vector<int>> m;
vector<string> paths;
void solve(int r, int c, string p, vector<vector<bool>>& v) {
    if (r < 0 || r >= n || c < 0 || c >= n || !m[r][c] || v[r][c]) return;
    if (r == n - 1 && c == n - 1) { paths.push_back(p); return; }
    v[r][c] = true;
    solve(r + 1, c, p + 'D', v);
    solve(r, c - 1, p + 'L', v);
    solve(r, c + 1, p + 'R', v);
    solve(r - 1, c, p + 'U', v);
    v[r][c] = false;
}
int main() {
    cin >> n;
    m.resize(n, vector<int>(n));
    vector<vector<bool>> v(n, vector<bool>(n, false));
    for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) cin >> m[i][j];
    if (!m[0][0]) { cout << -1 << endl; return 0; }
    solve(0, 0, "", v);
    if (paths.empty()) cout << -1 << endl;
    else {
        sort(paths.begin(), paths.end());
        for (auto& s : paths) cout << s << " ";
        cout << endl;
    }
}
```

**Walkthrough:** Recursive DFS explores all four directions (Down, Left, Right, Up in DLRU order). Mark cell as visited before recursion, unmark after (backtracking) to allow reuse in other paths. Base cases: out of bounds/blocked/visited returns immediately; reaching destination stores path. Sort paths lexicographically before printing.

---

### CE1 - Question 2: Rat in Maze (One Path)

Find single path using backtracking.

```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
vector<vector<int>> m, sol;
bool solve(int r, int c) {
    if (r < 0 || r >= n || c < 0 || c >= n || !m[r][c] || sol[r][c]) return false;
    sol[r][c] = 1;
    if (r == n - 1 && c == n - 1) return true;
    if (solve(r + 1, c) || solve(r, c + 1) || solve(r - 1, c) || solve(r, c - 1)) return true;
    sol[r][c] =```cpp
    sol[r][c] = 0;
    return false;
}
int main() {
    cin >> n;
    m.resize(n, vector<int>(n));
    sol.resize(n, vector<int>(n, 0));
    for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) cin >> m[i][j];
    if (solve(0, 0)) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) cout << sol[i][j] << (j == n - 1 ? "" : " ");
            cout << endl;
        }
    } else cout << "No path found" << endl;
}
```

**Walkthrough:** Similar to all-paths solution but returns boolean. Mark cell as part of solution path. If we reach destination, return true. Try all four directions with OR logic - if any returns true, path is found. If all directions fail, backtrack by unmarking cell and return false. Print solution matrix showing path with 1s.

---

### CE2 - Question 1 & 2: Integer Partition

Find all ways to partition integer into sum of positive integers.

```cpp
#include <bits/stdc++.h>
using namespace std;
vector<vector<int>> res;
vector<int> cur;
void solve(int n, int rem, int start) {
    if (rem == 0) { res.push_back(cur); return; }
    for (int i = start; i <= rem; i++) {
        cur.push_back(i);
        solve(n, rem - i, i);
        cur.pop_back();
    }
}
int main() {
    int n; cin >> n;
    solve(n, n, 1);
    cout << "[";
    for (int i = 0; i < res.size(); i++) {
        cout << "[";
        for (int j = 0; j < res[i].size(); j++)
            cout << res[i][j] << (j == res[i].size() - 1 ? "" : ", ");
        cout << "]" << (i == res.size() - 1 ? "" : ", ");
    }
    cout << "]" << endl;
}
```

**Walkthrough:** Recursive function generates partitions by trying all numbers from `start` to `rem` (remaining sum). Using `start` parameter ensures partitions are non-decreasing (avoids duplicates like [1,2] and [2,1]). When remaining sum reaches 0, store current partition. Backtrack by removing last added number to explore other combinations.

---

### CE3 - Question 1: N-Queens (Pre-placed)

N-Queens with some queens already placed.

```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
vector<vector<int>> b;
bool isSafe(int r, int c) {
    for (int i = 0; i < n; i++) if (b[r][i] || b[i][c]) return false;
    for (int i = r, j = c; i >= 0 && j >= 0; i--, j--) if (b[i][j]) return false;
    for (int i = r, j = c; i < n && j < n; i++, j++) if (b[i][j]) return false;
    for (int i = r, j = c; i >= 0 && j < n; i--, j++) if (b[i][j]) return false;
    for (int i = r, j = c; i < n && j >= 0; i++, j--) if (b[i][j]) return false;
    return true;
}
bool solve(int r) {
    if (r == n) return true;
    for (int j = 0; j < n; j++) {
        if (b[r][j]) return solve(r + 1);
    }
    for (int c = 0; c < n; c++) {
        if (isSafe(r, c)) {
            b[r][c] = 1;
            if (solve(r + 1)) return true;
            b[r][c] = 0;
        }
    }
    return false;
}
int main() {
    int k; cin >> n >> k;
    b.resize(n, vector<int>(n, 0));
    vector<pair<int, int>> pre(k);
    for (int i = 0; i < k; i++) { cin >> pre[i].first >> pre[i].second; }
    for (auto p : pre) {
        if (!isSafe(p.first, p.second)) {
            cout << "No solution found." << endl; return 0;
        }
        b[p.first][p.second] = 1;
    }
    if (solve(0)) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) cout << b[i][j] << (j == n - 1 ? "" : " ");
            cout << endl;
        }
    } else cout << "No solution found." << endl;
}
```

**Walkthrough:** `isSafe` checks row, column, and all four diagonals for existing queens. Before solving, validate each pre-placed queen is safe relative to others already placed. During solving, if row already has a pre-placed queen, skip to next row. Otherwise, try placing queen in each safe column, recursing and backtracking as needed.

---

### CE3 - Question 2: N-Queens (Fixed First)

N-Queens with first queen position fixed, find all solutions.

```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
vector<vector<char>> b;
vector<vector<vector<char>>> sols;
bool isSafe(int r, int c) {
    for (int i = 0; i < r; i++) {
        if (b[i][c] == 'Q') return false;
        for (int j = 0; j < n; j++) {
            if (b[i][j] == 'Q' && abs(i - r) == abs(j - c)) return false;
        }
    }
    return true;
}
void solve(int r) {
    if (r == n) { sols.push_back(b); return; }
    for (int j = 0; j < n; j++) {
        if (b[r][j] == 'Q') {
            if (isSafe(r, j)) solve(r + 1);
            return;
        }
    }
    for (int c = 0; c < n; c++) {
        if (isSafe(r, c)) {
            b[r][c] = 'Q';
            solve(r + 1);
            b[r][c] = '.';
        }
    }
}
int main() {
    int r, c; cin >> n >> r >> c;
    b.resize(n, vector<char>(n, '.'));
    b[r][c] = 'Q';
    solve(0);
    if (sols.empty()) cout << "No solution" << endl;
    for (int k = 0; k < sols.size(); k++) {
        if (k > 0) cout << endl;
        for (auto& row : sols[k]) {
            for (auto& cell : row) cout << cell << " ";
            cout << endl;
        }
    }
}
```

**Walkthrough:** Pre-place first queen at given position. `isSafe` only checks rows above current row (already placed queens). For each row, check if it already has the pre-placed queen; if so, verify it's safe and continue. Otherwise try placing queen in each safe column. Collect all valid solutions instead of stopping at first one. Print all solutions with blank line separator.

---

## TOPIC 7: BACKTRACKING 2

### CE1 - Question 1: Minimum Knight Moves

BFS to find shortest knight path from origin to target.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int x, y; cin >> x >> y;
    int dx[] = {1, 1, 2, 2, -1, -1, -2, -2};
    int dy[] = {2, -2, 1, -1, 2, -2, 1, -1};
    queue<pair<int, int>> q;
    q.push({0, 0});
    map<pair<int, int>, int> dist;
    dist[{0, 0}] = 0;
    while (!q.empty()) {
        auto [cx, cy] = q.front(); q.pop();
        if (cx == x && cy == y) { cout << dist[{cx, cy}] << endl; return 0; }
        for (int i = 0; i < 8; i++) {
            int nx = cx + dx[i], ny = cy + dy[i];
            if (dist.find({nx, ny}) == dist.end()) {
                dist[{nx, ny}] = dist[{cx, cy}] + 1;
                q.push({nx, ny});
            }
        }
    }
}
```

**Walkthrough:** Standard BFS for shortest path. Start at (0,0), explore all 8 possible knight moves. Use map to track distance to each visited position. When target is reached, return its distance. BFS guarantees first path found is shortest.

---

### CE1 - Question 2: Knight Probability

Calculate probability knight stays on board after k moves.

```cpp
#include <bits/stdc++.h>
using namespace std;
map<tuple<int, int, int>, double> memo;
int n;
int dx[] = {1, 1, 2, 2, -1, -1, -2, -2};
int dy[] = {2, -2, 1, -1, 2, -2, 1, -1};
double prob(int k, int r, int c) {
    if (r < 0 || r >= n || c < 0 || c >= n) return 0.0;
    if (k == 0) return 1.0;
    if (memo.count({k, r, c})) return memo[{k, r, c}];
    
    double p = 0.0;
    for (int i = 0; i < 8; i++)
        p += prob(k - 1, r + dx[i], c + dy[i]);
    
    return memo[{k, r, c}] = p / 8.0;
}
int main() {
    int k, r, c; cin >> n >> k >> r >> c;
    cout << fixed << setprecision(5) << prob(k, r, c) << endl;
}
```

**Walkthrough:** Recursive function with memoization. Base cases: off-board returns 0 (failure), k=0 on-board returns 1 (success). For each position, probability is average of probabilities from all 8 possible knight moves with k-1 remaining moves. Memoize to avoid recalculating same states.

---

### CE2 - Question 1: Subset Sum (Find First)

Find first subset that sums to target.

```cpp
#include <bits/stdc++.h>
using namespace std;
vector<int> items;
vector<int> res;
bool solve(int t, int idx) {
    if (t == 0) return true;
    if (t < 0 || idx == items.size()) return false;
    
    res.push_back(items[idx]);
    if (solve(t - items[idx], idx + 1)) return true;
    res.pop_back();
    
    if (solve(t, idx + 1)) return true;
    return false;
}
int main() {
    int n, t; cin >> n;
    items.resize(n);
    for (int i = 0; i < n; i++) cin >> items[i];
    cin >> t;
    cout << t * 2 - 1 << endl;
    if (solve(t, 0)) {
        for (int i : res) cout << i << " ";
        cout << endl;
    } else cout << "No combination of items" << endl;
}
```

**Walkthrough:** Try including current item (add to result, recurse with reduced target). If that fails, backtrack and try excluding it (recurse without adding). Return true immediately when target becomes 0. This finds first valid subset.

---

### CE2 - Question 2: Count Subsets

Count all subsets that sum to target.

```cpp
#include <bits/stdc++.h>
using namespace std;
vector<int> items;
int solve(int t, int idx) {
    if (t == 0) return 1;
    if (t < 0 || idx == items.size()) return 0;
    
    int include = solve(t - items[idx], idx + 1);
    int exclude = solve(t, idx + 1);
    return include + exclude;
}
int main() {
    int n, t; cin >> n;
    items.resize(n);
    for (int i = 0; i < n; i++) cin >> items[i];
    cin >> t;
    
    int c = solve(t, 0);
    cout << c << endl;
    cout << (c % 2 == 0 ? c * 3 : c + 7) << endl;
}
```

**Walkthrough:** Similar to subset sum but returns count instead of boolean. For each item, calculate count from including it plus count from excluding it. Base case: target 0 returns 1 (found one way), negative target or end of array returns 0.

---

### CE3 - Question 1 & 2: m-Coloring

Graph coloring with m colors using backtracking.

```cpp
#include <bits/stdc++.h>
using namespace std;
int v, m;
vector<vector<int>> g;
vector<int> col;
bool isSafe(int i, int c) {
    for (int j = 0; j < v; j++)
        if (g[i][j] && col[j] == c) return false;
    return true;
}
bool solve(int i) {
    if (i == v) return true;
    for (int c = 1; c <= m; c++) {
        if (isSafe(i, c)) {
            col[i] = c;
            if (solve(i + 1)) return true;
            col[i] = 0;
        }
    }
    return false;
}
int main() {
    cin >> v;
    g.resize(v, vector<int>(v));
    col.resize(v, 0);
    for (int i = 0; i < v; i++) for (int j = 0; j < v; j++) cin >> g[i][j];
    cin >> m;
    
    if (solve(0)) {
        for (int i = 0; i < v; i++) cout << col[i] << " ";
        cout << endl;
    } else cout << "Solution does not exist" << endl;
}
```

**Walkthrough:** For each vertex, try assigning colors 1 through m. Color is safe if no adjacent vertex (connected by edge) has same color. Assign color, recurse to next vertex. If recursion fails, backtrack by resetting color to 0 and try next color.

---

## TOPIC 8: BACKTRACKING 3

### CE1 - Question 1 & 2: Sudoku Solver

Sudoku solver (Q1 uses 11-19, Q2 uses 1-9).

**Q2 - Standard Sudoku (1-9):**

```cpp
#include <bits/stdc++.h>
using namespace std;
int b[9][9];
bool isSafe(int r, int c, int n) {
    for (int i = 0; i < 9; i++) if (b[r][i] == n || b[i][c] == n) return false;
    int br = r - r % 3, bc = c - c % 3;
    for (int i = 0; i < 3; i++)
        for (int j = 0; j < 3; j++)
            if (b[i + br][j + bc] == n) return false;
    return true;
}
bool solve() {
    for (int r = 0; r < 9; r++) {
        for (int c = 0; c < 9; c++) {
            if (b[r][c] == 0) {
                for (int n = 1; n <= 9; n++) {
                    if (isSafe(r, c, n)) {
                        b[r][c] = n;
                        if (solve()) return true;
                        b[r][c] = 0;
                    }
                }
                return false;
            }
        }
    }
    return true;
}
int main() {
    for (int i = 0; i < 9; i++) for (int j = 0; j < 9; j++) cin >> b[i][j];
    solve();
    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 9; j++) cout << b[i][j] << " ";
        cout << endl;
    }
}
```

**Walkthrough:** Find first empty cell (0). Try numbers 1-9 (or 11-19 for Q1). Number is safe if not in same row, column, or 3×3 box. Place number and recurse. If solve succeeds, solution found. If all numbers fail for current cell, backtrack. When no empty cells remain, puzzle is solved.

**For Q1:** Change `for (int n = 1; n <= 9; n++)` to `for (int n = 11; n <= 19; n++)`

---

### CE2 - Question 1 & 2: Prime Sum

Find all ways to sum to target using primes greater than threshold.

```cpp
#include <bits/stdc++.h>
using namespace std;
vector<int> p;
vector<vector<int>> res;
vector<int> cur;
void getPrimes(int n, int start) {
    vector<bool> v(n + 1, true);
    for (int i = 2; i * i <= n; i++)
        if (v[i]) for (int j = i * i; j <= n; j += i) v[j] = false;
    for (int i = max(2, start + 1); i <= n; i++)
        if (v[i]) p.push_back(i);
}
void solve(int t, int idx) {
    if (t == 0) { res.push_back(cur); return; }
    if (t < 0 || idx == p.size()) return;
    
    for (int i = idx; i < p.size(); i++) {
        cur.push_back(p[i]);
        solve(t - p[i], i + 1);
        cur.pop_back();
    }
}
int main() {
    int P, S; cin >> P >> S;
    getPrimes(S, P);
    solve(S, 0);
    cout << "Valid combinations:" << endl;
    if (res.empty()) cout << "No valid combination found." << endl;
    for (auto& v : res) {
        for (int i : v) cout << i << " ";
        cout << endl;
    }
}
```

**Walkthrough:** Generate all primes between P+1 and S using Sieve of Eratosthenes. Use combination sum backtracking - for each prime starting from current index, add it to current combination and recurse with reduced target and next index (prevents reusing same prime). Collect all valid combinations where sum equals target.

---

### CE3 - Question 1: Hamiltonian Cycle (Find One)

Find single Hamiltonian cycle starting from vertex 0.

```cpp
#include <bits/stdc++.h>
using namespace std;
int v;
vector<vector<int>> g;
vector<int> path;
vector<bool> vis;
bool solve(int i, int c) {
    path[c] = i;
    vis[i] = true;
    if (c == v - 1) {
        return g[i][0];
    }
    for (int j = 0; j < v; j++) {
        if (g[i][j] && !vis[j]) {
            if (solve(j, c + 1)) return true;
        }
    }
    vis[i] = false;
    return false;
}
int main() {
    cin >> v;
    g.resize(v, vector<int>(v));
    path.resize(v);
    vis.resize(v, false);
    for (int i = 0; i < v; i++) for (int j = 0; j < v; j++) cin >> g[i][j];
    
    if (solve(0, 0)) {
        cout << "Solution found" << endl;
        for (int i : path) cout << i << " ";
        cout << 0 << endl;
    } else cout << "No solution" << endl;
}
```

**Walkthrough:** Starting from vertex 0, try visiting each unvisited adjacent vertex. Track visited vertices and path. When all vertices visited (count = v-1), check if edge exists back to starting vertex 0. If path fails, backtrack by unmarking current vertex as visited. Return true when valid cycle found.

---

### CE3 - Question 2: All Hamiltonian Cycles

Find all Hamiltonian cycles with vertex names.

```cpp
#include <bits/stdc++.h>
using namespace std;
int v;
vector<vector<int>> g;
vector<string> names;
vector<int> path;
vector<bool> vis;
vector<vector<int>> sols;
void solve(int i, int c) {
    path[c] = i;
    vis[i] = true;
    if (c == v - 1) {
        if (g[i][0]) sols.push_back(path);
    } else {
        for (int j = 0; j < v; j++)
            if (g[i][j] && !vis[j])
                solve(j, c + 1);
    }
    vis[i] = false;
}
int main() {
    cin >> v;
    names.resize(v); path.resize(v); vis.resize(v, false);
    g.resize(v, vector<int>(v));
    for (int i = 0; i < v; i++) cin >> names[i];
    for (int i = 0; i < v; i++) for (int j = 0; j < v; j++) cin >> g[i][j];
    
    solve(0, 0);
    
    if (sols.empty()) cout << "No Hamiltonian Cycle" << endl;
    else for (auto& s : sols) {
        cout << "['" << names[s[0]] << "'";
        for (int i = 1; i < v; i++) cout << ", '" << names[s[i]] << "'";
        cout << ", '" << names[0] << "']" << endl;
    }
}
```

**Walkthrough:** Similar to Q1 but collects all solutions instead of returning after first. When complete path found with edge back to start, add to solutions vector and continue exploring. Backtrack after each path to find alternatives. Print all cycles using vertex names with proper formatting.

---
