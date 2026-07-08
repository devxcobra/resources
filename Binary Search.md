
lower bound
```cpp
    int lowerBound(vector<int> arr, int n, int x) {
        int low = 0;           
        int high = n - 1;      
        int ans = n;           // Default to n (not found)
        
        while (low <= high) {
            int mid = (low + high) / 2;  // Middle index
            
            if (arr[mid] >= x) {
                ans = mid;           // Store possible answer
                high = mid - 1;      // Try to find smaller index on left side
            } else {
                low = mid + 1;      // Move right if current element is less than x
            }
        }
        return ans;  // Return the index of the lower bound
    }
```

First and Last Position of element in sorted array
```cpp
class Solution {
public:
    int firstocc(vector<int>& nums, int target)
    {
        int n = nums.size();
        int low = 0;
        int high = n-1;
        int ans = -1;

        while(low<= high)
        {
            int mid = (low+high)/2;

            if(nums[mid]< target)
            {
                low = mid+1;
            }
            else if(nums[mid] > target)
            {
                high = mid -1;
            }
            else
            {
                ans = mid;
                high = mid -1;
            }
        }
        return ans;

    }
    int lastocc(vector<int>&nums, int target)
    {
        int n = nums.size();
        int low = 0;
        int high = n-1;
        int ans = -1;

        while(low<= high)
        {
            int mid = (low+high)/2;

            if(nums[mid]< target)
            {
                low = mid+1;
            }
            else if(nums[mid] > target)
            {
                high = mid -1;
            }
            else
            {
                ans = mid;
                low = mid+1;
            }
        }

        return ans;

    }
    vector<int> searchRange(vector<int>& nums, int target) {
       return {firstocc(nums, target), lastocc(nums, target)};
    }
};
```

Search element in rotated sorted array

```cpp
class Solution {
public:
    // Function to search for target using binary search in rotated sorted array
    int search(vector<int>& nums, int target) {

        // Set the search space to entire array
        int low = 0;
        int high = nums.size() - 1;

        // Continue until the search space becomes invalid
        while (low <= high) {
            int mid = (low + high) / 2;
            if (nums[mid] == target)
                return mid;
            // Check if the left half is sorted
            if (nums[low] <= nums[mid]) {
                // If target lies in the sorted left half, search there
                if (nums[low] <= target && target < nums[mid]) {
                    high = mid - 1;
                }
                // Else search in the right half
                else {
                    low = mid + 1;
                }
            }
            // Otherwise, right half is sorted
            else {
                // If target lies in the sorted right half, search there
                if (nums[mid] < target && target <= nums[high]) {
                    low = mid + 1;
                }
                // Else search in the left half
                else {
                    high = mid - 1;
                }
            }
        }
        return -1;
    }
};
```

Search in Rotated sorted array II
(contains duplicate elements)
```cpp
    bool search(vector<int>& nums, int target) {

        int n = nums.size();
        int low = 0;
        int high = n-1;

        while(low<= high)
        {
            int mid = (low+high)/2;
            if(nums[mid]== target)
            {
                return true;
            }

            if(nums[low] == nums[mid] && nums[mid] == nums[high]) {
                low++;
                high--;
                continue;
            }
            if(nums[low] <= nums[mid])
            {
                if(nums[low] <= target && target < nums[mid])
                {
                    high = mid -1;
                }
                else
                {
                    low = mid + 1;
                }
            }

            else
            {
                if(nums[mid] < target && target <= nums[high])
                {
                    low = mid + 1;
                }

                else
                {
                    high = mid - 1;
                }
            }
        }
        return false;
    }
```

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
            int mid = low + (high - low) / 2;

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
```
Find out how many times the array has been rotated (distinct values)
approach : find the index of min element
```cpp
    int findRotations(vector<int>& arr) {
        // Initialize low and high pointers
        int low = 0;
        int high = arr.size() - 1;

        // Loop until low meets high
        while (low < high) {
            // Find mid index
            int mid = low + (high - low) / 2;

            // If mid element is greater than element at high,
            // smallest element lies to the right of mid
            if (arr[mid] > arr[high]) {
                low = mid + 1;
            } else {
                // Else smallest element is at mid or to the left
                high = mid;
            }
        }

        // When low == high, we found the smallest element
        return low;
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

BS on answers:
find sqrt of a number
```cpp
    int mySqrt(int x) {
        // Handle small numbers directly
        if (x < 2) return x;
        int left = 1, right = x / 2, ans = 0;

        while (left <= right) {
            long long mid = left + (right - left) / 2;

            // Check if mid*mid is less than or equal to x
            if (mid * mid <= x) {
                // Store mid as potential answer
                ans = mid;
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return ans;
    }
```

Koko eating bananas
A monkey Koko is given ‘n’ piles of bananas, whereas the 'ith' pile has ‘a[i]’ bananas. An integer ‘h’ is also given, which denotes the time (in hours) for all the bananas to be eaten.  
Each hour, the monkey chooses a non-empty pile of bananas and eats ‘k’ bananas. If the pile contains less than ‘k’ bananas, then the monkey consumes all the bananas and won’t eat any more bananas in that hour.  
Find the minimum number of bananas ‘k’ to eat per hour so that the monkey can eat all the bananas within ‘h’ hours.
```cpp
class Solution {
public:
    // Function to calculate total hours at given speed
    int calculateTotalHours(vector<int>& piles, int speed) {
        int totalH = 0;
        for (int bananas : piles) {
            totalH += ceil((double)bananas / speed);
        }
        return totalH;
    }

    // Function to find minimum eating speed
    int minEatingSpeed(vector<int>& piles, int h) {
        // Find maximum element
        int maxPile = *max_element(piles.begin(), piles.end());

        // Initialize low and high pointers
        int low = 1, high = maxPile;
        int ans = maxPile;

        // Binary search on answer space
        while (low <= high) {
            int mid = (low + high) / 2;
            int totalH = calculateTotalHours(piles, mid);

            // If possible, try smaller speed
            if (totalH <= h) {
                ans = mid;
                high = mid - 1;
            }
            // Otherwise, try larger speed
            else {
                low = mid + 1;
            }
        }
        return ans;
    }
};
```

Minimum days to make m bouqets
```cpp
class Solution {
public:
    bool possible(vector<int>& arr, int day, int m, int k) {
        int n = arr.size();         // Total number of flowers
        int cnt = 0;                // Counter for consecutive bloomed flowers
        int bouquets = 0;           // Count of bouquets made

        for (int i = 0; i < n; i++) {
            if (arr[i] <= day) {
                // Flower bloomed, increment consecutive count
                cnt++;
                if (cnt == k) {
                    bouquets++;
                    cnt = 0; // reset for next bouquet
                }
            } else {
                cnt = 0;
            }
        }
        // Check if at least m bouquets can be made
        return bouquets >= m;
    }

    int roseGarden(vector<int>& arr, int k, int m) {
        long long total = 1LL * k * m; // Total flowers required

        // If total required flowers > available flowers, it's impossible
        if (total > arr.size()) return -1;

        // Find minimum and maximum bloom days from array
        int mini = *min_element(arr.begin(), arr.end());
        int maxi = *max_element(arr.begin(), arr.end());

        int low = mini, high = maxi;
        int result = -1;

        while (low <= high) {
            int mid = (low + high) / 2;

            if (possible(arr, mid, m, k)) {
    // If it's possible to make bouquets on this day, try to find an earlier day
                result = mid;
                high = mid - 1;
            } else {
                // Otherwise, try with a later day
                low = mid + 1;
            }
        }

        return result;
    }
};
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
```


Binary Search (optimal) : 
```cpp
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
// ans  = arr[high] + more 
// more = k -missing
// missing = arr[high] - (high + 1)
```
