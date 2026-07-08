
sorting of strings: happens lexographically

ompare s1 and s2:  
1. Compare characters one by one  
2. First mismatch decides  
3. If no mismatch → shorter string comes first

ex:
```cpp
["dog", "cat", "apple"]

sorted:

["apple", "cat", "dog"]
```

so longest common prefix can also be checked by just comparing the first and last (sorted) strings element by element.

Some syntax:

```cpp
string x = s.substr(start, length);


TC = SC = O(length)
```

this function can be used to extract a substring directly rather than using a for loop.


ASCII assigns a **number (0–255)** to characters.
for mapping we can use:

```cpp
vector<int> map(256, -1);
```

lets say there's a string s

```cpp
map[s[i]] = some int or even some char(yk)
```


**isomorphism question** :
Two strings s and t are isomorphic if the characters in s can be replaced to get t.  
All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character, but a character may map to itself.

```cpp
my soln : brute force O(n2)

class Solution {

public:

    bool isIsomorphic(string s, string t) {

        vector<pair <int, int>> pair1;

        vector<pair <int, int>> pair2;

        int n = s.length();

  
        for(int i = 0; i < n; i++)

        {
            for(int j = i+1; j < n; j++)
            {
                if(s[i] == s[j])
                {
                    pair1.push_back({i,j});
                }
            }
        }
  
        for(int i = 0; i < n; i++)
        {
            for(int j = i+1; j < n; j++)
            {
                if(t[i] == t[j])
                {
                    pair2.push_back({i,j});
                }
            }
        }

        int z1= pair1.size();
        int z2 = pair2.size();

        if(z1 == z2)
        {
            for(int i = 0; i< z1; i++)
            {
                if(pair1[i] == pair2[i])
                {
                    continue;
                }

                else
                {
                    return false;
                }
            }

            return true;
        }
        else
        {
            return false;
        }

    }

};
```

optimal code:

```cpp
class Solution {

public:

    bool isIsomorphic(string s, string t) {

        int n = s.length();

        vector<int> map_s(256, -1);
        vector<int> map_t(256, -1);

        for(int i = 0; i < n; i++)
        {
            if(map_s[s[i]] == -1 && map_t[t[i]] == -1)
            {
                map_s[s[i]] = t[i];
                map_t[t[i]] = s[i];
            }

        }
        for(int i = 0; i< n; i++)
        {
            if(map_s[s[i]] != -1 || map_t[t[i]] != -1)
            {
                if(map_s[s[i]] != t[i] || map_t[t[i]] != s[i])
                {
                    return false;
                }
            }
        }
        return true;
    }
};
```

**Rotation check = substring of doubled string**
syntax:
```cpp
string.find(substring)

|Return Value|

Found - Starting index (0-based)
Not found - `string::npos`


we check like this: 

if (doubledS.find(goal) != string::npos) // found condition

```

##### sort characters by frequency:

```cpp
class Solution {

public:

    string frequencySort(string s) {
  
        int n = s.length();
        vector<pair<int,int>> map(62);
  

        for(int i = 0; i < 62; i++)
        {
            map[i] = {0, i};
        }

        for(int i = 0 ; i < n; i++)

        {   if(s[i] >= 'a' && s[i] <= 'z')
                map[s[i] - 'a'].first++;

            else if(s[i] >= 'A' && s[i] <= 'Z')
                map[s[i] - 'A' + 26].first++;

            else if(s[i] >= '0' && s[i] <= '9')
                map[s[i] - '0' + 52].first++;
        }

        sort(map.begin(), map.end());
        reverse(map.begin() , map.end());
  
        string ans = "";

        for(int i = 0; i< 62; i++)
        {

            if( map[i].first > 0 )

            {   char term;

                if(map[i].second >= 0 && map[i].second <= 25)
                term = map[i].second + 'a';

                else if(map[i].second >= 26 && map[i].second <= 51)
                term = map[i].second - 26 + 'A';

                else if(map[i].second >= 52 && map[i].second <= 61)
                term = map[i].second - 52 + '0';

                for(int j = 0; j < map[i].first; j++)

                {
                     ans.push_back(term);
                }

            }

        }

        return ans;
  
    }
};
```

Note : 

> Using `ans = ans + term` caused O(n²) copying and MLE; switching to `push_back()` made it O(n) by avoiding repeated reallocations.
> ' `ans = ans + term` makes copy each time and adds the new term. whereas `push_back()` modifies string **in-place**

##### Return the number of substrings that contain exactly k distinct characters.

Brute force : o(n^2) .. checking every substring

optimal method: sliding window method
the concept is : exactly k distinct = atmost (k) - atmost (k-1)
O(n)
```cpp
class Solution {
  public:
  
    int atmost(string& s, int k)
    {
        int n = s.length();
        
        vector<int> freq(26, 0);
        
        int left = 0;
        int distinct = 0;
        int ans = 0;
        
        for(int right  = 0; right < n; right++)
        {
            
            if(freq[s[right] - 'a'] == 0)
            {
                distinct++;
            }
            freq[s[right] - 'a']++;
            
            while(distinct > k)
            {
                freq[s[left] - 'a']--;
                if(freq[s[left] - 'a'] == 0)
                {
                    distinct --;
                }
                left++;
            }
            
            ans = ans + (right - left + 1);
            
        }
        
        return ans;
    }
    int countSubstr(string& s, int k) {
        
        int final = atmost(s, k) - atmost(s, k-1);
        
        return final;
        
    }
};
```

Longest Palindromic Substring
**To enumerate all palindromic substrings of a given string, we first expand a given string at each possible starting position of a palindrome and also at each possible ending position of a palindrome and keep track of the length of the longest palindrome we found so far.**
```cpp
class Solution {
public:
    string longestPalindrome(string s) {

        if (s.length() <= 1)
            {return s;}

        auto expand_from_center = [&](int left, int right) {

            while (left >= 0 && right < s.length() && s[left] == s[right]) {

                left--;
                right++;
            }
            return s.substr(left + 1, right - left - 1);
        };

        string max_str = s.substr(0, 1);

        for (int i = 0; i < s.length() - 1; i++) {

            string odd = expand_from_center(i, i);
            string even = expand_from_center(i, i + 1);

            if (odd.length() > max_str.length()) {
                max_str = odd;
            }

            if (even.length() > max_str.length()) {
                max_str = even;
            }

        }

        return max_str;
    }
};
```

