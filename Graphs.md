# Graphs 

## Theory 
https://www.youtube.com/watch?v=s7zE4Nmc2Fg&list=PL5DyztRVgtRVLwNWS7Rpp4qzVVHJalt22&index=1&t=24s

## Questions

### DFS

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

4. https://www.interviewbit.com/problems/valid-path/
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

