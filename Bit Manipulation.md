
Power Set- 

```cpp
#include <bits/stdc++.h>
using namespace std;

// Solution class to generate all subsequences using bit manipulation
class Solution {
public:
    // Function to return all subsequences of string s
    vector<string> getSubsequences(string s) {
        // Length of input string
        int n = s.size();

        // Total subsequences = 2^n
        int total = 1 << n;

        // Vector to store all subsequences
        vector<string> subsequences;

        // Iterate over all bit masks from 0 to 2^n - 1
        for (int mask = 0; mask < total; mask++) {
            // Temporary subsequence string
            string subseq = "";

            // Check each bit position in mask
            for (int i = 0; i < n; i++) {
                // If i-th bit of mask is set, include s[i]
                if (mask & (1 << i)) {
                    subseq += s[i];
                }
            }

            // Store the formed subsequence
            subsequences.push_back(subseq);
        }

        // Return all generated subsequences
        return subsequences;
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



check if power of 2:

optimal approach (without loop) :

- Power of two type of numbers have exactly one bit set in their binary form.
- Subtracting one flips all bits after the set bit, creating no overlap with the original number.
- A bitwise AND between the number and one less than itself will be zero only for powers of two.
- This property allows for a fast check without looping or dividing.

basically n & (n-1) == 0

```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    // Function to check if a number is a power of two
    bool isPowerOfTwo(int n) {
        return n > 0 && (n & (n - 1)) == 0;  // Check if n is greater than 0 and has only one bit set
    }
};

```

count number of set bits:

brute force : O(32)

optimal: O(k) , k : number of set bits..

```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    // Function to count the number of set bits (1s) in the binary representation of n using Brian Kernighan's Algorithm
    int countSetBits(int n) {
        int count = 0;  // Variable to store the count of set bits

        // Step 1: While n is non-zero, turn off the rightmost set bit
        while (n) {
            n = n & (n - 1);  // Turn off the rightmost set bit
            count++;  // Increment the count
        }

        // Step 2: Return the count of set bits
        return count;
    }
};

// basically removes the rightmost bit in each iteration
```

set the rightmost unset bit

most elegant method : n = n | (n+1)

2 to the power n can be written as : 1<<n;

bitwise operations are O(1)


## Find the two numbers appearing odd number of times

```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    /* Function to get the single 
    numbers in the given array */
    vector<int> singleNumber(vector<int>& nums){
        // Variable to store size of array
        int n = nums.size();
        
        // Variable to store XOR of all elements
        long XOR = 0;
        
        // Traverse the array
        for(int i=0; i < n; i++) {
            
            // Update the XOR
            XOR = XOR ^ nums[i];
        }
        
        /* Variable to get the rightmost 
        set bit in overall XOR */
        int rightmost = (XOR & (XOR - 1)) ^ XOR;
        
        /* Variables to stores XOR of
        elements in bucket 1 and 2 */
        int XOR1 = 0, XOR2 = 0;
        
        // Traverse the array
        for(int i=0; i < n; i++) {
            
            /* Divide the numbers among bucket 1
             and 2 based on rightmost set bit */
            if(nums[i] & rightmost) {
                XOR1 = XOR1 ^ nums[i];
            }
            else {
                XOR2 = XOR2 ^ nums[i];
            }
        }
        
        // Return the result in sorted order
        if(XOR1 < XOR2) return {XOR1, XOR2};
        return {XOR2, XOR1};
    }
};
```

