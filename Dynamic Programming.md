
we apply dp where we apply recursion

it reduces the space and time complexity

Fibonacci numbers:
to find the nth fibnoacci:
recursion:
TC - 2^N
SC - O(N)

memoization:
TC - O(N)
SC - O(N) + O(N) ( ARRAY + STACK SPACE)
```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    // Function to calculate Fibonacci using memoization
    int fib(int n, vector<int>& dp) {
        // If base case return n
        if (n <= 1) return n;

        // If already computed, return stored value
        if (dp[n] != -1) return dp[n];

        // Otherwise compute and store
        dp[n] = fib(n - 1, dp) + fib(n - 2, dp);
        return dp[n];
    }
};

int main() {
    int n = 10;
    vector<int> dp(n + 1, -1);
    Solution sol;
    cout << sol.fib(n, dp);
    return 0;
}
```

Tabulation :
TC -  O(N)
SC -  O(N)
```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    // Function to calculate Fibonacci using tabulation
    int fib(int n) {
        // If n is 0 or 1, return n
        if (n <= 1) return n;

        // Create dp array
        vector<int> dp(n + 1, 0);

        // Initialize base cases
        dp[0] = 0;
        dp[1] = 1;

        // Fill dp array iteratively
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }

        // Return final answer
        return dp[n];
    }
};

int main() {
    int n = 10;
    Solution sol;
    cout << sol.fib(n);
    return 0;
}

```

Space optimization of Tabulation.. 

TC -  O(N)
SC -  O(1)
```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    int fib(int n) {
        // If n is 0 return 0
        if(n == 0) return 0;
        // If n is 1 return 1
        if(n == 1) return 1;

        // prev2 stores fib(n-2)
        int prev2 = 0;
        // prev stores fib(n-1)
        int prev = 1;
        // curr stores current fib
        int curr;

        // Loop from 2 to n
        for(int i = 2; i <= n; i++) {
            // Calculate current fib
            curr = prev + prev2;
            // Update prev2
            prev2 = prev;
            // Update prev
            prev = curr;
        }
        // Return final answer
        return prev;
    }
};

int main() {
    Solution s;
    int n = 10;
    cout << s.fib(n);
    return 0;
}
```


#### Maximum sum of non-adjacent elements

Given an array of N positive integers, we need to return the maximum sum of the subsequence such that no two elements of the subsequence are adjacent elements in the array.

```cpp
class Solution {

public:

    int chor(int ind , vector<int> &nums)
    {
        if(ind == 0) return nums[0];

        int take = nums[ind];  if(ind>1) {take += chor(ind -2, nums);}
        int nottake = 0 + chor(ind-1, nums);

        return max(take, nottake);

    }
  
    int rob(vector<int>& nums) {

        int n = nums.size();

        return chor(n-1, nums);

    }

};
```

dp solution :

```cpp
class Solution {

public:

    int chor(int ind , vector<int> &nums, vector<int> &dp)
    {

        if(ind == 0) return dp[0];

        if(dp[ind] != -1) return dp[ind];

        int take = nums[ind];  if(ind>1) {take += chor(ind -2, nums, dp);}

        int nottake = 0 + chor(ind-1, nums, dp);

        return dp[ind] = max(take, nottake);

    }


    int rob(vector<int>& nums) {

        int n = nums.size();

        vector<int> dp(n,-1);

        dp[0] = nums[0];

        return chor(n-1, nums, dp);

    }

};
```

## DP on grids

Ninja Training

A Ninja is undergoing a rigorous training program that lasts for $N$ days. On each day, the Ninja can choose to perform one of three different activities (for example: running, fighting, or practicing techniques).

Each activity yields a specific number of merit points, and the points for each activity can vary from day to day. You are given an $N \times 3$ 2D matrix called `points`, where `points[i][0]`, `points[i][1]`, and `points[i][2]` represent the points gained by performing activity 0, 1, and 2 on day $i$, respectively.

```cpp
// Recursive function to calculate the maximum points for the ninja training
int f(int day, int last, vector<vector<int>> &points, vector<vector<int>> &dp) {
    
    if (dp[day][last] != -1) return dp[day][last];

    // Base case:
    if (day == 0) {
        int maxi = 0;
        // Calculate the maximum points for the first day by choosing an activity
        // different from the last one
        for (int i = 0; i <= 2; i++) {
            if (i != last)
                maxi = max(maxi, points[0][i]);
        }
        // Store the result in dp array and return it
        return dp[day][last] = maxi;
    }

    int maxi = 0;
    // Iterate through the activities for the current day
    for (int i = 0; i <= 2; i++) {
        if (i != last) {
            // Calculate the points for the current activity and add it to the
            // maximum points obtained so far (recursively calculated)
            int activity = points[day][i] + f(day - 1, i, points, dp);
            maxi = max(maxi, activity);
        }
    }
    return dp[day][last] = maxi;
}

int ninjaTraining(int n, vector<vector<int>> &points) {
    // Create a memoization table (dp) to store intermediate results
    vector<vector<int>> dp(n, vector<int>(4, -1));
    // Start the recursive calculation from the last day with no previous activity
    return f(n - 1, 3, points, dp);
}
```

Grid Unique Paths
Given two integers m and n, representing the number of rows and columns of a 2d array named matrix. Return the number of unique ways to go from the top-left cell (matrix[0][0]) to the bottom-right cell (matrix [m-1][n-1]).
```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
private:
    int func(int i, int j, vector<vector<int>>& dp){
        // Base case
        if (i == 0 && j == 0)  return 1;

        /* If we go out of bounds or reach 
        a blocked cell, there are no ways.*/
        if (i < 0 || j < 0)  return 0;
        
        if (dp[i][j] != -1)  return dp[i][j];

        int up = func(i - 1, j, dp);
        int left = func(i, j - 1, dp);

        // Store the result in dp table and return it.
        return dp[i][j] = up + left;
    }
public:
    int uniquePaths(int m, int n) {
        
        vector<vector<int>> dp(m, vector<int>(n, -1));
        
        //Return the total count(0 based indexing)
        return func(m-1,n-1, dp);
    }
};

    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, -1));
        dp[0][0] = 1;
        return paths(m - 1, n - 1, dp);
    }
};

```

```cpp
class Solution {
public:
    int paths(int m, int n, vector<vector<int>>& dp) {
        if (dp[m][n] != -1) {
            return dp[m][n];
        }

        for (int i = 0; i <= m; i++) {
            for (int j = 0; j <= n; j++) {
                if (i == 0 && j == 0) {
                    continue;
                }
                int up = 0;
                int left = 0;

                if (i > 0) {
                    up = dp[i - 1][j];
                }
                if (j > 0) {
                    left = dp[i][j - 1];
                }

                dp[i][j] = up + left;
            }
        }
        return dp[m][n];
    }

    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, -1));
        dp[0][0] = 1;
        return paths(m - 1, n - 1, dp);
    }
};
```

Minimum Path Sum In a Grid (tabulation le)
```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    // Function to calculate minimum path sum
    int minPathSum(vector<vector<int>> &matrix) {
        int n = matrix.size();
        int m = matrix[0].size();

        // Create DP table
        vector<vector<int>> dp(n, vector<int>(m, 0));

        // Fill the table
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {

                // First cell initialization
                if (i == 0 && j == 0)
                    dp[i][j] = matrix[i][j];
                else {
                    // Calculate from top
                    int up = matrix[i][j];
                    if (i > 0) up += dp[i - 1][j];
                    else up += 1e9;

                    // Calculate from left
                    int left = matrix[i][j];
                    if (j > 0) left += dp[i][j - 1];
                    else left += 1e9;

                    // Take minimum
                    dp[i][j] = min(up, left);
                }
            }
        }
        // Return result
        return dp[n - 1][m - 1];
    }
};
```

Triangle
```cpp
#include <bits/stdc++.h>
using namespace std;

// Class to solve the triangle minimum path sum
class Solution {
public:
    // Function to compute the minimum path sum using tabulation
    int minimumPathSum(vector<vector<int>> &triangle, int n) {
        // Create a 2D dp array to store intermediate results
        vector<vector<int>> dp(n, vector<int>(n, 0));

        // Initialize the last row of dp with triangle values
        for (int j = 0; j < n; j++) {
            dp[n - 1][j] = triangle[n - 1][j];
        }

        // Traverse from second-last row to the top
        for (int i = n - 2; i >= 0; i--) {
            for (int j = i; j >= 0; j--) {
                // Calculate sum from down and diagonal paths
                int down = triangle[i][j] + dp[i + 1][j];
                int diag = triangle[i][j] + dp[i + 1][j + 1];

                // Store the minimum of the two paths
                dp[i][j] [[Welcome]]= min(down, diag);
            }
        }

        // Return the minimum path sum from top
        return dp[0][0];
    }
};
    vector<vector<int>> triangle{
        {1},
        {2, 3},
        {3, 6, 7},
        {8, 9, 6, 10}

```

Ninja and his friends
We are given an ‘N*M*’ matrix. Every cell of the matrix has some chocolates on it, mat[i][j] gives us the number of chocolates. We have two friends ‘Alice’ and ‘Bob’. initially, Alice is standing on the cell(0,0) and Bob is standing on the cell(0, M-1). Both of them can move only to the cells below them in these three directions: to the bottom cell (↓), to the bottom-right cell(↘), or to the bottom-left cell(↙). When Alica and Bob visit a cell, they take all the chocolates from that cell with them. It can happen that they visit the same cell, in that case, the chocolates need to be considered only once. They cannot go out of the boundary of the given matrix, we need to return the maximum number of chocolates that Bob and Alice can together collect.

```cpp
class Solution {
public:
    // Recursive function with memoization
    int solve(int i, int j1, int j2, int n, int m,
              vector<vector<int>>& grid,
              vector<vector<vector<int>>>& dp) {
        // Out of boundary check
        if (j1 < 0 || j1 >= m || j2 < 0 || j2 >= m)
            return -1e9;
        
        // Base case: last row
        if (i == n - 1) {
            if (j1 == j2) return grid[i][j1];
            else return grid[i][j1] + grid[i][j2];
        }
        
        // If already computed return it
        if (dp[i][j1][j2] != -1) return dp[i][j1][j2];
        
        // Take chocolates from current cell(s)
        int maxi = -1e9;
        int curr = (j1 == j2) ? grid[i][j1] : grid[i][j1] + grid[i][j2];
        
        // Try all 9 moves
        for (int dj1 = -1; dj1 <= 1; dj1++) {
            for (int dj2 = -1; dj2 <= 1; dj2++) {
                int ans = curr + solve(i + 1, j1 + dj1, j2 + dj2,
                                       n, m, grid, dp);
                maxi = max(maxi, ans);
            }
        }
        // Store result
        return dp[i][j1][j2] = maxi;
    }
    
    // Main function to call
    int maximumChocolates(int n, int m, vector<vector<int>>& grid) {
        vector<vector<vector<int>>> dp(n,
            vector<vector<int>>(m, vector<int>(m, -1)));
        return solve(0, 0, m - 1, n, m, grid, dp);
    }
};
```
tabulation
```cpp
class Solution {
public:
    int maximumChocolates(int n, int m, vector<vector<int>>& grid) {
        // 3D DP table
        vector<vector<vector<int>>> dp(n,
            vector<vector<int>>(m, vector<int>(m, 0)));
        
        // Base case: last row
        for (int j1 = 0; j1 < m; j1++) {
            for (int j2 = 0; j2 < m; j2++) {
                if (j1 == j2) dp[n-1][j1][j2] = grid[n-1][j1];
                else dp[n-1][j1][j2] = grid[n-1][j1] + grid[n-1][j2];
            }
        }
        
        // Fill DP table bottom-up
        for (int i = n - 2; i >= 0; i--) {
            for (int j1 = 0; j1 < m; j1++) {
                for (int j2 = 0; j2 < m; j2++) {
                    int maxi = -1e9;
                    int curr = (j1 == j2) ? grid[i][j1] 
                                          : grid[i][j1] + grid[i][j2];
                    // Try all 9 moves
                    for (int dj1 = -1; dj1 <= 1; dj1++) {
                        for (int dj2 = -1; dj2 <= 1; dj2++) {
                            int newJ1 = j1 + dj1;
                            int newJ2 = j2 + dj2;
                            if (newJ1 >= 0 && newJ1 < m &&
                                newJ2 >= 0 && newJ2 < m) {
                                maxi = max(maxi, curr + 
                                           dp[i+1][newJ1][newJ2]);
                            } else {
                                maxi = max(maxi, (int)-1e9);
                            }
                        }
                    }
                    dp[i][j1][j2] = maxi;
                }
            }
        }
        return dp[0][0][m-1];
    }
};
```


## DP on subsequences 

#### Subset sum equal to target

```cpp
class Solution {
  public:
  
    
    bool sub(int ind, int target, vector<int> &a, vector<vector<int>> &dp)
    {
        if(target == 0) return true;
        if(ind == 0) return a[0] == target; // base case
        
        if(dp[ind][target] != -1) return dp[ind][target];
        
        bool take = false;
        if(a[ind] <= target)
        {take = sub(ind -1, target - a[ind], a, dp);}
            
        
        bool not_take = sub(ind -1, target, a, dp);
        
        return dp[ind][target] = take || not_take;
    }
    
    
    bool isSubsetSum(vector<int>& arr, int sum) {
        // code here
        int n = arr.size();
        
        vector<vector<int>> dp(n+1, vector<int>(sum +1, -1));
        
        return sub(n-1, sum, arr, dp);
    }
};
```

tabulation method:
default state in the dp matrix should be 0 not -1. because any non 0 integer value is taken as true
```cpp
class Solution {
  public:
    
    bool isSubsetSum(vector<int>& arr, int sum) {
        // code here
        int n = arr.size();
        
        vector<vector<int>> dp(n+1, vector<int>(sum +1, 0)); // see here
        
// Base case: If the target sum is 0, we can always achieve it by taking no elements
        for(int i = 0; i< n; i++)
        {
            dp[i][0] = true;
        }
        
// Base case: If the first element of 'arr' is less than or equal to 'k', set dp[0][arr[0]] to true
        if(arr[0] <= sum)
        {dp[0][arr[0]] = true;}
        
        
        for(int ind = 1; ind < n; ind++) // bottom up
        {
            for(int target = 1; target <= sum; target++) // bottom up
            {   
 // If we don't take the current element, the result is the same as the previous row
                bool not_take = dp[ind -1][target];
                
// If we take the current element, subtract its value from the target and check the previous row
                bool take = false;
                
                if(arr[ind] <= target)
                {
                    take = dp[ind -1][target - arr[ind]];
                }
                // Store the result in the DP array for the current subproblem
                dp[ind][target] = take || not_take;
            }
        }
        
        return dp[n-1][sum];
    }
};
```

Partition Set Into 2 Subsets With Min Absolute Sum Diff
```cpp
bool subsetSumUtil(int ind, int target, vector<int>& arr, vector<vector<int>>& dp) {
    // Base case: If the target sum is 0, return true
    if (target == 0)
        return dp[ind][target] = true;

    // Base case: If we have considered all elements and the target is still not 0, return false
    if (ind == 0)
        return dp[ind][target] = (arr[0] == target);

    // If the result for this state is already calculated, return it
    if (dp[ind][target] != -1)
        return dp[ind][target];

    // Recursive cases
    // 1. Exclude the current element
    bool notTaken = subsetSumUtil(ind - 1, target, arr, dp);

    // 2. Include the current element if it doesn't exceed the target
    bool taken = false;
    if (arr[ind] <= target)
        taken = subsetSumUtil(ind - 1, target - arr[ind], arr, dp);

    // Store the result in the DP table and return
    return dp[ind][target] = notTaken || taken;
}

// Function to find the minimum absolute difference between two subset sums
int minSubsetSumDifference(vector<int>& arr, int n) {
    int totSum = 0;

    // Calculate the total sum of the array
    for (int i = 0; i < n; i++) {
        totSum += arr[i];
    }

    // Initialize a DP table to store the results of the subset sum problem
    vector<vector<int>> dp(n, vector<int>(totSum + 1, -1));

    // Calculate the subset sum for each possible sum from 0 to the total sum
    for (int i = 0; i <= totSum; i++) {
        bool dummy = subsetSumUtil(n - 1, i, arr, dp);
    }

    int mini = 1e9;
    for (int i = 0; i <= totSum; i++) {
        if (dp[n - 1][i] == true) {
            int diff = abs(i - (totSum - i));
            mini = min(mini, diff);
        }
    }
    return mini;
}
```

meet in the middle algo

### Partitions with Given Difference
Given an array ****arr[]*** and an integer ****diff****, count the ****number of ways*** to partition the array into two subsets such that the difference between their sums is equal to ****diff****.

```cpp
class Solution {
  public:
    int countPartitions(vector<int>& arr, int diff) {
    
    int total_sum = 0;
    int n = arr.size();
    
    for(int x : arr) total_sum += x;
    
    if(total_sum < diff) return 0;
    if((diff + total_sum) % 2) return 0;
    
    int target = (diff + total_sum)/2;
    
    vector<vector<int>> dp(n, vector<int>(target + 1, 0));

    // Base case
    if(arr[0] == 0)
        dp[0][0] = 2;
    else
        dp[0][0] = 1;

    if(arr[0] != 0 && arr[0] <= target)
        dp[0][arr[0]] = 1;

    // Fill DP
    for (int i = 1; i < n; i++) {
        for (int t = 0; t <= target; t++) {
            int notTake = dp[i - 1][t];
            int take = 0;
            if (arr[i] <= t)
                take = dp[i - 1][t - arr[i]];
            dp[i][t] = notTake + take;
        }
    }
    
    return dp[n-1][target];
}
};
```

#### Minimum Coins

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the fewest number of coins that you need to make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

```cpp
class Solution {
public:
    int count(int ind, int target, vector<int> &a, vector<vector<int>> &dp)
    {  
        if(dp[ind][target] != -1)
        {
            return dp[ind][target];
        }

        if(ind == 0)
        {
            if(target % a[ind] == 0)
            {
                return target/a[ind];
            }
            return 1e9;
        }
        // not take
        int take = INT_MAX;
        int not_take = 0 + count(ind-1, target, a, dp);

        if(a[ind] <= target)
        {
            take = 1 + count(ind, target - a[ind], a, dp);
        }

        return dp[ind][target] = min(not_take, take);

    }

    int coinChange(vector<int>& coins, int amount) {

        int n = coins.size();
        vector<vector<int>> dp(n , vector<int>(amount +1 , -1));
        int ans = count(n-1, amount, coins, dp);

        if(ans >= 1e9)

        {
            ans = -1;
        }

        return ans;
    }
};
```

## DP on strings

##### count LCS

##### tabulation method - shifting by 1

```cpp
class Solution {
  public:
    
    
    int lcs(string &s1, string &s2) {
        
        int n = s1.size();
        int m = s2.size();
        
        vector<vector<int>> dp(n+1, vector<int>(m+1, -1));
        
        for(int i = 0; i < n+1; i++)
        {
            dp[i][0] = 0;
        }
        for(int i = 0; i < m+1; i++)
        {
            dp[0][i] = 0;
        }
        
        for(int i = 0; i < n; i++)
        {
            for(int j = 0; j < m; j++)
            {
                if(s1[i] == s2[j])
                {
                    dp[i+1][j+1] = 1 + dp[i+1-1][j+1-1];
                }
                else
                {
                    dp[i+1][j+1] = 0 + max(dp[i+1- 1][j+1], dp[i+1][j+1 -1]);
                }
            }
        }
        
        
        return dp[n-1 + 1][m-1 + 1];
        
    }
};
```
same as

```cpp
class Solution {
  public:
    
    
    int lcs(string &s1, string &s2) {
        
        int n = s1.size();
        int m = s2.size();
        
        vector<vector<int>> dp(n+1, vector<int>(m+1, -1));
        
        for(int i = 0; i < n+1; i++)
        {
            dp[i][0] = 0;
        }
        for(int i = 0; i < m+1; i++)
        {
            dp[0][i] = 0;
        }
        
        for(int i = 1; i < n+1; i++)
        {
            for(int j = 1; j < m+1; j++)
            {
                if(s1[i-1] == s2[j-1]) // index shifted in loop,to access in                                                 string we have to do -1
                {
                    dp[i][j] = 1 + dp[i-1][j-1];
                }
                else
                {
                    dp[i][j] = 0 + max(dp[i- 1][j], dp[i][j -1]);
                }
            }
        }
        
        return dp[n-1+ 1][m-1 + 1]; //obvious
        
    }
};
```

print lcs

```cpp
class Solution {
public:
    string lcs(string &s1, string &s2) {
        
        int n = s1.size();
        int m = s2.size();
        
        // Initialize DP table with 0s. 
        // This automatically handles the base cases (i=0 or j=0), 
        // saving you from writing the two extra for-loops!
        vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
        
        // 1. Fill the DP table to find the lengths
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (s1[i - 1] == s2[j - 1]) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        
        int len = dp[n][m];
        
        // 2. Pre-allocate the result string with the exact length needed
        string lc(len, ' '); 
        
        int i = n;
        int j = m;
        int index = len - 1;
        
        // 3. Backtrack to build the string
        while (i > 0 && j > 0) {
            // If characters match, they are part of the LCS
            if (s1[i - 1] == s2[j - 1]) {
                lc[index] = s1[i - 1];
                index--;
                i--;
                j--;
            } 
            // Otherwise, move in the direction of the larger DP value
            else if (dp[i - 1][j] > dp[i][j - 1]) {
                i--;
            } 
            else {
                j--;
            }
        }
        
        return lc;
    }
};
```

length of longest common substring

```cpp
class Solution {
  public:
    int longCommSubstr(string& s1, string& s2) {
        
        int n = s1.size();
        int m = s2.size();
        
        vector<vector<int>> dp(n+1, vector<int>(m+1, -1));
        
        for(int i = 0; i < n+1; i++)
        {
            dp[i][0] = 0;
        }
        for(int i = 0; i < m+1; i++)
        {
            dp[0][i] = 0;
        }
        int ans = 0;
        for(int i = 1; i < n+1; i++)
        {
            for(int j = 1; j < m+1; j++)
            {
                if(s1[i-1] == s2[j-1]) // index shifted in loop,to access in                                                 string we have to do -1
                {
                    dp[i][j] = 1 + dp[i-1][j-1];
                }
                else
                {
                    dp[i][j] = 0;
                }
                
                ans = max(ans, dp[i][j]);
            }
        }
        
        return ans;
        
    }
};
```

Longest Palindromic Subsequence

usual method : make all the subsequence and check palindromes
LCS method: LCS of s and reverse(s) is the longest palindromic subsequence of string s..

another method using the patter of lcs:

```cpp
class Solution {
public:
    int solve(string& s, int i, int j, vector<vector<int>>& dp) {
        // Base case 1: Pointers crossed
        if (i > j) {
            return 0;
        }
        
        // Base case 2: Single character remaining
        if (i == j) {
            return 1;
        }

        // If we've already calculated this subproblem, return it!
        if (dp[i][j] != -1) {
            return dp[i][j];
        }

        // Transition 1: Characters match
        if (s[i] == s[j]) {
            return dp[i][j] = 2 + solve(s, i + 1, j - 1, dp);
        } 
        
        // Transition 2: Characters do not match
        // Try skipping the left char, then try skipping the right char. Take the max.
        return dp[i][j] = max(solve(s, i + 1, j, dp), solve(s, i, j - 1, dp));
    }

    int longestPalindromeSubseq(string s) {
        int n = s.length();
        
        // Initialize an n x n DP table with -1
        vector<vector<int>> dp(n, vector<int>(n, -1));
        
        // Start with pointers at the very beginning and very end of the string
        return solve(s, 0, n - 1, dp);
    }
};
```

Minimum insertions to make string palindrome

approach: keep the longest palindromic subsequence intact. and add the remaining char in reverse order

ans = n - length of palindromic subsequence of s

Shortest Common Supersequence

```cpp
class Solution {

public:
    string shortestCommonSupersequence(string word1, string word2) {

        int n = word1.length();
        int m = word2.length();

        vector<vector<int>> dp(n +1 , vector<int>(m + 1, 0));

        string ans = "";

        for(int i = 1; i < n+1; i++)
        {
            for(int j = 1; j < m+1; j++)
            {
                if(word1[i-1] == word2[j-1] )
                {
                    dp[i][j] = 1 + dp[i-1][j-1];
                }
                else
                {
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }

        int i = n;
        int j = m;

        while(i > 0 && j > 0)
        {

            if(word1[i-1] == word2[j-1])
            {
                ans.push_back(word1[i-1]);

                i--;
                j--;
            }
            else if(dp[i-1][j] > dp[i][j-1])
            {

                ans.push_back(word1[i-1]);
                i--;
            }
            else
            {
                ans.push_back(word2[j-1]);
                j--;
            }
        }

        while(i > 0) {
            ans.push_back(word1[i-1]);
            i--;
        }
        while(j > 0)
        {
            ans.push_back(word2[j-1]);
            j--;
        }
        reverse(ans.begin(), ans.end());

        return ans;

    }
};
```

##### New pattern : string matching
#### Distinct Subsequences
Given two strings s and t, return the number of distinct subsequences of s that equal t.
f(i,j) - no of subsequences of s[0...i] that equal t[0...j]
```cpp
class Solution {

public:

    int numDistinct(string s, string t) {

        int n = s.length();

        int m = t.length();

        vector<vector<int>> dp(n+1, vector<int>(m+1, 0));
// base case
        for(int i = 1; i < m+1; i ++)
        {
            dp[0][i] = 0;
            // str1 is of len 0 there is no way
        }
        for(int i = 0; i < n+1; i++)
        {
            dp[i][0] = 1; 
            // it means if str 2 is of len 0 then there is always a way
        }

        for(int i = 1; i < n+1; i++)
        {
            for(int j = 1; j < m+1; j++)
            {
                if(s[i-1] == t[j-1])
                {// take this char or move ahead to get another one(multiple cases)
                    dp[i][j] = dp[i-1][j-1] + dp[i-1][j];
                }
                else
                {
                    dp[i][j] = dp[i-1][j];
                }
            }
        }

        return dp[n][m];
    }

};
```

### edit distances
Given two strings `word1`- iand `word2` - j, return _the minimum number of operations required to convert `word1` to `word2`_.

You have the following three operations permitted on a word:
structured thinking:
- Insert a character - i, j-1 ( of the same charac)
- Delete a character - i-1,j (and try matching something else)
- Replace a character - i-1, j-1 (with correct character)

```cpp
class Solution {

public:
    int minDistance(string word1, string word2) {
    
        int n = word1.size();
        int m = word2.size();

        vector<vector<int>> dp(n+1, vector<int>(m+1, -1));

        for(int i = 1; i < m+1; i++)
        {
            dp[0][i] = i;
        }

        for(int i = 1; i < n+1; i++)
        {
            dp[i][0] = i;
        }

        dp[0][0] = 0;

        for(int i = 1; i < n+1; i++)
        {
            for(int j = 1; j < m+1; j++)
            {
                if(word1[i-1] == word2[j-1])
                {
                    dp[i][j] = dp[i-1][j-1];
                }
                else
                {
                   dp[i][j] = 1 + min(dp[i-1][j], min(dp[i-1][j-1], dp[i][j-1]));
                }
            }
        }

        return dp[n][m];
    }

};
```

### wildcard matching 
Given an input string (`s`) and a pattern (`p`), implement wildcard pattern matching with support for `'?'` and `'*'` where:

- `'?'` Matches any single character.
- `'*'` Matches any sequence of characters (including the empty sequence).

The matching should cover the **entire** input string (not partial).
```cpp
class Solution {

public:

    bool wild(int i , int j, string &s, string &p)
    {  
        if(i < 0 && j < 0)
        {
            return true;
        }
        else if(i < 0 && j >= 0)
        {
            for(int ind = 0; ind <= j; ind++)
            {
                if(p[ind] != '*') return false;
            }
            return true;
        }
        else if( i >= 0 && j < 0)
        {
            return false;

        }
  
        if(s[i] == p[j] || p[j] == '?')
        {
            return wild(i-1, j-1, s, p);
        }
        if(p[j] == '*')
        {
            return wild(i-1, j , s, p) || wild(i, j-1, s, p);
        }
        return false;
    }

    bool isMatch(string s, string p) {

        int n = s.length();
        int m = p.length();
        return wild(n-1, m-1, s, p);
    }
};
```

tabular: 
```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        int n = s.length();
        int m = p.length();

        vector<vector<int>> dp(n+1, vector<int> (m+1, 0));

        dp[0][0] =1;
        int ind = 0;

        while(ind < m)
        {
            if(p[ind] != '*') break;
            ind++;
        }

        for(int i = 1; i <=ind; i++)
        {
            dp[0][ind] = 1;
        }

        for(int i = 1; i < n+1; i++)
        {
            for(int j = 1; j < m+1; j++)
            {
                if(s[i-1] == p[j-1] || p[j-1] == '?')
                {
                    dp[i][j] = dp[i-1][j-1];
                }
                else if(p[j-1] == '*')
                {
                    dp[i][j] = dp[i-1][j] || dp[i][j-1];
                }
            }
        }

        return dp[n][m];
    }
};
```

##### Buy and sell stocks II - space opti required

```cpp
class Solution {

public:

    int maxProfit(vector<int>& prices) {

        int n = prices.size();

        vector<vector<int>> dp(n, vector<int> (2, 0));

        vector<int> ahead(2, 0), curr(2,0);

        ahead[1] = prices[n-1];
        ahead[0] = 0;

        for(int i = n-2; i >= 0; i--)
        {
            for(int j = 1; j >=0; j--)
            {
                if(!j)
                {
                    curr[j] = max(-prices[i] + ahead[!j], ahead[j]);
                }
                if(j)
                {
                    curr[j] = max(prices[i] + ahead[!j], ahead[j]);
                }
            }
            ahead = curr;
        }

        return ahead[0];

    }
};
```

#### Longest Increasing Subsequence

normal - DP method - TLE for big sizes - O(n^2) 

optimized Tabulation method :

```cpp
class Solution {

public:

    int lengthOfLIS(vector<int>& arr) {

        int maxi = 1;
        int n = arr.size();

        vector<int> dp(n, 1);

        for(int i = 0; i < n; i++)

        {
            for(int prev = 0; prev < i; prev++)
            {
                if(arr[prev] < arr[i])
                {
                    dp[i] = max(dp[i], 1 + dp[prev]);
                }
            }
            
            maxi = max(maxi, dp[i]);
        }

  

        return maxi;

    }

};
```

##### print lis

```cpp
class Solution {
  public:
    vector<int> getLIS(vector<int>& arr) {
        
        
        int n = arr.size();
        
        
        vector<int> dp(n,1);
        vector<int> hash(n);
        int maxi = 1;
        int lastIndex = 0;
        
        for(int i = 0; i <n; i++)
        {
            hash[i] = i;
            
            for(int prev = 0; prev< i; prev++)
            {
                if(arr[prev] < arr[i] && dp[prev] + 1 > dp[i])
                {
                    dp[i] = 1 + dp[prev];
                    hash[i] = prev;
                }
            }
            
            if(dp[i] > maxi)
            {   
                maxi = dp[i];
                lastIndex = i;
            }
        }
        
        vector<int> temp;
        
        temp.push_back(arr[lastIndex]);
        
        while(hash[lastIndex] != lastIndex)
        {
            lastIndex = hash[lastIndex];
            temp.push_back(arr[lastIndex]);
        }
        
        reverse(temp.begin(), temp.end());
        
        return temp;
    }
};
```

##### optimized way to count length of LIS: Binary Search - TC O(NlogN)

```cpp
#include<bits/stdc++.h>
class Solution {
public:

    int lengthOfLIS(vector<int>& nums) {

        int n = nums.size();

        vector<int> temp;

        temp.push_back(nums[0]);

        int len = 1;

        for(int i = 1; i < n; i++)
        {
            if(nums[i] > temp.back())
            {

                temp.push_back(nums[i]);
                len++;
            }
            else
            {
                int ind = lower_bound(temp.begin(),temp.end(), nums[i]) - temp.begin();

                temp[ind] = nums[i];
            }
        }
        return len;
    }
};
```

Longet divisble subset - same code as printing lis .. we have to sort first and check divisibility

Sorting isn't just about putting numbers in order; it is a fundamental requirement to ensure one-way mathematical chaining.
It ensures that if a new number $Y$ is a multiple of an old number $X$, $Y$ will also safely be a multiple of **everything that naturally divides $X$**.

```cpp
class Solution {

public:

    vector<int> largestDivisibleSubset(vector<int>& arr) {

        int n = arr.size();

        sort(arr.begin(), arr.end());

        vector<int> dp(n,1);
        vector<int> hash(n);
        int maxi = 1;
        int lastIndex = 0;

        for(int i = 0; i <n; i++)
        {
            hash[i] = i;
            for(int prev = 0; prev< i; prev++)
            {
                if(arr[i] % arr[prev] == 0 && dp[prev] + 1 > dp[i])
                {
                    dp[i] = 1 + dp[prev];
                    hash[i] = prev;
                }
            }
            if(dp[i] > maxi)
            {  
                maxi = dp[i];
                lastIndex = i;
            }
        }

        vector<int> temp;
        temp.push_back(arr[lastIndex]);
        while(hash[lastIndex] != lastIndex)
        {
            lastIndex = hash[lastIndex];
            temp.push_back(arr[lastIndex]);
        }
        reverse(temp.begin(), temp.end());
        return temp;
    }

};
```

Bitonic Subsequence:

LIS from front + LIS from back

```cpp
class Solution {
  public:
    int longestBitonicSequence(int n, vector<int> &nums) {
        int maxi = 0;
        
        vector<int> dp1(n, 1);
        
        for(int i = 0; i < n; i++)
        {
            for(int prev = 0; prev < i; prev++)
            {
                if(nums[prev] < nums[i])
                {
                    dp1[i] = max(dp1[i], 1 + dp1[prev]);
                }
            }
        }
        
        vector<int> dp2(n,1);
        
        for(int i = n-1; i >= 0; i--)
        {
            for(int prev = n-1; prev > i; prev--)
            {
                if(nums[prev] < nums[i])
                {
                    dp2[i] = max(dp2[i], 1 + dp2[prev]);
                }
            }
        }
        
        for(int i = 0; i < n; i++)
        {   if(dp2[i] >1 && dp1[i] > 1)
            {maxi = max(maxi, dp1[i] + dp2[i] - 1);}
        }
        
        return maxi;
        
    }
};
```

Number of LIS

```cpp
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        
        int n = nums.size();
        int maxi = 1;
        vector<int> count(n, 1);
        int ans = 0;

        vector<int> dp(n, 1);

        for(int i = 0; i < n; i++)
        {
            for(int prev = 0; prev < i; prev++)
            {
                if(nums[i] > nums[prev])
                {   
                    // We found a strictly longer sequence
                    if(dp[prev] + 1 > dp[i])
                    {
                        dp[i] = dp[prev] + 1;
                        count[i] = count[prev]; // Inherit ways from prev
                    }
                    // We found another sequence of the same length
                    else if(dp[prev] + 1 == dp[i])
                    {
                        count[i] += count[prev]; // Add ways from prev
                    }
                }
            }
            maxi = max(maxi, dp[i]);
        }
        for(int i = 0; i < n; i++)
        {
            if(dp[i] == maxi)
            {
                ans += count[i];
            }
        }
        return ans;
    }
};
```

Matrix Chain Multiplication:

Recursion/Memoization

```cpp
class Solution {
  public:
    
    int mcm(int i, int j, vector<int> &a, vector<vector<int>> &dp)
    {
        if(i == j) return 0;
        
        if(dp[i][j] != -1)
        {
            return dp[i][j];
        }
        
        int mini = 1e9;
        
        for(int k = i; k <j; k++)
        {
            int steps = a[i-1]*a[k]*a[j] + mcm(i,k, a, dp) + mcm(k+1,j, a, dp);
            
            mini = min(mini, steps);
        }
        
        return dp[i][j] = mini;
    }
  
    int matrixMultiplication(vector<int> &arr) {
       
       int n = arr.size();
       
       vector<vector<int>> dp(n , vector<int>(n , -1));
       
       return mcm(1,n-1, arr, dp);
       
    }
};
```

tabulation:

```cpp
class Solution {
  public:
    
    int mcm(int i, int j, vector<int> &a, vector<vector<int>> &dp)
    {
        if(i == j) return 0;
        
        if(dp[i][j] != -1)
        {
            return dp[i][j];
        }
        
        int mini = 1e9;
        
        for(int k = i; k <j; k++)
        {
            int steps = a[i-1]*a[k]*a[j] + mcm(i,k, a, dp) + mcm(k+1,j, a, dp);
            
            mini = min(mini, steps);
        }
        
        return dp[i][j] = mini;
    }
  
    int matrixMultiplication(vector<int> &arr) {
       
       int n = arr.size();
       
       vector<vector<int>> dp(n , vector<int>(n , -1));
       
       for(int i = 0; i < n; i++)
       {
           dp[i][i] = 0;
       }
       
       for(int i = n-1; i >= 1; i--)
       {
           for(int j = i+1; j < n; j++)
           {   
               int mini = 1e9;
               
               for(int k = i; k <j; k++)
               {
                int steps = arr[i-1]*arr[k]*arr[j] + dp[i][k]+ dp[k+1][j];
                mini = min(mini, steps);
               }
               
               dp[i][j] = mini;
           }
       }
       
       return dp[1][n-1];
       
    }
};
```

minimum cost to cut a stick

```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    // Your exact recursive function
    int cost(int i, int j, vector<int>& cuts, vector<vector<int>>& dp) {
        // Base case: if start index exceeds end index, no cost
        if (i > j) return 0;

        // Memoization check: return if already computed
        if (dp[i][j] != -1) return dp[i][j];

        int mini = INT_MAX;

        // Try making every valid cut in the current segment
        for (int ind = i; ind <= j; ind++) {
            int temp = cuts[j+1] - cuts[i-1] + 
                       cost(i, ind - 1, cuts, dp) + 
                       cost(ind + 1, j, cuts, dp);
            
            mini = min(mini, temp);
        }
        
        // Store and return the minimum cost
        return dp[i][j] = mini;
    }

    int minCost(int n, vector<int>& cuts) {
        int c = cuts.size(); // Store the original number of cuts
        
        // Setup the cuts array by adding the stick boundaries
        cuts.push_back(n);
        cuts.insert(cuts.begin(), 0);
        sort(cuts.begin(), cuts.end());
        
        // Initialize the DP table with -1. 
        // Size is (c + 2) x (c + 2) because we pass (ind + 1) in the recursion, 
        // which can reach c + 1. This prevents out-of-bounds access.
        vector<vector<int>> dp(c + 2, vector<int>(c + 2, -1));

        // Start the recursion considering the original cuts (indices 1 to c)
        return cost(1, c, cuts, dp);
    }
};
```

tabulation:
```cpp
int minCost(int n, vector<int>& cuts) {
    int c = cuts.size(); // Store the original number of cuts
    
    // Setup the cuts array
    cuts.push_back(n);
    cuts.insert(cuts.begin(), 0);
    sort(cuts.begin(), cuts.end());
    
    // DP table size only needs to be based on the number of cuts (c), NOT the stick length (n)
    // Size is (c + 2) x (c + 2) to accommodate the 0 and n we added, and prevent out-of-bounds
    vector<vector<int>> dp(c + 2, vector<int>(c + 2, 0));

    // Bottom-up DP
    // i goes from c down to 1
    for(int i = c; i >= 1; i--) {
        // j goes from i up to c
        for(int j = i; j <= c; j++) {
            int mini = INT_MAX;
            
            for(int ind = i; ind <= j; ind++) {
                // The length of the current piece is cuts[j+1] - cuts[i-1]
                int temp = cuts[j+1] - cuts[i-1] + dp[i][ind - 1] + dp[ind + 1][j];
                mini = min(mini, temp);
            }
            dp[i][j] = mini;
        }
    }

    // The answer is the cost to evaluate cuts from index 1 to c
    return dp[1][c];
}
```


Burst Ballons - Crazy Approach

You are given n balloons, indexed from 0 to n - 1. Each balloon is painted with a number on it represented by an array. You are asked to burst all the balloons.
If you burst the ith balloon, you will get arr[i - 1] * arr[i] * arr[i + 1] coins. If i - 1 or i + 1 goes out of the array's bounds, then treat it as if there is a balloon with a 1 painted on it.
Return the maximum coins you can collect by bursting the balloons wisely.

Approach : start from last, assume the balloon you are bursting is the last in this range(i,j).. so the neighbours are known and the subproblems are independent from each other.

```cpp
class Solution {
public:
    int earn(int i, int j, vector<int> &nums, vector<vector<int>> &memo) {
        // Base case: if the range is invalid, return 0
        if (i > j) return 0;

        // If we have already calculated this subproblem, return the cached result
        if (memo[i][j] != -1) return memo[i][j];

        int maxi = INT_MIN;

        // Assume k is the LAST balloon to burst in the range [i, j]
        for (int k = i; k <= j; k++) {
            // Because k is the last to burst, its adjacent balloons are i-1 and j+1
            int temp = nums[i-1] * nums[k] * nums[j+1] 
                     + earn(i, k-1, nums, memo) 
                     + earn(k+1, j, nums, memo);

            maxi = max(maxi, temp);
        }

        // Cache and return the result
        return memo[i][j] = maxi;
    }

    int maxCoins(vector<int>& nums) {
        int n = nums.size();
        
        // Add 1 to the beginning and end
        nums.insert(nums.begin(), 1);
        nums.push_back(1);

        // Initialize memoization table with -1
        // Size is n+2 because we added two elements to the original array
        vector<vector<int>> memo(n + 2, vector<int>(n + 2, -1));

        // The valid balloons to burst are now from index 1 to n
        return earn(1, n, nums, memo);
    }
};
```

### Boolean Parenthesization
You are given a boolean expression **s** containing
Count the number of ways we can **parenthesize** the expression so that the value of expression evaluates to **true**.
```cpp
#include <vector>
#include <string>

using namespace std;

class Solution {
private:
    // Helper function to evaluate the number of ways to parenthesize the expression
    int f(int i, int j, int isTrue, string &exp, vector<vector<vector<int>>> &dp) {
        // Base case 1: Invalid expression
        if (i > j) return 0;
        
        // Base case 2: Single character, evaluate it
        if (i == j) {
            if (isTrue == 1) return exp[i] == 'T' ? 1 : 0;
            else return exp[i] == 'F' ? 1 : 0;
        }

        // Return memoized result if it exists
        if (dp[i][j][isTrue] != -1) return dp[i][j][isTrue];
        
        long long ways = 0;
        
        // Iterate through the expression to pick operators
        for (int ind = i + 1; ind <= j - 1; ind += 2) {
            
            // Recursively calculate ways for left and right subexpressions
            long long lT = f(i, ind - 1, 1, exp, dp);
            long long lF = f(i, ind - 1, 0, exp, dp);
            long long rT = f(ind + 1, j, 1, exp, dp);
            long long rF = f(ind + 1, j, 0, exp, dp);

            // Calculate combinations based on the operator
            if (exp[ind] == '&') {
                if (isTrue) ways += (lT * rT);
                else ways += (lF * rT) + (lT * rF) + (lF * rF);
            }
            else if (exp[ind] == '|') {
                if (isTrue) ways += (lF * rT) + (lT * rF) + (lT * rT);
                else ways += (lF * rF);
            }
            else { // XOR operator '^'
                if (isTrue) ways += (lF * rT) + (lT * rF);
                else ways += (lF * rF) + (lT * rT);
            }
        }
        
        // Store and return the result
        return dp[i][j][isTrue] = ways;
    }

public:
    int countWays(string &s) {
        int n = s.size();
        
        // DP table: n x n x 2 initialized to -1
        // dp[i][j][1] stores ways to make substring i to j True
        // dp[i][j][0] stores ways to make substring i to j False
        vector<vector<vector<int>>> dp(n, vector<vector<int>>(n, vector<int>(2, -1)));
        
        // Start evaluation from index 0 to n-1, looking for True (1)
        return f(0, n - 1, 1, s, dp);
    }
};
```