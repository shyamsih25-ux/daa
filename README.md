# Complete DAA CIA 1 Guide with Detailed Walkthroughs & MCQs

## TOPIC 1: TIME COMPLEXITY

### CE1 - Question 1: Calculate Area

**Problem:** Calculate area of different shapes (rectangle, triangle, circle) based on input dimensions and shape identifier.

**Input Format:**
- Line 1: Integer `length`
- Line 2: Integer `breadth`
- Line 3: Character `shape` ('r' for rectangle, 't' for triangle, 'c' for circle)

**Output Format:**
- For rectangle: "Area of rectangle: {length × breadth}"
- For triangle: "Area of triangle: {(length × breadth) / 2}"
- For circle: "Area of circle: {3.14 × length²}" (2 decimal places)
- For invalid: "Invalid shape!"

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

**Detailed Walkthrough:**
1. Read three values: `l` (length), `b` (breadth), and `s` (shape character)
2. Use if-else chain to determine shape:
   - If 'r': Multiply length × breadth for rectangle area
   - If 't': Use formula (length × breadth) / 2 for triangle (integer division)
   - If 'c': Use formula 3.14 × length² (only first dimension matters for circle radius)
   - Use `fixed << setprecision(2)` to format circle output to 2 decimals
3. For any other character, output "Invalid shape!"

---

### CE1 - Question 2: Update Maximum

**Problem:** Find the maximum of two numbers and allow updating it with a new value using ternary operator reference.

**Input Format:**
- Line 1: Two integers `a` and `b` (space-separated)
- Line 2: Integer `n` (new value to assign to the maximum)

**Output Format:**
- Line 1: "Before: a = {a}, b = {b}"
- Line 2: "After: a = {updated_a}, b = {updated_b}"

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

**Detailed Walkthrough:**
1. Read three integers: `a`, `b`, and `n` (replacement value)
2. Print original values in format: "Before: a = {a}, b = {b}"
3. The expression `((a > b) ? a : b)` evaluates to a **reference** to whichever variable is larger
4. Assigning `n` to this expression **directly modifies** the original variable (not a copy)
5. If a > b, then a gets replaced; otherwise b gets replaced
6. Print updated values in format: "After: a = {a}, b = {b}"

---

### CE2 - Question 1: GCD and LCM

**Problem:** Calculate GCD and LCM of two numbers using recursion.

**Input Format:**
- Line 1: Two positive integers `a` and `b` (space-separated)

**Output Format:**
- Line 1: GCD of a and b
- Line 2: LCM of a and b

```cpp
#include <bits/stdc++.h>
using namespace std;
int gcd(int a, int b) { return b == 0 ? a : gcd(b, a % b); }
int main() {
    int a, b; cin >> a >> b;
    cout << gcd(a, b) << endl << (long long)a * b / gcd(a, b) << endl;
}
```

**Detailed Walkthrough:**
1. Read two positive integers `a` and `b`
2. **GCD Calculation (Euclidean Algorithm):**
   - Recursive function: if b is 0, return a (base case)
   - Otherwise, recursively call gcd(b, a % b)
   - This repeatedly divides until remainder becomes 0
3. **LCM Calculation:**
   - Formula: LCM(a, b) = (a × b) / GCD(a, b)
   - Cast to `long long` to prevent integer overflow during multiplication
4. Print GCD on first line, LCM on second line

**Example:** For input 10 and 15:
- GCD: gcd(10, 15) → gcd(15, 10) → gcd(10, 5) → gcd(5, 0) → 5
- LCM: (10 × 15) / 5 = 150 / 5 = 30

---

### CE2 - Question 2: Palindrome Check

**Problem:** Check if a number is a palindrome by reversing it recursively.

**Input Format:**
- Line 1: Integer `N` (the passcode to check)

**Output Format:**
- Line 1: "Reversed number: {reversed_number}"
- Line 2: "The number is a palindrome." OR "The number is not a palindrome."

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

**Detailed Walkthrough:**
1. Read integer `n` (the passcode)
2. **Recursive Reversal Process:**
   - Default parameter `r = 0` accumulates the reversed number
   - Extract last digit: `n % 10`
   - Build reversed number: `r * 10 + n % 10` (shift left and add digit)
   - Remove last digit from original: `n / 10`
   - Base case: when n becomes 0, return accumulated reversed number
3. Compare original `n` with reversed `r`:
   - If equal: print "The number is a palindrome."
   - If not equal: print "The number is not a palindrome."

**Example:** For n = 121:
- rev(121, 0) → rev(12, 1) → rev(1, 12) → rev(0, 121) → returns 121
- Since 121 == 121, it's a palindrome

---

### CE3 - Question 1: Tower of Hanoi

**Problem:** Calculate minimum number of moves to solve Tower of Hanoi puzzle for n disks.

**Input Format:**
- Line 1: Integer `n` (number of disks)

**Output Format:**
- Line 1: Minimum number of moves (2^n - 1)

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n; cin >> n;
    cout << (1 << n) - 1 << endl;
}
```

**Detailed Walkthrough:**
1. Read number of disks `n`
2. **Formula:** Minimum moves = 2^n - 1
3. **Bitwise Calculation:** `1 << n` computes 2^n efficiently
   - `1 << n` shifts bit 1 left by n positions
   - 1 << 2 = 4, 1 << 3 = 8, 1 << 4 = 16, etc.
4. Subtract 1 from result: 2^n - 1
5. Print the result

**Examples:**
- n=2: 2^2 - 1 = 4 - 1 = 3 moves
- n=4: 2^4 - 1 = 16 - 1 = 15 moves

---

### CE3 - Question 2: Sieve of Eratosthenes

**Problem:** Generate all prime numbers ≤ N using Sieve of Eratosthenes.

**Input Format:**
- Line 1: Integer `N` (upper limit for primes)

**Output Format:**
- Line 1: All prime numbers ≤ N, space-separated, in ascending order

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

**Detailed Walkthrough:**
1. Read upper limit `n`
2. Create boolean vector `p` of size n+1, initialized to true (assume all prime)
3. **Sieve Algorithm:**
   - For each number i from 2 to √n:
     - If p[i] is still true (i is prime):
       - Mark all multiples of i starting from i² as false (composite)
       - Start from i² because smaller multiples already marked by smaller primes
4. **Output Primes:**
   - If n ≥ 2, print 2 (only even prime) first
   - Then iterate through odd numbers only (3, 5, 7, ...) to save iterations
   - Print each number where p[i] is still true (prime)

**Example:** For n = 10:
- Initially all true except 0, 1
- i=2: Mark 4, 6, 8, 10 as false
- i=3: Mark 9 as false (6 already marked)
- Result: 2 3 5 7

---

### MCQs for Topic 1: Time Complexity

**Q1:** Tower of hanoi is a classic example of ______
- **Answer: Both divide and conquer & recursive approach**

**Q2:** What will be the output of the following code?
```cpp
#include <iostream>
using namespace std;
int func(int &a){
    a = a*2;
    if(a<=5)
        return func(a) * a;
    return a;
}
int main(){
    int a = 2;
    cout << func(a);
    return 0;
}
```
- **Answer: 64**

**Q3:** What is the objective of tower of hanoi puzzle?
- **Answer: To move all disks to some other rod by following rules**

**Q4:** Recursive solution of tower of hanoi problem is an example of which algorithm?
- **Answer: Divide and conquer**

**Q5:** What will be the output of the following code?
```cpp
#include <iostream>
using namespace std;
void mystery(int n) {
    if (n > 1) {
        mystery(n / 2);
        cout << n % 2;
    }
}
int main() {
    mystery(10);
    return 0;
}
```
- **Answer: 010**

**Q6:** Which of the following is NOT a rule of tower of hanoi puzzle?
- **Answer: No disk should be placed over a smaller disk**

**Q7:** What will be the output of the following code?
```cpp
#include <iostream>
using namespace std;
int factorial(int n) {
    if (n == 0)
        return 1;
    else
        return n * factorial(n - 1);
}
int main() {
    int result = factorial(5);
    cout << result;
    return 0;
}
```
- **Answer: 120**

**Q8:** What is the output of the following code?
```cpp
#include <iostream>
using namespace std;
int reverseNumber(int n, int sum) {
    if (n > 0) {
        int temp = n % 10;
        sum = sum + temp;
        return reverseNumber(n / 10, sum);
    }
    else {
        return sum;
    }
}
int main() {
    int n = 7854;
    int reversed = reverseNumber(n, 0);
    cout << reversed;
    return 0;
}
```
- **Answer: 24**

**Q9:** The recurrence relation capturing the optimal execution time of the Towers of Hanoi problem with n discs is
- **Answer: T(n) = 2T(n − 1) + 1**

**Q10:** What is the number of moves required to solve Tower of Hanoi problem for k disks?
- **Answer: 2^k - 1**

**Q11:** What is the output of the following code?
```cpp
#include <iostream>
using namespace std;
int n_to_the_kth_power(int n, int k){
    if (k == 0)
        return 1;
    else
        return n * n_to_the_kth_power(n, k-1);
}
int main (){
    int n = 2, k = 4;
    while (k >= 0) {
        cout << n << "**" << k << " = " << n_to_the_kth_power(n,k) << endl;
        k--;
    }
    return 0;
}
```
- **Answer: 2**4 = 16, 2**3 = 8, 2**2 = 4, 2**1 = 2, 2**0 = 1**

**Q12:** What will be the output of the following code?
```cpp
#include <iostream>
using namespace std;
void printBinary(int n) {
    if (n > 1)
        printBinary(n / 2);
    cout << n % 2;
}
int main() {
    printBinary(13);
    return 0;
}
```
- **Answer: 1101**

**Q13:** The solution to which of the following Tower of Hanoi puzzles will require the least moves?
- **Answer: Tower B with 3 pegs and 3 disks**

**Q14:** What is the output of the following code?
```cpp
#include <iostream>
using namespace std;
int sumOfDigits(int n) {
    if (n < 10) {
        return n;
    }
    return n % 10 + sumOfDigits(n / 10);
}
int main() {
    int number = 987;
    cout << sumOfDigits(number);
    return 0;
}
```
- **Answer: 24**

**Q15:** Which of the following are types of recursion?
- **Answer: Direct Recursion, Indirect Recursion and Tail Recursion**

---

## TOPIC 2: HEAPS

### CE1 - Question 1: Min Heap with Average

**Problem:** Build a min-heap with positive parcel weights and calculate their average.

**Input Format:**
- Line 1: Integer `n` (number of parcels)
- Line 2: `n` space-separated integers (parcel weights, may include negatives)

**Output Format:**
- Line 1: Min-heap elements in level-order, space-separated
- Line 2: Average weight with 2 decimal precision
- OR "No valid weight" if no positive weights

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

**Detailed Walkthrough:**
1. Read `n` (number of parcels)
2. Declare min-heap using `priority_queue<int, vector<int>, greater<int>>`
   - `greater<int>` comparator makes it a min-heap (smallest at top)
3. **Process each weight:**
   - Read weight `x`
   - If x > 0: push to heap, add to sum, increment count
   - Ignore non-positive weights
4. **Edge case:** If count is 0 (no valid weights), print "No valid weight" and exit
5. **Display heap:** Copy heap to `h2` to preserve original (popping destroys heap)
   - While h2 not empty: print top element, pop
6. Calculate average: sum / count (cast to double for precision)
7. Print average with 2 decimal places using `fixed << setprecision(2)`

**Example:** Input: 5 parcels with weights 3 9 2 6 8
- Min-heap structure: 2 at root, then 6, 3, 9, 8 in level-order
- Sum = 28, Count = 5, Average = 5.60

---

### CE1 - Question 2: Remove < 2×Root

**Problem:** Build min-heap, remove all elements less than twice the root value.

**Input Format:**
- Line 1: Integer `n` (number of tasks)
- Line 2: `n` space-separated integers (priority levels)

**Output Format:**
- Line 1: Remaining elements in level-order, space-separated

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

**Detailed Walkthrough:**
1. Read `n` (number of tasks)
2. Create two min-heaps: `h` (original) and `h2` (filtered)
3. **Build original heap:** Read all n priorities and push to heap `h`
4. **Calculate threshold:** `t = 2 * h.top()` (twice the minimum element)
5. **Filter elements:**
   - For each element in heap `h`:
     - If element ≥ threshold: push to new heap `h2`
     - Pop element from `h`
6. **Display result:** Print all elements from `h2` (automatically in min-heap order)

**Example:** Input: 7 elements [4 8 6 15 12 10 14]
- Min-heap: 4 at root
- Threshold = 2 × 4 = 8
- Keep: 8, 10, 12, 15, 14 (all ≥ 8)
- Output in min-heap order: 8 10 12 15 14

---

### CE2 - Question 1: Fibonacci Max Heap

**Problem:** Generate Fibonacci sequence starting at 2, 3 and insert into max-heap, showing state after each insertion.

**Input Format:**
- Line 1: Integer `n` (number of gems/Fibonacci numbers to generate)

**Output Format:**
- For each insertion: "Insert {value}: {heap_state}"
- Heap state shows elements in level-order

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n, sum = 0; 
    cin >> n;
    vector<int> heap; // this will store the heap
    for (int i = 1; i <= n; i++) {
        heap.push_back(i);             
        push_heap(heap.begin(), heap.end()); 
        sum += i;
    }
    for (int x : heap) cout << x << " ";
    cout << "\n" << sum;
    return 0;
}

```

**Detailed Walkthrough:**
1. Read `n` (number of Fibonacci numbers to generate)
2. Initialize: `a = 2` (first), `b = 3` (second)
3. **For each position i from 1 to n:**
   - **Generate Fibonacci number:**
     - If i == 1: use 2
     - If i == 2: use 3
     - Otherwise: cur = a + b (sum of previous two)
     - Update sliding window: a = b, b = cur
   - **Insert into max-heap:** h.push(cur)
   - **Display state:**
     - Copy heap to h2 (to preserve original)
     - Print "Insert {cur}: "
     - Print all elements from h2 (largest first due to max-heap)

**Example:** For n = 5:
- Insert 2: 2
- Insert 3: 3 2
- Insert 5: 5 2 3
- Insert 8: 8 5 3 2
- Insert 13: 13 8 3 2 5

---

### CE2 - Question 2: Max Heap with Sum

**Problem:** Create max-heap with numbers 1 to n, display heap and calculate sum.

**Input Format:**
- Line 1: Integer `n` (maximum ID)

**Output Format:**
- Line 1: Heap elements in level-order, space-separated
- Line 2: Sum of all elements

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n; 
    cin >> n;
    vector<int> heap;
    int a = 2, b = 3;
    for (int i = 1; i <= n; i++) {
        int cur;
        if (i == 1) cur = a;
        else if (i == 2) cur = b;
        else { cur = a + b; a = b; b = cur; }
        heap.push_back(cur);
        push_heap(heap.begin(), heap.end()); // maintain max heap
        cout << "Insert " << cur << ": ";
        for (int x : heap) cout << x << " ";
        cout << endl;
    }
    return 0;
}
```

**Detailed Walkthrough:**
1. Read `n` (maximum ID value)
2. Initialize sum = 0
3. **Build heap:**
   - For i from 1 to n:
     - Push i to max-heap
     - Add i to sum
4. **Display heap:** Copy to h2 and print all elements (largest to smallest order)
5. **Print sum:** Total of all numbers from 1 to n (formula: n×(n+1)/2)

**Example:** For n = 3:
- Heap: 3 1 2 (max-heap structure)
- Sum: 1 + 2 + 3 = 6

---

### CE3 - Question 1: Heap Sort Strings

**Problem:** Sort strings lexicographically using heap sort (min-heap).

**Input Format:**
- Line 1: Integer `N` (number of strings)
- Line 2: `N` space-separated strings

**Output Format:**
- Line 1: Sorted strings, space-separated

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

**Detailed Walkthrough:**
1. Read `N` (number of strings)
2. Create min-heap for strings: `priority_queue<string, vector<string>, greater<string>>`
   - `greater<string>` ensures lexicographical ascending order
3. **Insert all strings:**
   - For each of N strings: read and push to heap
4. **Output sorted strings:**
   - While heap not empty: print top (smallest lexicographically), pop
   - Min-heap automatically maintains sorted order

**Example:** Input: banana pineapple mango
- Heap maintains: banana < mango < pineapple
- Output: banana mango pineapple

---

### CE3 - Question 2: Kth Largest Element

**Problem:** Find Kth largest element using max-heap.

**Input Format:**
- Line 1: Integer `n` (number of products)
- Line 2: `n` space-separated integers (sales data)
- Line 3: Integer `K` (rank to find)

**Output Format:**
- Line 1: The Kth largest element

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

**Detailed Walkthrough:**
1. Read `n` (number of products)
2. Create max-heap (default priority_queue in C++)
3. **Build heap:** Read all n sales values and push to heap
4. Read `K` (desired rank)
5. **Find Kth largest:**
   - Pop K-1 elements from heap (removes 1st largest, 2nd largest, ..., (K-1)th largest)
   - The element now at top is the Kth largest
6. Print top element

**Example:** Input: [10 20 15 5 25], K = 2
- Max-heap: 25 at top
- Pop once (removes 25)
- New top: 20 (2nd largest)

---

### MCQs for Topic 2: Heaps

**Q1:** Given an array of elements 5, 7, 9, 1, 3, 10, 8, and 4, which of the following are the correct sequences of elements after inserting all the elements in a min-heap?
- **Answer: 1,3,4,5,7,8,9,10**

**Q2:** What is the purpose of the 'heapify' function in the heap sort algorithm?
- **Answer: It maintains the heap property by fixing the root node if it violates the property.**

**Q3:** Which of the following Binary Min Heap operation has the highest time complexity?
- **Answer: Merging with another heap under the assumption that the heap has capacity to accommodate items of other heap**

**Q4:** In heap sort, which of the following heap operations is performed to build a valid heap from an unsorted array?
- **Answer: Heapify**

**Q5:** Which of the following sorting algorithms uses a binary heap as its underlying data structure?
- **Answer: Heap Sort**

**Q6:** A priority queue is implemented as a Max-Heap. Initially, it has 5 elements. The level-order traversal of the heap is: 10, 8, 5, 3, 2. Two new elements 1 and 7 are inserted into the heap in that order. Which of the following is the level-order traversal of the heap after the insertion of the elements?
- **Answer: 10, 8, 7, 3, 2, 1, 5**

**Q7:** The essential part of the Heap sort is the construction of the max-heap. Consider a tree where node 24 violates the max-heap property. Once heapify procedure is applied to it, which position will it be in?
- **Answer: 9**

**Q8:** In heap sort, when does the actual sorting take place?
- **Answer: During the swap and heapify phase**

**Q9:** In a Max Heap, what is the condition that must be satisfied between a parent and its children?
- **Answer: The parent must have a higher value than its children**

**Q10:** What is the primary characteristic of a Max Heap?
- **Answer: The maximum element is at the root**

**Q11:** Which of the following is a disadvantage of using a binary heap?
- **Answer: It requires additional memory to maintain the heap property.**

**Q12:** In heap sort, the element that is swapped with the final element of the heap is _______, in the case of a max-heap, sorted in descending order.
- **Answer: Largest element**

**Q13:** Consider a max heap, represented by the array: 40, 30, 20, 10, 15, 16, 17, 8, 4. If a value 35 is inserted into this heap, then what will be the new max heap?
- **Answer: 40, 35, 20, 10, 30, 16, 17, 8, 4, 15**

**Q14:** What is the worst-case time complexity of the heap sort algorithm?
- **Answer: O(n log n)**

---

## TOPIC 3: GREEDY ALGORITHMS

### CE1 - Question 1: Activity Selection

**Problem:** Select maximum non-overlapping activities using greedy approach.

**Input Format:**
- Line 1: Integer `n` (number of activities)
- Line 2: `n` space-separated integers (start times)
- Line 3: `n` space-separated integers (finish times)

**Output Format:**
- Line 1: Indices of selected activities, space-separated

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

**Detailed Walkthrough:**
1. Read `n` (number of activities)
2. Read start times array `s` and finish times array `f`
3. **Create index-finish pairs:**
   - For each activity i: create pair {finish_time[i], original_index_i}
   - This preserves original indices after sorting
4. **Sort by finish time:** Activities with earliest end times come first
5. **Greedy selection:**
   - Initialize lastF = -1 (no previous activity selected)
   - For each activity in sorted order:
     - Check if start time ≥ last finish time (no overlap)
     - If compatible: print original index, update lastF
6. Activities with earliest finish times maximize opportunities for future selections

**Example:** Input: starts=[1,3,0,5,8,5], finishes=[2,4,6,7,9,9]
- Sort by finish: (0,2), (1,4), (3,7), (4,9), (5,9)
- Select: 0 (ends at 2), 1 (starts at 3≥2), 3 (starts at 5≥4), 4 (starts at 8≥7)
- Output: 0 1 3 4

---

### CE1 - Question 2: Match Scheduling

**Problem:** Schedule maximum non-overlapping matches with named activities.

**Input Format:**
- Line 1: Integer `n` (number of matches)
- Next `n` lines: String `name`, int `start`, int `finish`

**Output Format:**
- Line 1: "Selected Activities are:"
- Line 2: Names of selected matches, space-separated

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

**Detailed Walkthrough:**
1. Read `n` (number of matches)
2. **Define struct M:**
   - `n`: match name (string)
   - `s`: start time (int)
   - `f`: finish time (int)
3. **Read match data:** For each match, read name, start, finish
4. **Sort by finish time:** Use custom comparator `cmp` that compares finish times
5. **Greedy selection:**
   - Print header: "Selected Activities are:"
   - Initialize lastF = -1
   - For each match in sorted order:
     - If start ≥ lastF: compatible, print name and update lastF
6. Same greedy logic as previous problem but with named activities

**Example:** Input: match1(1,2), match5(3,4), match4(0,6), match3(5,7), match6(8,9), match2(5,9)
- Sort by finish: match1(1,2), match5(3,4), match4(0,6), match3(5,7), match6(8,9), match2(5,9)
- Select: match1, match5, match3, match6
- Output: "Selected Activities are:\nmatch1 match5 match3 match6"

---

### CE2 - Question 1: Fractional Knapsack (Detailed)

**Problem:** Fill knapsack with items to maximize value, items can be fractional.

**Input Format:**
- Line 1: Integer `n` (number of objects)
- Line 2: `n` space-separated integers (weights)
- Line 3: `n` space-separated integers (values)
- Line 4: Integer `W` (knapsack capacity)

**Output Format:**
- For each item: detailed message about addition (full or partial)
- Last line: "Filled the bag with objects worth Rs. {total_value}."

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

**Detailed Walkthrough:**
1. Read `n` (number of objects)
2. **Define struct I:** value, weight, index (1-based), ratio
3. Read weights first, assign 1-based indices
4. Read values, calculate value-to-weight ratios: r = v/w
5. Read knapsack capacity
6. **Sort by ratio (descending):** Items with best value/weight come first
7. **Greedy packing:**
   - For each item in sorted order:
     - If capacity exhausted: break
     - If item fits completely (w ≤ cap):
       - Take entire item: reduce capacity, add full value
       - Print: "Added object {i} (Rs. {v}, {w}Kg) completely in the bag. Space left: {cap}."
     - If item doesn't fit (w > cap):
       - Take fractional amount: (cap/w) portion
       - Add proportional value: ratio × remaining_capacity
       - Calculate percentage: (cap/w) × 100
       - Print: "Added {%}% (Rs.{v}, {w}Kg) of object {i} in the bag."
       - Set capacity to 0
8. Print total value with 2 decimal places

**Example:** weights=[10,20,30], values=[60,100,120], capacity=50
- Ratios: 6, 5, 4
- Take all of object 1 (10kg, 60Rs), space left: 40
- Take all of object 2 (20kg, 100Rs), space left: 20
- Take 66% of object 3 (20/30kg, 80Rs)
- Total: 240.00

---

### CE2 - Question 2: Fractional Knapsack (4 Items)

**Problem:** Same algorithm but simplified output for exactly 4 items.

**Input Format:**
- Lines 1-4: Two integers each (value weight) for each item
- Line 5: Integer `W` (capacity)

**Output Format:**
- Line 1: "Values: {all values}"
- Line 2: "Weights: {all weights}"
- Line 3: "Total weight in bag: {weight}" (2 decimals)
- Line 4: "Max value for {int_weight} weight is {value}" (2 decimals)

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

**Detailed Walkthrough:**
1. Read 4 items: each with value and weight on same line
2. Calculate ratios for each item
3. Read capacity
4. **Print original data:** Display all values, then all weights
5. Sort by ratio (descending)
6. **Pack items:**
   - For each item: take minimum of (item weight, remaining capacity)
   - Add proportional value: taken_weight × ratio
   - Track total weight and total value
7. Print total weight (2 decimals) and max value (2 decimals)
8. Cast totalW to int for "Max value for {int} weight" message

**Example:** items=[(300,6), (150,3), (120,3), (100,2)], capacity=10
- Ratios: 50, 50, 40, 50
- Pack greedily by ratio
- Total weight: 10.00, Max value: 500.00

---

### CE3 - Question 1 & 2: Graph Coloring (Chromatic Number)

**Problem:** Find chromatic number of graph using greedy coloring.

**Input Format:**
- Line 1: Integer `v` (number of vertices)
- Next `v` lines: `v` space-separated integers (adjacency matrix)

**Output Format:**
- "Chromatic Number of the graph is: {number}"

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

**Detailed Walkthrough:**
1. Read `v` (number of vertices)
2. Read adjacency matrix `g[i][j]`: 1 if edge exists, 0 otherwise
3. Initialize color array `col` (0 means uncolored)
4. **Greedy coloring algorithm:**
   - For each vertex i (0 to v-1):
     - Create boolean array `used` to track colors used by neighbors
     - For each neighbor j:
       - If edge exists (g[i][j]=1) AND j is already colored:
         - Mark col[j] as used
     - Find smallest unused color starting from 1:
       - Increment c until used[c] is false
     - Assign color c to vertex i
     - Update maximum color used
5. **Chromatic number:** Maximum color value used
6. Print result

**Example:** 4-vertex complete graph (all connected)
- Vertex 0: color 1
- Vertex 1: neighbor 0 has color 1, so use color 2
- Vertex 2: neighbors have 1,2, so use color 3
- Vertex 3: neighbors have 1,2,3, so use color 4
- Chromatic number: 4

---

### MCQs for Topic 3: Greedy Algorithms

**Q1:** Which is the optimal value in the case of the fractional knapsack problem, the capacity of the knapsack is 10?
Item: 1 2 3 4 5, Profit: 12 32 40 30 50, Weight: 4 8 2 6 1
- **Answer: 354**

**Q2:** Which of the following methods can be used to solve the Knapsack problem?
- **Answer: Brute force, Recursion and Dynamic Programming**

**Q3:** Which of the following approach should be used to find the solution of the activity selection problem?
- **Answer: Greedy approach**

**Q4:** Fractional knapsack problem is solved most efficiently by which of the following algorithm?
- **Answer: Greedy algorithm**

**Q5:** What is the primary criterion for selecting activities in the Activity Selection Problem?
- **Answer: Activities with the earliest end time.**

**Q6:** The fractional knapsack problem is also known as
- **Answer: Continuous knapsack problem**

**Q7:** What is the chromatic number of a graph?
- **Answer: Minimum number of colors needed to color the graph**

**Q8:** In the Fractional Knapsack Problem, what does the greedy algorithm's solution guarantee?
- **Answer: An optimal solution in all cases.**

**Q9:** Suppose two activities A and B, having start and finish time as SA, FA and SB, FB respectively. Both the activities are said to be compatible, under which of the following condition?
- **Answer: SA >= FB or SB >= FA**

**Q10:** The main time taking step in the fractional knapsack problem is ________.
- **Answer: Sorting**

**Q11:** What does the isSafe() function in greedy coloring check?
- **Answer: If a vertex can be assigned a specific color**

**Q12:** What is the goal of graph coloring in the greedy approach?
- **Answer: Minimize the number of colors used**

**Q13:** In the Fractional Knapsack Problem, which item is chosen next if items are sorted by their value-to-weight ratio in decreasing order?
- **Answer: The item with the highest value-to-weight ratio.**

**Q14:** Which among the following represents best-case time complexity for activity selection problem?
- **Answer: O(n)**

**Q15:** Consider the following algorithm to find the solution of the activity selection problem. Sort the given activity List, display the first activity, set i = 1, for j = 1 to n-1 do, if begin time of activity[j] >= completion of activity[i] then, i = j, ________
- **Answer: display the jth activity**

---

## TOPIC 4: STRING ALGORITHMS

### CE1 - Question 1 & 2: Naive Pattern Matching

**Problem:** Find all occurrences of pattern in text using simple string search.

**Input Format:**
- Line 1: String `text` (the text to search in)
- Line 2: String `pattern` (the pattern to find)

**Output Format:**
- For each match: "Pattern found at index {position}"

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

**Detailed Walkthrough:**
1. Read text `t` and pattern `p` using `getline` (handles spaces)
2. **Search algorithm:**
   - Use `t.find(p)` to find first occurrence
   - Returns index if found, `string::npos` if not found
3. **Loop through all occurrences:**
   - While position is valid (not npos):
     - Print "Pattern found at index {pos}"
     - Search again starting from pos+1 to find overlapping matches
4. **Overlapping matches:** Starting from pos+1 allows finding patterns like "AA" in "AAA" (at positions 0 and 1)

**Example:** text="AABAACAADAABAAABAA", pattern="AABA"
- First find at index 0: "AABA..."
- Search from index 1, find at index 9: "...DAABAA..."
- Search from index 10, find at index 13: "...AABAAA..."
- Output: Pattern found at index 0, 9, 13

---

### CE2 - Question 1: Rabin-Karp (Count)

**Problem:** Count total occurrences of pattern in text.

**Input Format:**
- Line 1: String `text`
- Line 2: String `pattern`

**Output Format:**
- Line 1: Integer count of occurrences

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

**Detailed Walkthrough:**
1. Read text and pattern using `getline`
2. Initialize counter `c = 0`
3. **Count matches:**
   - Find first occurrence
   - While match found:
     - Increment counter
     - Search from next position (pos+1)
4. Print final count

**Example:** text="hello hello world hello", pattern="hello"
- Find at 0: count=1
- Find at 6: count=2
- Find at 18: count=3
- Output: 3

---

### CE2 - Question 2: Rabin-Karp (Find Indices)

**Problem:** Find and store all occurrence indices, print them or -1 if none.

**Input Format:**
- Line 1: String `text`
- Line 2: String `pattern`

**Output Format:**
- All indices space-separated, OR -1 if no matches

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

**Detailed Walkthrough:**
1. Read text and pattern
2. Create vector `v` to store indices
3. **Find all matches:**
   - Search for pattern repeatedly
   - Store each found index in vector
4. **Output results:**
   - If vector empty: print -1 (no matches)
   - Otherwise: print all indices space-separated

**Example:** text="aaaaa", pattern="aa"
- Find at 0: store 0
- Find at 1: store 1
- Find at 2: store 2
- Find at 3: store 3
- Output: 0 1 2 3

---

### CE3 - Question 1: Z Algorithm (Count)

**Problem:** Count pattern occurrences with multiple test cases.

**Input Format:**
- Line 1: Integer `T` (number of test cases)
- For each test case:
  - Line 1: Two integers `N` and `M` (lengths)
  - Line 2: String `S` (text)
  - Line 3: String `P` (pattern)

**Output Format:**
- For each test case: count of occurrences (one per line)

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

**Detailed Walkthrough:**
1. Read number of test cases `T`
2. **For each test case:**
   - Read lengths N (text) and M (pattern)
   - Read text string S and pattern P
   - Count occurrences using same algorithm
   - Print count
3. **Pattern matching:** Standard find loop with counter

**Example:**
- Test 1: S="ababaab", P="ab" → 3 occurrences
- Test 2: S="abab", P="a" → 2 occurrences

---

### CE3 - Question 2: Z Algorithm (Find Indices)

**Problem:** Same as CE1 pattern matching - find and print all indices.

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

**Detailed Walkthrough:**
- Identical to CE1 Question 1
- Find all pattern occurrences and print each index

---

### MCQs for Topic 4: String Algorithms

**Q1:** What is commonly used as the base value (d) in the hash function for Rabin-Karp?
- **Answer: 256**

**Q2:** The naive pattern searching algorithm is an in place algorithm.
- **Answer: True**

**Q3:** What is a Rabin and Karp Algorithm?
- **Answer: String Matching Algorithm**

**Q4:** Who created the Rabin Karp Algorithm?
- **Answer: Richard Karp and Michael Rabin**

**Q5:** What does the Rabin-Karp algorithm do if the length of the pattern is greater than the length of the text?
- **Answer: It immediately returns no matches found**

**Q6:** What is the worst-case running time of the Rabin Karp Algorithm?
- **Answer: Theta((n-m+1)m)**

**Q7:** If the expected number of valid shifts is small and modulus is larger than the length of pattern what is the matching time of Rabin Karp Algorithm?
- **Answer: Big-Oh(n+m)**

**Q8:** What is the basic formula applied in Rabin Karp Algorithm to get the computation time as Theta(m)?
- **Answer: Horner's rule**

**Q9:** What is the basic principle in Rabin Karp algorithm?
- **Answer: Hashing**

**Q10:** What is the time complexity of Z algorithm for pattern searching (m = length of text, n = length of pattern)?
- **Answer: O(n + m)**

**Q11:** What is the pre-processing time of Rabin and Karp Algorithm?
- **Answer: Theta(m)**

**Q12:** In the Rabin-Karp algorithm, what role does the hash function play?
- **Answer: It computes a hash value for the pattern and substrings to enable quick comparisons**

**Q13:** What is the auxiliary space complexity of Z algorithm for pattern searching (m = length of text, n = length of pattern)?
- **Answer: O(m + n)**

**Q14:** What is the total complexity of NAIVE-STRING-MATCHER?
- **Answer: O(n-m+1)**

**Q15:** How does the Rabin-Karp algorithm handle hash collisions when multiple substrings have the same hash value?
- **Answer: It uses a secondary verification step to compare substrings that have the same hash value**

---

## TOPIC 5: STRING ALGORITHMS 2

### CE1 - Question 1: Shortest Palindrome (KMP)

**Problem:** Find shortest palindrome by adding characters to beginning of string.

**Input Format:**
- Line 1: String `s`

**Output Format:**
- Line 1: Shortest palindrome string

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

**Detailed Walkthrough:**
1. Read input string `s`
2. **Find longest palindromic prefix:**
   - Check substrings from full length down to 1
   - For each length i from n to 1:
     - Check if s.substr(0, i) is palindrome
     - If yes, this is longest palindromic prefix, break
3. **Build shortest palindrome:**
   - Remaining suffix = s.substr(i) (non-palindromic part)
   - Reverse this suffix
   - Prepend reversed suffix to original string
4. **Why this works:** Adding reversed suffix makes entire string palindromic

**Example:** s="aacecaaa"
- Longest palindromic prefix: "aacecaa" (length 7)
- Remaining suffix: "a"
- Reversed suffix: "a"
- Result: "a" + "aacecaaa" = "aaacecaaa"

---

### CE1 - Question 2: KMP Pattern Search

**Problem:** Standard pattern matching with "Found pattern" message.

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

**Detailed Walkthrough:**
- Same as Topic 4 CE1, but with "Found pattern" instead of "Pattern found"
- Find all occurrences and print indices

---

### CE2 - Question 1: Longest Palindromic Substring (Manacher's)

**Problem:** Find longest palindromic substring using expand-around-center approach.

**Input Format:**
- Line 1: Integer `T` (test cases)
- Next `T` lines: One string per line

**Output Format:**
- For each test case: longest palindromic substring

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

**Detailed Walkthrough:**
1. Read number of test cases
2. **For each test case:**
   - Read string
   - Call getPal function
   - Print result
3. **getPal function (Expand Around Center):**
   - Handle empty string edge case
   - For each position i (potential center):
     - **Odd-length palindromes:** Expand with center at i
       - Start with l=i, r=i
       - While characters match: expand outward (l--, r++)
       - Track longest palindrome found
     - **Even-length palindromes:** Expand with center between i and i+1
       - Start with l=i, r=i+1
       - While characters match: expand outward
       - Track longest palindrome found
   - Return substring from stored start position with maxLength

**Example:** s="abaca"
- At i=1: expand "b" → no expansion
- At i=2: expand "a" → "aba" (length 3)
- Result: "aba"

---

### CE2 - Question 2: Longest Palindromic Substring (Numeric)

**Problem:** Same algorithm without test case wrapper.

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

**Detailed Walkthrough:**
- Identical expand-around-center logic
- Single input string without test case loop

---

### CE3 - Question 1: Boyer-Moore (Count)

**Problem:** Count pattern occurrences using find.

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

**Detailed Walkthrough:**
- Standard counting algorithm
- Find all occurrences and increment counter

---

### CE3 - Question 2: Boyer-Moore (Reverse Count)

**Problem:** Count occurrences of reversed pattern in text.

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

**Detailed Walkthrough:**
1. Read text and pattern
2. **Reverse pattern:** Use `reverse(p.begin(), p.end())`
3. Count occurrences of reversed pattern in text
4. Print count

**Example:** text="desserts are often stressed over", pattern="stressed"
- Reverse pattern: "desserts"
- Find "desserts" in text at position 0
- Output: 1

---

### MCQs for Topic 5: String Algorithms 2

**Q1:** What is the main purpose of Manacher's Algorithm?
- **Answer: To find the longest palindromic substring in linear time**

**Q2:** What is the worst-case running time in the searching phase of Boyer-Moore's algorithm?
- **Answer: O(mn)**

**Q3:** The Boyer-Moore algorithm is particularly efficient when
- **Answer: The pattern contains repetitive elements**

**Q4:** What is the time complexity of Manacher's Algorithm?
- **Answer: O(N)**

**Q5:** What is the primary goal of string-matching algorithms?
- **Answer: To find the occurrence of a specific pattern within a text**

**Q6:** The Boyer-Moore algorithm uses which two rules to determine the shift amount?
- **Answer: Good suffix rule and bad character rule**

**Q7:** What is the time complexity of the KMP pattern searching algorithm in the worst case?
- **Answer: O(n + m)**

**Q8:** What does KMP stand for in the context of pattern-searching algorithms?
- **Answer: Knuth Morris Pratt**

**Q9:** Which String Matching Algorithm is based on the concept of shifting the pattern to the right by the Maximum Possible Distance when a mismatch occurs?
- **Answer: Boyer-Moore**

**Q10:** In the context of string matching, what is a 'pattern'?
- **Answer: A sequence of characters to be searched**

**Q11:** The Knuth-Morris-Pratt algorithm is efficient for pattern matching when
- **Answer: The pattern is shorter than the text**

**Q12:** What character shift tables does Boyer-Moore's search algorithm use?
- **Answer: both good and bad character shift tables**

**Q13:** What is the best-case time complexity of the Boyer-Moore Pattern Matching algorithm?
- **Answer: O(n/m)**

**Q14:** Which of the following pattern-matching algorithms is based on the concept of "bad character rule"?
- **Answer: Boyer-Moore algorithm**

**Q15:** Which of the following algorithms formed the basis for the Quick search algorithm?
- **Answer: Boyer-Moore**

---

## TOPIC 6: BACKTRACKING 1

### CE1 - Question 1: Rat in Maze (All Paths)

**Problem:** Find all paths from (0,0) to (n-1,n-1) in maze using DLRU order.

**Input Format:**
- Line 1: Integer `n` (maze size)
- Next `n` lines: `n` space-separated integers (0=blocked, 1=open)

**Output Format:**
- All valid paths in lexicographical order, space-separated
- OR -1 if no path exists

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

**Detailed Walkthrough:**
1. Read maze size `n` and maze grid (0=blocked, 1=open)
2. **DFS Backtracking:**
   - **Base cases:**
     - Out of bounds: return
     - Cell blocked (m[r][c]=0): return
     - Cell already visited: return
     - Reached destination (n-1, n-1): store path and return
   - **Recursive exploration:**
     - Mark current cell as visited
     - Try all 4 directions in DLRU order:
       - D (Down): row+1
       - L (Left): col-1
       - R (Right): col+1
       - U (Up): row-1
     - After exploring all directions, **backtrack**: unmark cell as visited
3. **Sort and output:**
   - Sort paths lexicographically
   - Print all paths or -1 if none found

**Example:** 4×4 maze
- Path 1: DDRDRR (Down, Down, Right, Down, Right, Right)
- Path 2: DRDDRR (Down, Right, Down, Down, Right, Right)

---

### CE1 - Question 2: Rat in Maze (One Path)

**Problem:** Find any single valid path, show as solution matrix.

**Input Format:**
- Line 1: Integer `n` (maze size)
- Next `n` lines: `n` space-separated integers (maze)

**Output Format:**
- Solution matrix with 1s showing path, 0s elsewhere
- OR "No path found"

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

**Detailed Walkthrough:**
1. Read maze size and grid
2. Initialize solution matrix `sol` with all 0s
3. **Backtracking algorithm:**
   - **Base cases:**
     - Out of bounds/blocked/visited: return false
     - Mark cell as part of path: sol[r][c] = 1
     - If destination reached: return true
   - **Try all directions:**
     - Try Down, Right, Up, Left
     - Use OR logic: if ANY direction returns true, path found
   - **Backtrack if all fail:**
     - Unmark cell: sol[r][c] = 0
     - Return false
4. **Output:**
   - If path found: print solution matrix
   - Otherwise: print "No path found"

**Example:** Input 4×4 maze
- Solution matrix shows path with 1s:
```
1 0 0 0
1 1 0 0
0 1 0 0
0 1 1 1
```

---

### CE2 - Question 1 & 2: Integer Partition

**Problem:** Find all ways to partition integer into sum of positive integers.

**Input Format:**
- Line 1: Integer `n` (number to partition)

**Output Format:**
- List of all partitions in format: [[partition1], [partition2], ...]
- Partitions in non-decreasing order

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

**Detailed Walkthrough:**
1. Read integer `n` to partition
2. **Recursive partitioning:**
   - Parameters: original n, remaining sum, minimum value to use
   - **Base case:** If remaining sum is 0, store current partition
   - **Recursive case:**
     - Try all values from `start` to `rem`
     - Add value to current partition
     - Recurse with reduced remaining sum
     - Use same `i` as new start (allows repeating numbers)
     - **Backtrack:** Remove last added value
3. **Why start parameter:** Ensures partitions are non-decreasing
   - Avoids duplicates like [1,2] and [2,1]
4. **Output formatting:** Nested list notation

**Example:** n=4
- Partitions: [[1,1,1,1], [1,1,2], [1,3], [2,2], [4]]

---

### CE3 - Question 1: N-Queens (Pre-placed)

**Problem:** Solve N-Queens with some queens already placed.

**Input Format:**
- Line 1: Two integers `n` and `k` (board size, pre-placed queens)
- Next `k` lines: Two integers each (row, column of pre-placed queen)

**Output Format:**
- Solution board with 1s for queens, 0s for empty
- OR "No solution found."

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

**Detailed Walkthrough:**
1. Read board size `n` and number of pre-placed queens `k`
2. Read positions of k pre-placed queens
3. **Validate pre-placed queens:**
   - Check each pre-placed position with isSafe
   - If any invalid: print "No solution found" and exit
   - Place all valid pre-placed queens on board
4. **isSafe function:** Check if position is safe
   - Check entire row and column
   - Check all four diagonals
5. **Solve function (row by row):**
   - **Base case:** If r == n, all rows filled, return true
   - **Check for pre-placed queen:** If row already has queen, skip to next row
   - **Try placing queen:**
     - For each column in current row:
       - If position safe: place queen, recurse
       - If recursion succeeds: return true
       - Otherwise: remove queen (backtrack)
6. Print solution board or failure message

**Example:** n=4, k=1, pre-placed at (0,1)
- Solution:
```
0 1 0 0
0 0 0 1
1 0 0 0
0 0 1 0
```

---

### CE3 - Question 2: N-Queens (Fixed First)

**Problem:** Find all solutions with first queen at given position.

**Input Format:**
- Line 1: Three integers `n`, `r`, `c` (board size, first queen position)

**Output Format:**
- All valid solutions, each on separate lines
- Use 'Q' for queen, '.' for empty
- Blank line between solutions
- OR "No solution"

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

**Detailed Walkthrough:**
1. Read board size and first queen position (r, c)
2. Initialize board with '.' (empty), place 'Q' at (r, c)
3. **isSafe function (optimized):**
   - Only check rows ABOVE current (already placed)
   - Check column for existing queen
   - Check diagonals for conflicts
4. **Solve function:**
   - **Base case:** r == n, store complete solution
   - **Check for fixed queen:** If row has pre-placed 'Q', verify safe and continue
   - **Try placing queens:**
     - For each column: if safe, place 'Q', recurse, backtrack
5. **Collect all solutions:** Store in sols vector
6. **Output all solutions:**
   - Print each solution with blank line separator
   - Use 'Q' and '.' notation

**Example:** n=4, first queen at (0,1)
- Solution:
```
. Q . .
. . . Q
Q . . .
. . Q .
```

---

### MCQs for Topic 6: Backtracking 1

**Q1:** Placing N-queens so that no two queens attack each other is called ________.
- **Answer: N-queen's problem**

**Q2:** Which of the following methods can be used to solve N-Queen's problem?
- **Answer: Backtracking**

**Q3:** Which of the following best describes the backtracking approach to generating permutations of a string?
- **Answer: Fix one character, recursively permute the rest, and backtrack**

**Q4:** For N Queens problem, the time complexity is
- **Answer: O(N!)**

**Q5:** Which of the following is not a backtracking algorithm?
- **Answer: Tower of hanoi**

**Q6:** The backtracking algorithm is implemented by constructing a tree of choices known as?
- **Answer: State-space tree**

**Q7:** In generating all combinations of size r from n elements using backtracking, when do we stop recursion and store the result?
- **Answer: When the current combination size becomes r**

**Q8:** The "Eight Queens Puzzle" involves placing 8 queens on an 8x8 chessboard such that no two queens threaten each other. How many solutions are there for this problem?
- **Answer: 92**

**Q9:** In the classic Rat in a Maze problem, what does a cell value of '0' usually represent?
- **Answer: Blocked cell (not traversable)**

**Q10:** What is the time complexity of the algorithm commonly used to solve the N-Queens problem using backtracking?
- **Answer: O(N!)**

**Q11:** What is the primary technique used to solve the Rat in a Maze problem?
- **Answer: Backtracking**

**Q12:** In general, backtracking can be used to solve?
- **Answer: Combinatorial problems**

**Q13:** What happens when the backtracking algorithm reaches a complete solution?
- **Answer: It continues searching for other possible solutions**

**Q14:** In backtracking solutions for maze problems, when do we backtrack from a cell?
- **Answer: When all directions from the current cell are either visited or blocked**

**Q15:** Backtracking may lead to a solution that is ________.
- **Answer: suboptimal**

---

## TOPIC 7: BACKTRACKING 2

### CE1 - Question 1: Minimum Knight Moves

**Problem:** Find minimum moves for knight from (0,0) to (x,y).

**Input Format:**
- Line 1: Two integers `x` and `y` (target position)

**Output Format:**
- Line 1: Minimum number of moves

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

**Detailed Walkthrough:**
1. Read target position (x, y)
2. **Define knight moves:** 8 possible L-shaped moves
   - Arrays dx, dy represent all 8 directions
3. **BFS algorithm:**
   - Start from (0, 0) with distance 0
   - Use queue for BFS traversal
   - Use map to track distance to each position
4. **Process each position:**
   - Dequeue current position
   - If target reached: print distance and exit
   - Try all 8 knight moves:
     - Calculate new position
     - If not visited: record distance, enqueue
5. **BFS guarantees shortest path:** First path to reach target is optimal

**Example:** Target (2, 4)
- Move 1: (0,0) → (1,2) or (2,1)
- Move 2: (1,2) → (2,4) or (2,1) → (1,3) or (0,3)
- Minimum: 2 moves

---

### CE1 - Question 2: Knight Probability

**Problem:** Calculate probability knight stays on board after k moves.

**Input Format:**
- Line 1: Four integers `n k r c` (board size, moves, start position)

**Output Format:**
- Line 1: Probability (5 decimal places)

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

**Detailed Walkthrough:**
1. Read board size `n`, number of moves `k`, starting position (r, c)
2. **Recursive probability calculation with memoization:**
   - **Base cases:**
     - If off board: return 0.0 (failure)
     - If k=0 (no moves left) and on board: return 1.0 (success)
   - **Memoization:** Check if state already computed
   - **Recursive case:**
     - Try all 8 knight moves
     - Sum probabilities from all moves
     - Each move has 1/8 probability
     - Return average: sum / 8.0
3. **Memoization key:** Tuple of (remaining moves, row, col)
4. Print probability with 5 decimal precision

**Example:** n=3, k=2, start=(0,0)
- Probability that knight stays on 3×3 board after 2 random moves
- Many paths go off-board, low probability

---

### CE2 - Question 1: Subset Sum (Find First)

**Problem:** Find first subset that sums to target.

**Input Format:**
- Line 1: Integer `n` (number of items)
- Line 2: `n` space-separated integers (item values)
- Line 3: Integer `t` (target sum)

**Output Format:**
- Line 1: Expression result (target × 2 - 1)
- Line 2: First subset that sums to target, space-separated
- OR "No combination of items"

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

**Detailed Walkthrough:**
1. Read number of items `n`, item values, target sum `t`
2. Print mathematical expression: t × 2 - 1
3. **Backtracking subset sum:**
   - **Base cases:**
     - If target becomes 0: found valid subset, return true
     - If target negative or no items left: return false
   - **Try including current item:**
     - Add to result vector
     - Recurse with reduced target and next index
     - If successful: return true (stop searching)
     - Otherwise: backtrack (remove from result)
   - **Try excluding current item:**
     - Recurse without adding item
4. Print first found subset or failure message

**Example:** items=[10, 20, 30], target=50
- Expression: 50 × 2 - 1 = 99
- Subset: [20, 30]

---

### CE2 - Question 2: Count Subsets

**Problem:** Count all subsets that sum to target, apply formula.

**Input Format:**
- Line 1: Integer `n` (number of items)
- Line 2: `n` space-separated integers (item values)
- Line 3: Integer `t` (target sum)

**Output Format:**
- Line 1: Count of valid subsets
- Line 2: Count × 3 if even, Count + 7 if odd

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

**Detailed Walkthrough:**
1. Read items and target
2. **Count subsets recursively:**
   - **Base cases:**
     - Target = 0: return 1 (found one way)
     - Target < 0 or no items: return 0 (no way)
   - **Recursive case:**
     - Count including current item
     - Count excluding current item
     - Return sum of both counts
3. **Apply formula:**
   - If count is even: multiply by 3
   - If count is odd: add 7
4. Print count and formula result

**Example:** items=[1, 3, 5, 2], target=6
- Subsets: {1,5}, {1,3,2}
- Count: 2
- Formula: 2 × 3 = 6

---

### CE3 - Question 1 & 2: m-Coloring

**Problem:** Color graph with m colors using backtracking.

**Input Format:**
- Line 1: Integer `v` (number of vertices)
- Next `v` lines: `v` space-separated integers (adjacency matrix)
- Last line: Integer `m` (number of colors)

**Output Format:**
- Line 1: Color assignment for each vertex, space-separated
- OR "Solution does not exist"

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

**Detailed Walkthrough:**
1. Read number of vertices `v`
2. Read adjacency matrix (1=edge exists, 0=no edge)
3. Read number of colors `m` available
4. **isSafe function:** Check if color `c` can be assigned to vertex `i`
   - For each vertex j: if edge exists (g[i][j]=1) and j has color c, return false
   - Otherwise safe, return true
5. **Backtracking coloring:**
   - **Base case:** If all vertices colored (i == v), return true
   - **Try each color 1 to m:**
     - If color safe for vertex i: assign it
     - Recurse to next vertex
     - If successful: return true
     - Otherwise: backtrack (reset color to 0)
   - If no color works: return false
6. Print color assignment or failure message

**Example:** 4 vertices, complete graph, m=3
- Need at least 4 colors for complete graph
- With m=3: "Solution does not exist"

---

### MCQs for Topic 7: Backtracking 2

**Q1:** In Backtracking, what happens when all possible paths are explored and the graph is still not connected?
- **Answer: The graph is considered disconnected**

**Q2:** How many unique colors will be required for proper vertex coloring of an empty graph with n vertices?
- **Answer: 1**

**Q3:** What is the worst case time complexity of dynamic programming solution of the subset sum problem (sum=given subset sum)?
- **Answer: O(sum*n)**

**Q4:** How many unique colors will be required for the vertex coloring of a complete graph with 4 vertices?
- **Answer: 4**

**Q5:** What is the minimum number of unique colors required for vertex coloring of a graph called?
- **Answer: Chromatic number**

**Q6:** Which of the following is not true about the subset sum problem?
- **Answer: the dynamic programming solution has a time complexity of O(n log n)**

**Q7:** How many unique colors will be required for proper vertex coloring of a line graph having n vertices?
- **Answer: 2**

**Q8:** What is the vertex coloring of a graph?
- **Answer: A condition where any two vertices having a common edge should not have same color**

**Q9:** For the set {5, 6, 11, 13, 20} and a target sum of 26, which of the following subsets will sum to the target value?
- **Answer: {6, 20}**

**Q10:** In Backtracking, what is the purpose of marking vertices as visited during graph traversal?
- **Answer: To avoid revisiting the same vertex**

**Q11:** If you want to solve the subset sum problem for a set with duplicate elements, what approach should you use?
- **Answer: Use a modified version of dynamic programming to handle duplicates.**

**Q12:** What is the main idea behind Backtracking to check if a graph is connected?
- **Answer: Trying all possible paths from a starting vertex and marking visited vertices**

**Q13:** For the set {1, 2, 3, 6} and target sum 5, which of the following subsets sums to the target value?
- **Answer: {2, 3}**

**Q14:** How many unique colors will be required for proper vertex coloring of a complete graph having n vertices?
- **Answer: n**

**Q15:** What is a subset sum problem?
- **Answer: Checking for the presence of a subset that has the sum of elements equal to a given number and printing true or false based on the result**

---

## TOPIC 8: BACKTRACKING 3

### CE1 - Question 1: Sudoku Solver (11-19)

**Problem:** Solve Sudoku using numbers 11-19 instead of 1-9.

**Input Format:**
- 9 lines: 9 space-separated integers each (0=empty, 11-19 for filled)

**Output Format:**
- 9 lines: Complete Sudoku board with all cells filled

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
                for (int n = 11; n <= 19; n++) {
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

**Detailed Walkthrough:**
1. Read 9×9 Sudoku board (0=empty, 11-19 for filled cells)
2. **isSafe function:** Check if number `n` can be placed at (r, c)
   - Check entire row r for number n
   - Check entire column c for number n
   - **Check 3×3 box:**
     - Calculate box top-left corner: (br, bc)
     - br = r - (r % 3), bc = c - (c % 3)
     - Check all 9 cells in box for number n
   - Return true if n not found in row, column, or box
3. **Solve function (backtracking):**
   - Scan board row by row, column by column
   - When empty cell (0) found:
     - **Try numbers 11-19:** (change to 1-9 for standard Sudoku)
       - If safe: place number, recurse
       - If solve succeeds: return true
       - Otherwise: backtrack (reset to 0)
     - If no number works: return false
   - **Base case:** If no empty cells found, puzzle solved
4. Print completed board

**Example:** Input with some cells filled (11-19), output has all cells filled

---

### CE1 - Question 2: Sudoku Solver (1-9)

**Problem:** Standard Sudoku solver with numbers 1-9.

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

**Detailed Walkthrough:**
- Identical to Q1 but uses numbers 1-9 instead of 11-19
- Change loop: `for (int n = 1; n <= 9; n++)`
- All other logic remains same

---

### CE2 - Question 1 & 2: Prime Sum

**Problem:** Find all ways to sum to target using primes greater than threshold.

**Input Format:**
- Line 1: Integer `P` (threshold prime)
- Line 2: Integer `S` (target sum)

**Output Format:**
- Line 1: "Valid combinations:"
- Following lines: Each valid combination, space-separated
- OR "No valid combination found."

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

**Detailed Walkthrough:**
1. Read threshold `P` and target sum `S`
2. **Generate primes > P and ≤ S:**
   - Use Sieve of Eratosthenes
   - Mark composites as false
   - Collect primes from P+1 to S
3. **Find combinations:**
   - **Base cases:**
     - Target = 0: store current combination
     - Target < 0 or no primes left: return
   - **Recursive case:**
     - For each prime from current index:
       - Add prime to current combination
       - Recurse with reduced target and next index
       - Backtrack (remove prime)
   - **Note:** Using i+1 prevents reusing same prime
4. **Output:**
   - Print header "Valid combinations:"
   - Print all combinations or "No valid combination found."

**Example:** P=5, S=80
- Primes > 5: 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, ...
- Find all combinations that sum to 80
- Example: [7, 73], [7, 11, 19, 43], etc.

---

### CE3 - Question 1: Hamiltonian Cycle (Find One)

**Problem:** Find any Hamiltonian cycle starting from vertex 0.

**Input Format:**
- Line 1: Integer `v` (number of vertices)
- Next `v` lines: `v` space-separated integers (adjacency matrix)

**Output Format:**
- "Solution found" followed by the cycle (vertices then 0)
- OR "No solution"

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

**Detailed Walkthrough:**
1. Read number of vertices and adjacency matrix
2. **Hamiltonian cycle:** Visit each vertex exactly once, return to start
3. **Solve function:**
   - Parameters: current vertex `i`, position in path `c`
   - Store vertex in path, mark as visited
   - **Base case:** If all vertices visited (c == v-1):
     - Check if edge exists from last vertex back to vertex 0
     - Return true if cycle possible
   - **Recursive case:**
     - Try each unvisited adjacent vertex
     - Recurse with that vertex and incremented position
     - If successful: return true
   - **Backtrack:** Unmark vertex as visited, return false
4. **Output:**
   - If solution found: print "Solution found", then path, then 0 (complete cycle)
   - Otherwise: print "No solution"

**Example:** 8-vertex graph
- Find path: 0 → 1 → 2 → 3 → 7 → 6 → 5 → 4 → 0
- Output: "Solution found\n0 1 2 3 7 6 5 4 0"

---

### CE3 - Question 2: All Hamiltonian Cycles

**Problem:** Find all Hamiltonian cycles with vertex names.

**Input Format:**
- Line 1: Integer `v` (number of vertices)
- Next `v` lines: One string each (vertex names)
- Next `v` lines: `v` space-separated integers (adjacency matrix)

**Output Format:**
- All cycles in format: ['name1', 'name2', ..., 'name1']
- OR "No Hamiltonian Cycle"

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

**Detailed Walkthrough:**
1. Read number of vertices, vertex names, adjacency matrix
2. **Find all cycles (not just first):**
   - When complete path found with edge back to start:
     - Store path in solutions vector
     - Continue exploring (don't return immediately)
   - Backtrack to find alternative paths
3. **Output formatting:**
   - For each solution:
     - Print in format: ['name1', 'name2', ..., 'name1']
     - Use actual vertex names instead of indices
     - Add starting vertex name at end to complete cycle

**Example:** 8 vertices named a, b, c, d, e, f, g, h
- Output: ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'a']
- May have multiple valid cycles

---

### MCQs for Topic 8: Backtracking 3

**Q1:** When solving Prime Sum using backtracking, what is the benefit of sorting the list of primes?
- **Answer: Allows early stopping when the current sum exceeds target**

**Q2:** What is a base case in a backtracking algorithm?
- **Answer: A condition when a solution is found or invalid**

**Q3:** In the Hamiltonian Cycle problem, what does "complete graph" refer to?
- **Answer: A graph where every vertex is connected to every other vertex.**

**Q4:** Why is backtracking preferred for solving constraint satisfaction problems like Sudoku?
- **Answer: It avoids exploring invalid paths early**

**Q5:** In the Hamiltonian Cycle problem, if a graph contains a Hamiltonian cycle, it is called a
- **Answer: Hamiltonian graph**

**Q6:** In the Hamiltonian Cycle problem using backtracking, what must be true for a path to be valid?
- **Answer: All vertices are included once, and the last connects to the first**

**Q7:** Which data structure is commonly used in backtracking to store partial solutions?
- **Answer: Stack or List**

**Q8:** In a Sudoku solver, when does the backtracking algorithm backtrack?
- **Answer: When no valid number can be placed in a cell**

**Q9:** The problem of finding a path in a graph that visits every vertex exactly once is called?
- **Answer: Hamiltonian path problem**

**Q10:** Which of the following problems can not be solved using backtracking?
- **Answer: Finding shortest path in weighted graph**

**Q11:** What is the primary difficulty in finding a Hamiltonian Path in a general graph?
- **Answer: It is an NP-complete problem.**

**Q12:** Which of the following is essential for backtracking to function correctly?
- **Answer: A recursive call with a rollback (backtrack step)**

**Q13:** Which of the following constraints are typically used in Sudoku backtracking?
- **Answer: All the mentioned options** (Row, Column, and 3x3 grid must contain unique digits)

**Q14:** Which algorithmic paradigm is commonly used to solve the Hamiltonian Cycle problem?
- **Answer: Backtracking**

**Q15:** In a Sudoku solver using backtracking, what is the first step when solving an empty cell?
- **Answer: Try all digits 1-9 and validate**

---

## Summary

This comprehensive guide covers all 8 topics with:
- **Detailed problem statements** with input/output formats
- **Complete working code** solutions
- **Step-by-step walkthroughs** explaining logic
- **Examples** demonstrating the algorithms
- **All 120 MCQs** organized by topic with correct answers

Each topic includes multiple CE questions with varying difficulty levels and comprehensive explanations to help you understand the concepts thoroughly for your CIA 1 exam.
