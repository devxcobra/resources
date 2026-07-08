
stack<data_type> st;

Valid Parantheses
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.
approach : last opening i saw is of my type(current parantheses type) or not

```cpp
class Solution {
public:
    bool isValid(string s) {

        stack<char> st;

        for(int i = 0; i< s.length(); i++)

        {
            if(s[i] == '(' || s[i] == '{' || s[i] == '[')

            {
                st.push(s[i]);
            }
            else

            {

                if(st.empty()) return false;

                else

                {
                    char c = st.top();
                    st.pop();

                    if(( c == '(' && s[i] == ')') || ( c == '{' && s[i] == '}' )                             || ( c == '[' && s[i] == ']' ))

                    {
                        continue;
                    }
                    else
                    return false;
                }
            }
        }
        return st.empty();
    }
};
```



## Infix to postfix conversions

rules:
- **Approach to Convert Infix Expression to Postfix:**

- Start by scanning the infix expression from left to right.
#### - If the scanned character is an operand, print it immediately.
#### - If the scanned character is an operator:

- If the precedence of the operator is greater than the operator in the stack, or the stack is empty, or the stack contains a ‘(’, push the operator into the stack.
- Otherwise, pop all operators from the stack with higher or equal precedence than the scanned operator, then push the scanned operator into the stack.

- If the scanned character is a ‘(’, push it into the stack.
- If the scanned character is a ‘)’, pop the stack and output the operators until a ‘(’ is encountered, and discard both parentheses.

```cpp
#include<bits/stdc++.h>
using namespace std;

// Function to return precedence of operators
int prec(char c) {
    if (c == '^')  // Exponent operator has highest precedence
        return 3;
    else if (c == '/' || c == '*')  // Multiplication and division have higher precedence than addition
        return 2;
    else if (c == '+' || c == '-')  // Addition and subtraction have lowest precedence
        return 1;
    else
        return -1;
}

// The main function to convert infix expression to postfix expression
void infixToPostfix(string s) {
    stack<char> st; // Stack to hold operators and parentheses
    string result;  // String to hold the resulting postfix expression

    for (int i = 0; i < s.length(); i++) {
        char c = s[i];

        // If the scanned character is an operand, add it to the result string
        if ((c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z') || (c >= '0' && c <= '9'))
            result += c;

        // If the scanned character is an ‘(‘, push it to the stack
        else if (c == '(')
            st.push('(');

        // If the scanned character is a ‘)’, pop from stack until an ‘(‘ is encountered
        else if (c == ')') {
            while (st.top() != '(') {
                result += st.top();
                st.pop();
            }
            st.pop();  // Pop the ‘(‘ from the stack
        }

        // If an operator is scanned
        else {
            while (!st.empty() && prec(s[i]) <= prec(st.top())) {
                result += st.top();
                st.pop();
            }
            st.push(c);  // Push the current operator to the stack
        }
    }

    // Pop all the remaining elements from the stack
    while (!st.empty()) {
        result += st.top();
        st.pop();
    }

    cout << "Postfix expression: " << result << endl;  // Output the result
}
```

Prefix to Infix 

- Traverse the prefix expression from right to left.
- Use a stack to store operands.
- For each operator, pop two operands from the stack, wrap them in parentheses, and push the resulting expression back.
- The final item in the stack will be the infix expression.

```cpp
#include <bits/stdc++.h>
using namespace std;

// Function to convert prefix to infix
string prefixToInfix(string prefix) {
    stack<string> s;
    int n = prefix.size();

    // Traverse the prefix expression from right to left
    for (int i = n - 1; i >= 0; i--) {
        char c = prefix[i];

        // If the character is an operand, push it to the stack
        if (isalnum(c)) {
            s.push(string(1, c));
        } else {
            // Pop two operands from the stack
            string op1 = s.top(); s.pop();
            string op2 = s.top(); s.pop();

            // Form the new infix expression and push back to stack
            s.push("(" + op1 + c + op2 + ")");
        }
    }

    // The final element in the stack is the result
    return s.top();
}
```

Prefix to Postfix 

- Traverse the prefix expression from right to left.
- Use a stack to store operands.
- For each operator, pop two operands from the stack, combine them with the operator like 
- **top1 +top2 + operator** and push the result back.
- The final item in the stack will be the postfix expression.

```cpp
#include <bits/stdc++.h>
using namespace std;

// Function to convert prefix to postfix
string prefixToPostfix(string prefix) {
    stack<string> s;
    int n = prefix.size();

    // Traverse the prefix expression from right to left
    for (int i = n - 1; i >= 0; i--) {
        char c = prefix[i];

        // If the character is an operand, push it to the stack
        if (isalnum(c)) {
            s.push(string(1, c));
        } else {
            // Pop two operands from the stack
            string op1 = s.top(); s.pop();
            string op2 = s.top(); s.pop();

            // Form the new postfix expression and push back to stack
            s.push(op1 + op2 + c);
        }
    }

    // The final element in the stack is the result
    return s.top();
}

int main() {
    string prefix = "*-A/BC-/AKL";
    cout << "Postfix Expression: " << prefixToPostfix(prefix) << endl;
    return 0;
}

```

## Next greater element

Given an integer array A, return the next greater element for every element in A. The next greater element for an element x is the first element greater than x that we come across while traversing the array in a clockwise manner. If it doesn't exist, return -1 for this element.

code:
```cpp
#include <bits/stdc++.h>
using namespace std;

// Solution class to find next greater elements
class Solution {
public:
    // Function to find next greater elements
    vector<int> nextGreater(vector<int>& nums) {
        // Stack to store elements
        stack<int> st;

        // Result array of same size
        int n = nums.size();
        vector<int> res(n);

        // Traverse from right to left
        for (int i = n - 1; i >= 0; i--) {

            // Pop all smaller or equal elements
            while (!st.empty() && st.top() <= nums[i]) {
                st.pop();
            }

            // If stack is empty, no greater element
            if (st.empty()) res[i] = -1;

            // Else top of stack is the answer
            else res[i] = st.top();

            // Push current element
            st.push(nums[i]);
        }

        // Return the result
        return res;
    }
};
```

for another variation .. in which the **array is circular, use the double array method**.. and traverse from i = 2n-1 to 0 ... store answers for i < n

## Trapping Rainwater:
 Given an array of non-negative integers representation elevation of ground. Your task is to find the water that can be trapped after rain .

Approach:

```
At any index `i`, the water stored depends on:

water at i=min(max height on left,max height on right)−height[i]
```

computing lmax and rmax for every i would take O(n^2) overall

two pointer method:

approach: 
- If `height[left] <= height[right]`, then:
    - There _definitely exists_ a taller bar on the right
    - So water at `left` depends only on `lmax`
- If `height[right] < height[left]`, then:
    - There exists a taller bar on the left
    - So water at `right` depends only on `rmax`

```cpp
class Solution {

public:
    int trap(vector<int>& height) {

        int n = height.size();
        int ans = 0;
        int lmax = 0;
        int rmax = 0;
        int left = 0;
        int right = n-1;

        while(left<right)

        {
            if(height[left] <= height[right])
            {
                if(height[left] < lmax)
                {
                    ans += lmax - height[left];
                }
                else
                {
                    lmax = height[left];
                }

                left++;
            }
            else
            {
                if(height[right] < rmax)

                {
                    ans += rmax - height[right];
                }
                else
                {
                    rmax = height[right];
                }
                right--;
            }
        }

        return ans;

    }

};
```
## sum of subarrays minimums..

 Given an array of integers arr of size n, calculate the sum of the minimum value in each (contiguous) subarray of arr. Since the result may be large, return the answer modulo 10⁹ +7

approach:
For every element, count in how many subarrays it is the minimum

For each element `arr[i]`, find:

- How far it can extend to the **left** while still being the minimum
- How far it can extend to the **right** while still being the minimum

We find:
### 👉 Previous Smaller or Equal Element (PSEE) -- returns index here

- Nearest element on left which is **≤ arr[i]**

### 👉 Next Smaller Element (NSE) -- returns index here

- Nearest element on right which is **< arr[i]**

```cpp
#include <bits/stdc++.h>
using namespace std;
class Solution {
private:
    /* Function to find the indices of 
    next smaller elements */
    vector<int> findNSE(vector<int> &arr) {
        
        int n = arr.size();
        
        vector<int> ans(n);
        
        stack<int> st;
        
        for(int i = n - 1; i >= 0; i--) {
            
            int currEle = arr[i];
            
            /* Pop the elements in the stack until 
            the stack is not empty and the top 
            element is not the smaller element */
            while(!st.empty() && arr[st.top()] >= arr[i]){
                st.pop();
            }
            // Update the answer
			if(!st.empty())
			    ans[i] = st.top();
			else
			    ans[i] = n;
// because later we do right = nse[i] - i.. meaning it can extend to the end of the array       
            
            /* Push the index of current 
            element in the stack */
            st.push(i);
        }
        
        // Return the answer
        return ans;
    }
    /* Function to find the indices of 
    previous smaller or equal elements */
    vector<int> findPSEE(vector<int> &arr) {
        
        
        int n = arr.size();
        
        vector<int> ans(n);
    
        stack<int> st;
        
        for(int i=0; i < n; i++) {
            
            // Get the current element
            int currEle = arr[i];
            /* Pop the elements in the stack until 
            the stack is not empty and the top 
            elements are greater than the current element */
            while(!st.empty() && arr[st.top()] > arr[i]){
                st.pop();
            }
            // Update the answer
            ans[i] = !st.empty() ? st.top() : -1;
            
            /* Push the index of current 
            element in the stack */
            st.push(i);
        }
        // Return the answer
        return ans;
    }
    
public:
    /* Function to find the sum of the 
    minimum value in each subarray */
    int sumSubarrayMins(vector<int> &arr) {
        
        vector<int> nse = 
            findNSE(arr);
        
        vector<int> psee =
            findPSEE(arr);
        
        // Size of array
        int n = arr.size();
        
        int mod = 1e9 + 7; // Mod value
        // To store the sum
        int sum = 0;
        
        // Traverse on the array
        for(int i=0; i < n; i++) {
            
            // Count of first type of subarrays
            int left = i - psee[i];
            
            // Count of second type of subarrays
            int right = nse[i] - i;
            
            /* Count of subarrays where 
            current element is minimum */
            long long freq = left*right*1LL;
            
            // Contribution due to current element 
            int val = (freq*arr[i]*1LL) % mod;
            
            // Updating the sum
            sum = (sum + val) % mod;
        }
        
        // Return the computed sum
        return sum;
    }
};
```

#### remove K digits

Given string num representing a non-negative integer `num`, and an integer `k`, return _the smallest possible integer after removing_ `k` _digits from_ `num`.

approach :
```cpp
class Solution {

public:
    string removeKdigits(string num, int k) {
        stack<char> st;
        int n = num.size();
        for(int i = 0; i< n; i++)
        {
            while(!st.empty() && k > 0 && st.top() > num[i]) // always pick smaller numbers from left.. retaining the order
            {
                st.pop();
                k--;
            }

            st.push(num[i]);
        }

        while(k > 0)
        {
            st.pop();
            k--;
        }

        if(st.empty()) return "0"; 

        string res = "";

        while(!st.empty())
        {
            res.push_back(st.top());
            st.pop();
        }

        while(res.size() != 0 && res.back() == '0') //remove leading zeros

        {
            res.pop_back();
        }

        reverse(res.begin(), res.end());

        if(res.empty()) return "0";

        return res;

    }

};
```

#### Largest rectangle in a histogram

```cpp

class Solution {

public:

    int largestRectangleArea(vector<int>& heights) {

        stack<int> st;
        int maxarea = 0;
        int n = heights.size();

        for(int i = 0; i < n; i++)
        {

            while(!st.empty() && heights[st.top()] > heights[i])

            {
                int nse = i;
                int element = st.top();
                st.pop();
                int pse;

                if(st.empty())
                {
                    pse = -1;
                }
                else
                {
                    pse = st.top();
                }
                maxarea = max(maxarea, heights[element] * (nse - pse -1));
            }
            st.push(i);

        }
        while(!st.empty())

        {
            int nse = n;
            int element = st.top();
            st.pop();
            int pse;

            if(st.empty())
                {
                    pse = -1;
                }
                else
                {
                    pse = st.top();
                }
            maxarea = max(maxarea, heights[element] * (nse - pse -1));

        }
        return maxarea;

    }
};
```

#### Sliding Window Maximum:

Given an array of integers arr, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window..

approach:

Maintain decreasing order
```cpp
while (!dq.empty() && nums[dq.back()] < nums[i])  
dq.pop_back();
```

### Why remove smaller elements
Because:

- If `nums[i]` is bigger than elements before it,
- Those smaller elements will **never become max** in any future window

```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    // Function to return the max of each sliding window of size k
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        
        deque<int> dq;

        vector<int> result;

        for (int i = 0; i < nums.size(); i++) {
            // Remove elements from the front if they are out of this window's range
            if (!dq.empty() && dq.front() <= i - k) {
                dq.pop_front();
            }

            // Remove all elements from the back that are smaller than current element
            while (!dq.empty() && nums[dq.back()] < nums[i]) {
                dq.pop_back();
            }

            // Add the current index to the deque
            dq.push_back(i);

            // Once the first window is completed, add front element to result
            if (i >= k - 1) {
                result.push_back(nums[dq.front()]);
            }
        }

        // Return the final result
        return result;
    }
};
```

