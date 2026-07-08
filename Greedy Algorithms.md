
Fractional knapsacks

**Problem Statement:** The weight of N items and their corresponding values are given. We have to put these items in a knapsack of weight W such that the total value obtained is maximized.  
  
**Note:** We can either take the item as a whole or break it into smaller units.

approach: sort according to val/wt (unitary method)

```cpp
class Solution {
public:
    
    static bool comp(pair<int,int> a, pair<int,int> b) {
        return (1LL * a.first * b.second) > (1LL * b.first * a.second);
    }
    
    double fractionalKnapsack(vector<int>& val, vector<int>& wt, int capacity) {
        
        int n = val.size();
        vector<pair<int,int>> item(n);
        
        for(int i = 0; i < n; i++) {
            item[i] = {val[i], wt[i]};
        }
        
        double totalVal = 0;
        double capacit = capacity;
        
        sort(item.begin(), item.end(), comp);
        
        for(int i = 0; i < n; i++) {
            if(item[i].second <= capacit) {
                totalVal += item[i].first;
                capacit -= item[i].second;
            } else {
                double temp = ((double)item[i].first / item[i].second) * capacit;
                totalVal += temp;
                break;
            }
        }
        
        return totalVal;
    }
};
```

Valid parenthesis modified.

new : `'*'` could be treated as a single right parenthesis `')'` or a single left parenthesis `'('` or an empty string `""`

approach:
traditional : maintain a counter .. as counter goes less than 0 return false, if at end counter = 0 return true.

modified approach:
maintain a range of counter to account for multiple cases.
```cpp
class Solution {
public:
    bool checkValidString(string s) {

        int min = 0;
        int max = 0;

        for(char c: s)
        {
            if(c == '(')
            {
                min++;
                max++;
            }

            else if(c == ')')
            {
                min--;
                max--;
            }

            else // for *
            {
                min--; 
                max++;
            }

            if(min < 0) min = 0; // we never let min go less than 0, that case wont contribute to the answer .. for the case of ) also
            if(max < 0) return false; // whenever max goes less than 0 the ) has outnumbered ( till there, so not valid

        }
        return (min == 0);

    }
};
```

N meetings in a room

Given two arrays **s****[] and **f[]**, where **s[i]** denotes the start time and **f[i]** denotes the finish time of the i-th meeting. There is only one meeting room, find the **maximum** number of meetings that can be scheduled in the room such that no two selected meetings overlap in time. Return the indices(1-based) of the selected meetings in sorted (increasing) order.

approach : sort by ending time and compare next meetings starting time

- Choose the meeting that **ends earliest**
- This ensures:
    - Minimum blocking of timeline
    - Maximum remaining room for future meetings

```cpp
class Solution {
  public:
  
    struct node
    {
        int start;
        int end;
        int index;
        
    };
    
    static bool comp(node a, node b)
    {
        return a.end < b.end;
    }
    
    vector<int> maxMeetings(vector<int> &s, vector<int> &f) {
        
        int n = s.size();
        
        vector<node> a(n);
        
        for(int i = 0; i < n; i++)
        {
            a[i].start = s[i];
            a[i].end = f[i];
            a[i].index = i+1;
            
        }
        
        sort(a.begin(), a.end(), comp);
        
        
        int freetime = a[0].end;
        int count = 1;
        vector<int> order;
        order.push_back(a[0].index);
        
        for(int i = 1; i < n; i++)
        {
            if(a[i].start > freetime)
            {
                count++;
                freetime = a[i].end;
                order.push_back(a[i].index);
            }
            else
            {
                continue;
            }
        }
        
        sort(order.begin(), order.end());
        
        return order;
        
        
    }
};
```
