syntax:

```cpp
struct Node {    int data; 
	            Node* next;};
```
Struct is defined **outside `main()`**

struct -- // public by default
class --  // private by default

### Constructor

Constructor ensures every node is initialized properly without relying on memory or manual setup.
Constructor belongs to the class, but its implementation can be inside or outside.
**Mostly written inside the struct/class.**

```cpp
Node(int data1) 
{  
data = data1;  
next = nullptr;  
}
```

....
```
Node* temp;
```

👉 temp is a **pointer**  
👉 It stores **address of a Node**

---

### Difference:

```
Node a;     // actual object
Node* b;    // pointer (stores address)
```

# 4. Creating a Node (FULL BREAKDOWN)

```
Node* newNode = new Node();
newNode->data = 10;
newNode->next = NULL; // Node is not connected yet
```

## Visualization

```
newNode ─────► [ data | next ]
```

## **Inserting Node**

```cpp
Node* temp = head; // not a copy , temp stores address of real node

for (int i = 0; i < pos - 1; i++) {
    temp = temp->next;
}

newNode->next = temp->next;
temp->next = newNode; // the actual node before the insertion now points to the new node, not any copy it..modifies original list
```


curr->next - address
curr - pointer
curr = curr->next // assigning that address to the pointer..

### Delete Last Node of Linked List

```cpp
#include <bits/stdc++.h>
using namespace std;

// Definition for singly linked list
struct Node {
    int data;
    Node* next;
    Node(int val) {
        data = val;
        next = NULL;
    }
};

class Solution {
public:
    // Function to delete tail node of linked list
    Node* deleteTail(Node* head) {
        // If list is empty or has one node
        if (head == NULL || head->next == NULL) {
            delete head;
            return NULL;
        }

        // Traverse to the second last node
        Node* curr = head;
        while (curr->next->next != NULL) {
            curr = curr->next;
        }

        // Delete tail node
        delete curr->next;
        curr->next = NULL;

        // Return updated head
        return head;
    }
};

// Driver code
int main() {
    Node* head = new Node(1);
    head->next = new Node(2);
    head->next->next = new Node(3);

    Solution obj;
    head = obj.deleteTail(head);

    // Print list after deletion
    Node* temp = head;
    while (temp) {
        cout << temp->data << " ";
        temp = temp->next;
    }
    return 0;
}

```

another way to define nodes 
```cpp
Node* head = new Node(10);
head->next = new Node(20);
head->next->next = new Node(30);
```

### DOUBLY LINKED LIST

Insert new node at pth position (DLL)

```cpp
class Solution {
  public:
    Node* insertAtPosition(Node* head, int p, int x) {
        
        Node* nNode = new Node(x);

        // Case 1: Insert at head
        if(p == 1) {
            nNode->next = head;
            if(head != NULL)
                head->prev = nNode;
            return nNode;
        }

        Node* temp = head;

        // Move to (p-1)th node
        for(int i = 1; i < p-1; i++) {
            if(temp == NULL) return head; // invalid p
            temp = temp->next;
        }

        if(temp == NULL) return head; // safety

        // Insert after (p-1)th node
        nNode->next = temp->next;
        nNode->prev = temp;

        if(temp->next != NULL)
            temp->next->prev = nNode;

        temp->next = nNode;

        return head;
    }
};
```

### Delete Last Node of a Doubly Linked List

Some edge cases:

- If the list is empty, return immediately as there is nothing to delete.
- If list has only one node, delete the node and return an empty list.

```cpp
#include <bits/stdc++.h>
using namespace std;

// Node structure for DLL
struct Node {
    int data;
    Node* prev;
    Node* next;
    Node(int val) {
        data = val;
        prev = NULL;
        next = NULL;
    }
};

class Solution {
public:
    // Function to delete tail of DLL
    Node* deleteTail(Node* head) {
        // If list is empty
        if (head == NULL) return NULL;

        // If only one node present
        if (head->next == NULL) {
            delete head;
            return NULL;
        }

        // Traverse to the last node
        Node* temp = head;
        while (temp->next != NULL) {
            temp = temp->next;
        }

        // Update second last node's next to NULL
        temp->prev->next = NULL;

        // Delete tail node
        delete temp;

        // Return head
        return head;
    }
};

int main() {
    // Create a sample DLL: 1 <-> 2 <-> 3
    Node* head = new Node(1);
    head->next = new Node(2);
    head->next->prev = head;
    head->next->next = new Node(3);
    head->next->next->prev = head->next;

    Solution obj;
    head = obj.deleteTail(head);

    // Print list after deletion
    Node* curr = head;
    while (curr != NULL) {
        cout << curr->data << " ";
        curr = curr->next;
    }
    return 0;
}

```

### Reversing a DOUBLY linked list

Naive method : A brute-force approach involves replacing data in a doubly linked list. First, we traverse the list and store node data in a stack. Then, in a second pass, we assign elements from the stack to nodes, ensuring a reverse order replacement since stacks follow the **Last-In-First-Out (LIFO)** principle.

code:
```cpp
#include <bits/stdc++.h>
using namespace std;

// Class representing a Node in a doubly linked list
class Node {
public:
    // Data stored in the node
    int data;

    // Pointer to the next node
    Node* next;

    // Pointer to the previous node
    Node* back;

    // Constructor with data, next, and back references
    Node(int data1, Node* next1, Node* back1) {
        data = data1;
        next = next1;
        back = back1;
    }

    // Constructor with only data, next and back are null
    Node(int data1) {
        data = data1;
        next = nullptr;
        back = nullptr;
    }
};

// Function to convert a vector into a doubly linked list
Node* convertArr2DLL(vector<int> arr) {
    // Create head node using the first array element
    Node* head = new Node(arr[0]);

    // Initialize previous node as head
    Node* prev = head;

    // Iterate through the remaining elements
    for (int i = 1; i < arr.size(); i++) {
        // Create new node with current value and back link to prev
        Node* temp = new Node(arr[i], nullptr, prev);

        // Set the next pointer of previous node to new node
        prev->next = temp;

        // Move prev to the new node
        prev = temp;
    }

    // Return the head of the DLL
    return head;
}

// Function to print elements of a doubly linked list
void print(Node* head) {
    // Traverse till the end of the list
    while (head != nullptr) {
        // Print current node's data
        cout << head->data << " ";

        // Move to the next node
        head = head->next;
    }
}

// Function to reverse a doubly linked list using a stack (brute force)
Node* reverseDLL(Node* head) {
    // If list is empty or has only one node, return as-is
    if (head == nullptr || head->next == nullptr) {
        return head;
    }

    // Stack to store node data
    stack<int> st;

    // Pointer to traverse the list
    Node* temp = head;

    // Push all node values to stack
    while (temp != nullptr) {
        st.push(temp->data);
        temp = temp->next;
    }

    // Reset temp to head for second pass
    temp = head;

    // Replace node values with those from stack
    while (temp != nullptr) {
        temp->data = st.top();
        st.pop();
        temp = temp->next;
    }

    // Return head of reversed list
    return head;
}

// Driver code
int main() {
    // Input array
    vector<int> arr = {12, 5, 8, 7, 4};

    // Convert array to doubly linked list
    Node* head = convertArr2DLL(arr);

    // Print original DLL
    cout << endl << "Doubly Linked List Initially: " << endl;
    print(head);

    // Reverse the DLL
    head = reverseDLL(head);

    // Print reversed DLL
    cout << endl << "Doubly Linked List After Reversing: " << endl;
    print(head);

    return 0;
}

```

optimal approach : swap pointers(next and prev of all the nodes)
like we normally swap variables..

```cpp
#include <bits/stdc++.h>
using namespace std;
// Class representing a Node in a doubly linked list
class Node {
public:
     // To store value of the node
    int data;  
    // Pointer to the next node    
    Node* next;    
    // Pointer to the previous node
    Node* back; 

    // Constructor with data, next, and back references
    Node(int data1, Node* next1, Node* back1) {
        data = data1;
        next = next1;
        back = back1;
    }
    // Constructor with only data (default next and back to null)
    Node(int data1) {
        data = data1;
        next = nullptr;
        back = nullptr;
    }
};

// Function to reverse the doubly linked list in-place
Node* reverseDLL(Node* head) {
    // If list is empty or has one node, nothing to reverse
    if (head == nullptr || head->next == nullptr) return head;

    // Pointer to track the current node
    Node* curr = head;

    // Traverse the DLL
    while (curr != nullptr) {
        // Swap next and back pointers of current node
        Node* temp = curr->next;
        curr->next = curr->back;
        curr->back = temp;

        // Move to the next node in original order
        head = curr;          
        curr = temp;    // curr ko aage le jao      
    }

    // Return new head after full reversal
    return head;
}

```



### middle of a linked list

brute force:  count the length (1 traversal ) and then find the middle index and traverse again upto middle node
tc : O(N + N/2)
optimal approach:
##### Tortoise and Hare Algorithm

```cpp

#include <iostream>
#include <bits/stdc++.h>

using namespace std;

// Function to find the middle
// node of a linked list
Node *findMiddle(Node *head) {
    
     // Initialize the slow pointer to the head.
    Node *slow = head; 
    
     // Initialize the fast pointer to the head.
    Node *fast = head; 

    // Traverse the linked list using the
    // Tortoise and Hare algorithm.
    while (fast != NULL && fast->next != NULL) {
        // Move slow one step.
        slow = slow->next; 
         // Move fast two steps.
        fast = fast->next->next; 
    }
    
    
     // Return the slow pointer,
     // which is now at the middle node.
    return slow; 
}


```

ex: 5 nodes                                                ex 6 nodes
slow: 1->2->3 ( middle)                             slow : 1-2-3-4 (middle)
fast: 1->3->**5**                                              fast: 1->3->5->NULL


# reverse a linked list

```cpp

class Solution {

public:

    ListNode* reverseList(ListNode* head) {

        ListNode* prev = NULL;

        ListNode* curr = head;

        while(curr!= NULL)

        {

            ListNode* front = curr->next;

  

            curr->next = prev;

            prev = curr;

            curr = front;

        }
        return prev;

    }

};
```

#### Remove N-th node from the end of a Linked List

optimal approach : first take fast to N steps ahead from slow and then make them move together till slow reaches the node before the target node..

then do the deletion..

```
dummy -> 1 -> 2 -> 3 -> 4 -> 5
  ↑
 slow, fast initially
```


```cpp
// Class to hold the solution logic
class Solution {
public:
    // Function to print the linked list
    void printLL(Node* head) {
        while (head != NULL) {
            cout << head->data << " ";
            head = head->next;
        }
    }

    // Function to delete the Nth node from the end 
    // using the optimized two-pointer method
    Node* deleteNthNodeFromEnd(Node* head, int N) {
        // Create a dummy node before head to handle edge cases
        Node* dummy = new Node(0, head);

        // Initialize slow and fast pointers at dummy
        Node* slow = dummy;
        Node* fast = dummy;

        // Move fast pointer N+1 steps ahead to create a gap
        for (int i = 0; i <= N; i++) {
            fast = fast->next;
        }

        // Move both pointers until fast reaches the end
        while (fast != NULL) {
            slow = slow->next;
            fast = fast->next;
        }

        // Slow is now at node before target → delete target node
        slow->next = slow->next->next;

        // Return updated head
        return dummy->next;
    }
};

```

Note about Tortoise and Hare algo:

to make the slow stop just before the middle..
initialize fast thoda sa shifted..

fast = head->next;
or fast = head->next->next; dekh lena..

#### Add two numbers represented as Linked Lists

this version does not reverse.. but the logic is same..

```cpp

class Solution {

public:

    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {

        ListNode* dummy = new ListNode; // helps in initiating the loop

        ListNode* temp = dummy;

        int carry  = 0;

        while(l1 || l2 || carry) // imp either is true then loop will run

        {   int sum = 0;

            if(l1)

            {
                sum = sum + l1->val;
                l1 = l1->next;
            }

            if(l2)

            {
                sum = sum + l2->val;

                l2 = l2->next;
            }

            sum = sum + carry;
            carry = sum/10;
            ListNode* tempi = new ListNode(sum % 10);
            
            temp->next = tempi;
            temp = temp->next;

        }

        return dummy->next;

    }

};
```

##### intersection point of LL(Y)

```cpp

class Solution {

public:

    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {

        ListNode* p1 = headA;

        ListNode* p2 = headB;

        while(p1 != p2) // if they start coinciding

        {
            p1 = p1->next;

            p2 = p2->next;

            if(p1 == p2)

            {
                return p1; // takes care of all the cases 
            }

            if(!p1) p1 = headB;

            if(!p2) p2 = headA;
        }
        return p1; // whem the loop condition breaks
    }
};
```

##### merge two sorted linked list ( sorted)

Brute force: store all the values of both the LL in an array and sort it. Then create a new LL,

Optimal : keep comparing the smallest element of each LL together and move one by one.

```cpp


class Solution {

public:

    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {

  
        ListNode* t1 = list1;

        ListNode* t2 = list2;

        ListNode* dummy = new ListNode(-1);

        ListNode* temp = dummy;

        while(t1 && t2)
        {
            if(t1->val > t2->val)
            {
                temp->next = t1;

                temp = temp->next;

                t1 = t1->next;
            }
            else
            {
                temp->next = t2;
                temp = temp->next;
                t2 = t2->next;
            }
        }

        if(t1) temp->next = t1;

        else temp->next = t2;
  
        return dummy->next;

    }

};
```

##### deleting a key from DLL

```cpp
Node* deleteAllOccurOfX(Node* head, int x) {

        Node* temp = head;
        
        while(temp!= NULL)
        {
            if(head->data == x)
            {
                head = head->next;
                temp = head;
            }
            else if(temp->data == x)
            {
                Node* nextNode = temp->next;
                Node* prevNode = temp->prev;
                
                if(nextNode) nextNode->prev = prevNode;
                if(prevNode) prevNode->next = nextNode;
                
                free(temp); //Yeh **current node ki memory delete karta hai**
				
	            temp = nextNode;  // variable khud delete nahi hota 
            }
            else
            {
                temp = temp->next;
            }
        }
        return head;
    }
```
