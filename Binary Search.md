Finding the minimum element in rotated sorted array

Core Idea:
- **One half is always sorted**
- The **minimum lies in the unsorted part**
We use binary search to **narrow down where the unsorted part is**.
```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    // Function to find the minimum element using binary search
    int findMin(vector<int>& nums) {

        // Initialize low and high pointers
        int low = 0, high = nums.size() - 1;

        // we want low == high at the end , if we include equality it will be infinite loop
        
        
        while (low < high) {

            // Calculate mid index
            int mid = low + (hixgh - low) / 2;

            // - Left part is sorted and big
			//- Right part contains the rotation + minimum
            if (nums[mid] > nums[high])
             {

                // Minimum lies in right half
                low = mid + 1;

            } 
	            // Right half is sorted , minimum is either at mid or in the left half
            else {

                // Minimum lies in left half (so we keep mid also)
                high = mid;
            }
        }

        // Return the minimum element
        return nums[low];
    }
};

int main() {

    // Input array
    vector<int> nums = {4, 5, 6, 7, 0, 1, 2};

    // Create object of Solution
    Solution sol;

    // Call function and store result
    int result = sol.findMin(nums);

    // Output the result
    cout << "Minimum element is " << result << endl;

    return 0;
}

```


General note

If you think you can dodge overflow by defining the product of (int * int) as long long , please note that the overflow happens before only, so add 1LL OR (long long)

```cpp
int a = 100000, b = 100000;
long long x = a * b;  // overflow happens first
```

correct
```cpp
long long x = 1LL * a * b;

or

long long x = (long long)a * b;

```

The high and low are set according to the question conditions , always think according to the question while setting them.
for example:
```cpp
int high = accumulate(weights.begin(), weights.end(), 0); // function used to find the sum of an array.
```

Find Kth missing number : Find the 'kth' positive integer missing from 'vec'.

Brute force  , tc = O(N):

```cpp
#include <bits/stdc++.h>
using namespace std;

// Class to find the k-th missing number in a sorted array
class MissingKFinder {
public:
    // Function to find the k-th missing number
    int missingK(vector<int> vec, int n, int k) {
        for (int i = 0; i < n; i++) {
            if (vec[i] <= k) {
                k++;  // If current number is less than or equal to k, increment k
            } else {
                break; // Stop when we reach a number greater than k
            }
        }
        return k;  // Return the final value of k which is the missing number
    }
};

int main() {
    vector<int> vec = {4, 7, 9, 10};  // Sorted input array
    int n = vec.size();              // Size of the array
    int k = 4;                       // We are looking for the 4th missing number

    MissingKFinder finder;               // Create object
    int ans = finder.missingK(vec, n, k);  // Call method

    cout << "The missing number is: " << ans << "\n";  // Output the result
    return 0;
}

```


Binary Search (optimal) : 
```cpp
#include <bits/stdc++.h>
using namespace std;

// Class to find the k-th missing number using binary search
class MissingKFinder {
public:
    // Function to return the k-th missing number
    int missingK(vector<int> vec, int n, int k) {
        int low = 0, high = n - 1;

        // Perform binary search
        while (low <= high) {
            int mid = (low + high) / 2;

            // Calculate how many numbers are missing till vec[mid]
            int missing = vec[mid] - (mid + 1);

            if (missing < k) {
                low = mid + 1;  // Move right to find more missing numbers
            } else {
                high = mid - 1; // Move left to find a smaller valid index
            }
        }

        // After loop, 'high' points to the largest index such that
        // number of missing elements till there < k
        return k + high + 1;
    }
};

int main() {
    vector<int> vec = {4, 7, 9, 10};  // Sorted array
    int n = vec.size();              // Size of array
    int k = 4;                       // k-th missing number to find

    MissingKFinder finder;          // Create object
    int ans = finder.missingK(vec, n, k);  // Call method

    cout << "The missing number is: " << ans << "\n";  // Print result
    return 0;
}

```
