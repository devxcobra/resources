stack space ke bare me padhna hai
auxillary space complexity

what exactly is backtracking.. ek bar ache se padhna hai

pow(x, n)

```cpp

    double myPow(double x, int n) {

    if (x==0) return 0;

    long long nn = n; // long long take to encounter -ve integer overflow

    if(nn<0) nn = (-1)*nn;

    double ans  = 1;

    while(nn)

    {
        if(nn%2 == 1)
        {
            ans = ans * x;
            nn = nn- 1;
        }
        else
        {
            x = x * x;
            nn = nn/2;
        }
    }
    if(n<0)
    {
        ans = 1/ans;
    }
    
    return ans;
    
    }

```


ATOI - ghot ke peeja isse
learnings - read the question carefully - non digit character pe break karna hai
dont hardcode too much
check the conditions in sequence

```cpp
class Solution {

public:

    const int INT_MAX_VALUE = 2147483647;
    const int INT_MIN_VALUE = -2147483648;

    bool isdigit(char c)

    {

        if(c >= '0' && c <= '9') return true;

        else return false;

    }

    int myAtoi(string s) {

        int i = 0;

        long long num = 0;

        int sign = 1;

        int n = s.length();

    // whitespace

        while(i < n && s[i] == ' ')

        {
            i++;
        }

    // sign
        if(i <n)
        {
            if(s[i] == '-')
            {
            sign = -1;
            i++;
            }

            else if (s[i] == '+') i = i+ 1;

        }

        while(i < n)
        {
        if(isdigit(s[i]))
        {  
            int val = s[i] - '0';

            num = num* 10 + val;
            i++;

            if((long long) sign * num >= INT_MAX_VALUE)

            {
                return INT_MAX_VALUE;
            }
            if((long long) sign* num <= INT_MIN_VALUE)

            {
                return  INT_MIN_VALUE;
            }
        }
        else
        {
            break;
        }
        }

      return num*sign;

    }
};
```

learnt one thing here : was checking palindrome through recursion.. it was some randi question of leetcode .. first we had to convert that string..

```cpp
class Solution {
public:

    bool ispal(int i , string &a) // used &a pass by reference.. otherwise it creates copies
  
    {   int n = a.length();

        if(i >= n/2)

        {
            return true;
        }

        if(a[i] != a[n-i -1])

        {
            return false;
        }
        return ispal(i+1, a);
    }

    bool isPalindrome(string s) {

        s = correct(s);

        return ispal(0,s);

    }

};

```

##### crazzy way to print N to 1 using recursion (backtracking)

```cpp
class Solution {
  public:
  
    void f(int i , int n)
    {
        if(i > n) return;
        
        f(i+1, n); // pehle ye return karega fir print hoga
        
        cout << i << " "; // isilye chhote wale ka print hona ruka hua hai
        
        return;
    }
    void printNos(int n) {
        
        
        f(1, n);
        
        return;
        
    }
};
```

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.
logic : always look at the previous char before adding the next
```cpp
class Solution {

public:

    void para(string curr, vector<string>& result,

              int rcount, int lcount, int count, int n)

    {
        if(curr.length() == 2*n) {

            result.push_back(curr);
            return;
        }

        if(rcount < n && count < n)
            para(curr + "(", result, rcount+1, lcount, count+1, n);

        if(lcount < n && count > 0)

            para(curr + ")", result, rcount, lcount+1, count-1, n);
    }


    vector<string> generateParenthesis(int n) {

        vector<string> result;

        string curr = "";

        para(curr, result, 0, 0, 0, n);

        return result;

    }

};
```

#### Power Set

generate all the possible strings

- Start with an empty subsequence. For each character, recursively make a decision.
- Either include it in the subsequence or exclude it from the subsequence.
- When you reach the end of the string, print the current subsequence.
TC and SC : O(n* 2^n)

![[Pasted image 20260604233543.png]]
```cpp
#include <bits/stdc++.h>
using namespace std;

// Solution class to generate all subsequences using recursion
class Solution {
public:
    // Helper recursive function to generate subsequences
    void helper(string &s, int index, string ¤t, vector<string> &result) {
        // Base case: If index reaches string length, add current subsequence to result
        if (index == s.size()) {
            result.push_back(current);
            return;
        }

        // Exclude current character and recurse
        helper(s, index + 1, current, result);

        // Include current character and recurse
        current.push_back(s[index]);
        helper(s, index + 1, current, result);

        // Backtrack: remove last character before returning to previous call
        current.pop_back();
    }

    // Function to return all subsequences of string s
    vector<string> getSubsequences(string s) {
        // Vector to store all subsequences
        vector<string> result;  
        // Current subsequence being built
        string current = "";    
        helper(s, 0, current, result);
        return result;
    }
};

int main() {
    // Input string
    string s = "abc";

    // Create Solution object
    Solution sol;

    // Get all subsequences
    vector<string> subsequences = sol.getSubsequences(s);

    // Print all subsequences
    for (auto &subseq : subsequences) {
        cout << "\"" << subseq << "\"" << endl;
    }

    return 0;
}

```


#### Printing Subsequences whose sum is k

approach : take or not take

```cpp
#include <bits/stdc++.h>  
using namespace std;  
  
void prints(int ind, vector<int> &ds, int s, int sum, int arr[], int n) {  
if (ind == n) {  
if (s == sum) {  
for (auto it : ds) cout << it << " ";  
cout << endl;  
}  
return;  
}  
  
ds.push_back(arr[ind]);  
s += arr[ind];  
  
prints(ind + 1, ds, s, sum, arr, n);  
  
s -= arr[ind];  
ds.pop_back();  
  
// not pick  
prints(ind + 1, ds, s, sum, arr, n);  
}  
  
int main() {  
#ifndef ONLINE_JUDGE  
freopen("input.txt", "r", stdin);  
freopen("output.txt", "w", stdout);  
#endif  
  
int arr[] = {1, 2, 1};  
int n = 3;  
int sum = 2;  
vector<int> ds;  
  
prints(0, ds, 0, sum, arr, n);  
  
return 0;  
}
```


##### Technique to print only one answer: 

```cpp
#include <bits/stdc++.h>  
using namespace std;  
  
bool prints(int ind, vector<int> &ds, int s, int sum, int arr[], int n) {  
if (ind == n) {  
if (s == sum) {  
	for (auto it : ds) cout << it << " ";  
	cout << endl;
	return true;  } 
 
else return false;  
}  
  
ds.push_back(arr[ind]);  
s += arr[ind];  
  
if(prints(ind + 1, ds, s, sum, arr, n) == true)
{
return true;
}
  
s -= arr[ind];  
ds.pop_back();  
  
// not pick  
if(prints(ind + 1, ds, s, sum, arr, n) == true)  return true;

return false;
}  
  
int main() {  
#ifndef ONLINE_JUDGE  
freopen("input.txt", "r", stdin);  
freopen("output.txt", "w", stdout);  
#endif  
  
int arr[] = {1, 2, 1};  
int n = 3;  
int sum = 2;  
vector<int> ds;  
  
prints(0, ds, 0, sum, arr, n);  
  
return 0;  
}
```

##### modification: print the number of subsequences..

```cpp
using namespace std;  
  
int prints(int ind, vector<int> &ds, int s, int sum, int arr[], int n) {  
if (ind == n) {  
if (s == sum) return 1;
 
else return 0;  
}  
  
ds.push_back(arr[ind]);  
s += arr[ind];  
  
int l = prints(ind + 1, ds, s, sum, arr, n);
  
s -= arr[ind];  
ds.pop_back();  
  
// not pick  
int r = prints(ind + 1, ds, s, sum, arr, n);

return l + r;
}  
  
int main() {  
#ifndef ONLINE_JUDGE  
freopen("input.txt", "r", stdin);  
freopen("output.txt", "w", stdout);  
#endif  
  
int arr[] = {1, 2, 1};  
int n = 3;  
int sum = 2;  
vector<int> ds;  
  
prints(0, ds, 0, sum, arr, n);  
  
return 0;  
}
```

combination 1:

Given an array of distinct integers and a **target**, you have to return _the list of all unique combinations where the chosen numbers sum to_ target_._ You may return the combinations in any order.

The same number may be chosen from the given array an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

approach:

Whenever the problem is related to picking up elements from an array to form a combination, start thinking about the **“pick and non-pick”** approach.

We use a recursive backtracking approach to find all combinations that sum up to the target.

- We define a recursive function with the following parameters:
    - Index — current position in the array.
    - Target — remaining sum we need to achieve.
    - DS (data structure) — to store the current combination.
- At every step, we have two choices:
    - **Pick** the element at the current index:
        - We reduce the target by `arr[index]`.
        - Add `arr[index]` to the DS.
        - We stay on the same index since we can reuse the same element.
    - **Not pick** the element:
        - We move to the next index.
        - Target remains unchanged.
        - Element is not added to the DS.
- While backtracking, remove the last inserted element to explore new paths.
- This process is repeated while `index < array.size()` for a given recursion call.
- We can optionally stop recursion when `target == 0`, but here we allow the recursion to run fully for generalization.
```cpp
#include<bits/stdc++.h>
using namespace std;

class Solution {
  public:
    // Function to find all combinations of numbers that sum up to the target
    void findCombination(int ind, int target, vector<int>& arr, vector<vector<int>>& ans, vector<int>& ds) {
        // Base case: if we have considered all elements in the array
        if (ind == arr.size()) {
            // If the target is zero, we have found a valid combination
            if (target == 0) {
                ans.push_back(ds);  // Add the current combination to the result
            }
            return;
        }

        // Recursive case: pick the element if it's less than or equal to the target
        if (arr[ind] <= target) {
            ds.push_back(arr[ind]);  // Add the current element to the combination
            findCombination(ind, target - arr[ind], arr, ans, ds);  // Continue with the same index to allow repeated elements
            ds.pop_back();  // Backtrack by removing the last added element
        }

        // Skip the current element and move to the next index
        findCombination(ind + 1, target, arr, ans, ds);
    }

  public:
    // Main function to get all combinations
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> ans;  // To store the result
        vector<int> ds;  // To store a current combination
        findCombination(0, target, candidates, ans, ds);  // Start the recursive search
        return ans;  // Return all valid combinations
    }
};

int main() {
    Solution obj;
    vector<int> v {2, 3, 6, 7};  // Candidate numbers
    int target = 7;  // Target sum

    // Get all combinations
    vector<vector<int>> ans = obj.combinationSum(v, target);

    // Output the combinations
    cout << "Combinations are: " << endl;
    for (int i = 0; i < ans.size(); i++) {
        for (int j = 0; j < ans[i].size(); j++) {
            cout << ans[i][j] << " ";  // Print each element of the combination
        }
        cout << endl;  // Print a newline after each combination
    }

    return 0;
}

```


Combination II: 

Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sum to target. Each number(element) in candidates may only be used once in the combination.

also isme ek tarah ke combinations nahi aa sakte jaise 112 aur 121 .. isiliye sort kar rahe hai
lexographically dena hai answer..


brute force:

combination I jaise hi take aur not take karna hai but set banana badega because of unique combinations ... usko store karne ki complexity logn type hoti hai.. not optimal..

optimal approach:

- Sort the array before starting recursion to ensure combinations are in sorted order and to avoid duplicates.
- Begin recursion from index 0 and explore each element for inclusion in the current combination.
- If the current element is suitable (≤ target), add it to the combination and move to the next index.
- Skip over duplicate elements to avoid generating the same combination again.
- After the recursive call, backtrack by removing the last added element from the combination.
- Terminate early if the current element exceeds the target, as further elements (being sorted) will only be larger.
![[Pasted image 20260606184040.png]]

```cpp
#include<bits/stdc++.h>
using namespace std;

// Function to find all combinations of numbers that sum up to the target
void findCombination(int ind, int target, vector<int>& arr, vector<vector<int>>& ans, vector<int>& ds) {
    // Base case: If the target becomes 0, we found a valid combination
    if (target == 0) {
        ans.push_back(ds);  // Add the current combination to the result
        return;
    }

    // Loop through the elements starting from index 'ind'
    for (int i = ind; i < arr.size(); i++) {
        // Skip duplicates to avoid repeating combinations
        if (i > ind && arr[i] == arr[i - 1]) continue;

        // If the current element is greater than the remaining target, break the loop
        if (arr[i] > target) break;

        // Include the current element in the combination
        ds.push_back(arr[i]);

        // Recur with the updated target and next index (i + 1 to avoid repetition)
        findCombination(i + 1, target - arr[i], arr, ans, ds);

        // Backtrack by removing the last added element
        ds.pop_back();
    }
}

// Function to calculate all unique combinations that sum up to the target
vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
    sort(candidates.begin(), candidates.end());  // Sort the candidates to handle duplicates
    vector<vector<int>> ans;  // To store the final answer
    vector<int> ds;  // To store the current combination
    findCombination(0, target, candidates, ans, ds);  // Call the helper function
    return ans;  // Return all valid combinations
}

int main() {
    // Example input
    vector<int> v{10, 1, 2, 7, 6, 1, 5};

    // Get all combinations that sum up to 8
    vector<vector<int>> comb = combinationSum2(v, 8);

    // Output the combinations
    cout << "[ ";
    for (int i = 0; i < comb.size(); i++) {
        cout << "[ ";
        for (int j = 0; j < comb[i].size(); j++) {
            cout << comb[i][j] << " ";
        }
        cout << "]";
    }
    cout << " ]";

    return 0;
}

```
**Time Complexity:** O(2n * k), For each of the 2n subsequences, storing takes O(k) time where k is the average length of each combination.

**Space Complexity:** O(k * x), To store all x valid combinations, each of average length k.


.push_back() is O(1)

set .insert() is O(logN)

## Palindrome Partitioning 

```cpp
class Solution {
public:
    vector<vector<string>> partition(string s) {
        vector<vector<string>> res;
        vector<string> path;
        func(0, s, path, res);
        return res;
    }

    void func(int index, string s, vector<string> &path,
              vector<vector<string>> &res) {

        if (index == s.size()) {
            res.push_back(path);
            return;
        }

        for (int i = index; i < s.size(); ++i) {
            if (isPalindrome(s, index, i)) {
                path.push_back(s.substr(index, i - index + 1));
                func(i + 1, s, path, res);
                path.pop_back();
            }
        }
    }

    bool isPalindrome(string s, int start, int end) {
        while (start <= end) {
            if (s[start++] != s[end--])
                return false;
        }
        return true;
    }
};
```

## N Queens

```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    // Function to solve N-Queens problem
    void solve(int col, vector<string>& board, int n,
               vector<int>& leftRow, vector<int>& upperDiagonal, vector<int>&                      lowerDiagonal,
               vector<vector<string>>& ans) {
        // If all queens are placed
        if (col == n) {
            ans.push_back(board);
            return;
        }

        // Iterate through all rows
        for (int row = 0; row < n; row++) {
            // Check if it's safe to place the queen
            if (leftRow[row] == 0 && lowerDiagonal[row + col] == 0 &&
                upperDiagonal[n - 1 + col - row] == 0) {

                // Place the queen
                board[row][col] = 'Q';

                // Mark the row and diagonals
                leftRow[row] = 1;
                lowerDiagonal[row + col] = 1;
                upperDiagonal[n - 1 + col - row] = 1;

                // Recurse to next column
                solve(col + 1, board, n, leftRow, upperDiagonal, lowerDiagonal,                     ans);

                // Backtrack and remove the queen
                board[row][col] = '.';
                leftRow[row] = 0;
                lowerDiagonal[row + col] = 0;
                upperDiagonal[n - 1 + col - row] = 0;
            }
        }
    }

    // Main function
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> ans;
        vector<string> board(n, string(n, '.'));
        vector<int> leftRow(n, 0), upperDiagonal(2 * n - 1, 0), lowerDiagonal(2 *        n - 1, 0);
        solve(0, board, n, leftRow, upperDiagonal, lowerDiagonal, ans);
        return ans;
    }
};

int main() {
    Solution obj;
    int n = 4;
    vector<vector<string>> res = obj.solveNQueens(n);
    for (auto& board : res) {
        for (auto& row : board) {
            cout << row << "\n";
        }
        cout << "\n";
    }
    return 0;
}

```

Word Break
Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

Gives TLE using recursion only.. needs DP

```cpp
class Solution {

public:

    bool solve(int index, string &s, unordered_set<string> &dict) {
        // base case
        if (index == s.length()) return true;

        string temp = "";

        for (int i = index; i < s.length(); i++) {

            temp.push_back(s[i]);  // build substring

            if (dict.count(temp)) {

                if (solve(i + 1, s, dict)) {

                    return true;
                }
            }
        }
        return false;

    }

    bool wordBreak(string s, vector<string>& wordDict) {

        unordered_set<string> dict(wordDict.begin(), wordDict.end());

        return solve(0, s, dict);

    }

};
```

Note:

```
find(wordDict.begin(), wordDict.end(), temp)
```

- Time = **O(n)** (linear search)
- For every substring, you scan the whole list 😬

---

### If you use `unordered_set<string>`:

```
dict.count(temp)
```

- Time = **O(1)** (average)
- Direct hash lookup 🚀


