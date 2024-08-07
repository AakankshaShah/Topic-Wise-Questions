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
       queue<pair<string,int>>q;
    q.push({A,1});
    unordered_set<string> st(C.begin(), C.end());
    st.erase(A);
    while(!q.empty())
    {
        string word=q.front().first;
        int step=q.front().second;
        q.pop();
        if(word==B)
        return step;
        
        for(int i=0;i<word.size();i++)
        {
            char org=word[i];
            
            
            for(char k='a';k<='z';k++)
            {
                word[i]=k;
                
                if (st.find(word) != st.end())
                    {
                        st.erase(word);
                        q.push({word, step + 1});
                    }
            }    
                word[i] = org;
          }
        
        
        }
        return 0;

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
