
Implement MIn Heap:

```cpp
class BinaryHeap {
public:
    // Maximum elements that can be stored in heap
    int capacity;  
    
    // Current no of elements in heap
    int size;  
    
    // Array for storing the keys
    int* arr;  

    BinaryHeap(int cap) {
        // Assigning the capacity
        capacity = cap;  
        
        // Initially size of heap is zero
        size = 0;  
        
        // Creating an array
        arr = new int[capacity];  
    }

    // Returns the parent of ith Node
    int parent(int i) {
        return (i - 1) / 2;
    }

    // Returns the left child of ith Node
    int left(int i) {
        return 2 * i + 1;
    }

    // Returns the right child of the ith Node
    int right(int i) {
        return 2 * i + 2;
    }

    // Insert a new key x
    void Insert(int x) {
        if (size == capacity) {
            cout << "Binary Heap Overflow" << endl;
            return;
        }

        // Insert new element at end
        arr[size] = x;  

        // Store the index, for checking heap property
        int k = size;  

        // Increase the size
        size++;  

        // Fix the min heap property
        while (k != 0 && arr[parent(k)] > arr[k]) {
            swap(&arr[parent(k)], &arr[k]);
            k = parent(k);
        }
    }

    void Heapify(int ind) {
        // Right child
        int ri = right(ind);  
        
        // Left child
        int li = left(ind);  
        
        // Initially assume violated value is minimum
        int smallest = ind;  

        if (li < size && arr[li] < arr[smallest])
            smallest = li;

        if (ri < size && arr[ri] < arr[smallest])
            smallest = ri;

        // If the Minimum among the three nodes is not the parent itself,
        // then swap and call Heapify recursively
        if (smallest != ind) {
            swap(&arr[ind], &arr[smallest]);
            Heapify(smallest);
        }
    }

    int getMin() {
        return arr[0];
    }

    int ExtractMin() {
        if (size <= 0)
            return INT_MAX;

        if (size == 1) {
            size--;
            return arr[0];
        }

        int mini = arr[0];

        // Copy last Node value to root Node
        arr[0] = arr[size - 1];  

        size--;

        // Call heapify on root node
        Heapify(0);  

        return mini;
    }

    void Decreasekey(int i, int val) {
        // Updating the new value
        arr[i] = val;  

        // Fixing the Min heap
        while (i != 0 && arr[parent(i)] > arr[i]) {
            swap(&arr[parent(i)], &arr[i]);
            i = parent(i);
        }
    }

    void Delete(int i) {
        Decreasekey(i, INT_MIN);
        ExtractMin();
    }

    void swap(int* x, int* y) {
        int temp = *x;
        *x = *y;
        *y = temp;
    }

    void print() {
        for (int i = 0; i < size; i++)
            cout << arr[i] << " ";
        cout << endl;
    }
};
```


A Binary Heap is a Binary Tree that satisfies the following conditions.

- It should be a Complete Binary Tree.
- It should satisfy the Heap property.

- **Min Heap property:** For every node in a binary heap, if the **node value is less than its right and left child’s value** then Binary Heap is known as Min Heap. The property of Node’s value less than its children’s value is known as Min Heap property. In Min Heap, the root value is always the Minimum value in Heap.
- - **Max Heap property:** For every node in a binary heap, if the **node value is greater than its right and left child’s value** then Binary Heap is known as Max Heap. The property of being Node’s value greater than its children's value is known as Max Heap property. In Max Heap, the root value is always the maximum value in Heap.

A Binary Heap is represented as an array.
- The root value is at arr[0]
Node index : i
Left child index : 2 i + 1
Right child index: 2 i + 2



Use heaps:

```cpp
max heap:
priority_queue<int> maxh;

min heap:
priority_queue<int, vector<int> , greater<int>> minh;
```

Min heap : smallest element is on the top
Max heap: largest element is on the top




Kth smallest element: max heap use hoga
```cpp
    int kthSmallest(vector<int> &arr, int k) {
        // code here
        priority_queue<int> maxh;
        
        for(int i = 0; i < arr.size(); i++)
        {
            maxh.push(arr[i]);
            
            if(maxh.size() > k)
            {
                maxh.pop();
            }
        }
        
        return maxh.top();
    }
};
```

Sort k sorted array
```cpp
class Solution {
  public:
    void nearlySorted(vector<int>& arr, int k) {
        
        int n = arr.size();
       priority_queue<int, vector<int> , greater<int>> minh;
       int j = 0;
        
        for(int i = 0; i < n ; i++)
        {
            minh.push(arr[i]);
            
            if(minh.size() > k)
            {
                arr[j] = minh.top();
                minh.pop();
                j++;
            }
        }
        while(minh.size())
        {
            arr[j] = minh.top();
            minh.pop();
            j++;
        }
    }
};
```

Top K frequent elements
```cpp
class Solution {

public:

    vector<int> topKFrequent(vector<int>& nums, int k) {

        unordered_map<int,int> mp;

  

        priority_queue<pair<int,int> , vector<pair<int,int>> , greater<pair<int,int>>> minh;

  

        for(int i = 0; i< nums.size(); i++)

        {

            mp[nums[i]]++;

        }

  
  

        for(auto it: mp)

        {

            minh.push({ it.second , it.first});

            if(minh.size() > k)

            {

                minh.pop();

            }

        }

  

        vector<int> ans;

  

        while(minh.size())

        {   auto it = minh.top();

            ans.push_back(it.second);

            minh.pop();

        }

  

        return ans;

    }

};
```

Given an array, **arr[]** of rope lengths, connect all ropes into a single rope with the **minimum total cost**. The **cost** to connect two ropes is the **sum of their lengths**.

```cpp
class Solution {
  public:
    int minCost(vector<int>& arr) {
        // code here
        priority_queue<int, vector<int>, greater<int>> minh;
        
        for(int i = 0; i < arr.size(); i++)
        {
            minh.push(arr[i]);
        }
        int rope = 0;
        int cost = 0;
        while(minh.size() >= 2)
        {
            int top1 = minh.top();
            minh.pop();
            int top2 = minh.top();
            minh.pop();
            
            rope = top1 + top2;
            cost += rope;
            
            minh.push(rope);
            
        }
        return cost;
    }
};
```

Merge k sorted lists
```cpp
class Solution {

      struct CompareNode {
        bool operator()(ListNode* const& p1, ListNode* const& p2) {
            // return "true" if p1 is ordered after p2 (creates a min-heap)
            return p1->val > p2->val;
        }

    };
  
public:

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        priority_queue <ListNode* , vector<ListNode*> , CompareNode> pq;

        for(auto it: lists)

        {   if(it)
            {pq.push(it);}
        }

        ListNode* dummy = new ListNode(0);
        ListNode* tail = dummy;

        while(pq.size())

        {
         ListNode* node = pq.top();
            pq.pop();

            tail->next = node;
            tail = tail->next;

            if(node->next)

            {
                pq.push(node->next);
            }
        }
        return dummy->next;
    }

};
```

Hands of Straight
```cpp
class Solution {
public:

    bool isNStraightHand(vector<int>& hand, int groupSize) {

        if(hand.size() % groupSize) return false;

        priority_queue<int> pq;

  

        for(auto it: hand)

        {
            pq.push(it);
        }

        int k = 0;
        int prev_card = 0;
        vector<int> temp;

        while(!pq.empty())
        {

            int card = pq.top();
            pq.pop();

            if (k > 0 && card == prev_card)
            {temp.push_back(card);
            continue;
            }

            if(k > 0 && card - prev_card != -1)
            {
                return false;
            }

            k++;
            prev_card = card;

            if(k == groupSize)
            {
                for(auto it: temp)
                {
                    pq.push(it);
                }

                temp.clear();

                k = 0;

            }
        }
        return k ==0;

    }
};
```

 Maximum Sum Combination 
Given two integer arrays nums1 and nums2 and an integer k, **return the maximum k valid sum combinations from all possible sum combinations using the elements of nums1 and nums2**
```cpp
class Solution {
  public:
    vector<int> topKSumPairs(vector<int>& a, vector<int>& b, int k) {
        // code here
        sort(a.begin(), a.end(), greater<int>());
        sort(b.begin(), b.end(), greater<int>());
        
        priority_queue<tuple<int,int,int>> maxh;
        
        set<pair<int,int>> visited;
        
        maxh.push({a[0] + b[0], 0, 0});
        visited.insert({0,0});
        vector<int> result;
         
        
        while(k-- && !maxh.empty() )
        {
            auto [sum ,i, j] = maxh.top();
            maxh.pop();
            
            result.push_back(sum);
            
            if(i + 1 < a.size() && !visited.count({i+1, j}))
            {
                maxh.push({a[i+1] + b[j] , i+ 1, j});
                visited.insert({i+1, j});
            }
            
            if(j+ 1 < b.size() && !visited.count({i, j+1}))
            {
                maxh.push({a[i] + b[j+1] , i, j+1});
                visited.insert({i, j+1});
            }
        }
        
        return result;
    }
};
```


