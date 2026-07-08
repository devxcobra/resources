

1. Reverse a vector
```cpp
int start = 0, end = v.size() - 1;
while(i < j) {
    swap(v[start], v[end]);
    start++;
    end--;
}	
```


k = k % n

To rotate an array to the right by k

1. Reverse the whole array
2. Reverse first k elements
3. Reverse remaining n-k elements

To rotate an array to the left by k
1. Reverse first k elements
2. Reverse remaining n-k elements
3. Reverse the whole array

Move Zeros to the end

my code:

```cpp
class Solution {

public:

    void moveZeroes(vector<int>& nums) {

        int n = nums.size();

        int pa = 0;

        int pb = 0;

        while(pa < n && pb < n)

        {  
            if(nums[pa] != 0 && nums[pb] == 0)
            {
              swap(nums[pa], nums[pb]);
                pa++;
                pb++;
            }
            
            if(pa <n && pb <n)
            {
            
            if(nums[pa] == 0)
            {
                pa++;
            }
            if(nums[pb] != 0)
            {
                pb++;
            }
            
            }
        }
    }
};
```

Correct code

```cpp
class Solution {

public:

    void moveZeroes(vector<int>& nums) {

        int n = nums.size();

        int ind = -1;
  
        for(int j = 0; j< n; j++) // search for first zero

        {
            if(nums[j] == 0)

            {
                ind = j;
                break;
            }
        }

        if(ind == -1) // if no zero found

        {
            return;
        }

  

        for(int i = ind+1; i < n; i++) // obviously all non zero are ahead of ind

        {
            if(nums[i] != 0)

            {
                swap(nums[i], nums[ind]);
                ind++; // this always makes ind land on zero as they get swapped or there were zero in between i and ind.
            }
        }

        return;

    }

};
```


note that XOR Property:

```
a^a  =0
b^0 = b
```

can be used to find the the element that occurs only once in an array where every other element occurs twice.

#### Longest Consecutive Sequence in an Array

Optimal approach: TC: O(n), SC: O(n)
approach : **only start counting a sequence if you are at the absolute beginning of it.**

st.find(x) - TC: O(1)

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& a) {
        int n = a.size();
        // If the array is empty
        if (n == 0) return 0; 
    
        // Initialize the longest sequence length
        int longest = 1; 
        unordered_set<int> st;
    
        // Put all the array elements into the set
        for (int i = 0; i < n; i++) {
            st.insert(a[i]);
        }
    
        /* Traverse the set to 
           find the longest sequence  */
        for (auto it : st) {
            // Check if 'it' is a starting number of a sequence
            if (st.find(it - 1) == st.end()) {
                // Initialize the count of the current sequence
                int cnt = 1; 
                // Starting element of the sequence
                int x = it; 
    
                // Find consecutive numbers in the set
                while (st.find(x + 1) != st.end()) {
                    // Move to the next element in the sequence
                    x = x + 1; 
                    // Increment the count of the sequence
                    cnt = cnt + 1; 
                }
                // Update the longest sequence length
                longest = max(longest, cnt);
            }
        }
        return longest;
    }
};
```

#### Set matrix zeroes

Given a matrix if an element in the matrix is 0 then you will have to set its entire column and row to 0 and then return the matrix.

Most optimal approach: TC: O(m* n) , SC: O(1)

Instead of using separate arrays, we use the first row and first column of the matrix itself to store whether a row or column needs to be zeroed. We also store two flags:

```cpp
class Solution {
public:
    // Function to set entire row and column to 0 if an element in the matrix is 0 (Optimal O(1) space)
    void setZeroes(vector<vector<int>>& matrix) {
        // Get dimensions of matrix
        int m = matrix.size();
        int n = matrix[0].size();
        // Flag to track if first row should be zeroed
        bool firstRowZero = false;
        // Flag to track if first column should be zeroed
        bool firstColZero = false;

        // Check if first row has any zero
        for (int j = 0; j < n; j++) {
            if (matrix[0][j] == 0) {
                firstRowZero = true;
                break;
            }
        }
        // Check if first column has any zero
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0) {
                firstColZero = true;
                break;
            }
        }
        // Mark rows and columns in first row/column
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        // Set matrix cells to zero based on markers
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        // Handle first row
        if (firstRowZero) {
            for (int j = 0; j < n; j++) {
                matrix[0][j] = 0;
            }
        }
        // Handle first column
        if (firstColZero) {
            for (int i = 0; i < m; i++) {
                matrix[i][0] = 0;
            }
        }
    }
};
```

#### Rotate image/matrix by 90 deg clockwise
Approach1 : (i , j) --> new position : (j , n - i -1) 
TC : O(n^2)  SC: O(n^2)
Optimal Approach:
first, transpose the matrix
second, reverse all the arrays
TC: O(n^2)  SC: O(1)
```cpp
class Solution {
public:
    // Function to rotate matrix 90 degrees clockwise in-place
    void rotateClockwise(vector<vector<int>>& matrix) {
        int n = matrix.size();

        // Step 1: Transpose the matrix
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                // Swap element at (i, j) with (j, i) to transpose
                swap(matrix[i][j], matrix[j][i]);
            }
        }

        // Step 2: Reverse each row
        for (int i = 0; i < n; ++i) {
            // Reverse the current row to complete clockwise rotation
            reverse(matrix[i].begin(), matrix[i].end());
        }
    }
};
```

#### Longest Subarray with given sum K

Optimal approach: TC: O(N) and SC: O(1)

```cpp
class Solution{
public:
    // Function to find the length of longest subarray having sum k
    int longestSubarray(vector<int> &nums, int k){
        int n = nums.size();
        
        // To store the maximum length of the subarray
        int maxLen = 0;
        // Pointers to mark the start and end of window
        int left = 0, right = 0;
        // To store the sum of elements in the window
        int sum = nums[0];
        // Traverse all the elements
        while(right < n) {
            // If the sum exceeds K, shrink the window
            while(left <= right && sum > k) {
                sum -= nums[left];
                left++;
            }
            // store the maximum length
            if(sum == k) {
                maxLen = max(maxLen, right - left + 1);
            }
            right++;
            if(right < n) sum += nums[right];
        }
        
        return maxLen;
    }
};
```

#### Spiral Traversal of Matrix:
TC - O(m * n) , SC - O(1)
![[Pasted image 20260701122321.png|292]]
```cpp
class Solution {
public:
    // Function to return matrix in spiral order
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        // Vector to store final spiral order result
        vector<int> result;

        // Get number of rows and columns
        int top = 0;
        int bottom = matrix.size() - 1;
        int left = 0;
        int right = matrix[0].size() - 1;

        // Traverse the matrix in spiral order
        while(top <= bottom && left <= right) {

            // Traverse from left to right across the top row
            for(int i = left; i <= right; i++) {
                result.push_back(matrix[top][i]);
            }
            top++; // Move top boundary down

            // Traverse from top to bottom on the right column
            for(int i = top; i <= bottom; i++) {
                result.push_back(matrix[i][right]);
            }
            right--; // Move right boundary left

            // Check if there are rows remaining
            if(top <= bottom) {
                // Traverse from right to left on the bottom row
                for(int i = right; i >= left; i--) {
                    result.push_back(matrix[bottom][i]);
                }
                bottom--; // Move bottom boundary up
            }

            // Check if there are columns remaining
            if(left <= right) {
                // Traverse from bottom to top on the left column
                for(int i = bottom; i >= top; i--) {
                    result.push_back(matrix[i][left]);
                }
                left++; // Move left boundary right
            }
        }

        // Return the final spiral order
        return result;
    }
};
```


#### Count Subarray sum Equals K

Optimal approach: Prefix Sum + hash map
TC - O(n) , SC - O(n) worst case
```
### Count Subarray Sum Equals K (Prefix Sum + Hash Map)

[  ... past elements ...  |  === valid subarray ===  ]
^                         ^                          ^
|                         |                          |
Start (0)         (current_sum - k)             current_sum
                 (Found in Hash Map!)

Math logic: current_sum - (current_sum - k) = k
```

```cpp
class Solution {
public:
    // Function to find count of subarrays with sum equal to k using prefix sums and hashmap
    int subarraySum(vector<int>& arr, int k) {
        // Size of the array
        int n = arr.size();

        // Map to store frequency of prefix sums
        unordered_map<int, int> prefixSumCount;

        // Initialize prefix sum and count of subarrays
        int prefixSum = 0;
        int count = 0;

        // Base case: prefix sum 0 has occurred once
        prefixSumCount[0] = 1;

        // Traverse through the array
        for (int i = 0; i < n; i++) {
            // Add current element to prefix sum
            prefixSum += arr[i];

            // Calculate the prefix sum that needs to be removed
            int remove = prefixSum - k;

            // If this prefix sum has been seen before,
            // add its count to the result
            if (prefixSumCount.find(remove) != prefixSumCount.end()) {
                count += prefixSumCount[remove];
            }

            // Update the frequency of the current prefix sum
            prefixSumCount[prefixSum]++;
        }

        // Return the total count of subarrays
        return count;
    }
};
```