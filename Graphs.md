# Graphs 

## Theory 
https://www.youtube.com/watch?v=s7zE4Nmc2Fg&list=PL5DyztRVgtRVLwNWS7Rpp4qzVVHJalt22&index=1&t=24s

## Questions



1. https://www.interviewbit.com/problems/path-with-good-nodes/

   ```
     void dfs(int node,vector<int>adj[],int c,int C,vector<int>&good,vector<int>&vis){

    vis[node]=1;

    if(good[node]==1){

        c++;

    }

    if(adj[node].size()==1){

        

        if(c<=C)ans++;

        return;

    }  

    for(auto it:adj[node]){

        if(!vis[it])

        dfs(it,adj,c,C,good,vis);

    }

   }

   
   ```
2. https://leetcode.com/problems/pacific-atlantic-water-flow/description/

       ```
           class Solution {
       public:
       vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        int m = heights.size(), n = heights[0].size();

        vector<vector<int>> pacific(m, vector<int>(n, -1)),
            atlantic(m, vector<int>(n, -1));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 || j == 0)
                    pacific[i][j] = 1;
                if (i == m - 1 || j == n - 1)
                    atlantic[i][j] = 1;
            }
        }

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (pacific[i][j] == 1)
                    dfs(heights, i, j, m, n, pacific);
                if (atlantic[i][j] == 1)
                    dfs(heights, i, j, m, n, atlantic);
            }
        }

        vector<vector<int>> result;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (pacific[i][j] == 1 && atlantic[i][j] == 1)
                    result.push_back({i, j});
            }
        }

        return result;
       }

        private:
        void dfs(vector<vector<int>>& heights, int r, int c, int& m, int& n,
             vector<vector<int>>& ocean) {
        int DIR[][2] = {
            {0, -1}, {1, 0}, {0, 1}, {-1, 0}}; // left, up, right, down

        for (int i = 0; i < 4; i++) {
            int nr = r + DIR[i][0], nc = c + DIR[i][1];
            // invalid index or visited
            if (nr < 0 || nc < 0 || nr >= m || nc >= n || ocean[nr][nc] == 1)
                continue;

            if (heights[nr][nc] >= heights[r][c]) {
                ocean[nr][nc] = 1; // visited & can flow
                dfs(heights, nr, nc, m, n, ocean);
            }
        }
       }
       };
       ```


 3. Stepping Num
     ```
       void dfs (int n, int A, int B, vector<int> &ans) {
    if (n > B)
        return;
    
    if (n >= A && n <= B)
        ans.push_back(n);
    
    int lastDigit = n % 10;
    
    if (lastDigit != 0) 
        dfs(n*10 + lastDigit-1, A, B, ans);
    
    if (lastDigit != 9)
        dfs(n*10 + lastDigit+1, A, B, ans);
    }

    vector<int> Solution::stepnum(int A, int B) {
    vector<int> ans;
    
    for (int i=1; i <= 9; i++) {
        dfs(i, A, B, ans);
    }
    
    if (A == 0) // in the question A >= 1, but in one test-case A is 0
        ans.push_back(0);
    
    sort(ans.begin(), ans.end());
    
    return ans;
    }
    ```
4. Capture regions on board 
     ```
      void dfs(int row,int col,vector<vector<int>>&vis,vector<vector<char>>&A){
    vis[row][col]=1;
    A[row][col]='#';
    int n=A.size(),m=A[0].size();
    int dr[]={-1,0,+1,0};
    int dc[]={0,+1,0,-1};
    for(int i=0;i<4;i++){
        int nr=row+dr[i];
        int nc=col+dc[i];
        if(nr>=0 && nc>=0 && nr<n && nc<m && !vis[nr][nc] && A[nr][nc]=='O'){
            dfs(nr,nc,vis,A);
        }
    }
   }

    void Solution::solve(vector<vector<char> > &A) {
    int n=A.size(),m=A[0].size();
    vector<vector<int>>vis(n,vector<int>(m,0));    
    for(int i=0;i<n;i++){
        for(int j=0;j<m;j++){
            if(i==0 || j==0 || i==n-1 || j==m-1){
                if(A[i][j]=='O' && !vis[i][j]){
                    dfs(i,j,vis,A);
                }
            }
        }
    }
    for(int i=0;i<n;i++){
        for(int j=0;j<m;j++){
            if(A[i][j]=='#'){
                A[i][j]='O';
            }
            else if(A[i][j]=='O'){
                A[i][j]='X';
            }
        }
    }
    return ;
    }
   ```

5. Word search board 

```
    bool safe(int i, int j, int n, int m){
   
    if(i<0 || j<0 || i>=n || j>=m)
     return false;
     
     return true;
     
}

bool DFS(vector<string> &A, int i, int j, string B, int idx , vector<vector<bool>> &vis){
   
   
    int n = A.size();
    int m = A[0].size();
     
    if(idx==B.size())
      return true;
     
    int dx[4]={-1,1,0,0};
    int dy[4]={0,0,1,-1};
   
    for(int k=0;k<4;k++){
       
        int new_x = i + dx[k];
        int new_y = j + dy[k];
       
        if(safe(new_x,new_y,n,m) && A[new_x][new_y]==B[idx]) {
           if( DFS(A,new_x,new_y,B,idx+1,vis) )
             return true;
        }
       
    }
   
    return false;  
}


     int Solution::exist(vector<string> &A, string B) {
   
      int n = A.size();
      int m = A[0].size();
     
     
      for(int i=0;i<n;i++){
          for(int j=0;j<m;j++){
           
            vector<vector<bool>> vis(n, vector<bool>(m,0));
           
            if(A[i][j]==B[0]){
             if(  DFS(A , i , j , B, 1, vis) )
               return 1;  
            }
           
          }
      }
   
    return 0;
  }
 ```

6. https://www.interviewbit.com/problems/valid-path/
    ```
     int dx[8] = {1, 1, 1, 0, 0, -1, -1, -1};
     int dy[8] = {0, 1, -1, 1, -1, 0, 1, -1};

    string bfs(vector<vector<bool>>& mat, int x, int y, int i, int j) {
    mat[i][j] = true;
    queue<pair<int, int>> q;
    q.push({i, j});

    while (!q.empty()) {
        pair<int, int> top = q.front();
        q.pop();
        int i = top.first;
        int j = top.second;
        if (i == x && j == y)
            return "YES";

        for (int k = 0; k < 8; k++) {
            int ny = j + dy[k];
            int nx = i + dx[k];

            if (nx >= 0 && nx <= x && ny >= 0 && ny <= y && mat[nx][ny] == false) {
                mat[nx][ny] = true;
                q.push({nx, ny});
            }
        }
    }
    return "NO";
     }

    string Solution::solve(int A, int B, int C, int D, vector<int>& E, vector<int>& F) {
    vector<vector<bool>> mat(A + 1, vector<bool>(B + 1));

    for (int i = 0; i <= A; i++) {
        for (int j = 0; j <= B; j++) {
            bool flag = false;
            for (int k = 0; k < C; k++) {
                int x = (E[k] - i) * (E[k] - i);
                int y = (F[k] - j) * (F[k] - j);
                if (x + y <= D * D) {
                    flag = true;
                    break;
                }
            }
            mat[i][j] = flag;
        }
    }

    if (mat[0][0] == true)
        return "NO";

    return bfs(mat, A, B, 0, 0);
    }

    ```

7. Delete edge *** Doubt

8. Snake & ladder 
    ``` 
     int Solution::snakeLadder(vector<vector<int> > &A, vector<vector<int> > &B) {
    unordered_map<int, int>ladder;
    for(auto it : A){
    ladder[it[0]] = it[1];
    }
    unordered_map<int, int>snake;
   for(auto it : B){
    snake[it[0]] = it[1];
   }
   priority_queue<pair<int,int>, vector<pair<int, int>>, greater<pair<int, int>>>q;
   q.push({0, 1});
   vector<int>vis(101, 0);
   vis[1] = 1;
   while(!q.empty()){
    int step = q.top().first;
    int node = q.top().second;
    q.pop();
   
    if(node==100){
      return step;
    }
   
    for(int i=1; i<=6; i++){
      int newNode = node+i;
      if(!vis[newNode]){
        if(newNode<=100 && ladder[newNode]>0){
          q.push({step+1, ladder[newNode]});
          vis[ladder[newNode]]=1;
        }else if(newNode<=100 && snake[newNode]>0){
          q.push({step+1, snake[newNode]});
          vis[snake[newNode]]=1;
        }else{
          if(newNode<=100){
            q.push({step+1, newNode});
            vis[newNode]=1;
          }
        }
      }
    }
   }
   return -1;
   }

    ```

    
9. Level Order Traversal 

   ```
    vector<vector<int>> Solution::levelOrder(TreeNode* A) {
    vector<vector<int>> ans;
    if (A == NULL)
        return ans;

    // we will do BFS
    queue<TreeNode*> q;  // Change the queue declaration to hold pointers
    q.push(A);
    vector<int> curr;

    while (!q.empty()) {
        int sz = q.size();
        while (sz--) {
            TreeNode* current = q.front();
            q.pop();
            curr.push_back(current->val);
            if (current->left != NULL)
                q.push(current->left);
            if (current->right != NULL)
                q.push(current->right);
        }
        ans.push_back(curr);
        curr.clear();
    }
    return ans;
   }
   ```
10. Smallest multiple with 0 & 1 https://www.youtube.com/watch?v=Om47LiGTy8o
11. Mother vertex 
    ```
       int Solution::motherVertex(int A, vector<vector<int> > &B) {
    vector<int> indeg (A+1, 0);
    for (auto i : B) {
        if (i[0] != i[1]) indeg[i[1]]++;
    }
    
    int count = 0;
    for (int i : indeg) {
        if (i == 0) count++;
    }
    
    // accounting for the 0 at indeg[0] (unaccessed)
    return (count<=2);
    }
    
    ```

12.  Word ladder 1 

     ```
        int ladderLength(string beginWord, string endWord,
                     vector<string>& wordList) {

        unordered_set<string> st(wordList.begin(), wordList.end());
        if (st.find(beginWord) != st.end())
            st.erase(beginWord);
        queue<pair<string, int>> q;
        q.push({beginWord, 1});
       

        int shortestLevel = 0;
        while (!q.empty()) {
            auto [word, level] = q.front();
            q.pop();

            if (word == endWord) {
                shortestLevel = level;
                break;
            }

            for (int i = 0; i < word.size(); i++) {
                string str = word;

                for (char ch = 'a'; ch <= 'z'; ch++) {
                    str[i] = ch;

                    if (st.find(str) != st.end()) {
                        q.push({str, level + 1});
                        st.erase(str);
                    }
                }
            }
        }
        return shortestLevel;
      }
    
     ```
    
13.  Word ladder 2 https://www.youtube.com/watch?v=DREutrv2XD0 ***

14.  Clone Graph https://www.youtube.com/watch?v=z7mPg_xT5xk

```
    class Solution {
      public:
      unordered_map<Node*, Node*> mp;

        void dfs(Node* node, Node* clone) {
        for (Node* n : node->neighbors) {
            if (mp.find(n) == mp.end()) {
                Node* clone1 = new Node(n->val);
                mp[n] = clone1;
                clone->neighbors.push_back(clone1);
                dfs(n, clone1);

            }

            else {
                clone->neighbors.push_back(mp[n]);
            }
        }
    }

    Node* cloneGraph(Node* node) {
        if (node == NULL)
            return NULL;

        Node* clone = new Node(node->val);
        mp[node] = clone;

        dfs(node, clone);

        return clone;
    }
    };
 ```

15. Alien dictionary Topological sort
    ```
      vector<int> topoSort(int V, vector<int> adj[])
	{
		int indegree[V] = {0};
		for (int i = 0; i < V; i++) {
			for (auto it : adj[i]) {
				indegree[it]++;
			}
		}

		queue<int> q;
		for (int i = 0; i < V; i++) {
			if (indegree[i] == 0) {
				q.push(i);
			}
		}
		vector<int> topo;
		while (!q.empty()) {
			int node = q.front();
			q.pop();
			topo.push_back(node);
			// node is in your topo sort
			// so please remove it from the indegree

			for (auto it : adj[node]) {
				indegree[it]--;
				if (indegree[it] == 0) q.push(it);
			}
		}

		return topo;
	}
        public:
	string findOrder(string dict[], int N, int K) {
		vector<int>adj[K];
		for (int i = 0; i < N - 1; i++) {
			string s1 = dict[i];
			string s2 = dict[i + 1];
			int len = min(s1.size(), s2.size());
			for (int ptr = 0; ptr < len; ptr++) {
				if (s1[ptr] != s2[ptr]) {
					adj[s1[ptr] - 'a'].push_back(s2[ptr] - 'a');
					break;
				}
			}
		}

		vector<int> topo = topoSort(K, adj);
		string ans = "";
		for (auto it : topo) {
			ans = ans + char(it + 'a');
		}
		return ans;
	}
     };


    ```
16. Sum of all distances in a tree
17. Validate binary tree 
     ```
         vector<int> indegree(n, 0);
        for (int i = 0; i < n; i++) {
            if (leftChild[i] != -1) {
                indegree[leftChild[i]]++;
            }
            if (rightChild[i] != -1) {
                indegree[rightChild[i]]++;
            }
        }
        int root = -1;
        for (int i = 0; i < n; i++) {
            if (indegree[i] == 0) {
                if (root == -1) {
                    root = i;
                } else {
                    return false;
                }
            }
        }
        if (root == -1) {
            return false;
        }

        vector<int> visited(n, 0);
        queue<int> q;
        q.push(root);
        int x = 0;

        while (!q.empty()) {

            int temp = q.front();
            q.pop();
            if (visited[temp]) {
                return false;
            }
            visited[temp] = 1;
            x++;
            if (leftChild[temp] != -1) {
                q.push(leftChild[temp]);
            }
            if (rightChild[temp] != -1) {
                q.push(rightChild[temp]);
            }
        }
        int sum = 0;
        for (auto i : visited) {
            if (i == 1) {
                sum++;
            }
        }
        return sum == n;
     ```

 18. Detonate the maximum bombs

       ```
        void dfs(vector<int>& vis, vector<vector<int>>& temp, int& t, int& i) {
        vis[i] = 1;
        t++;
        for (int j = 0; j < temp[i].size(); j++) {
            if (!vis[temp[i][j]]) {

                dfs(vis, temp, t, temp[i][j]);
            }
        }
      }

       int maximumDetonation(vector<vector<int>>& bombs) {
        int n = bombs.size();
        vector<vector<int>> temp(n);

        for (int i = 0; i < n; i++) {
            long long x = bombs[i][0];
            long long y = bombs[i][1];
            long long r = bombs[i][2];
            for (int j = 0; j < n; j++) {
                if (i != j) {
                    long long x1 = abs(x - bombs[j][0]);
                    long long y1 = abs(y - bombs[j][1]);
                    if (x1 * x1 + y1 * y1 <= r * r) {
                        temp[i].push_back(j);
                    }
                }
            }
        }

        int ans = INT_MIN;
        for (int i = 0; i < n; i++) {
            int t = 0;
            vector<int> vis(n, 0);
            dfs(vis, temp, t, i);
            ans = max(ans, t);
        }
        return ans;  
       
       ```
 19. Possible recipes
       ```
          vector<string> findAllRecipes(vector<string>& recipes, vector<vector<string>>& ingredients, vector<string>& supplies) {
        int numberOfRecipes = recipes.size();
        vector<string> result;
        unordered_map<string,vector<string>> adj;
        for(int i=0;i<ingredients.size();i++){
            for(int j=0;j<ingredients[i].size();j++){
                adj[ingredients[i][j]].push_back(recipes[i]);
            }
        }
        unordered_map<string,int> inDegree;
        for(int i=0;i<recipes.size();i++){
            inDegree[recipes[i]] = ingredients[i].size();
        }
        queue<string>q;
        for(int i=0;i<supplies.size();i++){
            q.push(supplies[i]);
        }
        while(!q.empty()){
            string currentSupply = q.front();
            q.pop();
            vector<string> recipesThatCanBeMade = adj[currentSupply];
            for(int i=0;i<recipesThatCanBeMade.size();i++){
                inDegree[recipesThatCanBeMade[i]]--;
                if(inDegree[recipesThatCanBeMade[i]]==0){
                    q.push(recipesThatCanBeMade[i]);
                    result.push_back(recipesThatCanBeMade[i]);
                }
            }
        }
        return result;
       ```

20. Count unreachable pairs 

     ```
       int dfs(vector<vector<int>>& adj, vector<bool>& visited, int currNode) {
        visited[currNode] = true;
        int nodeCount = 1;
        for (int adjNode : adj[currNode]) {
            if (visited[adjNode])
                continue;
            nodeCount += dfs(adj, visited, adjNode);
        }
        return nodeCount;
    }
    long long countPairs(int n, vector<vector<int>>& edges) {
        vector<vector<int>> adj(n);
        for (vector<int> edge : edges) {
            adj[edge[0]].push_back(edge[1]);
            adj[edge[1]].push_back(edge[0]);
        }
        //=======================================================================
        vector<bool> visited(n, false);
        vector<int> components;
        for (int node = 0; node < n; node++) {
            if (visited[node])
                continue;
            int componentSize = dfs(adj, visited, node);
            components.push_back(componentSize);
        }
        //=======================================================================
        long long postSum = components[components.size() - 1];
        long long ans = 0;
        for (int i = components.size() - 2; i >= 0; i--) {
            ans += (components[i] * postSum);
            postSum += components[i];
        }
        //=============================================================================
        return ans;
        }
     ```
21. Reorder Routes to Make All Paths Lead to the City Zero
    ```
      int count =0;
     void dfs(int node,int parent,unordered_map<int,vector<pair<int,int>>> & adj )
     {
    for( auto & child : adj[node])
    {
        int a =child.first;
        int check=child.second;
        if(a!=parent)
        {
            if(check==1)
            count++;
            dfs(a,node,adj);
        }
        

    }
    }
    int minReorder(int n, vector<vector<int>>& connections) {
        unordered_map<int,vector<pair<int,int>>> adj;


        for(auto &connect : connections)
        {
            int a =connect[0];
            int b=connect[1];
            adj[a].push_back({b,1});
            adj[b].push_back({a,0});

        }
        dfs(0,-1,adj);
        return count ;
        
    }
    ```
22. Min number of operations to make x,y equal
     ```
       int minimumOperationsToMakeEqual(int x, int y) {
       map<int,int>vis;
        queue<pair<int, int>> q;
        vis[x] = 1;
        q.push({x, 0});
        while (q.size() > 0) {
            auto it = q.front();
            q.pop();

            int a = it.first;
            int b = it.second;
            if (a == y)
                return b;
            if (a % 11 == 0) {
                if (vis[a /11] == 0) {
                    if (a / 11 == y)
                        return b + 1;
                    vis[a / 11] = 1;
                    q.push({a / 11, b + 1});
                }
            }
            if (a % 5 == 0) {
                if (vis[a / 5] == 0) {
                    if (a / 5 == y)
                        return b + 1;
                    vis[a / 5] = 1;
                    q.push({a / 5, b + 1});
                }
            }
            if (a > 1) {
                if (vis[a - 1] == 0) {
                    if (a - 1 == y)
                        return b + 1;
                    vis[a - 1] = 1;
                    q.push({a - 1, b + 1});
                }
            }
            if (vis[a + 1] == 0) {
                if (a + 1 == y)
                    return b + 1;
                vis[a + 1] = 1;
                q.push({a + 1, b + 1});
            }
        }

        return -1;
    }
     ```
23. Jump Game 3
   ```
         queue<int> q;
        set<int> s;
        q.push(start);
        unordered_map<int, int> mp;
        mp[start]=1;
        for (int i = 0; i < arr.size(); i++) {
            if (arr[i] == 0)
                s.insert(i);
        }
        while (!q.empty()) {
            int a = q.front();
            q.pop();
            if (s.find(a) != s.end())
                return true;
            if (a - arr[a] >= 0 && mp[a - arr[a]] == 0) {
                mp[a-arr[a]]=1;
                q.push(a - arr[a]);
            }
            if (a + arr[a] < arr.size() && mp[a + arr[a]] == 0) {
                mp[a+arr[a]]=1;
                q.push(a + arr[a]);
            }
        }
        return false;
    }
   ```
24. Water jug 
```
bool dfs(int x, int y, int z, int m, int curr, vector<int>& vis) {
        if (curr < 0 || curr > m || vis[curr] == 1)
            return false;
        if (curr == z)
            return true;
        vis[curr] = 1;
        bool a = dfs(x, y, z, m, curr + x, vis);
        bool b = dfs(x, y, z, m, curr - x, vis);
        bool c = dfs(x, y, z, m, curr + y, vis);
        bool d = dfs(x, y, z, m, curr - y, vis);
        return a || b || c || d;
    }
    bool canMeasureWater(int jug1Capacity, int jug2Capacity,
                         int targetCapacity) {
        int x = jug1Capacity;
        int y = jug2Capacity;
        int z = targetCapacity;
        if (x + y == z || x == z || y == z)
            return true;
        if (x + y < z)
            return false;
        int m = x + y; // max capacity
        vector<int> vis(m + 1, 0);
        return dfs(x, y, z, m, 0, vis);
    }
   
```
25. Possible bipartition
    ```
       bool dfs(vector<int>adj[], vector<int>& color, int node){
        for(auto it: adj[node]){ 
            if(color[it]==-1){ 
                color[it]=1-color[node]; 
                if(!dfs(adj,color,it)) return false;
            }
            else if(color[it]!=1-color[node]) return false;
        }
        return true;
    }
    bool possibleBipartition(int n, vector<vector<int>>& dislikes) {
         vector<int>adj[n+1];
        for(int i=0;i<dislikes.size();i++){
            adj[dislikes[i][0]].push_back(dislikes[i][1]);
            adj[dislikes[i][1]].push_back(dislikes[i][0]);
        }
        vector<int>color(n+1,-1);
        for(int i=1;i<=n;i++){
            if(color[i]==-1){
                color[i]=0;
                if(!dfs(adj,color,i)) return false;
            }
        }
        return true;
    }
    ```
26. Find the Safest Path in a Grid
    ```
       class Solution {
    public:
    int n;

    vector<vector<int>> directions{{1, 0}, {-1, 0}, {0, -1}, {0, 1}};

    bool check(vector<vector<int>>& distNearestThief, int sf) {
        queue<pair<int, int>> que;

        vector<vector<bool>> visited(n, vector<bool>(n, false));
        // 0,0 --> n-1, n-1
        que.push({0, 0});
        visited[0][0] = true;

        if (distNearestThief[0][0] < sf)
            return false;

        while (!que.empty()) {
            int curr_i = que.front().first;
            int curr_j = que.front().second;

            que.pop();

            if (curr_i == n - 1 && curr_j == n - 1) {
                return true;
            }

            for (vector<int>& dir : directions) {
                int new_i = curr_i + dir[0];
                int new_j = curr_j + dir[1];

                if (new_i >= 0 && new_i < n && new_j >= 0 && new_j < n &&
                    visited[new_i][new_j] != true) {
                    if (distNearestThief[new_i][new_j] < sf) {
                        continue; // reject this cell
                    }
                    que.push({new_i, new_j});
                    visited[new_i][new_j] = true;
                }
            }
        }

        return false;
    }

    int maximumSafenessFactor(vector<vector<int>>& grid) {
        n = grid.size();

        // Step-1 Precalculation of distNearestThief - for each cell
        vector<vector<int>> distNearestThief(n, vector<int>(n, -1));
        queue<pair<int, int>> que;
        vector<vector<bool>> visited(n, vector<bool>(n, false));

        // push all cells in queue where theives are present
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    que.push({i, j});
                    visited[i][j] = true;
                }
            }
        }

        int level = 0;
        while (!que.empty()) {
            int size = que.size();

            while (size--) {
                int curr_i = que.front().first;
                int curr_j = que.front().second;
                que.pop();
                distNearestThief[curr_i][curr_j] = level;
                for (vector<int>& dir : directions) {
                    int new_i = curr_i + dir[0];
                    int new_j = curr_j + dir[1];

                    if (new_i < 0 || new_i >= n || new_j < 0 || new_j >= n ||
                        visited[new_i][new_j]) {
                        continue;
                    }

                    que.push({new_i, new_j});
                    visited[new_i][new_j] = true;
                }
            }
            level++;
        }

        // Step-2 Apply binary search on SF
        int l = 0;
        int r = 400;
        int result = 0;

        while (l <= r) {
            int mid_sf = l + (r - l) / 2;

            if (check(distNearestThief, mid_sf)) {
                result = mid_sf;
                l = mid_sf + 1;
            } else {
                r = mid_sf - 1;
            }
        }

        return result;
    }
     };

    ```
27. Number of regions cut by slashes 
    ```
      vector<vector<int>> directions{{0, 1}, {0, -1}, {-1, 0}, {1, 0}};

    void numberOfIslandsDFS(vector<vector<int>>& matrix, int i, int j) {
        if (i < 0 || i >= matrix.size() || j < 0 || j >= matrix[0].size() ||
            matrix[i][j] == 1)
            return;

        matrix[i][j] = 1; // mark visited

        for (const auto& dir : directions) {
            int new_i = i + dir[0];
            int new_j = j + dir[1];
            numberOfIslandsDFS(matrix, new_i, new_j);
        }
    }
    int regionsBySlashes(vector<string>& grid) {
        int rows = grid.size();
        int cols = grid[0].size();
        int regions = 0;

        vector<vector<int>> matrix(rows * 3, vector<int>(cols * 3, 0));
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == '/') {
                    matrix[i * 3][j * 3 + 2] = 1;
                    matrix[i * 3 + 1][j * 3 + 1] = 1;
                    matrix[i * 3 + 2][j * 3] = 1;
                }
                if (grid[i][j] == '\\') {
                    matrix[i * 3][j * 3] = 1;
                    matrix[i * 3 + 1][j * 3 + 1] = 1;
                    matrix[i * 3 + 2][j * 3 + 2] = 1;
                }
            }
        }
        for (int i = 0; i < matrix.size(); i++) {
            for (int j = 0; j < matrix[0].size(); j++) {
                if (matrix[i][j] == 0) {
                    numberOfIslandsDFS(matrix, i, j);
                    regions += 1;
                }
            }
        }

        return regions;
    }
    ```
28. Keys & rooms 
    ```
     void dfs(vector<vector<int>>& rooms, int u, vector<bool>& visited) {
        visited[u] = true;

        for (int& node : rooms[u]) {
            if (!visited[node])
                dfs(rooms, node, visited);
        }
    }

    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        int n = rooms.size();
        vector<bool> visited(n, false);

        dfs(rooms, 0, visited);

        for (bool x : visited) {
            if (x == false)
                return false;
        }
        return true;
    }
    ```
29. Shortest path in a grid with obstacles elimination
    ```
      vector<vector<int>> directions{{-1, 0}, {0, -1}, {1, 0}, {0, 1}};
    int shortestPath(vector<vector<int>>& grid, int k) {
        int m = grid.size();
        int n = grid[0].size();

        queue<vector<int>> que;

        int i = 0, j = 0;
        que.push({i, j, k});

        bool visited[41][41][1601];
        memset(visited, false, sizeof(visited));

        int steps = 0;
        while (!que.empty()) {
            int size = que.size();
            while (size--) {
                vector<int> temp = que.front();
                que.pop();
                int curr_i = temp[0];
                int curr_j = temp[1];
                int obs = temp[2];

                if (curr_i == m - 1 && curr_j == n - 1)
                    return steps;

                for (vector<int>& dir : directions) {

                    int new_i = curr_i + dir[0];
                    int new_j = curr_j + dir[1];

                    if (new_i < 0 || new_i >= m || new_j < 0 || new_j >= n)
                        continue;

                    if (grid[new_i][new_j] == 0 &&
                        !visited[new_i][new_j][obs]) {
                        que.push({new_i, new_j, obs});
                        visited[new_i][new_j][obs] = true;
                    } else if (grid[new_i][new_j] == 1 && obs > 0 &&
                               !visited[new_i][new_j][obs - 1]) {
                        que.push({new_i, new_j, obs - 1});
                        visited[new_i][new_j][obs - 1] = true;
                    }
                }
            }
            steps++;
        }

        return -1;
    }
    ```
30. Course schedules 
    ```
      bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        int n = prerequisites.size();
        vector<int> adj[numCourses];
        vector<int> indegree(numCourses);

        for (int i = 0; i < n; i++) {
            int a = prerequisites[i][0];
            int b = prerequisites[i][1];

            adj[b].push_back(a);

            indegree[a]++;
        }

        queue<int> q;
        int count = 0;

        for (int i = 0; i < indegree.size(); i++) {
            if (indegree[i] == 0)
                q.push(i);
        }

        while (!q.empty()) {
            int curr = q.front();
            q.pop();
            count++;

            for (auto& it : adj[curr]) {
                indegree[it]--;
                if (indegree[it] == 0)
                    q.push(it);
            }
        }

        return count == numCourses;
    }
    ```
31. Walls & Gates 
    ```
      int emptyRoom = 2147483647;
    int wall = -1;
    int gate = 0;
    vector<pair<int, int>> dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    void wallsAndGates(vector<vector<int>>& rooms) {
        vector<vector<int>> ans = rooms;
        int m = rooms.size();
        int n = rooms[0].size();
        queue<pair<int, int>> q;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (rooms[i][j] == gate)
                    q.push({i, j});
            }
        }
        int distance = 0;
        while (!q.empty()) {
            int size = q.size();
            distance++;
            while (size--) {
                auto [x, y] = q.front();
                q.pop();
                for (auto [dx, dy] : dirs) {
                    int i = x + dx;
                    int j = y + dy;
                    if (i >= 0 && i < m && j >= 0 && j < n &&
                        rooms[i][j] == emptyRoom) {
                        rooms[i][j] = distance;
                        q.push({i, j});
                    }
                }
            }
        }
        
    }
    ```
32. Smallest rectangle enclosing black pixels 
     ```
        int minArea(const vector<vector<char>>& image, int xx, int yy) {
        x = xx;
        y = yy;
        img = image;
        int r = right(), l = left(), b = bottom(), t = top();
        return (r - l + 1) * (b - t + 1);
    }

     private:
    vector<vector<char>> img;
    int x, y;

    int left() const {
        int l = 0, r = y - 1;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            if (goodCol(mid)) {
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return r + 1;
    }

    int right() const {
        int l = y + 1, r = img[0].size() - 1;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            if (goodCol(mid)) {
                l = mid + 1;
            } else {
                r = mid - 1;
            }
        }
        return l - 1;
    }

    int bottom() const {
        int l = x + 1, r = img.size() - 1;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            if (goodRow(mid)) {
                l = mid + 1;
            } else {
                r = mid - 1;
            }
        }
        return l - 1;
    }

    int top() const {
        int l = 0, r = x - 1;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            if (goodRow(mid)) {
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return r + 1;
    }

    bool goodCol(int c) const {
        for (int i = 0; i < img.size(); ++i) {
            if (img[i][c] == '1') {
                return true;
            }
        }
        return false;
    }

    bool goodRow(int r) const {
        for (int i = 0; i < img[0].size(); ++i) {
            if (img[r][i] == '1') {
                return true;
            }
        }
        return false;
    }
     ```
33. Min height tree
   ```
     //A tree can have at most two centers (roots of MHTs). This is a well-known property in graph theory for trees. If you keep trimming the leaf nodes (nodes with a single connection), 
      you eventually reach one or two nodes that form the core of the tree.

     vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        if (n == 1)
            return {0};

        vector<vector<int>> adj(n);
        vector<int> indegree(n, 0);

        for (int i = 0; i < edges.size(); i++) {
            adj[edges[i][0]].push_back(edges[i][1]);
            adj[edges[i][1]].push_back(edges[i][0]);
            indegree[edges[i][0]]++;
            indegree[edges[i][1]]++;
        }

        vector<int> ans;
        queue<int> q;

        for (int i = 0; i < n; i++) {
            if (indegree[i] == 1)
                q.push(i);
        }

        while (n > 2) {
            int size = q.size();
            n -= size;

            while (size--) {
                int a = q.front();
                q.pop();
                for (int v : adj[a]) {
                    indegree[v]--;
                    if (indegree[v] == 1)
                        q.push(v);
                }
            }
        }

        while (!q.empty()) {
            ans.push_back(q.front());
            q.pop();
        }

        return ans;
    }
   ```
34. Number of connected components in a graph 
    ```
      static constexpr int N = 2000;
    int countComponents(int n, vector<vector<int>>& edges) {
        vector<vector<int>> adj(n);
        bool visit[N] = {false};
        int connected = 0;

        for (const auto& edge : edges) {
            adj[edge[0]].push_back(edge[1]);
            adj[edge[1]].push_back(edge[0]);
        }

        auto bfs = [=, &visit](int i) {
            queue<int> q;
            q.push(i);
            visit[i] = true;

            while (!q.empty()) {
                int node = q.front();
                q.pop();
                for (const auto& neighbor : adj[node]) {
                    if (!visit[neighbor]) {
                        q.push(neighbor);
                        visit[neighbor] = true;
                    }
                }
            }
        };

        for (int i = 0; i < n; ++i) {
            if (!visit[i]) {
                bfs(i);
                ++connected;
            }
        }

        return connected;
    }
    ```
35. Evaluate division
    ```
      void dfs(unordered_map<string, vector<pair<string, double>>>& adj,
             string src, string dst, unordered_set<string>& visited,
             double product, double& ans) {
        if (visited.find(src) != visited.end())
            return;

        visited.insert(src);
        if (src == dst) {
            ans = product;
            return;
        }

        for (auto p : adj[src]) {

            string v = p.first;
            double val = p.second;

            dfs(adj, v, dst, visited, product * val, ans);
        }
    }
    vector<double> calcEquation(vector<vector<string>>& equations,
                                vector<double>& values,
                                vector<vector<string>>& queries) {
        int n = equations.size();
        unordered_map<string, vector<pair<string, double>>> adj;

        for (int i = 0; i < n; i++) {
            string u = equations[i][0];
            string v = equations[i][1];
            double val = values[i];
            adj[u].push_back({v, val});
            adj[v].push_back({u, 1.0 / val});
        }
        vector<double> result;
        for (auto& query : queries) {

            string src = query[0];
            string dst = query[1];

            double ans = -1.0;
            double product = 1.0;

            if (adj.find(src) != adj.end()) {
                unordered_set<string> visited;

                dfs(adj, src, dst, visited, product, ans);
            }

            result.push_back(ans);
        }

        return result;
    }
    ```
36. Trapping rainwater 2
     ```
       class Solution {
     public:
    int m, n;
    bool isValid(int x, int y) { return min(x, y) >= 0 && x < m && y < n; }

    int trapRainWater(vector<vector<int>>& matrix) {
        m = matrix.size();
        n = matrix[0].size();

        vector<vector<bool>> visited(m + 1, vector<bool>(n + 1, false));
        priority_queue<piii, vector<piii>, greater<piii>> pq;
        int dx[4] = {1, -1, 0, 0};
        int dy[4] = {0, 0, 1, -1};

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 || i == m - 1 || j == 0 || j == n - 1) {
                    pq.push({matrix[i][j], {i, j}});
                }
            }
        }

        int ans = 0;
        while (!pq.empty()) {
            int i = pq.top().s.f;
            int j = pq.top().s.s;
            int h = pq.top().f;
            pq.pop();

            if (visited[i][j])
                continue;
            visited[i][j] = 1;
            ans += h - matrix[i][j];

            for (int k = 0; k < 4; k++) {
                int nx = i + dx[k];
                int ny = j + dy[k];
                if (isValid(nx, ny) && !visited[nx][ny]) {
                    pq.push({max(h, matrix[nx][ny]), {nx, ny}});
                }
            }
        }

        return ans;
    }
    };
     ```
37. The maze 
   ```
     lass Solution {
public:
    vector<pair<int, int>> directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

    bool isValid(int row, int col, int m, int n, vector<vector<int>>& maze) {
        return row >= 0 && row < m && col >= 0 && col < n &&
               maze[row][col] == 0;
    }
    bool bfs(vector<int> start, vector<vector<int>>& maze,
             vector<vector<bool>>& visited, vector<int>& dst) {
        queue<vector<int>> q;
        q.emplace(start);

        while (!q.empty()) {
            auto start = q.front();
            q.pop();

            visited[start[0]][start[1]] = true;

            if (start == dst)
                return true;

            for (auto [dx, dy] : directions) {
                int row = start[0] + dx, col = start[1] + dy;

                while (isValid(row, col, maze.size(), maze[0].size(), maze))
                    row += dx, col += dy;

                row -= dx, col -= dy;

                if (!visited[row][col])
                    q.emplace(vector<int>{row, col});
            }
        }
        return false;
    }
    bool hasPath(vector<vector<int>>& maze, vector<int>& start,
                 vector<int>& destination) {
        int m = maze.size(), n = maze[0].size();
        vector<vector<bool>> visited(m, vector<bool>(n, false));

        return bfs(start, maze, visited, destination);
    }
};
   ```
37. The maze II 
    ```
      vector<int> row_offset = {0, 0, 1, -1}; // right, left, down, up
    vector<int> col_offset = {1, -1, 0, 0};

    void BFS(vector<vector<int>>& maze, vector<int> start,
             vector<vector<int>>& distance) {
        queue<vector<int>> Q;
        Q.push({start[0], start[1]});
        while (!Q.empty()) {
            auto S = Q.front();
            Q.pop();
            for (int offset = 0; offset < 4; offset++) {
                int new_row = S[0] + row_offset[offset];
                int new_col = S[1] + col_offset[offset];
                int count = 0;
                while (new_row >= 0 && new_row < maze.size() && new_col >= 0 &&
                       new_col < maze[0].size() &&
                       maze[new_row][new_col] == 0) {
                    new_row += row_offset[offset];
                    new_col += col_offset[offset];
                    count += 1;
                }
                if (distance[S[0]][S[1]] + count <
                    distance[new_row - row_offset[offset]]
                            [new_col - col_offset[offset]]) {
                    distance[new_row - row_offset[offset]]
                            [new_col - col_offset[offset]] =
                                distance[S[0]][S[1]] + count;
                    Q.push({new_row - row_offset[offset],
                            new_col - col_offset[offset]});
                }
            }
        }
    }
    int shortestDistance(vector<vector<int>>& maze, vector<int>& start,
                         vector<int>& destination) {
        vector<vector<int>> distance(maze.size(),
                                     vector<int>(maze[0].size(), INT_MAX));
        distance[start[0]][start[1]] = 0;
        BFS(maze, start, distance);
        return distance[destination[0]][destination[1]] == INT_MAX
                   ? -1
                   : distance[destination[0]][destination[1]];
    }
     
    ```
38. Island perimeter
    ```
       public:
    int m;
    int n;
    int peri;

    void dfs(vector<vector<int>>& grid, int i, int j) {

        if (i < 0 || i >= m || j < 0 || j >= n || grid[i][j] == 0) {
            peri++;
            return;
        }

        if (grid[i][j] == -1) {
            return;
        }

        grid[i][j] = -1; // mark visited

        dfs(grid, i + 1, j);
        dfs(grid, i - 1, j);
        dfs(grid, i, j + 1);
        dfs(grid, i, j - 1);
    }

    int islandPerimeter(vector<vector<int>>& grid) {
        m = grid.size();
        n = grid[0].size();
        peri = 0;

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {

                if (grid[i][j] == 1) {
                    dfs(grid, i, j);
                    return peri;
                }
            }
        }

        return -1;
    }
    ```
39. 01 Matrix
    ```
      vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
        int n = mat.size(), m = mat[0].size();
        vector<vector<int>> mat1(n, vector<int>(m, 0));
        int vis[n][m];
        queue<pair<pair<int, int>, int>> q;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (mat[i][j] == 0) {
                    vis[i][j] = 1;
                    mat1[i][j] = 0;
                    q.push({{i, j}, 0});
                } else {
                    vis[i][j] = 0;
                }
            }
        }
        int dx[4] = {-1, 0, 1, 0};
        int dy[4] = {0, 1, 0, -1};
        while (!q.empty()) {
            int i1 = q.front().first.first;
            int j1 = q.front().first.second;
            int c = q.front().second;
            q.pop();
            for (int i = 0; i < 4; i++) {
                int n1 = i1 + dx[i];
                int m1 = j1 + dy[i];
                if (n1 < n && n1 >= 0 && m1 < m && m1 >= 0 &&
                    vis[n1][m1] == 0) {
                    vis[n1][m1] = 1;
                    mat1[n1][m1] = c + 1;
                    q.push({{n1, m1}, c + 1});
                }
            }
        }
        return mat1;
    }
    ```
40. Swim in rising water
    ```
      static int[][] arr;
    static int n;
    static int dir[][] = { { 0, 1 }, { 0, -1 }, { -1, 0 }, { 1, 0 } };

    static boolean helper(int x) {
        if (arr[0][0] > x)
            return false;
        Queue<int[]> q = new LinkedList<>();
        q.add(new int[] { 0, 0 });
        int visited[][] = new int[n][n];
        visited[0][0] = 1;

        while (q.size() != 0) {
            int a[] = q.remove();
            int r = a[0];
            int c = a[1];
            if (r == n - 1 && c == n - 1)
                return true;

            for (int i = 0; i < 4; i++) {
                int nr = r + dir[i][0];
                int nc = c + dir[i][1];
                if (nr >= 0 && nc >= 0 && nr < n && nc < n && visited[nr][nc] != 1 && arr[nr][nc] <= x) {
                    q.add(new int[] { nr, nc });
                    visited[nr][nc] = 1;
                }
            }
        }
        return false;
    }

    public int swimInWater(int[][] grid) {

        arr = grid;
        n = arr.length;
        int st = 0;
        int end = 2505;
        int ans = -1;
        while (st <= end) {
            int mid = st + (end - st) / 2;
            if (helper(mid)) {
                ans = mid;
                end = mid - 1;
            } else
                st = mid + 1;
        }
        return ans;
    }
    ```
41. Find eventual safe nodes
    ```
      bool isCycleDFS(vector<vector<int>>& adj, int u, vector<bool>& visited, vector<bool>& inRecursion) {
        visited[u] = true;
        inRecursion[u] = true;
        
        
        for(int &v : adj[u]) {
            //if not visited, then we check for cycle in DFS
            if(visited[v] == false && isCycleDFS(adj, v, visited, inRecursion))
                return true;
            else if(inRecursion[v] == true)
                return true;
        }
        
        inRecursion[u] = false;
        return false;
        
    }
    
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        int V = graph.size();
        vector<bool> visited(V, false);
        vector<bool> inRecursion(V, false);
        
        for(int i = 0; i<V; i++) {
            if(!visited[i])
                isCycleDFS(graph, i, visited, inRecursion);
        }
        
        vector<int> safeNodes;
        for(int i = 0; i<V; i++) {
            if(!inRecursion[i])
                safeNodes.push_back(i);
        }
        
        return safeNodes;
    }
    ```
    ```
      public:
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        int V = graph.size();

        vector<vector<int>> adj(V);
        
        
        queue<int> que;
	    vector<int> indegree(V, 0);
	    int count = 0;
	    //1
	    for(int u = 0; u < V; u++) {
	        for(int &v : graph[u]) {
                adj[v].push_back(u);
	            indegree[u]++;
	        }
	    }
	    
	    //2. Fill que, indegree with 0
	    for(int i = 0; i < V; i++) {
	        if(indegree[i] == 0) {
	            que.push(i);
	            count++;
	        }
	    }
	    
	    //3. Simple BFS
        vector<bool> safe(V, false);
	    while(!que.empty()) {
	        int u = que.front();
	        que.pop();
            safe[u] = true;
	        
	        for(int &v : adj[u]) {
	            indegree[v]--;
	            
	            if(indegree[v] == 0) {
	                que.push(v);
	                count++;
	            }
	            
	        }
	    }
	    
	    vector<int> safeNodes;
        for(int i = 0; i < V; i++) {
            if(safe[i]) {
                safeNodes.push_back(i);
            }
        }
        return safeNodes;
    }
    ```
42. Max number of islands
    ```
      nt dx[4] = {1, 0, -1, 0};
    int dy[4] = {0, 1, 0, -1};
    int m, n;

    void dfs(vector<vector<int>>& grid, vector<vector<int>>& vis, int i, int j,int& ans) {
        vis[i][j] = 1;
       ans++;
        for (int k = 0; k < 4; k++) {
            int x = i + dx[k];
            int y = j + dy[k];
            if (x >= 0 && x < m && y >= 0 && y < n && vis[x][y] == 0 &&
                grid[x][y] == 1) {
                dfs(grid, vis, x, y, ans);
            }
        }
    }

    int maxAreaOfIsland(vector<vector<int>>& grid) {
        m = grid.size();
        n = grid[0].size();
        vector<vector<int>> vis(m, vector<int>(n, 0));
        int mans = 0; // Maximum area across all islands

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1 && vis[i][j] == 0) {
                    int ans = 0; // Start with area 1 for the new island
                    dfs(grid, vis, i, j,ans);
                    mans =
                        max(mans, ans); // Update the maximum area found so far
                }
            }
        }
        return mans;
    }
    ```
43. Rotteen oranges
    ```
      int dx[4] = {0, 0, 1, -1};
    int dy[4] = {1, -1, 0, 0};

    int orangesRotting(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();
        queue<vector<int>> q;
        vector<vector<int>> vis(n, vector<int>(m, 0));
        int days = 0;
        int count = 0;
        int r = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 2) {
                    q.push({i, j, days});
                    vis[i][j] = 1;
                }
                if (grid[i][j] == 1 || grid[i][j] == 2)
                    count++;
            }
        }
        while (!q.empty()) {
            auto it = q.front();
            r++;
            int i = it[0];
            int j = it[1];
            days = it[2];
            q.pop();
            for (int k = 0; k < 4; k++) {
                int nx = i + dx[k];
                int ny = j + dy[k];
                if (nx >= 0 & nx < n && ny >= 0 && ny < m &&
                    grid[nx][ny] == 1 && vis[nx][ny] == 0) {
                    vis[nx][ny] = 1;
                    q.push({nx, ny, days + 1});
                }
            }
        }

        if (count == r)
            return days;
        return -1;
    }
    ```
44. Making a large island 
    ```
      class DisjointSet {

    public:
    vector<int> rank, parent, size;
    DisjointSet(int n) {
        rank.resize(n + 1, 0);
        parent.resize(n + 1);
        size.resize(n + 1);
        for (int i = 0; i <= n; i++) {
            parent[i] = i;
            size[i] = 1;
        }
    }

    int findUPar(int node) {
        if (node == parent[node])
            return node;
        return parent[node] = findUPar(parent[node]);
    }

    void unionByRank(int u, int v) {
        int ulp_u = findUPar(u);
        int ulp_v = findUPar(v);
        if (ulp_u == ulp_v) return;
        if (rank[ulp_u] < rank[ulp_v]) {
            parent[ulp_u] = ulp_v;
        }
        else if (rank[ulp_v] < rank[ulp_u]) {
            parent[ulp_v] = ulp_u;
        }
        else {
            parent[ulp_v] = ulp_u;
            rank[ulp_u]++;
        }
    }

    void unionBySize(int u, int v) {
        int ulp_u = findUPar(u);
        int ulp_v = findUPar(v);
        if (ulp_u == ulp_v) return;
        if (size[ulp_u] < size[ulp_v]) {
            parent[ulp_u] = ulp_v;
            size[ulp_v] += size[ulp_u];
        }
        else {
            parent[ulp_v] = ulp_u;
            size[ulp_u] += size[ulp_v];
        }
    }
    };
    class Solution {
    public:
    bool isValid(int newr, int newc, int n) {
        return newr >= 0 && newr < n && newc >= 0 && newc < n;
    }

    int largestIsland(vector<vector<int>>& grid) {
        int n = grid.size();
        DisjointSet ds(n * n);
        // step - 1
        for (int row = 0; row < n; row++) {
            for (int col = 0; col < n; col++) {
                if (grid[row][col] == 0)
                    continue;
                int dr[] = {-1, 0, 1, 0};
                int dc[] = {0, -1, 0, 1};
                for (int ind = 0; ind < 4; ind++) {
                    int newr = row + dr[ind];
                    int newc = col + dc[ind];
                    if (isValid(newr, newc, n) && grid[newr][newc] == 1) {
                        int nodeNo = row * n + col;
                        int adjNodeNo = newr * n + newc;
                        ds.unionBySize(nodeNo, adjNodeNo);
                    }
                }
            }
        }
        // step 2
        int mx = 0;
        for (int row = 0; row < n; row++) {
            for (int col = 0; col < n; col++) {
                if (grid[row][col] == 1)
                    continue;
                int dr[] = {-1, 0, 1, 0};
                int dc[] = {0, -1, 0, 1};
                set<int> components;
                for (int ind = 0; ind < 4; ind++) {
                    int newr = row + dr[ind];
                    int newc = col + dc[ind];
                    if (isValid(newr, newc, n)) {
                        if (grid[newr][newc] == 1) {
                            components.insert(ds.findUPar(newr * n + newc));
                        }
                    }
                }
                int sizeTotal = 0;
                for (auto it : components) {
                    sizeTotal += ds.size[it];
                }
                mx = max(mx, sizeTotal + 1);
            }
        }
        for (int cellNo = 0; cellNo < n * n; cellNo++) {
            mx = max(mx, ds.size[ds.findUPar(cellNo)]);
        }
        return mx;
    }
    };
    ```
45. Min fuel to report to capital 
    ```
      long long ans;
    int dfs(vector<vector<int>>& v, int node, vector<int>& vis, int seats) {
        vis[node] = 1;
        long long cnt = 1;
        for (int child : v[node]) {
            if (vis[child] == 0)
                cnt +=dfs(v, child, vis, seats);
        }
        long long x = cnt / seats;
        if (cnt % seats)
            x++;
        if (node != 0)
            ans += x;
        return cnt;
    }
    long long minimumFuelCost(vector<vector<int>>& roads, int seats) {
        if (roads.size() == 0)
            return 0;
        int n = roads.size();
        ans = 0;
        vector<vector<int>> v(n + 1);
        for (int  i= 0; i < roads.size(); i++) {
            int x = roads[i][0];
            int y = roads[i][1];
            v[x].push_back(y);
            v[y].push_back(x);
        }
        vector<int> vis(n + 1, 0);
        dfs(v, 0, vis, seats);
        return ans;
    }
    ```
46. Delete tree nodes 
   ```
     vector<int> vis;
    int res = 0;

    int deleteTreeNodes(int nodes, vector<int>& parent, vector<int>& value) {
        vector<vector<int>> graph(nodes);
        vis.resize(nodes, 0);

        for (int i = 1; i < nodes; i++) {
            graph[parent[i]].push_back(i);
        }

        dfs(graph, 0, value);
        return nodes - res;
    }

    int dfs(vector<vector<int>>& graph, int node, vector<int>& value) {
        int sum = value[node];
        int subtreeCount = 1;

        for (auto& nei : graph[node]) {
            sum += dfs(graph, nei, value);
        }

        if (sum == 0) {
            res += subtreeCountNodes(graph, node);
        }
        return sum;
    }

    int subtreeCountNodes(vector<vector<int>>& graph, int node) {
        vis[node] = 1;
        int count = 1;
        for (auto& nei : graph[node]) {
            if (!vis[nei]) {
                count += subtreeCountNodes(graph, nei);
            }
        }
        return count;
    }
   ```
47. Longest path with different adjacent character 
    ```
      int result;

    int DFS(unordered_map<int, vector<int>>& adj, int curr, int parent,
            string& s) {

        int longest = 0;
        int second_longest = 0;

        for (int& child : adj[curr]) {
            if (child == parent)
                continue;

            int child_longest_length = DFS(adj, child, curr, s);

            if (s[child] == s[curr])
                continue;

            if (child_longest_length > second_longest)
                second_longest = child_longest_length;

            if (second_longest > longest)
                swap(longest, second_longest);
        }

        int koi_ek_acha =
            max(longest, second_longest) +
            1; // Why this 1 ? Because including the curr node itself

        int only_root_acha = 1; // only curr node is valid, rest children have
                                // duplicate character

        int neeche_hi_milgaya_answer = longest + second_longest + 1;

        result = max(
            {result, neeche_hi_milgaya_answer, koi_ek_acha, only_root_acha});

        return max(koi_ek_acha, only_root_acha);
    }

    int longestPath(vector<int>& parent, string s) {
        int n = parent.size();
        result = 0;
        unordered_map<int, vector<int>> adj;

        for (int i = 1; i < n; i++) {
            int u = i;
            int v = parent[i];

            adj[u].push_back(v);
            adj[v].push_back(u);
        }

        DFS(adj, 0, -1, s);

        return result;
    }
    ```
48. Battleships in a board 
    ```
      void dfs(vector<vector<char>>& board, int i, int j)
    {
        if (i<0 || i>=board.size() || j<0 || j>=board[0].size() ) 
        return;
        if (board[i][j]=='.') 
        return;
        board[i][j]='.';
        dfs(board, i-1, j);
        dfs(board, i+1, j);
        dfs(board, i, j-1);
        dfs(board, i, j+1);
    }
    
    public:
    int countBattleships(vector<vector<char>>& board) 
    {
        if (board.size()==0) 
        return 0;
        int count = 0;
        for (int i=0; i<board.size(); i++)
        {
            for (int j=0; j<board[0].size();j++)
            {
                if(board[i][j]=='X') 
                {
                    count++;
                    dfs(board, i, j);
                }
            }
        }
        return count;
    }
    ```
49. Cheapest flights with  k stop 
    ```
       vector<int> distance(n, INT_MAX);

        unordered_map<int, vector<pair<int, int>>> adj;

        for (vector<int>& vec : flights) {
            int u = vec[0];
            int v = vec[1];
            int cost = vec[2];

            adj[u].push_back({v, cost});
        }

        queue<pair<int, int>> que;
        que.push({src, 0});
        distance[src] = 0;

        int level = 0;

        while (!que.empty() && level <= k) {
            int N = que.size();

            while (N--) {
                int u = que.front().first;
                int d = que.front().second;
                que.pop();

                for (pair<int, int>& P : adj[u]) {

                    int v = P.first;
                    int cost = P.second;

                    if (distance[v] > d + cost) {
                        distance[v] = d + cost;
                        que.push({v, d + cost});
                    }
                }
            }
            level++;
        }

        return distance[dst] == INT_MAX ? -1 : distance[dst];
    }
    ```
50. Graph Valid Tree
    ```
      bool validTree(int n, vector<vector<int>>& edges) {
        if (edges.size() == 0 and n == 1) {
            return true;
        }
        vector<vector<int>> v(n);
        for (int i = 0; i < edges.size(); i++) {
            v[edges[i][0]].push_back(edges[i][1]);
            v[edges[i][1]].push_back(edges[i][0]);
        }
        vector<bool> visited(n, false);
        queue<int> q;
        q.push(0);
        visited[0] = true;
        vector<int> parent(n, -1);
        while (!q.empty()) {
            int temp = q.front();
            q.pop();
            for (auto item : v[temp]) {
                if (item == parent[temp]) {
                    continue;
                }
                if (visited[item]) {
                    return false;
                }
                parent[item] = temp;
                visited[item] = true;
                q.push(item);
            }
        }

        for (int i = 0; i < visited.size(); i++) {
            cout << i << endl;
            if (!visited[i])
                return false;
        }

        return true;
    }
    ```
51. 0 1 Matrix
     ```
       vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
        int n = mat.size(), m = mat[0].size();
        vector<vector<int>> mat1(n, vector<int>(m, 0));
        int vis[n][m];
        queue<pair<pair<int, int>, int>> q;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (mat[i][j] == 0) {
                    vis[i][j] = 1;
                    mat1[i][j] = 0;
                    q.push({{i, j}, 0});
                } else {
                    vis[i][j] = 0;
                }
            }
        }
        int dx[4] = {-1, 0, 1, 0};
        int dy[4] = {0, 1, 0, -1};
        while (!q.empty()) {
            int i1 = q.front().first.first;
            int j1 = q.front().first.second;
            int c = q.front().second;
            q.pop();
            for (int i = 0; i < 4; i++) {
                int n1 = i1 + dx[i];
                int m1 = j1 + dy[i];
                if (n1 < n && n1 >= 0 && m1 < m && m1 >= 0 &&
                    vis[n1][m1] == 0) {
                    vis[n1][m1] = 1;
                    mat1[n1][m1] = c + 1;
                    q.push({{n1, m1}, c + 1});
                }
            }
        }
        return mat1;
    }
     ```
52. Surrounded regions
    ```
      int n;
    int m;
    void dfs(int row, int col, vector<vector<char>>& board,
             vector<vector<int>>& vis, int delrow[], int delcol[]) {
        vis[row][col] = 1;
        int n = board.size();
        int m = board[0].size();
        for (int i = 0; i < 4; i++) {
            int nr = row + delrow[i];
            int nc = col + delcol[i];
            if (nr >= 0 && nc >= 0 && nr < n && nc < m &&
                board[nr][nc] == 'O' && vis[nr][nc] != 1) {
                dfs(nr, nc, board, vis, delrow, delcol);
            }
        }
    }
    void solve(vector<vector<char>>& board) {
        int n = board.size();
        int m = board[0].size();
        int delrow[] = {-1, 0, 1, 0};
        int delcol[] = {0, 1, 0, -1};
        vector<vector<int>> vis(n, vector<int>(m, 0));
        for (int i = 0; i < m; i++) {
            if (board[0][i] == 'O' && vis[0][i] != 1) {
                dfs(0, i, board, vis, delrow, delcol);
            }
            if (board[n - 1][i] == 'O' && vis[n - 1][i] != 1) {
                dfs(n - 1, i, board, vis, delrow, delcol);
            }
        }
        for (int i = 0; i < n; i++) {
            if (board[i][0] == 'O' && vis[i][0] != 1) {
                dfs(i, 0, board, vis, delrow, delcol);
            }
            if (board[i][m - 1] == 'O' && vis[i][m - 1] != 1) {
                dfs(i, m - 1, board, vis, delrow, delcol);
            }
        }
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (board[i][j] == 'O' && vis[i][j] != 1) {
                    board[i][j] = 'X';
                }
            }
        }
    }
    ```
53. Number of provinces
    ```
      void dfs(int m, int n, vector<vector<int>>& Connected, vector<bool>&visited){
        visited[m] = true;
        vector<int>adj;
        for(int i=0; i<n; i++ ){
            int x =  Connected[m][i]; 
            if(x == 1)
                adj.push_back(i);
        }
        for(auto x: adj){
            if(!visited[x]){
                dfs(x, n, Connected, visited);
            }
        }
        
      }   
    int findCircleNum(vector<vector<int>>& Connected) {
         int n=Connected.size();
         vector<bool> visited(n,0);
         int ans=0;
         for(int i=0;i<Connected.size();i++){
             if(!visited[i]){
                 ans++;
                 dfs(i,n,Connected,visited);
             }
         }
        return ans;
        
    }
    ```
54. No of closed islands
    ```
      void dfs(int row, int col, vector<vector<int>>& grid,
             vector<vector<int>>& vis, int delrow[], int delcol[]) {
        vis[row][col] = 1;
        grid[row][col] = 1;
        int n = grid.size();
        int m = grid[0].size();
        for (int i = 0; i < 4; i++) {
            int nr = row + delrow[i];
            int nc = col + delcol[i];
            if (nr >= 0 && nc >= 0 && nr < n && nc < m && grid[nr][nc] == 0 &&
                vis[nr][nc] != 1) {
                dfs(nr, nc, grid, vis, delrow, delcol);
            }
        }
    }
    int closedIsland(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();
        int delrow[] = {-1, 0, 1, 0};
        int delcol[] = {0, 1, 0, -1};
        vector<vector<int>> vis(n, vector<int>(m, 0));
        for (int i = 0; i < m; i++) {
            if (grid[0][i] == 0 && vis[0][i] != 1) {
                dfs(0, i, grid, vis, delrow, delcol);
            }
            if (grid[n - 1][i] == 0 && vis[n - 1][i] != 1) {
                dfs(n - 1, i, grid, vis, delrow, delcol);
            }
        }
        for (int i = 0; i < n; i++) {
            if (grid[i][0] == 0 && vis[i][0] != 1) {
                dfs(i, 0, grid, vis, delrow, delcol);
            }
            if (grid[i][m - 1] == 0 && vis[i][m - 1] != 1) {
                dfs(i, m - 1, grid, vis, delrow, delcol);
            }
        }
        int ans = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 0 && vis[i][j] != 1) {
                    ans++;
                    dfs(i, j, grid, vis, delrow, delcol);
                }
            }
        }
        return ans;
    }
    ```
    55. Course schedules II
      ```
        vector<int> findOrder(int n, vector<vector<int>>& prerequisites) 
    {
        vector<int> adj[n];
        vector<int> indegree(n,0);
        for (int i=0; i<prerequisites.size(); i++){
            adj[prerequisites[i][1]].push_back(prerequisites[i][0]);
            indegree[prerequisites[i][0]]++;
        }
        vector<int> topo;
        queue<int> q;
       
        
        for (int i=0; i<n; i++){
            if (indegree[i]==0)q.push(i);
        }
        while (!q.empty()){
            int fr= q.front();
            q.pop();
            topo.push_back(fr);
            for (auto val: adj[fr]){
                indegree[val]--;
                if (indegree[val]==0)q.push(val);
            }
        }
        cout<<topo.size()<<endl;
        if (topo.size()==n)return topo;
        return {};
       

    }
      ```
