

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