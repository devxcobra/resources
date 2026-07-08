

Search in BST
```cpp
TreeNode* searchBST(TreeNode* root, int val) {
        while(root!= NULL && root->val != val)
        {
            if(root->val > val) root = root->left;
            else root = root->right;
        }
        return root;
    }
```
ceil in BST
```cpp
    int findCeil(Node* root, int x) {
        
        int cl = -1;
        while(root)
        {   
            if(root->data == x)
            {
                cl = root->data;
                return cl;
            }
            if(root->data > x)
            {   
                cl = root->data;
                root = root->left;
            }
            else
            {
                root = root->right;
            }
        }
        return cl;
    }
};
```
insert Node in BST
```cpp
TreeNode* insertIntoBST(TreeNode* root, int val) {

        if(root == NULL) return new TreeNode(val);
        TreeNode* cur = root;
        while(true)
        {
            if(cur->val <= val)
            {
                if(cur->right) cur = cur->right;
                else
                {
                    cur->right = new TreeNode(val);
                    break;
                }
            }
            else{
                if(cur->left) cur = cur->left;
                else
                {
                    cur->left = new TreeNode(val);
                    break;

                }
            }
        }
        return root;
    }
```
Delete Node
```cpp
    TreeNode* deleteNode(TreeNode* root, int key) {

        if(root == NULL){
            return NULL;
        }
        if(root->val == key){
            return helper(root);
        }
        TreeNode* dummy = root;
        while(root != NULL){
            if(root->val > key)
            {
                if(root->left != NULL && root->left->val == key)
                {
                    root->left = helper(root->left);
                    break;
                }
                else
                {
                    root = root->left;
                }
            }
            else
            {
                if(root->right != NULL && root->right->val == key){
                   root->right = helper(root->right);
                    break;
                } else{
                    root = root->right;
                }
            }
        }
        return dummy;
    }


    TreeNode* helper(TreeNode* root)
    {
        if(root->left == NULL)
        {
            return root->right;
        }
        else if(root->right == NULL)
        {
            return root->left;
        }

        TreeNode* rightchild = root->right;
        TreeNode* lastright = findlastright(root->left);
        lastright->right = rightchild;
        return root->left;
    }
  

    TreeNode* findlastright(TreeNode* root)
    {
        if(root->right == NULL)
        {
            return root;
        }
        return findlastright(root->right);
    }

};
```

Kth smallest element in a BST: **inorder in BST is always sorted**
Kth largest can be found by doing **reverse inorder (right root left)** or n-k+1th smallest element.
```cpp
class Solution {
public:

    void inorder(TreeNode* root, int k , int &count, TreeNode* &ans)
    {
        if(root == NULL) return;
        inorder(root->left, k , count, ans);
        count++;
        if(count == k) ans = root;
        inorder(root->right, k ,count , ans);
        return;
    }


    int kthSmallest(TreeNode* root, int k) {
        TreeNode* ans = root;
        int count = 0;

        inorder(root, k , count, ans);
        return ans->val;

    }
};
```

check if BST 
```cpp

    bool valid(TreeNode* root, long long mini , long long maxi)
    {
        if(root == NULL) return true;
        if(root->val >= maxi || root->val <= mini) return false;

        return valid(root->left, mini, root->val) && valid(root->right, root->val, maxi);

    }

    bool isValidBST(TreeNode* root) {
        return valid(root, LLONG_MIN, LLONG_MAX);
    }
};
```
Construct a BST from a preorder traversal
```cpp
TreeNode* bstFromPreorder(vector<int>& preorder) {
         int i  = 0;
         return build(preorder, i , INT_MAX);
    }

    TreeNode* build(vector<int> &A, int &i, int bound)
    {
        if(i == A.size() || A[i] > bound) return NULL;
        TreeNode* root = new TreeNode(A[i]);
        i++;
        root->left  = build(A, i, root->val);
        root->right = build(A, i, bound);
        return root;
    }
```
Predecessor and Successor 
```cpp
void findcl(Node* root, int key, Node* &cl) {
        if(root == NULL) return;
        
        if(root->data > key) {
            // Node is strictly greater, it is a potential successor.
            // Save it, and try to find a tighter (smaller) one on the left.
            cl = root;
            findcl(root->left, key, cl);
        } else {
            // If root->data <= key, the successor MUST be on the right.
            findcl(root->right, key, cl);
        }
    }
    // Find Predecessor (Floor)
    void findflr(Node* root, int key, Node* &flr) {
        if(root == NULL) return;
        
        if(root->data < key) {
            // Node is strictly smaller, it is a potential predecessor.
            // Save it, and try to find a tighter (larger) one on the right.
            flr = root;
            findflr(root->right, key, flr);
        } else {
            // If root->data >= key, the predecessor MUST be on the left.
            findflr(root->left, key, flr);
        }
    }
    vector<Node*> findPreSuc(Node* root, int key) {
        
        Node* cl = NULL;
        Node* flr = NULL;
        findcl(root, key, cl);
        findflr(root, key, flr);
        return {flr, cl};
        
    }
```




Recover BST

```cpp
class Solution {

    private:
    TreeNode* first;
    TreeNode* prev;
    TreeNode* middle;
    TreeNode* last;

    private:
    void inorder(TreeNode* root)
    {

        if(root == NULL) return;

        inorder(root->left);
        if(prev != NULL && (root->val < prev->val))
        {

            // if this is the first violation mark these two nodes as

            // first and middle
            if(first == NULL)
            {
                first = prev;
                middle = root;
            }

            else
                last = root;
        }

        prev = root;
        inorder(root->right);
    }

public:
    void recoverTree(TreeNode* root) {
        first = middle = last = NULL;
        prev = new TreeNode(INT_MIN);
        inorder(root);
        if(first && last) swap(first->val, last->val);
        else if(first && middle) swap(first->val, middle->val);
    }
};
```

Two sum - space complexity: 2 * O(H)

```cpp

class BSTIterator {

    stack<TreeNode*> myStack;
    // reverse->true->before

    bool reverse = true;

    public:
    BSTIterator(TreeNode* root, bool isReverse)
    {
        reverse = isReverse;
        pushAll(root);
    }

    bool hasNext()
    {
        return !myStack.empty();
    }

    int next(){

        TreeNode* tmpNode = myStack.top();
        myStack.pop();
        if(!reverse) pushAll(tmpNode->right);
        else pushAll(tmpNode->left);
        return tmpNode->val;
    }

  

    private:
    void pushAll(TreeNode* node)
    {
        for(;node!= NULL;)
        {
            myStack.push(node);
            if(reverse == true)
            {
                node = node->right;
            }

            else
            {
                node = node->left;
            }

        }

    }

};

class Solution {
public:
    bool findTarget(TreeNode* root, int k) {

        if(!root) return false;

        BSTIterator l(root, false);
        BSTIterator r(root, true);

        int i = l.next();
        int j = r.next();

        while(i<j)
        {
            if(i + j == k) return true;
            else if(i + j < k) i = l.next();
            else j = r.next();
        }
        return false;

    }

};
```

