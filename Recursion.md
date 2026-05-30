
pow(x, n)

```cpp

    double myPow(double x, int n) {

    if (x==0) return 0;

    long long nn = n; // long long take to encounter -ve integer overflow

    if(nn<0) nn = (-1)*nn;

    double ans  = 1;

    while(nn)

    {

        if(nn%2)

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

    bool isalpha(char c)

    {

        if (  c -'a' >= 0 && c-'a' <= 25 )

        {
            return true;
        }

        else if( c - 'A' >= 0 && c - 'A' <= 25)

        {
            return true;
        }

        else if( c - '0' >= 0 && c-'0' <=9)

        {
            return true;
        }

        else

        {
            return false;
        }

    }

    string correct(string s)

    {  

        int i = 0;

        string ans;

        while(i < s.length())

        {

            if(isalpha(s[i]))

            {   char c;

                if( s[i] - 'A' >= 0 && s[i] - 'A' <= 25)

                {
                    c = 'a' + (s[i] - 'A');
                }

                else
                {
                    c = s[i];
                }

                ans += c;
                i++;

            }

            else

            {
                i++;
            }
        }
  
        return ans;

    }

    bool isPalindrome(string s) {

        s = correct(s);

        return ispal(0,s);

    }

};

```

crazzy way to print N to 1 using recursion (backtracking)

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

