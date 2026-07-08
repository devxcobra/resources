
note: whenever there are multiple components in graph, run for loop for all the nodes and check if visited.. in the main function.

BFS
```cpp
class Solution {
  public:
    vector<int> bfs(vector<vector<int>> &adj) {
      
      int n = adj.size();
      vector<int> vis(n , 0);
      vis[0] = 1;
      queue<int> q;
      q.push(0);
      
      vector<int> bfs;
      while(!q.empty())
      {
          int node = q.front();
          q.pop();
          bfs.push_back(node);
          
          for(auto it : adj[node])
          {     
              if(!vis[it])
              {
              q.push(it);
              vis[it] = 1;
              }
          }
          
      }
      
      return bfs;
        
    }
};
```

DFS
```cpp
class Solution {
  public:
    
    void dfs(int node, vector<int> &vis, vector<int> &list, vector<vector<int>> &adj)
    {
        vis[node] = 1;
        list.push_back(node);
        
        for(auto it: adj[node])
        {
            if(!vis[it]) dfs(it, vis, list, adj);
        }
    }
    
    vector<int> dfs(vector<vector<int>>& adj) {
        
        int n = adj.size();
        
        vector<int> vis(n, 0);
        
        vector<int> list;
        
        vis[0] = 1;
        
        dfs(0, vis, list, adj);
        
        return list;
         
        
    }
};
```

Number of provinces

 Given an undirected graph with V vertices. Two vertices u and v belong to a single province if there is a path from u to v or v to u. Find the number of provinces. The graph is given as an n x n matrix adj where adj[i][j] = 1 if the ith city and the jth city are directly connected, and adj[i][j] = 0 otherwise.
 
approach:
We can do this using either DFS or BFS. Each time we start a new search from an unvisited vertex, we discover a new component and mark all vertices in that component as visited. This ensures that we don’t count the same component more than once.

```cpp
class Solution {

public:
    void dfs(int i, vector<vector<int>> &adj, vector<int> &vis)
    {
        vis[i] = 1;

        for(auto it : adj[i])

        {   if(!vis[it]){

            dfs(it, adj, vis );

        }

        }

    }

    int findCircleNum(vector<vector<int>>& matrix) {

        int n = matrix[0].size();
        vector<vector<int>> adj(n);
        for(int i = 0; i < n; i++)
        {

            for(int j =0; j < n ; j++)

            {   if(matrix[i][j] == 1 && i != j)

                {
                adj[i].push_back(j);
                adj[j].push_back(i);
                }
            }
        }

        vector<int> vis(n, 0);
        int count = 0;

        for(int i = 0; i < n; i++)
        {
            if(vis[i] == 0)
            {
                count++;
                dfs(i, adj , vis);
            }
        }

        return count;

    }
};
```

#### rotting oranges

Given an n x m grid, where each cell has the following values :  
2 - represents a rotten orange , 1 - represents a Fresh orange , 0 - represents an Empty Cell.  
  
Every minute, if a fresh orange is adjacent to a rotten orange in 4-direction ( upward, downwards, right, and left ) it becomes rotten.  
Return the minimum number of minutes required such that none of the cells has a Fresh Orange. If it's not possible, return -1

```cpp
class Solution {
public:
    int orangesRotting(vector<vector<int>>& grid) {

        int n = grid.size();
        int m = grid[0].size();
        
        // Queue stores {{row, column}, time_taken}
        // We need 'time_taken' to track how many minutes have passed for each orange
        queue<pair<pair<int,int>, int>> q;
        
        // Visited array to keep track of which oranges have already rotted.
        // Initialized to 0 (unvisited)
        vector<vector<int>> vis(n , vector<int>(m , 0));

        // STEP 1: Initialization
        // Scan the grid to find all initially rotten oranges.
        // We push ALL of them into the queue at time = 0 because they rot simultaneously.
        for(int i = 0; i < n; i++)
        {
            for(int j = 0; j < m; j++)
            {
                if(grid[i][j] == 2)
                {
                    vis[i][j] = 2; // Mark as visited/rotten
                    q.push({{i,j}, 0});
                }
            }
        }

        int tm = 0; // Variable to store the maximum time needed

        
        vector<int> drow = {-1, 0, 1, 0};
        vector<int> dcol = {0, 1, 0, -1};

        // STEP 2: Breadth-First Search (BFS) Traversal
        while(!q.empty())
        {
            int r = q.front().first.first;
            int c = q.front().first.second;
            int t = q.front().second;
            
            // Keep updating the max time as we process the queue
            tm = max(tm , t);
            q.pop();

            // Check all 4 adjacent neighbors
            for(int i = 0; i < 4; i++)
            {   
                int nrow = r + drow[i];
                int ncol = c + dcol[i];
                
                // VALIDATION CHECK:
                // 1. nrow & ncol are within grid boundaries
                // 2. The neighbor has NOT been visited yet (vis == 0)
                // 3. The neighbor is a FRESH orange (grid == 1)
if(nrow >= 0 && ncol >= 0 && nrow < n && ncol < m && vis[nrow][ncol] == 0 && grid[nrow][ncol] == 1)
                {
                    // Mark as visited so we don't process it again
                    vis[nrow][ncol] = 2;
                    // Push to queue, incrementing the time by 1 minute
                    q.push({{nrow, ncol} , t+1});
                }
            }
        }

        // STEP 3: Final Verification
        // If there are any fresh oranges left that were completely isolated (unreachable),
        // our BFS wouldn't have visited them.
        for(int i = 0; i < n; i++)
        {
            for(int j = 0; j < m ;j++)
            {
                // If it's a fresh orange in the original grid but wasn't visited
                if(vis[i][j] != 2 && grid[i][j] == 1) 
                    return -1; // Impossible to rot all oranges
            }
        }

        // If we successfully rotted everything, return the max time recorded
        return tm;
    }
};
```

### Undirected Graph Cycle Detection (BFS)

```cpp
class Solution {
  public:
    bool bfs(int start, vector<int> &vis, vector<vector<int>> &adj)
    {
        queue<pair<int,int>> q;
        q.push({start , -1});
        vis[start] = 1;
       
       while(!q.empty())
       {
           int node = q.front().first;
           int parent = q.front().second;
           q.pop();
           
           for(auto adjnode : adj[node])
           {
               if(!vis[adjnode])
               {
                   vis[adjnode] = 1;
                   q.push({adjnode, node});
               }
               else if(parent != adjnode)
               {
                   return true;
               }
           }
           
       }
       
       return false;
        
    }
    bool isCycle(int V, vector<vector<int>>& edges) {
       
       vector<vector<int>> adj(V);
       for (auto it : edges)
       {
           int u = it[0];
           int v = it[1];
           
           adj[u].push_back(v);
           adj[v].push_back(u);
           
       }
       
       vector<int> vis(V, 0);
       
       for(int i = 0; i < V; i++) {
        if(!vis[i]) {
        if(bfs(i, vis, adj)) {
            return true; 
        }
    }
}  
    }
};
```

**detect cycle in undirected graph (DFS)**

```cpp
class Solution {
public:
    // DFS function to detect cycle
    bool dfs(int node, int parent, vector<int> adj[], vector<int>& visited) {
        // Mark current node visited
        visited[node] = 1;

        // Traverse neighbors
        for (int neighbor : adj[node]) {

            // If neighbor not visited, recurse
            if (!visited[neighbor]) {
                if (dfs(neighbor, node, adj, visited)) return true;
            }

            // If neighbor visited and not parent, cycle exists
            else if (neighbor != parent) {
                return true;
            }
        }

        // No cycle found from this path
        return false;
    }

    // Function to check cycle in graph
    bool isCycle(int V, vector<int> adj[]) {
        vector<int> visited(V, 0);

        // Check all components
        for (int i = 0; i < V; i++) {
            if (!visited[i]) {
                if (dfs(i, -1, adj, visited)) return true;
            }
        }
        return false;
    }
};
```

**Surrounded Regions** - pattern dfs

```cpp
class Solution {
public:

    void dfs(int row, int col , vector<vector<int>> & vis, vector<vector<char>> &board)

    {

        vis[row][col] = 1;

        int n = board.size();
        int m = board[0].size();

        // check for bottom top right left
        vector<int> drow = {-1, 0, 1, 0 };
        vector<int> dcol = {0, +1, 0, -1};

        for(int i = 0 ; i< 4; i++)
        {

            int nrow = row  + drow[i];
            int ncol = col + dcol[i];

            if(nrow >= 0 && nrow < n && ncol >= 0 && ncol < m && !vis[nrow][ncol] && board[nrow][ncol] == 'O')
            {
                dfs(nrow, ncol,vis, board );
            }
        }
    }

    void solve(vector<vector<char>>& board) {

        int n = board.size();
        int m = board[0].size();

        vector<vector<int>> vis(n, vector<int> (m, 0));

        for(int i = 0; i < m; i++)
        {
            if(!vis[0][i] && board[0][i] == 'O')
            {
                dfs(0, i, vis, board);
            }


            if(!vis[n-1][i] && board[n-1][i] == 'O')

            {
                dfs(n-1, i, vis, board);
            }

        }

        for(int i = 0; i< n; i++)
        {
            if(!vis[i][0] && board[i][0] == 'O')

            {
               dfs(i, 0, vis, board);
            }

            if(!vis[i][m-1] && board[i][m-1] == 'O')

            {
                dfs(i, m-1, vis, board);
            }
        }

        for(int i = 0; i< n; i++)
        {

            for(int j = 0; j < m; j++)
            {
                if(!vis[i][j] && board[i][j] == 'O')
                {
                    board[i][j] = 'X';
                }
            }
        }
    }

};
```

**Word Ladder**

```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        // Queue stores {current_word, steps_taken} for BFS
        queue<pair<string, int>> q;
        q.push({beginWord, 1});
        
        // Use a set for O(1) lookups and to keep track of unvisited words
        unordered_set<string> st(wordList.begin(), wordList.end());
        st.erase(beginWord); // Remove start word so we don't revisit it

        // Standard BFS loop
        while(!q.empty())
        {
            string word = q.front().first;
            int steps = q.front().second;
            q.pop();
            
            // If we found the target word, return the current step count
            if(word == endWord) return steps;
            
            // Try mutating the current word one character at a time
            for(int i = 0; i < word.size(); i++)
            {
                char original = word[i]; // Save the original char
                
                // Replace current character with every letter from 'a' to 'z'
                for(char ch = 'a'; ch <= 'z'; ch++)
                {
                    word[i] = ch;
                    
                    // If the mutated word exists in our unvisited set
                    if(st.find(word) != st.end())
                    {
                    st.erase(word); // Mark as visited by removing it, very imp
                    q.push({word, steps + 1}); // Add to queue for next BFS level
                    }
                }
// Backtrack: restore the original character before moving to the next index
                word[i] = original;
            }
        }
// If the queue empties and we never reached endWord, no valid sequence exists
        return 0;
    }
};
```

Bipartite Graph (BFS)

```cpp
class Solution {
public:

    bool check(int start , vector<int> &color , vector<vector<int>> &adj)
    {  

        int n = adj.size();
        color[start] = 0;

        queue<int> q;
        q.push(start);

        while(!q.empty())
        {
            int node = q.front();
            q.pop();

            for(auto it : adj[node])
            {
                if(color[it] == -1)
                {
                    color[it] = !color[node];
                    q.push(it);
                }

                else if(color[it] == color[node])
                {
                    return false;
                }
            }
        }

        return true;
    }

  

    bool isBipartite(vector<vector<int>>& graph) {

        int n = graph.size();
        vector<int> color(n , -1);

        for(int i = 0; i < n; i++)

        {
            if(color[i] == -1)
            {
                if(check(i, color, graph))
                {
                   continue;
                }
                else
                {
                    return false;
                }
            }
        }
        return true;
    }
};
```

##### Bipartite DFS

```cpp
class Solution {

public:

    bool check(int node ,int col,  vector<int> &color , vector<vector<int>> &adj)
    {  

        int n = adj.size();
        color[node] = col;

        for(auto it : adj[node])
            {
                if(color[it] == -1)
                {
                    if(check(it, !col, color, adj)) continue;
                    else return false;
                }

                else if(color[it] == color[node])

                {
                    return false;
                }
            }


        return true;

    }


    bool isBipartite(vector<vector<int>>& graph) {

        int n = graph.size();
        vector<int> color(n , -1);

        for(int i = 0; i < n; i++)
        {

            if(color[i] == -1)
            {

                if(check(i, 0, color, graph))
                {
                    continue;
                }
                else
                {
                    return false;
                }

            }

        }

        return true;

    }

};
```

#### Cycle detection in Directed Graph (DFS)

Note for directed graphs : on the same path the node has to be visited again to be a cycle

```cpp
class Solution {
public:
    bool dfsCheck(int node, vector<vector<int>> &adj, vector<int> &vis, vector<int> &pathVis) {
        vis[node] = 1;
        pathVis[node] = 1;

        // traverse for adjacent nodes
        for (auto it : adj[node]) {
            // when the node is not visited
            if (!vis[it]) {
                if (dfsCheck(it, adj, vis, pathVis) == true)
                    return true;
            }
            // if the node has been previously visited
            // but it has to be visited on the same path
            else if (pathVis[it]) {
                return true;
            }
        }
        pathVis[node] = 0;
        return false;
    }

    bool isCyclic(int V, vector<vector<int>> &edges) {
        // 1. Build the adjacency list
        vector<vector<int>> adj(V);
        for(int i = 0; i < edges.size(); i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            adj[u].push_back(v); // Add the directed edge
        }
        
        // 2. Initialize visited and path-visited arrays
        vector<int> vis(V, 0);
        vector<int> pathVis(V, 0);
        
        // 3. Call DFS for all components
        for(int i = 0; i < V; i++) {
            if(!vis[i]) { // If the node hasn't been visited yet
                if(dfsCheck(i, adj, vis, pathVis) == true) {
                    return true; // Cycle found
                }
            }
        }
        
        return false; // No cycle found in any component
    }
};
```

#### Topo Sort - whenever there is mention of something before something
#### using dfs : 
```cpp
#include <bits/stdc++.h>
using namespace std;

// Class containing the solution logic
class Solution {
public:
    // Function to perform DFS
    void dfs(int node, vector<int> adj[], vector<int>& vis, stack<int>& st) {
        // Mark the current node as visited
        vis[node] = 1;
        // Explore all neighbors of this node
        for (auto it : adj[node]) {
            // If the neighbor is not visited, recursively perform DFS
            if (!vis[it]) {
                dfs(it, adj, vis, st);
            }
        }

        // After visiting all neighbors, push this node into the stack
        st.push(node);
    }
    // Function to perform Topological Sort
    vector<int> topoSort(int V, vector<int> adj[]) {
        // Create a visited array to mark visited vertices
        vector<int> vis(V, 0);
        // Stack to store vertices in finishing order
        stack<int> st;
        // Perform DFS from each unvisited vertex
        for (int i = 0; i < V; i++) {
            if (!vis[i]) {
                dfs(i, adj, vis, st);
            }
        }
        // Prepare the result array
        vector<int> ans;
        while (!st.empty()) {
            ans.push_back(st.top());
            st.pop();
        }

        // Return the topological ordering
        return ans;
    }
};
```
tc - O(V + E)
sc - O(V + E) v+e - adj list, v - vis arr , v - recursion stack in worst case

**using BFS**
```cpp
#include <bits/stdc++.h>
using namespace std;
// Creating a class named Solution
class Solution {
public:
    // Function to perform BFS-based Topological Sort
    vector<int> topologicalSort(int V, vector<int> adj[]) {
        // Create a vector to store the in-degree of each vertex
        vector<int> indegree(V, 0);
        
        // Loop through all vertices to calculate in-degree
        for (int i = 0; i < V; i++) {
            for (auto it : adj[i]) {
                indegree[it]++;
            }
        }
        // Create a queue to store vertices with in-degree zero
        queue<int> q;
        
        // Loop through all vertices
        for (int i = 0; i < V; i++) {
            // If in-degree is zero, add to queue
            if (indegree[i] == 0) {
                q.push(i);
            }
        }
       
        vector<int> topo;
        
        // Process until queue is empty
        while (!q.empty()) {
            // Get the front vertex from the queue
            int node = q.front();
            q.pop();
            
            // Add it to the topological order
            topo.push_back(node);
            
            // Reduce in-degree of its adjacent vertices
            for (auto it : adj[node]) {
                indegree[it]--;
                // If in-degree becomes zero, push it into queue
                if (indegree[it] == 0) {
                    q.push(it);
                }
            }
        }
        // Return the topological ordering
        return topo;
    }
};
```

##### alien dictionary

```cpp
#include <bits/stdc++.h>
using namespace std;

// Class to represent the solution
class Solution {
private:
    // Function to perform Topological Sort using Kahn's Algorithm (BFS)
    vector<int> topoSort(int V, vector<int> adj[]) {

        return topo;
    }

public:
    // Function to find the order of characters in the alien dictionary
    string findOrder(string dict[], int N, int K) {
        // Graph represented as adjacency list
        vector<int> adj[K];

        // Build graph by comparing adjacent words in dictionary
        for (int i = 0; i < N - 1; i++) {
            string s1 = dict[i];
            string s2 = dict[i + 1];
            int len = min(s1.size(), s2.size());

            // Find the first different character and create edge
            for (int ptr = 0; ptr < len; ptr++) {
                if (s1[ptr] != s2[ptr]) {
                    adj[s1[ptr] - 'a'].push_back(s2[ptr] - 'a');
                    break; // only the first mismatch matters
                }
            }
        }

        // Perform topological sort on the graph
        vector<int> topo = topoSort(K, adj);

        // Convert numeric values back to characters
        string ans = "";
        for (auto node : topo) {
            ans += char(node + 'a');
        }

        return ans;
    }
};
```

**shortest path in undirected graph**
```cpp
class Solution {
  public:
    vector<int> shortestPath(int n, int m, vector<vector<int>>& edges) {
        
        vector<vector<pair<int,int>>> adj(n+1);
        
        for(auto it : edges)
        {
            adj[it[0]].push_back({it[1], it[2]});
            adj[it[1]].push_back({it[0], it[2]});
        }
        
priority_queue<pair<int, int>, vector<pair<int,int>> , greater<pair<int,int>>> pq;
        
        // Note: 1e9 is generally okay, but 1e8 is safer to prevent overflow 
        // when adding edge weights, though 1e9 usually passes GFG constraints.
        vector<int> dist(n+1, 1e9), parent(n+1);
        for(int i = 1; i <= n; i++) parent[i] = i;
        
        dist[1] = 0;
        
        pq.push({0, 1}); 
        
        while(!pq.empty())
        {
            auto current = pq.top();
            int dis = current.first;
            int node = current.second;
            pq.pop();
            
            for(auto neighbor : adj[node])
            {
                int adjNode = neighbor.first;
                int edW = neighbor.second;
                
                if(dis + edW < dist[adjNode]){
                    dist[adjNode] = dis + edW;
                    pq.push({dist[adjNode] , adjNode});
                    parent[adjNode] = node;
                }
            }
        }
        
        // If we can't reach the destination
        if(dist[n] == 1e9) return {-1};
        
        vector<int> path;
        int currNode = n; 
        
        while(parent[currNode] != currNode) 
        {
            path.push_back(currNode);
            currNode = parent[currNode];
        }
        
        path.push_back(1); // Push the starting node
        
        // Reverse the path so it goes from 1 to n
        reverse(path.begin(), path.end());
        
        // THE FIX: Insert the total shortest distance at the very beginning
        path.insert(path.begin(), dist[n]);
        
        return path;
    }
};
```

**Shortest Path in Directed Acyclic Graph** - TC: O(N+M)
```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
  private:
    // Helper function to perform DFS-based Topological Sort
    void topoSort(int node, vector<vector<pair<int, int>>> &adj, vector<int> &vis, stack<int> &st) {

    st.push(node);
    }
    
  public:
    // Function to find the shortest paths from node 0 to all other nodes
    vector<int> shortestPath(int N, int M, vector<vector<int>> &edges) {

      // Create adjacency list for the graph
      vector<vector<pair<int, int>>> adj(N);
      
      // Fill the adjacency list with edges
      for (int i = 0; i < M; i++) {
        int u = edges[i][0];
        int v = edges[i][1];
        int wt = edges[i][2];

        // Store (target node, weight)
        adj[u].push_back({v, wt});
      }

      // Initialize visited array to keep track of visited nodes
      vector<int> vis(N, 0);

      // Stack to store the topological order
      stack<int> st;

      // Call topoSort for all unvisited nodes
      for (int i = 0; i < N; i++) {
        if (!vis[i]) {
          topoSort(i, adj, vis, st);
        }
      }

      // Initialize the distance vector with infinity
      vector<int> dist(N);
      for (int i = 0; i < N; i++) {
        dist[i] = 1e9;
      }

      // Distance to the source node (0) is 0
      dist[0] = 0;

      // Process all nodes in topological order
      while (!st.empty()) {
        int node = st.top();
        st.pop();

        // Relax all outgoing edges from the current node
        for (auto it : adj[node]) {
          int v = it.first;
          int wt = it.second;

          // Update distance if a shorter path is found
          if (dist[node] + wt < dist[v]) {
            dist[v] = wt + dist[node];
          }
        }
      }

      // Replace all unreachable nodes' distances with -1
      for (int i = 0; i < N; i++) {
        if (dist[i] == 1e9) {
          dist[i] = -1;
        }
      }
      // Return the final distance array
      return dist;
    }
};
```

- Because every move costs `1`, **BFS** uses a standard `queue` (First-In, First-Out). It naturally expands outward in uniform layers: it checks all paths of length 1, then all paths of length 2, and so on. The very first time it hits the target cell, it is mathematically guaranteed to be the shortest path. Pushing and popping from a standard queue takes **O(1)** time.
    
- **Dijkstra** uses a `priority_queue` (a Min-Heap) to constantly sort and pull the node with the lowest current total cost. Sorting that heap takes **O(log V)** time per operation.

**`.top()` vs `.front()`:** A standard queue uses `.front()`, but a priority queue uses `.top()`

priority queue: 
- `push(value)`: Inserts an element into the queue and automatically sorts it into its proper priority position. _(Time: O(log N))_
    
- `pop()`: Removes the "top" (highest priority) element from the queue. It does **not** return the value. _(Time: O(log N))_
    
- `top()`: Returns the value of the highest priority element without removing it. _(Time: O(1))_
    
- `empty()`: Returns `true` if the queue has no elements, `false` otherwise. _(Time: O(1))_
    
- `size()`: Returns the number of elements currently in the queue. _(Time: O(1))_

priority_queue<pair<int, pair<int,int>>, vector<pair<int, pair<int,int>>> , greater<pair<int, pair<int,int>>>> pq;

The Min-Heap (For Dijkstra's Algorithm)
```cpp
// Syntax: priority_queue< Type, Container, Comparator > 
priority_queue<int, vector<int>, greater<int>> min_pq;
```

#### Cheapest Flights Within K Stops

There are `n` cities connected by some number of flights. You are given an array `flights` where `flights[i] = [fromi, toi, pricei]` indicates that there is a flight from city `fromi` to city `toi` with cost `pricei`.

You are also given three integers `src`, `dst`, and `k`, return _**the cheapest price** from_ `src` _to_ `dst` _with at most_ `k` _stops._ If there is no such route, return `-1`.

here do not apply djisktra on price, we rather apply on stops and use a queue instead

```cpp
class Solution {
public:

    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {

        vector<vector<pair<int,int>>> adj(n);
        vector<int> dist(n ,1e9);
        dist[src] = 0;

        for(auto it: flights)
        {
            adj[it[0]].push_back({it[1], it[2]});
        }

        queue<pair<int, pair<int,int>>> q;

        q.push({0 , {src, 0}});

  

        while(!q.empty())

        {

            auto it = q.front();
            int stops = it.first;
            int node = it.second.first;
            int price = it.second.second;
            q.pop();


            if(stops > k) continue;

            for(auto x : adj[node])

            {

                int desti = x.first;

                int newprice = x.second;

                if(price + newprice < dist[desti])

                {
                    dist[desti] = price + newprice;
                    q.push({stops + 1 , {desti , price + newprice}});
                }
            }
        }

        if(dist[dst] == 1e9) return -1;
        else return dist[dst];

    }
};
```

Number of Ways to Arrive at Destination

**Problem Statement:** You are in a city that consists of n intersections numbered from 0 to n - 1 with bi-directional roads between some intersections. The inputs are generated such that you can reach any intersection from any other intersection and that there is at most one road between any two intersections.  
  
You are given an integer n and a 2D integer array ‘roads’ where roads[i] = [ui, vi, timei] means that there is a road between intersections ui and vi that takes timei minutes to travel. You want to know in **how many ways you can travel from intersection 0 to intersection n - 1 in the shortest amount of time.**

in this question : obviously we will use pq with djikstra on time, and also we will maintain a ways array to count for ways to reach each and every node. because the ways get accumulated/passed on to other nodes.. unlike we counting just the ways to reach last node

```cpp
 // If a shorter path is found, update the distance and number of ways
                if (dis + edW < dist[adjNode]) {

                  // Update the distance
                   dist[adjNode] = dis + edW;

                   // Push the new node with updated distance
                   pq.push({dis + edW, adjNode});

                  // Copy the number of ways to the new node
                   ways[adjNode] = ways[node];
                }

              // If the same shortest path is found, update the number of ways
               else if (dis + edW == dist[adjNode]) {

                   // Increment the number of ways
                   ways[adjNode] = (ways[adjNode] + ways[node]);
              }
         }
     }
```


Bellman ford algo - for negative weights .. follow up fo dijkstra algo

```cpp
class Solution {
  public:
    vector<int> bellmanFord(int V, vector<vector<int>>& edges, int src) {
       
       vector<int> dist(V , 1e8);
       dist[src] = 0;
       
       for(int i = 0; i < V-1; i++)
       {
           for(auto it : edges)
           {
               int u = it[0];
               int v = it[1];
               int wt = it[2];
               
               if(dist[u] != 1e8 && dist[u] + wt < dist[v])
               {
                   dist[v] = dist[u] + wt;
               }
           }
       }
       for(int i = 0; i < V-1; i++)
       {
           for(auto it : edges)
           {
               int u = it[0];
               int v = it[1];
               int wt = it[2];
               
               if(dist[u] != 1e8 && dist[u] + wt < dist[v])
               {
                   return {-1};
               }
           }
       }
       return dist;
    }
};
```

Floyd Warshall algo - shortest path, negative edges. detect negative edges if "dist(i)(i) <0"

```cpp
class Solution {
  public:
    void floydWarshall(vector<vector<int>> &dist) {
        int n = dist.size();
        for(int k = 0; k < n ; k++)
        {
            for(int i = 0; i < n; i++)
            {
                for(int j = 0; j < n; j++)
                {   if(dist[i][k] != 1e8 && dist[k][j] != 1e8 )
                    {dist[i][j] = min(dist[i][j] , dist[i][k] + dist[k][j]); }
                }
            }
        }
        
    }
};
```

**Pair Access:** When dealing with a `std::pair<int, int>`, you must access its elements using `.first` and `.second`. Using array indexing like `it[0]` or `iter[1]` will throw a compilation error.

Minimum spanning tree

```cpp
class Solution {
  public:
    int spanningTree(int n, vector<vector<int>>& edges) {
        
        
        vector<vector<pair<int,int>>> adj(n);
        
        for(auto it: edges)
        {
            int u = it[0];
            int v = it[1];
            int w = it[2];
            
            adj[u].push_back({v, w});
            adj[v].push_back({u, w});
        }
        
        vector<int> vis(n, 0);
        
        priority_queue<pair<int,int> , vector<pair<int,int>> , greater<pair<int,int>>> pq;
        
        int sum = 0;
        
        pq.push({0, 0});
        
        while(!pq.empty())
        {
            auto it = pq.top();
            pq.pop();
            int node = it.second;
            int wt = it.first;
            
            if(vis[node]) continue;
            
            vis[node] = 1;
            
            for(auto iter: adj[node])
            {
                int nbr = iter.first;
                int edw = iter.second;
                
                if(vis[nbr]) continue;
                
                pq.push({edw, nbr});
            }
            
            sum += wt;
        }
        
        return sum;
        
    }
};
```


Disjoint Sets

```cpp

class disjointset{
    vector<int> rank, parent, size;
    
    public:
    disjointset(int n)
    {
        rank.resize(n+1, 0);
        parent.resize(n+1);
        size.resize(n+1);
        
        for (int i = 0; i<= n; i++)
        {
            parent[i] = i;
            size[i] = 1;
        }
    }
    
    int findupar(int node)
        {
            if(node == parent[node]) return node;
            return parent[node] = findupar(parent[node]);
        }
        
        void unionbyrank(int u, int v)
        {
            int ulp_u = findupar(u);
            int ulp_v = findupar(v);
            if(ulp_u == ulp_v) return;
            
            if(rank[ulp_u] < rank[ulp_v])
            {
                parent[ulp_u] = ulp_v;
            }
            else if(rank[ulp_v] < rank[ulp_u])
            {
                parent[ulp_v] = ulp_u;
            }
            else
            {
                parent[ulp_v] = ulp_u;
                rank[ulp_u]++;
            }
            
        }
        
        void unionbysize(int u, int v)
        {
            int ulp_u = findupar(u);
            int ulp_v = findupar(v);
            if(ulp_u == ulp_v) return;
            
            if(size[ulp_u] < size[ulp_v])
            {
                parent[ulp_u] = ulp_v;
                size[ulp_v] += size[ulp_u];
            }
            else
            {
                parent[ulp_v] = ulp_u;
                size[ulp_u] += size[ulp_v];
            }
        }
    };
```

In C++, a constructor's name must match the class name **exactly**.

#### Making A Large Island
You are given an `n x n` binary matrix `grid`. You are allowed to change **at most one** `0` to be `1`.

Return _the size of the largest **island** in_ `grid` _after applying this operation_.

An **island** is a 4-directionally connected group of `1`s.

```cpp
class disjointset {
public:
    vector<int> rank, parent, size;
    
    disjointset(int n) {
        rank.resize(n + 1, 0);
        parent.resize(n + 1);
        size.resize(n + 1);
        
        for (int i = 0; i <= n; i++) {
            parent[i] = i;
            size[i] = 1;
        }
    }
    
    // Path compression: points node directly to ultimate parent
    int findupar(int node) {
        if (node == parent[node]) return node;
        return parent[node] = findupar(parent[node]);
    }
        
    void unionbyrank(int u, int v) {
        int ulp_u = findupar(u);
        int ulp_v = findupar(v);
        if (ulp_u == ulp_v) return;
        
        if (rank[ulp_u] < rank[ulp_v]) {
            parent[ulp_u] = ulp_v;
        } else if (rank[ulp_v] < rank[ulp_u]) {
            parent[ulp_v] = ulp_u;
        } else {
            parent[ulp_v] = ulp_u;
            rank[ulp_u]++;
        }
    }
        
    // Keeps track of the total number of nodes in each component
    void unionbysize(int u, int v) {
        int ulp_u = findupar(u);
        int ulp_v = findupar(v);
        if (ulp_u == ulp_v) return;
        
        if (size[ulp_u] < size[ulp_v]) {
            parent[ulp_u] = ulp_v;
            size[ulp_v] += size[ulp_u];
        } else {
            parent[ulp_v] = ulp_u;
            size[ulp_u] += size[ulp_v];
        }
    }
};

class Solution {
public:
    int largestIsland(vector<vector<int>>& grid) {
        int n = grid.size();
        disjointset ds(n * n);
        int maxland = 0;

        // STEP 1: Build the initial islands (Connect all adjacent 1s)
        for (int row = 0; row < n; row++) {
            for (int col = 0; col < n; col++) {
                if (grid[row][col] == 0) continue;

                vector<int> dr = {-1, 0, 1, 0};
                vector<int> dc = {0, -1, 0, 1};
                
                for (int ind = 0; ind < 4; ind++) {
                    int nrow = row + dr[ind];
                    int ncol = col + dc[ind];

                    // If neighbor is valid and is also land, union them
                    if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < n && grid[nrow][ncol] == 1) {
                        int nodeno = row * n + col;
                        int adjnodeno = nrow * n + ncol;
                        ds.unionbysize(nodeno, adjnodeno);
                    }
                }
            }
        }

        // STEP 2: Try converting each 0 to a 1 to see the max possible island
        for (int row = 0; row < n; row++) {
            for (int col = 0; col < n; col++) {
                if (grid[row][col] == 1) continue;

                vector<int> dr = {-1, 0, 1, 0};
                vector<int> dc = {0, -1, 0, 1};
                
                // Use a set to prevent counting the same surrounding island twice
                set<int> components;
                
                for (int ind = 0; ind < 4; ind++) {
                    int nrow = row + dr[ind];
                    int ncol = col + dc[ind];
                    
                    if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < n && grid[nrow][ncol] == 1) {
                        components.insert(ds.findupar(nrow * n + ncol));
                    }
                }
                
                // Calculate total size if we flip this 0
                int sizetotal = 0;
                for (auto it : components) {
                    sizetotal += ds.size[it];
                }

                // +1 accounts for the 0 we just flipped to a 1
                maxland = max(maxland, sizetotal + 1);
            }
        }

        // STEP 3: Edge Case (What if the grid contains NO 0s?)
        // Iterate through all cells to find the max component size
        for (int i = 0; i < n * n; i++) {
            maxland = max(maxland, ds.size[ds.findupar(i)]);
        }
    
        return maxland;
    }
};
};
```

#### accounts merge

Given a list of `accounts` where each element `accounts[i]` is a list of strings, where the first element `accounts[i][0]` is a name, and the rest of the elements are **emails** representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some common email to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails **in sorted order**. The accounts themselves can be returned in **any order**.

```cpp
// Standard Disjoint Set Union (DSU) implementation
class disjointset {
    vector<int> rank, parent, size;
    
public:
    disjointset(int n) {
        rank.resize(n + 1, 0);
        parent.resize(n + 1);
        size.resize(n + 1);
        
        for (int i = 0; i <= n; i++) {
            parent[i] = i;
            size[i] = 1;
        }
    }
    
    // Path compression: Flattens the tree for nearly O(1) lookups
    int findupar(int node) {
        if(node == parent[node]) return node;
        return parent[node] = findupar(parent[node]);
    }
        
    void unionbyrank(int u, int v) {
        int ulp_u = findupar(u);
        int ulp_v = findupar(v);
        if(ulp_u == ulp_v) return;
        
        if(rank[ulp_u] < rank[ulp_v]) {
            parent[ulp_u] = ulp_v;
        } else if(rank[ulp_v] < rank[ulp_u]) {
            parent[ulp_v] = ulp_u;
        } else {
            parent[ulp_v] = ulp_u;
            rank[ulp_u]++;
        }
    }
        
    // Union by size: Attaches smaller tree to larger tree to keep it shallow
    void unionbysize(int u, int v) {
        int ulp_u = findupar(u);
        int ulp_v = findupar(v);
        if(ulp_u == ulp_v) return;
        
        if(size[ulp_u] < size[ulp_v]) {
            parent[ulp_u] = ulp_v;
            size[ulp_v] += size[ulp_u];
        } else {
            parent[ulp_v] = ulp_u;
            size[ulp_u] += size[ulp_v];
        }
    }
};

class Solution {
public:
    vector<vector<string>> accountsMerge(vector<vector<string>>& details) {
        
        int n = details.size();
        disjointset ds(n);

        // Map to remember which account index an email first belonged to
        // Format: { "email@example.com" : account_index }
        unordered_map<string, int> mapMailNode;

        // STEP 1: Connect accounts that share common emails
        for (int i = 0; i < n; i++) {
            // Start at j = 1 to skip the account name (which is at j = 0)
            for (int j = 1; j < details[i].size(); j++) {
                string mail = details[i][j];

                // If this is a new email, map it to the current account index
                if (mapMailNode.find(mail) == mapMailNode.end()) {
                    mapMailNode[mail] = i;
                }
                // If we've seen this email before, union current account with the previous owner's account
                else {
                    ds.unionbysize(i, mapMailNode[mail]);
                }
            }
        }

        // STEP 2: Group all emails under their Ultimate Parent's index
        // Using vector of vectors to avoid Variable Length Arrays (VLA)
        vector<vector<string>> mergedMail(n); 
        
        for (auto it : mapMailNode) {
            string mail = it.first;
            // Find the ultimate parent index for this email's group
            int node = ds.findupar(it.second); 
            mergedMail[node].push_back(mail);
        }

        // STEP 3: Format the final result to match the problem requirements
        vector<vector<string>> ans;
        for (int i = 0; i < n; i++) {
            // Skip empty groups
            if (mergedMail[i].empty()) continue;

            // Problem requires emails to be sorted alphabetically
            sort(mergedMail[i].begin(), mergedMail[i].end());

            vector<string> temp;
            
            // Add the account name first (always at index 0 of the original details)
            temp.push_back(details[i][0]);

            // Add all the sorted emails for this group
            for (auto &mail : mergedMail[i]) {
                temp.push_back(mail);
            }
            
            ans.push_back(temp);
        }

        // Sort the final combined accounts array
        sort(ans.begin(), ans.end());
        return ans;
    }
};
```

