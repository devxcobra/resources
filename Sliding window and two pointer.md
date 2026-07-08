
whenever there is a question like finding the maximum substring or something like that always think of two pointer and sliding window approach

Longest Substring without repeating characters

optimal approach:

```cpp

class Solution {

public:

    int lengthOfLongestSubstring(string s) {

        vector<int> hash(256,-1);
        
        int r = 0;
        int l = 0;
        int maxlen = 0;
        int n = s.length();
  
        while(r < n)

        {
            if(hash[s[r]] != -1)
            {
                if(l <= hash[s[r]])
                {
                    l = hash[s[r]] + 1;
                }
            }
  
             int temp = r - l + 1;

            maxlen = max(temp, maxlen);

            hash[s[r]] = r;

            r++;

        }
  
        return maxlen;
    }

};

```

#### maximum consecutive ones III

Given a binary array nums and an integer k, return the maximum number of consecutive 1's in the array if you can flip at most k 0's.

think of it like the longest substring with k zeros.

```cpp

class Solution {

public:

    int longestOnes(vector<int>& nums, int k) {

        int l = 0;
        int r = 0;
        int maxlen = 0;
        int zeros = 0;
        int n = nums.size();

        while(r < n)

        {
            if(nums[r] == 0)

            {
                zeros++;

            }

            while(zeros > k)

            {
                if(nums[l] == 0) zeros--;

                l++;

            }

            if(zeros <= k)

            {
                maxlen = max(maxlen, r- l + 1);
            }

            r++;

        }

        return maxlen;
    }

};
```

#### longest repeating character replacement

You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return _the length of the longest substring containing the same letter you can get after performing the above operations_.

###### most optimal approach:

the logic is : **(length) - max_freq <= k** .. for the string to be valid..

there no need to recompute maxfreq every time we remove from left because.. it is not going to contribute to our answer..

the answer will only update when we encounter a valid string whose length is bigger than previous.
for that to happen: **r increases. l remains same.. and max freq increases**.. 
so while((r-l+1) - maxfreq > k) doesnt get executed and maxlen increases.. 

```cpp
class Solution {

public:
    int characterReplacement(string s, int k) {

        int l = 0;
        int r = 0;
        int maxlen = 0;
        int maxfreq = 0;
        int n = s.length();

        vector<int> hash(26, 0);


        while(r < n)

        {

            hash[s[r] -'A']++;

            maxfreq = max(maxfreq, hash[s[r]- 'A']); // depends only on right element.. purane max freq se fark ni padta kyoki max len already stored hai

            while((r-l+1) - maxfreq > k)

            {
                hash[s[l] -'A']--;
                l++;
            }

            maxlen = max(maxlen, r-l+1);

            r++;
        }

        return maxlen;
    }

};
```


##### Binary subarray with sum

You are given a binary array nums (containing only 0s and 1s) and an integer goal. Return the number of non-empty subarrays of nums that sum to goal. A subarray is a contiguous part of the array.

Approach : traditional sliding window approach wont work here, because there are zeros in the array.. moving through the zeros wont affect the sum..

we will solve a question like **no of subarrays with sum <= k.**

**ans = f(k) - f(k-1)**

```cpp
#include <bits/stdc++.h>
using namespace std;
class Solution {
public:
    // Function to calculate number of subarrays with sum exactly equal to goal
    int numSubarraysWithSum(vector<int>& nums, int goal) {
        // Return difference between subarrays with sum at most goal and at most (goal - 1)
        return atMost(nums, goal) - atMost(nums, goal - 1);
    }

private:
    // Helper function to compute number of subarrays with sum at most k
    int atMost(vector<int>& nums, int k) {
        // If k is negative, no such subarrays exist
        if (k < 0) return 0;

        int left = 0;
        int sum = 0;
        int count = 0;

        // Traverse the array using right pointer
        for (int right = 0; right < nums.size(); right++) {
            // Add current element to sum
            sum += nums[right];

            // Shrink the window from the left if sum exceeds k
            while (sum > k) {
                sum -= nums[left];
                left++;
            }

            // Add the number of valid subarrays ending at right
            count += (right - left + 1);
        }

        return count;
    }
};
```

- Sliding window needs **continuous valid region**
- atMost(k) → continuous ✅
- exactly(k) → scattered ❌
- so direct sliding window fails for exactly k.

