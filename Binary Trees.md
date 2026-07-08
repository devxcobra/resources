#### Level Order Traversal
Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

```cpp
class Solution {
public:
    // Function to perform level-order traversal of a binary tree
    vector<vector<int>> levelOrder(TreeNode* root) {
        // Create a 2D vector to store levels
        vector<vector<int>> ans; 
        if (root == nullptr) {
            // If the tree is empty, return an empty vector
            return ans; 
        }
        
        // Create a queue to store nodes for level-order traversal
        queue<TreeNode*> q; 
        // Push the root node to the queue
        q.push(root); 

        while (!q.empty()) {
            // Get the size of the current level
            int size = q.size(); 
            // Create a vector to store nodes at the current level
            vector<int> level; 

            for (int i = 0; i < size; i++) {
                // Get the front node in the queue
                TreeNode* node = q.front(); 
                // Remove the front node from the queue
                q.pop(); 
                // Store the node value in the current level vector
                level.push_back(node->data); 

                // Enqueue the child nodes if they exist
                if (node->left != nullptr) {
                    q.push(node->left);
                }
                if (node->right != nullptr) {
                    q.push(node->right);
                }
            }
            // Store the current level in the answer vector
            ans.push_back(level); 
        }
        // Return the level-order traversal of the tree
        return ans; 
    }
};

```


#### Vertical Order Traversal 

- **1. Coordinate System:** Treat the tree like a grid. The root is at `(0,0)`. Moving left decreases the vertical index `(x - 1)`, moving right increases it `(x + 1)`. Moving down increases the level `(y + 1)`.
    
- **2. Level-by-Level (BFS):** Use a Queue to perform Breadth-First Search. This guarantees that nodes closer to the top are processed before nodes further down.
    
- **3. Map Magic:** Use a nested data structure: `map<vertical, map<level, multiset<value>>>`. Maps automatically sort their keys, so verticals are ordered left-to-right, levels top-to-bottom, and the `multiset` sorts nodes that occupy the _exact same coordinate_.
    
- **4. Flattening:** Iterate through the naturally sorted map and dump the elements of each vertical column into a 2D array to return as the final answer.

```cpp
// This class contains the solution logic
class Solution {
public:
    // Function to perform vertical order traversal
    vector<vector<int>> findVertical(Node* root) {
        // Data structure: map<vertical, map<level, multiset<values>>>
        // Maps automatically sort by keys. Multiset automatically sorts values if they overlap.
        map<int, map<int, multiset<int>>> nodes;
        
        // Queue for BFS: stores {Node, {vertical (x), level (y)}}
        queue<pair<Node*, pair<int, int>>> todo;

        // Start BFS at the root with coordinates (0, 0)
        todo.push({root, {0, 0}});

        // Perform BFS traversal
        while (!todo.empty()) {
            auto p = todo.front();
            todo.pop();

            Node* temp = p.first;
            int x = p.second.first;  // Vertical index
            int y = p.second.second; // Level index

            // Insert the node's value into the correct coordinate
            nodes[x][y].insert(temp->data);

            // Left child goes down one level (y+1) and left one vertical (x-1)
            if (temp->left) {
                todo.push({temp->left, {x - 1, y + 1}});
            }

            // Right child goes down one level (y+1) and right one vertical (x+1)
            if (temp->right) {
                todo.push({temp->right, {x + 1, y + 1}});
            }
        }

        // Prepare the 2D array for the final answer
        vector<vector<int>> ans;

        // Iterate through the verticals (automatically sorted from negative to positive)
        for (auto p : nodes) {
            vector<int> col;
            
            // Iterate through the levels in the current vertical
            for (auto q : p.second) {
                // Append all nodes at this exact (x, y) coordinate to the column
                col.insert(col.end(), q.second.begin(), q.second.end());
            }
            // Push the completed vertical column into the final result
            ans.push_back(col);
        }

        return ans;
    }
};
```



#### Print Root to Node Path in a Binary Tree

if it's not the target, recursively search the **left** and **right** subtrees. If either returns `true`, pass that `true` upward to preserve the successful path

If the target is not found in either subtree, this branch is a dead end. **Pop** the current node out of the array (backtracking) and return `false` to move back up and try a different route.


```cpp
class Solution {
public:
    // Helper function that uses DFS and Backtracking to build the path
    bool getPath(TreeNode* root, vector<int>& arr, int x) {
        // Base case: If we hit a dead end (null node), return false
        if (!root) {
            return false;
        }

        // 1. ADD: Assume this node is part of the correct path
        arr.push_back(root->val);

        // 2. CHECK: Did we find the target? If yes, stop searching and return true
        if (root->val == x) {
            return true;
        }

        // 3. RECURSE: Check if the target is hiding in the left or right subtree
        // If either returns true, the target was found below, so we keep this node in 'arr'
        if (getPath(root->left, arr, x) || getPath(root->right, arr, x)) {
            return true;
        }

        // 4. BACKTRACK: Target isn't in this branch. 
        // Remove this node from our path array and return false to the parent.
        arr.pop_back();
        return false;
    }

    // Main function to get the path
    vector<int> solve(TreeNode* A, int B) {
        vector<int> arr;

        // If the tree is empty, just return the empty array
        if (A == NULL) {
            return arr;
        }

        // Populate 'arr' using our recursive helper function
        getPath(A, arr, B);

        // Return the final path (will be empty if node B wasn't in the tree)
        return arr;
    }
};
```






#### Least Common Ancestor

If the current node is `NULL`, or if it matches either target node (`p` or `q`), immediately return the current node.

Recursively traverse down the tree, searching for `p` and `q` in both the **left** and **right** subtrees.
If _both_ the left and right recursive calls return a non-null value, it means `p` and `q` split at this exact point. The current node is your Lowest Common Ancestor.

If only _one_ side returns a non-null value, pass that non-null value up to the parent. This handles cases where we are still bubbling the result up the tree, or where one target node is a direct ancestor of the other.

```cpp

class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        // Base case
        if (root == NULL || root == p || root == q) {
            return root;
        }
        
        // Search in left and right subtrees
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        
        // Result
        if (left == NULL) {
            return right;
        } else if (right == NULL) {
            return left;
        } else { // Both left and right are not null, we found our result
            return root;
        }
    }
};
```

Construct Binary Tree from preorder and inorder traversal

```
instart                                                inend
          ↓                                                     ↓
INORDER: [ ... Left Subtree Nodes ... | ROOT | ... Right Subtree Nodes ... ]
                                        ↑
                                      inroot 
          
          |------- numsleft ---------|
          (Size of the left subtree)
          
prestart                                                 preend
          ↓                                                      ↓
PREORDER:[ ROOT | ... Left Subtree Nodes ... | ... Right Subtree Nodes ... ]
          
                  |------- numsleft --------|
```


```cpp
class Solution {
public:
    TreeNode* build(vector<int>& preorder, int prestart, int preend, vector<int>& inorder, int instart, int inend, map<int, int>& inmap) {
        // Base case: If the start index exceeds the end index, the subtree is empty
        if (prestart > preend || instart > inend) {
            return NULL;
        }
        
        // The first element in the current preorder range is ALWAYS the root
        TreeNode* root = new TreeNode(preorder[prestart]);
        
        // Find where this root is located in the inorder array
        int inroot = inmap[root->val];
        
        // Calculate how many nodes belong to the left subtree
        int numsleft = inroot - instart;
        
        // Recursively build the left subtree
        root->left = build(preorder, prestart + 1, prestart + numsleft, inorder, instart, inroot - 1, inmap);
        
        // Recursively build the right subtree
        root->right = build(preorder, prestart + numsleft + 1, preend, inorder, inroot + 1, inend, inmap);
        
        return root;
    }

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        map<int, int> inmap;
        
        // Map the values to their indices in the inorder array for O(1) lookup
        for (int i = 0; i < inorder.size(); i++) {
            inmap[inorder[i]] = i;
        }
        
        // Call the recursive helper function covering the entire array ranges
        TreeNode* root = build(preorder, 0, preorder.size() - 1, inorder, 0, inorder.size() - 1, inmap);
        
        return root;
    }
};
```

LCA 
```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
         if(root == NULL) return NULL;
         int curr = root->val;
         if(p->val < curr && q->val < curr)
         {
            return lowestCommonAncestor(root->left, p, q);
         }
         if(p->val > curr && q->val > curr)
         {
            return lowestCommonAncestor(root->right, p , q);
         }
         return root;
    }
```
