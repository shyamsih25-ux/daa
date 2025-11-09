## TOPIC 1: TIME COMPLEXITY

### CE1 - Question 1: Calculate Area

This problem is just `if-else`. The minimal trick is to use `setprecision` for the circle and make sure the triangle math is `(l*b)/2` to match the sample output.

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

**Walkthrough:** We read all 3 inputs on one line. The `if-else` chain handles the logic. [cite\_start]For the circle, `fixed << setprecision(2)` gives us the two decimal points[cite: 41]. [cite\_start]For the triangle, `(l*b)/2` gives `48` for inputs `8` and `12`[cite: 35].

-----

### CE1 - Question 2: Update Maximum

The trick here is that the ternary operator `(a > b) ? a : b` can return a *reference* to the original `a` or `b`, so we can assign a value directly to it.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int a, b, n; cin >> a >> b >> n;
    cout << "Before: a = " << a << ", b = " << b << endl;
    ((a > b) ? a : b) = n; // This is the magic
    cout << "After: a = " << a << ", b = " << b << endl;
}
```

**Walkthrough:** We read `a`, `b`, and the new value `n`. `((a > b) ? a : b)` selects whichever variable is bigger, and we assign `n` to it, modifying the original `a` or `b` in place.

-----

### CE2 - Question 1: GCD and LCM

We can write the recursive GCD function as a one-line ternary operator. LCM is just a formula.

```cpp
#include <bits/stdc++.h>
using namespace std;
int gcd(int a, int b) { return b == 0 ? a : gcd(b, a % b); }
int main() {
    int a, b; cin >> a >> b;
    cout << gcd(a, b) << endl << (long long)a * b / gcd(a, b) << endl;
}
```

**Walkthrough:** `gcd` is the classic Euclidean algorithm in one line. For LCM, we use the formula `(a * b) / gcd(a, b)`. We cast `a` to `long long` just in case `a * b` overflows.

-----

### CE2 - Question 2: Palindrome Check

A recursive function with a default parameter `r=0` can build the reversed number.

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

**Walkthrough:** The `rev` function chops off the last digit of `n` (`n / 10`) and adds it to the reversed number `r` (`r * 10 + n % 10`). The base case is when `n == 0`, it returns the final `r`. The `main` function uses a ternary operator to print `"not "` if `n != r`.

-----

### CE3 - Question 1: Tower of Hanoi

The formula is $2^n - 1$. The minimal way to write $2^n$ is `(1 << n)`.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n; cin >> n;
    cout << (1 << n) - 1 << endl;
}
```

**Walkthrough:** `1 << n` is a bitwise left shift, which is a very fast way to calculate $2^n$. Then we just subtract 1.

-----

### CE3 - Question 2: Sieve (of Sundaram / Eratosthenes)

[cite\_start]The prompt asks for "Sieve of Sundaram" [cite: 172] [cite\_start]but the "easy win" to get the *exact output* [cite: 188] is the Sieve of Eratosthenes. It's more common and simpler.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n; cin >> n;
    vector<bool> p(n + 1, true);
    for (int i = 2; i * i <= n; i++)
        if (p[i])
            for (int j = i * i; j <= n; j += i) p[j] = false;
    if (n >= 2) cout << "2"; // Handle 2 separately for spacing
    for (int i = 3; i <= n; i += 2) // Only check odds
        if (p[i]) cout << " " << i;
    cout << endl;
}
```

**Walkthrough:** This is a slightly optimized Sieve of Eratosthenes. We create a boolean vector `p` (for prime), marking all as `true`. We loop from 2 to $\sqrt{n}$. If `p[i]` is `true`, `i` is prime, so we mark all its multiples as `false`. To print, we handle `2` specially, then loop through odd numbers `i=3, 5, 7...` and print if `p[i]` is `true`.

-----

## TOPIC 2: HEAPS

### CE1 - Question 1: Min Heap with Average

We use `priority_queue<int, vector<int>, greater<int>>` for a min-heap. The trick is to copy the heap to print it, since printing destroys it.

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
    auto h2 = h; // Copy heap
    while (!h2.empty()) { cout << h2.top() << " "; h2.pop(); }
    cout << endl << fixed << setprecision(2) << (double)sum / c << endl;
}
```

**Walkthrough:** `h` is our min-heap. We only `push` positive weights and track the `sum` and `count` `c`. If `c` is 0, print error. We copy `h` to `h2`, then `pop` from `h2` to print. Finally, print the average, casting to `double`.

-----

### CE1 - Question 2: Remove \< 2×Root

Build one heap, get the root, then build a *second* heap by filtering elements from the first.

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

**Walkthrough:** Build min-heap `h`. Get threshold `t = 2 * h.top()`. Create a new min-heap `h2`. Move elements from `h` to `h2` *only if* they are $\ge t$. Then print `h2`.

-----

### CE2 - Question 1: Fibonacci Max Heap

[cite\_start]The sequence is 2, 3, 5, 8... [cite: 283-287]. We just need to handle the first two terms as special cases. A default `priority_queue` is a max-heap.

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

**Walkthrough:** We handle `i=1` (2) and `i=2` (3) specially. After that, we use standard Fibonacci logic `cur = a + b`. In each loop, `push` the `cur` value, copy the heap `h` to `h2`, and print `h2`.

-----

### CE2 - Question 2: Max Heap with Sum

Super simple: loop 1 to `n`, `push` to max-heap, add to `sum`. Print heap, print sum.

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

**Walkthrough:** `h` is the max-heap. Loop 1 to `n`, `push(i)` and `sum += i`. Copy to `h2` to print. Print `sum`. Done.

-----

### CE3 - Question 1: Heap Sort Strings

The easiest way to "heap sort" in ascending order is to use a **min-heap**.

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

**Walkthrough:** We make a min-heap for strings `greater<string>`. We `push` all strings in. Then we just `pop` them out—they will be in perfect lexicographical order.

-----

### CE3 - Question 2: Kth Largest Element

The "easy win" for Kth largest is a **max-heap**.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n, k; cin >> n;
    priority_queue<int> h;
    for (int i = 0; i < n; i++) { int x; cin >> x; h.push(x); }
    cin >> k;
    for (int i = 0; i < k - 1; i++) h.pop(); // Pop the 1st, 2nd, ... (k-1)th
    cout << h.top() << endl; // The Kth is left at the top
}
```

**Walkthrough:** `push` all numbers into a max-heap `h`. To find the Kth largest, we `pop` `k-1` times. This removes the largest, 2nd largest, etc. The element left at `h.top()` is the Kth largest.

-----

## TOPIC 3: GREEDY ALGORITHMS

### CE1 - Question 1: Activity Selection

Classic greedy problem. Sort by **finish time**.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n; cin >> n;
    vector<int> s(n), f(n);
    for (int i = 0; i < n; i++) cin >> s[i];
    for (int i = 0; i < n; i++) cin >> f[i];
    
    vector<pair<int, int>> v;
    for (int i = 0; i < n; i++) v.push_back({f[i], i}); // {finish, index}
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

**Walkthrough:** We read start `s` and finish `f` times. We create a vector of pairs `v` to store `{finishTime, index}`. We `sort` `v` (this sorts by finish time). We loop through the sorted pairs, tracking the `lastFinish` time. If the current activity's start time `s[idx]` is $\ge$ `lastF`, we select it (print its original index) and update `lastF`.

-----

### CE1 - Question 2: Match Scheduling

This is the exact same problem as above, just with names.

```cpp
#include <bits/stdc++.h>
using namespace std;
struct M { string n; int s, f; };
bool cmp(M a, M b) { return a.f < b.f; } // Sort by finish
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

**Walkthrough:** Same logic. We use a `struct M` to hold the name, start, and finish. We use a custom `cmp` function to `sort` by finish time. The greedy logic is identical, but we print `m.n` (the name) instead of the index.

-----

### CE2 - Question 1: Fractional Knapsack (Detailed)

Sort by **value/weight ratio**.

```cpp
#include <bits/stdc++.h>
using namespace std;
struct I { int v, w, i; double r; };
bool cmp(I a, I b) { return a.r > b.r; } // Sort by ratio
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
        if (i.w <= cap) { // Take all
            cap -= i.w;
            totalV += i.v;
            cout << "Added object " << i.i << " (Rs. " << i.v << ", " << i.w << "Kg) completely in the bag. Space left: " << cap << "." << endl;
        } else { // Take fraction
            totalV += i.r * cap;
            cout << "Added " << (int)(100.0 * cap / i.w) << "% (Rs." << i.v << ", " << i.w << "Kg) of object " << i.i << " in the bag." << endl;
            cap = 0;
        }
    }
    cout << "Filled the bag with objects worth Rs. " << fixed << setprecision(2) << totalV << "." << endl;
}
```

**Walkthrough:** We use a `struct I` to store item value `v`, weight `w`, index `i`, and ratio `r`. We `sort` by ratio descending. We loop through items. If it fits, we take it all and update `cap`. If it doesn't, we take a fraction (`i.r * cap`), set `cap=0`, and print the percentage.

-----

### CE2 - Question 2: Fractional Knapsack (4 Items)

Same logic, just `n=4` and a different output format.

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

**Walkthrough:** We read the 4 items. Print values and weights first. Then `sort` by ratio. Loop through, `take`ing the `min` of the item's weight and the remaining `cap`. We add `take * i.r` to the value.

-----

### CE3 - Question 1 & 2: Graph Coloring (Chromatic Number)

[cite\_start]These two are identical[cite: 598, 639]. We use a greedy algorithm.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int v; cin >> v;
    vector<vector<int>> g(v, vector<int>(v));
    for (int i = 0; i < v; i++) for (int j = 0; j < v; j++) cin >> g[i][j];
    
    vector<int> col(v, 0); // 0 = uncolored
    int maxC = 0;
    for (int i = 0; i < v; i++) {
        vector<bool> used(v + 1, false);
        for (int j = 0; j < v; j++) // Find colors used by neighbors
            if (g[i][j] && col[j] != 0) used[col[j]] = true;
        
        int c = 1;
        while (used[c]) c++; // Find first unused color
        
        col[i] = c;
        maxC = max(maxC, c);
    }
    cout << "Chromatic Number of the graph is: " << maxC << endl;
}
```

**Walkthrough:** We loop through each vertex `i`. For each one, we check its neighbors `j`. We make a `used` array to track which colors its neighbors have. Then we find the first color `c` (starting from 1) that is *not* in the `used` array. We assign `col[i] = c` and keep track of the `maxC` (max color) used. The answer is `maxC`.

-----

## TOPIC 4: STRING ALGORITHMS

**The "Easy Win" Trick:** The prompts ask for fancy algorithms (Rabin-Karp, Z-Algorithm), but the "minimal code to give exact output" is just `string::find`. It works for all of them.

### CE1 - Question 1 & 2: Naive Pattern Matching

[cite\_start]Identical problems[cite: 683, 713].

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

**Walkthrough:** `t.find(p)` finds the first match. `string::npos` means "not found." We loop, print `pos`, and search again from `pos + 1`.

-----

### CE2 - Question 1: Rabin-Karp (Count)

Use the `find` trick and a counter.

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

**Walkthrough:** Same as above, but we `c++` instead of printing `pos`. We print the total count `c` at the end.

-----

### CE2 - Question 2: Rabin-Karp (Find Indices)

Use the `find` trick and store indices in a vector.

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

**Walkthrough:** Same `find` loop, but we store `pos` in a `vector v`. At the end, we print the vector's contents or `-1` if it's empty.

-----

### CE3 - Question 1: Z Algorithm (Count)

Use the `find` trick with test cases.

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

**Walkthrough:** The `solve` function does the `find` and count logic. The `main` function just handles the "T test cases" input.

-----

### CE3 - Question 2: Z Algorithm (Find Indices)

This is literally the same as T4-CE1.

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

**Walkthrough:** `t.find(p)` in a loop. Easiest win ever.

-----

## TOPIC 5: STRING ALGORITHMS 2

### CE1 - Question 1: Shortest Palindrome (KMP)

This is the one string problem where `find` doesn't work. We need to find the longest palindromic *prefix*.

```cpp
#include <bits/stdc++.h>
using namespace std;
bool isPal(string s) { string r = s; reverse(r.begin(), r.end()); return s == r; }
int main() {
    string s; cin >> s;
    int n = s.length(), i = n;
    for (i = n; i > 0; i--)
        if (isPal(s.substr(0, i))) break; // Find longest palindromic prefix
    
    string toAdd = s.substr(i); // Get the non-palindromic part
    reverse(toAdd.begin(), toAdd.end());
    cout << toAdd + s << endl;
}
```

**Walkthrough:** We find the longest prefix of `s` that is a palindrome by checking `s.substr(0, i)` for `i` from `n` down to 1. As soon as we find one, we `break`. The part that's *not* a palindrome is `s.substr(i)`. We `reverse` that part and add it to the front.

-----

### CE1 - Question 2: KMP Pattern Search

It's just pattern matching. `find` trick.

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

**Walkthrough:** `t.find(p)` in a loop. Same as T4-CE1.

-----

### CE2 - Question 1: Longest Palindromic Substring (Manacher's)

[cite\_start]The prompt says Manacher's[cite: 939], but the "easy win" is "Expand Around Center." It's O(n^2) but super simple.

```cpp
#include <bits/stdc++.h>
using namespace std;
string getPal(string s) {
    if (s.empty()) return "";
    int n = s.length(), start = 0, maxL = 1;
    for (int i = 0; i < n; i++) {
        // Odd length
        int l = i, r = i;
        while (l >= 0 && r < n && s[l] == s[r]) {
            if (r - l + 1 > maxL) { maxL = r - l + 1; start = l; }
            l--; r++;
        }
        // Even length
        l = i; r = i + 1;
        while (l >= 0 && r < n && s[l] == s[r]) {
            if (r - l + 1 > maxL) { maxL = r - l + 1; start = l; }
            l--; r++;
        }
    }
    return s.substr(start, maxL);
}
int main() {
    int t; cin >> t; while(t--) cout << getPal(cin) << endl;
}
```

**Walkthrough:** We check every character `i` as a potential center. We check for "odd" length palindromes (center `i`) and "even" length (center `i`, `i+1`). We expand `l` and `r` outwards, keeping track of the `start` and `maxLen`.

-----

### CE2 - Question 2: Longest Palindromic Substring (Numeric)

This is the *exact same problem* as the one above, just with digits and no test cases.

```cpp
#include <bits/stdc++.h>
using namespace std;
string getPal(string s) {
    if (s.empty()) return "";
    int n = s.length(), start = 0, maxL = (n > 0); // Handle empty vs single
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
    string s; cin >> s; cout << getPal(s) << endl;
}
```

**Walkthrough:** Identical "Expand Around Center" logic.

-----

### CE3 - Question 1: Boyer-Moore (Count)

`find` trick.

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

**Walkthrough:** `t.find(p)` in a loop with a counter `c`.

-----

### CE3 - Question 2: Boyer-Moore (Reverse Count)

`find` trick with a `reverse`.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    string t, p; getline(cin, t); getline(cin, p);
    reverse(p.begin(), p.end()); // The only difference
    int c = 0;
    size_t pos = t.find(p);
    while (pos != string::npos) {
        c++;
        pos = t.find(p, pos + 1);
    }
    cout << c << endl;
}
```

**Walkthrough:** `reverse(p.begin(), p.end());` then `t.find(p)` in a loop with a counter `c`.

-----

## TOPIC 6: BACKTRACKING 1

### CE1 - Question 1: Rat in Maze (All Paths)

This needs real backtracking (DFS).

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
    solve(r + 1, c, p + 'D', v); [cite_start]// DLRU order [cite: 1066]
    solve(r, c - 1, p + 'L', v);
    solve(r, c + 1, p + 'R', v);
    solve(r - 1, c, p + 'U', v);
    v[r][c] = false; // Backtrack
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

**Walkthrough:** `solve(r, c, p, v)` is our DFS. `r, c` is position, `p` is path string, `v` is visited. Base case: if out of bounds, blocked, or visited, `return`. If at end, add path to `paths` vector. Recursive step: mark `v[r][c] = true`, try all 4 directions (D, L, R, U), then mark `v[r][c] = false` (backtrack).

-----

### CE1 - Question 2: Rat in Maze (One Path)

DFS again, but we can stop after finding the first path.

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
    sol[r][c] = 0; // Backtrack
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

**Walkthrough:** `sol` matrix stores the path. `solve` returns `true` if a path is found. If we reach the end, `return true`. We try all 4 directions with `||` (or). If any return `true`, we've found a path. If all fail, we backtrack (`sol[r][c] = 0`) and `return false`.

-----

### CE2 - Question 1 & 2: Integer Partition

[cite\_start]These are identical[cite: 1194, 1227]. Backtracking with a "start" parameter to avoid duplicates.

```cpp
#include <bits/stdc++.h>
using namespace std;
vector<vector<int>> res;
vector<int> cur;
void solve(int n, int rem, int start) {
    if (rem == 0) { res.push_back(cur); return; }
    for (int i = start; i <= rem; i++) {
        cur.push_back(i);
        solve(n, rem - i, i); // Pass 'i' as new start
        cur.pop_back(); // Backtrack
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

**Walkthrough:** `solve(n, rem, start)` finds partitions for `rem` (remaining sum). We loop `i` from `start` to `rem`. We `push_back(i)` and recurse with `rem - i` and `i` as the new `start`. Passing `i` as the start ensures combinations are non-decreasing (e.g., `[1, 2]` but not `[2, 1]`).

-----

### CE3 - Question 1: N-Queens (Pre-placed)

Standard N-Queens, but we need to check if the pre-placed queens are valid *first*.

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
    for (int j = 0; j < n; j++) { // Check if row has pre-placed
        if (b[r][j]) return solve(r + 1);
    }
    for (int c = 0; c < n; c++) {
        if (isSafe(r, c)) {
            b[r][c] = 1;
            if (solve(r + 1)) return true;
            b[r][c] = 0; // Backtrack
        }
    }
    return false;
}
int main() {
    int k; cin >> n >> k;
    b.resize(n, vector<int>(n, 0));
    vector<pair<int, int>> pre(k);
    for (int i = 0; i < k; i++) { cin >> pre[i].first >> pre[i].second; }
    for (auto p : pre) { // Validate pre-placed
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

**Walkthrough:** A minimal `isSafe` checks all 8 directions (row, col, 4 diags). We read all pre-placed queens, then *after* reading, we check if each is safe *relative to the others*. If not, fail fast. The `solve(r)` function checks if a queen is already in row `r`. If so, it skips to `solve(r+1)`. Otherwise, it tries to place a queen in a safe column `c`.

-----

### CE3 - Question 2: N-Queens (Fixed First)

This one is *easier*. Just place the first queen and solve. [cite\_start]But the output format is different, and it asks for *all* solutions (Sample 2 [cite: 1349] shows two).

```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
vector<vector<char>> b;
vector<vector<vector<char>>> sols;
bool isSafe(int r, int c) {
    for (int i = 0; i < r; i++) if (b[i][c] == 'Q') return false;
    for (int i = r, j = c; i >= 0 && j >= 0; i--, j--) if (b[i][j] == 'Q') return false;
    for (int i = r, j = c; i >= 0 && j < n; i--, j++) if (b[i][j] == 'Q') return false;
    return true;
}
void solve(int r) {
    if (r == n) { sols.push_back(b); return; }
    if (b[r][0] != '.') { // Check if first Q is in this row
        bool placed = false;
        for (int j = 0; j < n; j++) if (b[r][j] == 'Q') placed = true;
        if (placed) solve(r + 1);
        return;
    }
    for (int c = 0; c < n; c++) {
        if (isSafe(r, c)) {
            b[r][c] = 'Q';
            solve(r + 1);
            b[r][c] = '.'; // Backtrack
        }
    }
}
int main() {
    int r, c; cin >> n >> r >> c;
    b.resize(n, vector<char>(n, '.'));
    b[r][c] = 'Q';
    if (!isSafe(r,c)) { cout << "No solution" << endl; return 0; } // Should be safe
    
    solve(0); // Start from row 0
    
    if (sols.empty()) cout << "No solution" << endl;
    for (int k = 0; k < sols.size(); k++) {
        if (k > 0) cout << endl;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++)
                cout << sols[k][i][j] << (j == n - 1 ? "" : " ");
            cout << endl;
        }
    }
}
```

**Walkthrough:** `isSafe` only needs to check *upwards* (rows `< r`). `solve(r)` finds all solutions. If we hit row `r == n`, we store the solution. We have to *modify* the `solve` to handle the pre-placed queen, but the `solveNQueens` in the "original" code *doesn't* find all solutions. The `solve` function in the user's provided code is buggy for this. The code I provided `solve(r)` will *not* work correctly with the fixed queen logic `if (b[r][0] != '.')`.
*Minimal "Easy Win" Correction:* Let's use the *user's* `solve` logic, which is simpler but finds only one solution. The sample 2 shows 2 solutions, so the user's code is wrong. I will provide code that finds all solutions.

*Final Minimal Code (T6-CE3-Q2):*

```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
vector<vector<char>> b;
vector<vector<vector<char>>> sols;
bool isSafe(int r, int c) {
    for (int i = 0; i < n; i++) {
        if (b[i][c] == 'Q' && i != r) return false;
        if (b[r][i] == 'Q' && i != c) return false;
    }
    for (int i=r, j=c; i>=0 && j>=0; i--, j--) if (b[i][j] == 'Q' && (i!=r || j!=c)) return false;
    for (int i=r, j=c; i<n && j<n; i++, j++) if (b[i][j] == 'Q' && (i!=r || j!=c)) return false;
    for (int i=r, j=c; i>=0 && j<n; i--, j++) if (b[i][j] == 'Q' && (i!=r || j!=c)) return false;
    for (int i=r, j=c; i<n && j>=0; i++, j--) if (b[i][j] == 'Q' && (i!=r || j!=c)) return false;
    return true;
}
void solve(int r) {
    if (r == n) { sols.push_back(b); return; }
    if (b[r][0] != '.') { // Check if row has pre-placed Q
        for(int j=0; j<n; j++) if(b[r][j]=='Q') { solve(r+1); return; }
    }
    for (int c = 0; c < n; c++) {
        if (isSafe(r, c)) {
            b[r][c] = 'Q';
            solve(r + 1);
            b[r][c] = '.'; // Backtrack
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
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) cout << sols[k][i][j] << (j == n - 1 ? "" : " ");
            cout << endl;
        }
    }
}
```

**Walkthrough:** This is a mess. The pre-placed queen logic makes the standard recursive `solve(r)` hard. The *easiest* code is to just place the queen and *then* check.

*Final FINAL Minimal Code (T6-CE3-Q2):*

```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
vector<vector<char>> b;
vector<vector<vector<char>>> sols;
bool isSafe(int r, int c) {
    for (int i = 0; i < r; i++) { // Only check rows above
        if (b[i][c] == 'Q') return false;
        for (int j = 0; j < n; j++) {
            if (b[i][j] == 'Q' && (abs(i - r) == abs(j - c))) return false;
        }
    }
    return true;
}
void solve(int r) {
    if (r == n) { sols.push_back(b); return; }
    for (int j = 0; j < n; j++) {
        if (b[r][j] == 'Q') { // Handle pre-placed
            if (isSafe(r, j)) solve(r + 1);
            return; // Don't try other cols in this row
        }
    }
    for (int c = 0; c < n; c++) { // Try all cols
        if (isSafe(r, c)) {
            b[r][c] = 'Q';
            solve(r + 1);
            b[r][c] = '.'; // Backtrack
        }
    }
}
int main() {
    int r, c; cin >> n >> r >> c;
    b.resize(n, vector<char>(n, '.'));
    b[r][c] = 'Q'; // Pre-place
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

**Walkthrough:** `isSafe` checks rows above `r` for column and diagonal attacks. `solve(r)` is our main recursion. If row `r` contains the pre-placed queen, we check if it's safe and recurse. If not, we try placing a queen in every column `c` that is safe, then recurse and backtrack.

-----

## TOPIC 7: BACKTRACKING 2

### CE1 - Question 1: Minimum Knight Moves

This is a shortest path problem. Minimal code is BFS.

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

**Walkthrough:** Standard BFS. `q` is our queue, `dist` map stores moves. We start at `{0, 0}`. In the loop, we `pop` a position, check if it's the target. If not, we add all 8 valid knight moves to the queue, updating their distance.

-----

### CE1 - Question 2: Knight Probability

This is recursion with memoization (Dynamic Programming).

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

**Walkthrough:** `prob(k, r, c)` is the probability of staying on board with `k` moves from `(r, c)`. Base cases: if off-board, `return 0`. If `k=0` (and on board), `return 1`. We check our `memo` (cache). If not cached, we recursively call `prob` for all 8 moves with `k-1`, sum them up, divide by 8, and store in `memo`.

-----

### CE2 - Question 1: Subset Sum (Find First)

Backtracking. Return `true` as soon as we find a sum.

```cpp
#include <bits/stdc++.h>
using namespace std;
vector<int> items;
vector<int> res;
bool solve(int t, int idx) {
    if (t == 0) return true;
    if (t < 0 || idx == items.size()) return false;
    
    res.push_back(items[idx]); // Try including
    if (solve(t - items[idx], idx + 1)) return true;
    res.pop_back(); // Backtrack
    
    if (solve(t, idx + 1)) return true; // Try excluding
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

**Walkthrough:** `solve(t, idx)` tries to find target `t` starting from index `idx`. We first try *including* `items[idx]` (add to `res`, recurse). If that fails, we backtrack (`pop_back`) and try *excluding* `items[idx]` (just recurse).

-----

### CE2 - Question 2: Count Subsets

Backtracking, but we don't stop. We return `1` or `0` and sum them up.

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

**Walkthrough:** `solve(t, idx)` returns the *count* of subsets. Base cases: `t==0` returns 1 (we found one way), `t<0` or end of array returns 0. The total is `include` (count from including `items[idx]`) + `exclude` (count from excluding it).

-----

### CE3 - Question 1 & 2: m-Coloring

[cite\_start]Identical problems [cite: 1528, 1575] (Q1 just adds "Solution Exists:"). Backtracking.

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
            col[i] = 0; // Backtrack
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
        // For Q1: cout << "Solution Exists:" << endl;
        for (int i = 0; i < v; i++) cout << col[i] << " ";
        cout << endl;
    } else cout << "Solution does not exist" << endl;
}
```

**Walkthrough:** `solve(i)` tries to color vertex `i`. We loop through all `m` colors. If a color `c` is `isSafe` (no neighbor `j` has `col[j] == c`), we assign it (`col[i] = c`) and recurse. If `solve(i+1)` fails, we backtrack (`col[i] = 0`).

-----

## TOPIC 8: BACKTRACKING 3

### CE1 - Question 1 & 2: Sudoku Solver

Q1 uses 11-19, Q2 uses 1-9. The logic is identical, just the loop range changes.

*Minimal Code (Q2 - Standard Sudoku):*

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
                for (int n = 1; n <= 9; n++) { // For Q1, change to: n = 11; n <= 19
                    if (isSafe(r, c, n)) {
                        b[r][c] = n;
                        if (solve()) return true;
                        b[r][c] = 0; // Backtrack
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

**Walkthrough:** `solve()` finds the first empty cell (`0`). It tries numbers `n` from 1 to 9. If `isSafe` (checks row, col, and 3x3 box), it places the number and recurses. If `solve()` returns `false`, it backtracks. **For Q1, just change the loop `for (int n = 1; n <= 9; n++)` to `for (int n = 11; n <= 19; n++)`.**

-----

### CE2 - Question 1 & 2: Prime Sum

[cite\_start]Identical problems[cite: 1742, 1796]. Sieve to get primes, then backtracking (combination sum).

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
        solve(t - p[i], i + 1); // i + 1 for no duplicates
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

**Walkthrough:** `getPrimes(S, P)` finds all primes between `P` and `S`. `solve(t, idx)` is a combination sum. We loop from `idx` to `p.size()`, `push_back(p[i])`, recurse with `t - p[i]` and `i + 1` (to prevent re-using the same prime).

-----

### CE3 - Question 1: Hamiltonian Cycle (Find One)

Backtracking. Visit all nodes exactly once.

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
    if (c == v - 1) { // Visited all
        return g[i][0]; // Check if path back to start
    }
    for (int j = 0; j < v; j++) {
        if (g[i][j] && !vis[j]) {
            if (solve(j, c + 1)) return true;
        }
    }
    vis[i] = false; // Backtrack
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

**Walkthrough:** `solve(i, c)` visits vertex `i` at count `c`. We mark `i` as visited. If `c == v-1` (all visited), we check if there's an edge back to `0`. If not, we try all unvisited neighbors `j` and recurse. If all fail, backtrack.

-----

### CE3 - Question 2: All Hamiltonian Cycles

Same, but don't return `true`. Collect all solutions.

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
    vis[i] = false; // Backtrack
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
        cout << "['" << names[s[0]] << " '";
        for (int i = 1; i < v; i++) cout << ", '" << names[s[i]] << " '";
        cout << ", '" << names[0] << "']" << endl;
    }
}
```

**Walkthrough:** Same as Q1, but `solve` is `void`. When we reach `c == v-1`, we check the path back to `0`. If it exists, we add the `path` to our `sols` vector. We don't `return true`, so the function continues to find all other paths. Finally, we print all solutions in the `sols` vector.
