# Arrays

## Questions

1. Container with  most water

    ```
      int maxArea(vector<int>& height) {
        int i=0;
        int j=height.size()-1;
        int ans=0;

        while(i<j)
        {
            
            int w=j-i;
            int h=min(height[i],height[j]);
            ans=max(ans,w*h);
            if(height[j]>height[i])
            i++;
            else
            j--;
        }
        return ans;
        
      }
    ```
2. 3sum
    ```
      sort(nums.begin(), nums.end());
        vector<vector<int>> ans;

        int n = nums.size();

        for (int i = 0; i < n - 2; ++i) {
            // Skip duplicate values for i
            if (i > 0 && nums[i] == nums[i - 1])
                continue;

            int j = i + 1;
            int k = n - 1;

            while (j < k) {
                int sum = nums[i] + nums[j] + nums[k];
                if (sum == 0) {
                    ans.push_back({nums[i], nums[j], nums[k]});
                    // Skip duplicate values for j
                    while (j < k && nums[j] == nums[j + 1])
                        ++j;
                    // Skip duplicate values for k
                    while (j < k && nums[k] == nums[k - 1])
                        --k;
                    ++j;
                    --k;
                } else if (sum > 0) {
                    --k;
                } else {
                    ++j;
                }
            }
        }

        return ans;
    ```
3. Closet 3 sum
    ```
      sort(nums.begin(), nums.end());
        
        
        int closetSum=100000;

        int n = nums.size();

        for (int i = 0; i < n - 2; ++i) {
            

            int j = i + 1;
            int k = n - 1;

            while (j < k) {
                int sum = nums[i] + nums[j] + nums[k];

               if(abs(target-sum)<abs(target-closetSum))
               closetSum=sum;

               if(sum<target)
               j++;
               else
               k--;
                
            }
        }

        return closetSum;
    ```
4. 4 sum 

     ```
       sort(nums.begin(), nums.end());
        vector<vector<int>> ans;
        int n = nums.size();
        for (int i = 0; i < n - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            for (int j = i + 1; j < n - 2; j++) {
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }

                long long  tSum = (long long)target - (nums[i] + nums[j]);
                int l = j + 1, h = n - 1;
                while (l < h) {
                    if (nums[l] + nums[h] == tSum) {
                        vector<int> oneAns = {nums[i], nums[j], nums[l],
                                              nums[h]};
                        ans.push_back(oneAns);
                        while (l < h && nums[l] == nums[l + 1]) {
                            l++;
                        }
                        while (l < h && nums[h] == nums[h - 1]) {
                            h--;
                        }
                        l++;
                        h--;
                    } else if (nums[l] + nums[h] < tSum) {
                        l++;
                    } else {
                        h--;
                    }
                }
            }
        }

        return ans;
    }
    ```
5. Next permutation

     ```
      int ind = -1;
        for (int i = nums.size() - 1; i > 0; i--) {
            if (nums[i] > nums[i - 1]) {
                ind = i - 1;
                break;
            }
        }

        if (ind != -1) {
            int swap_index = ind;
            for (int j = nums.size() - 1; j > ind; j--) {
                if (nums[j] > nums[ind]) {
                    swap_index = j;
                    break;
                }
            }
            swap(nums[swap_index], nums[ind]);
        }
        reverse(nums.begin() + ind + 1, nums.end());
     ```
6. Valid Sudoku

     ``` 
       //Approach 1
       bool isvalidBox(vector<vector<char>>& board, int sr, int er, int sc, int ec) {
        unordered_set<char> s;
        for (int i = sr; i < er; i++) {
            for (int j = sc; j < ec; j++) {
                if (board[i][j] != '.' && s.find(board[i][j]) != s.end())
                    return false;
                s.insert(board[i][j]);
            }
        }
        return true;
    }
    
    bool isValidSudoku(vector<vector<char>>& board) {
        // Row validate
        for (int i = 0; i < 9; i++) {
            unordered_set<char> s;
            for (int j = 0; j < 9; j++) {
                if (board[i][j] != '.' && s.find(board[i][j]) != s.end())
                    return false;
                s.insert(board[i][j]);
            }
        }
        
        // Column validate
        for (int i = 0; i < 9; i++) {
            unordered_set<char> s;
            for (int j = 0; j < 9; j++) {
                if (board[j][i] != '.' && s.find(board[j][i]) != s.end())
                    return false;
                s.insert(board[j][i]);
            }
        }
        
        // Sub square validate
        for (int sr = 0; sr < 9; sr += 3) {
            for (int sc = 0; sc < 9; sc += 3) {
                if (!isvalidBox(board, sr, sr + 3, sc, sc + 3))
                    return false;
            }
        }
        
        return true;
    }
    
    
    // Approach 2
      bool isValidSudoku(vector<vector<char>>& board) {
        unordered_set<string> st;
        
        for(int i = 0; i<9; i++) {
            for(int j = 0; j<9; j++) {
                if(board[i][j] == '.') continue;
                
                string row = string(1, board[i][j]) + "_row_" + to_string(i);
                string col = string(1, board[i][j]) + "_col_" + to_string(j);
                string box = string(1, board[i][j]) + "_box_" + to_string(i/3) + "_" + to_string(j/3);
                if(st.count(row) || st.count(col) || st.count(box)) return false;
                st.insert(row);
                st.insert(col);
                st.insert(box);
            }
        }
        
        return true;
    }
    
7. Sudoko solver [Backtracking]
    ```
      bool isSafe(int i, int j, char val, vector<vector<char>>& board) {
        for (int k = 0; k < 9; k++) {
            if (board[i][k] == val)
                return false;
            if (board[k][j] == val)
                return false;
        }
        int r = i - i % 3;
        int c = j - j % 3;

        for (int m = r; m < r + 3; m++) {
            for (int n = c; n < c + 3; n++) {
                if (board[m][n] == val)
                    return false;
            }
        }
        return true;
    }

    bool solve(vector<vector<char>>& board) {
        int n = board.size();
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == '.') {
                    // insert
                    for (char value = '1'; value <= '9'; value++) {
                        // checking for safety of row,col and 3*3 box
                        if (isSafe(i, j, value, board)) {
                            board[i][j] = value;
                            // baaki recursion sambhaal leha
                            bool aageKaSolution = solve(board);
                            if (aageKaSolution == true)
                                return true;
                            // else backtracking
                            board[i][j] = '.';
                        }
                    }
                    return false;
                }
            }
        }
        return true;
    }
    void solveSudoku(vector<vector<char>>& board) { solve(board); }
    ```
8. Combination Sum I
    ```
       void findCombination(vector<int>& candidates, int ind, int target,
                         vector<int>& ds, vector<vector<int>>& ans) {
        if (ind == candidates.size()) {
            if (target == 0) {
                ans.push_back(ds);
            }
            return;
        }
        if (candidates[ind] <= target) {
            ds.push_back(candidates[ind]);
            findCombination(candidates, ind, target - candidates[ind], ds, ans);
            ds.pop_back();
        }
        findCombination(candidates, ind + 1, target, ds, ans);
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> ans;
        vector<int> ds;

        findCombination(candidates, 0, target, ds,
                        ans); // Corrected function name
        return ans;
    }
    ```
9. Combination Sum 2 
     ```
     void findCombination(int index, int target, vector<int>& ds,
                         vector<vector<int>>& ans, vector<int> arr, int n) {

        if (target == 0) {

            ans.push_back(ds);
            return;
        }

        for (int i = index; i < n; i++) {

            if ((i > index) and (arr[i] == arr[i - 1])) {
                continue;
            }

            if (arr[i] > target) {
                break;
            }
            ds.push_back(arr[i]);

            findCombination(i + 1, target - arr[i], ds, ans, arr, n);

            ds.pop_back();
        }
    }

    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {

        int n = candidates.size();
        vector<vector<int>> ans;

        vector<int> ds;

        sort(candidates.begin(), candidates.end());

        findCombination(0, target, ds, ans, candidates, n);

        return ans;
    }
     ```
10. Misisng positive number 

     ```
      int n = A.size();

        for (int i = 0; i < n; i++) {
            int a = A[i];

            if (a >= 1 && a <= n) {
                int pos = a - 1;
                if (A[pos] != A[i]) {
                    swap(A[pos], A[i]);
                    i--;
                }
            }
        }
        for (int i = 0; i < n; i++) {
            if (i + 1 != A[i])
                return i + 1;
        }

        return n + 1;
   
     ```
11. Jump Game II https://www.youtube.com/watch?v=wLPdkLM_BWo
     
    ```
      int jumps=0;
        int curr=0;
        int far=0;

        for(int i=0;i<nums.size()-1;i++)
        {
            far=max(far,nums[i]+i);
            if(i==curr)
            {
                curr=far;
                jumps++;
            }
        }
        return jumps;

    ```
12. Permutations

     ```
      void recur(vector<vector<int>>& ans, vector<int>& ds, int freq[], vector<int>& nums) {
    if (ds.size() == nums.size()) {
        ans.push_back(ds);
        return;
    }

    for (int i = 0; i < nums.size(); i++) {
        if (freq[i] == 0) {
            freq[i]++;
            ds.push_back(nums[i]);
            recur(ans, ds, freq, nums);
            freq[i]--;
            ds.pop_back();
        }
    }
    }

    vector<vector<int>> permute(vector<int>& nums) {
    vector<vector<int>> ans;
    vector<int> ds;
    int freq[nums.size()];
    memset(freq,0,sizeof(freq));
     recur(ans, ds, freq, nums);
    return ans; 
     }
       
     ```
13. Permutation II

        ```
         private: 
         void permuteUnique(vector<int>& nums, vector<vector<int>>& output, vector<int> temp, vector<bool>& visited){
        if(temp.size() == nums.size()){
            output.push_back(temp);
            return;
        }
        for(int i=0; i<nums.size(); i++){
            if(visited[i] || (i>0 && nums[i] == nums[i-1] && !visited[i-1])) continue;
            visited[i] = true;
            temp.push_back(nums[i]);
            permuteUnique(nums, output, temp, visited);
            temp.pop_back();
            visited[i] = false;
        }
        }
         public:
        vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> output;
        vector<int> temp;
        vector<bool> visited(nums.size(), 0); 
        permuteUnique(nums, output, temp, visited);
        return output;
        }      
        ```
14. Rotate array - Transpose and reverse row
     ```
      void rotate(vector<vector<int>>& matrix) {
        int row = matrix.size();
        for(int i=0;i<row; i++){
            for(int j=0; j<=i;j++){
                swap(matrix[i][j], matrix[j][i]);
            }
        }
        for(int i=0;i<row;i++){
            reverse(matrix[i].begin(), matrix[i].end());
        }
    }
     ```
   15. Group Anagrams 
       ```
          string generate(string& s) {
        vector<int> count(26, 0);
        for (char c : s) {
            count[c - 'a']++;
        }

        string ss;
        for (int i = 0; i < 26; i++) {
            if (count[i] != 0) {
                ss += string(count[i], 'a' + i);
            }
        }
        return ss;
    }
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> mp;
        vector<vector<string>> ans;

        for (int i = 0; i < strs.size(); i++) {
            string temp = strs[i];
            string new_word = generate(temp);
            mp[new_word].push_back(temp);
        }
        for (auto it : mp) {
            ans.push_back(it.second);
        }
        return ans;
    }
       ```
16. N - Queens O(n!)
    ```
      bool isSafe(vector<string>& board, int row, int col, int n) {
        int i = row;
        int j = col;

        while (row >= 0 && col >= 0) {
            if (board[row][col] == 'Q')
                return false;
            row--;
            col--;
        }
        col = j;
        row = i;

        while (col >= 0) {
            if (board[row][col] == 'Q')
                return false;
            col--;
        }
        col = j;
        while (col >= 0 && row < n) {
            if (board[row][col] == 'Q')
                return false;
            col--;
            row++;
        }

        return true;
    }

    void solve(vector<string>& board, int col, int n,
               vector<vector<string>>& ans) {
        if (col == n) {
            ans.push_back(board);
            return;
        }
        for (int row = 0; row < n; row++) {
            if (isSafe(board, row, col, n)) {
                board[row][col] = 'Q';
                solve(board, col + 1, n, ans); // Corrected the recursive call
                board[row][col] = '.';
            }
        }
    }

    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> ans;
        vector<string> board(n);
        string s(n, '.');
        for (int i = 0; i < n; i++)
            board[i] = s;

        solve(board, 0, n, ans);
        return ans;
    }
    ```




17. Circular tour
18.  Maximum non negative subarray sum

     ```
     vector<int> result;
    long long sum = 0;
    int count = 0;
    int maxcount = 0;
    long long  maxsum = 0;
    int index = 0;
    for(int i = 0; i < A.size(); i++)
    {
        if(A[i] >= 0)
        {
            count++;
            sum = sum + A[i];
        }
        else
        {
            count = 0;
            sum = 0;
        }
        if(sum >= maxsum && maxsum != 0 || count >= maxcount && sum >= maxsum)
        {
            maxcount = count;
            maxsum = sum;
            index = i;
        }
    }

    for(int i = index - maxcount+1; i <=index ; i++)
    {
        result.push_back(A[i]);
    }
    return result;
    
     }

     ```

   19. Factorial of a large number https://www.youtube.com/watch?v=O3fwYjcMV_M

       ```
        void multiplier( vector<int> &arr, int &size,int fact)
         { void carry=0;
         for (int i=0;i<size;i++)
         {
        int res=arr[i]*fact;
        res=res+carry;
        arr[i]=res%10;
        carry=res/10;
         }
    
          while(carry>0)
         {
           arr[size++]=carry%10;
          carry=carry/10;
         }
    
        }
       vector<int> factorial(int N){
    
        vector<int> arr(10000,0);
        vector<int> res;
        arr[0] =1;
        int size=1;
        
        
        for(int fact=2;fact<=N;fact++)
        {
            multiplier(arr,size,fact);
        }
        
        
        for(int i=size-1;i>=0;i--)
        {
            res.push_back(arr[i]);
        }
        return res;

        
      }
      ```
  20. Spiral order matrix traversal 

   ```
    int n=A.size();
    int m=A[0].size();
    int left=0;
    int right=m-1;
    int top=0;
    int bottom=n-1;
    vector<int>arr;
    
    while(left<=right&&top<=bottom)
    {
        for(int i=left;i<=right;i++)
        arr.push_back(A[top][i]);
        top++;
        
        for(int i=top;i<=bottom;i++)
        arr.push_back(A[i][right]);
        right--;
        if(top<=bottom)
        {
        for(int i=right;i>=left;i--)
        arr.push_back(A[bottom][i]);
        bottom--;
        }
        if(left<=right)
       {
        for(int i=bottom;i>=top;i--)
        arr.push_back(A[i][left]);
        left++;
       } 
        
        
    }
    return arr;
   ```
   Part II
 ```
   int r1 = 0; int r2 = n-1; int c1 = 0; int c2 = n-1;
    vector<vector<int>> v(n, vector<int> (n,0));
    int val = 1;
   
    while(r1 <= r2 && c1 <= c2)
    {
        //left to right
        for(int c= c1; c <= c2; c++) v[r1][c] = val++;
        //up to down
        for(int r = r1+1; r <= r2; r++) v[r][c2] = val++;
       
        if(r1 < r2 && c1 < c2){
            //right to left
            for(int c= c2-1; c >= c1; c--) v[r2][c] = val++;
            //bottom to top
            for(int r = r2-1; r > r1; r--) v[r][c1] = val++;
        }
        r1++;
        r2--;
        c1++;
        c2--;
    }
    return v;
 ```
    
 21. Pick from both sides! https://www.youtube.com/watch?v=XJZczN4wts0 Another approach 
       ```
         int left_sum[B + 1], right_sum[B + 1], max, i;
        int n = A.size();

        left_sum[0] = right_sum[0] = 0;

        for (i = 1; i < B + 1; ++i)

        {

            left_sum[i] = left_sum[i - 1] + A[i - 1];

            right_sum[i] = right_sum[i - 1] + A[n - i];
        }

        max = left_sum[0] + right_sum[B];

        for (i = 0; i < B + 1; ++i)

        {

            if (left_sum[i] + right_sum[B - i] > max)

            {

                max = left_sum[i] + right_sum[B - i];
            }
        }
       return max;

       ```
       
22. Min steps in infinite grid 

     ```
      int dist= 0;
    for(int i = 0; i<A.size()-1; i++){
        int a = abs(A[i+1] - A[i]);
        int b = abs(B[i+1] - B[i]);
        if(a>=b) dist += a;
        else dist+= b;
    }
    return dist;

    ```

23. Min lights to activate 

    ```
      int ans = 0;
    int n = A.size();
    for(int i=0;i<A.size();){
        int r = min(i+B-1, n-1), l = max(i-B+1, 0);
        bool canLight = false;
        while(r >= l){
            if(A[r] == 1){
                canLight = true;
                break;
            }
            r--;
        }
        if(canLight == false)
            return -1;
        ans++;
        i = r+B;// r is last position so for r+1 farthest is i+B-1 = r+1+B-1 hence r+B
    }
    return ans;
    ```

24. Maximum sum triplet

    ```
     vector<int> suffix(A.size());
        int n = A.size();

        suffix[n - 1] = A[n - 1];

        for (int i = A.size() - 2; i >= 0; i--) {

            suffix[i] = max(suffix[i + 1], A[i]);
        }

        set<int> left;

        int maxSum = 0, sum;

        left.insert(A[0]);

        for (int i = 1; i < A.size(); i++) {

            left.insert(A[i]);

            auto it = left.find(A[i]);

            if (it != left.begin() && suffix[i] != A[i])

                sum = (*--it) + A[i] + suffix[i];

            maxSum = max(sum, maxSum);
        }

        return maxSum;
    

    ```

25. Add 1 to number 

   ```
     int carry =1;
    vector<int>ans;
    
    for(int i=A.size()-1;i>=0;i--)
    {
        int res=A[i]+carry;
        ans.push_back(res%10);
        carry=res/10;
    }
    while(carry>0)
    {
        ans.push_back(carry%10);
        carry=carry/10;
    }
    int i=ans.size()-1;
    while(ans[i]==0) {
        ans.pop_back();
        i--;
    }
    
    reverse(ans.begin(),ans.end());
    return ans;
   ```


26. Max absolute difference https://www.youtube.com/watch?v=KqDIeQ9S5Vc

    ```
      int val1=INT_MAX;
    int val2=INT_MAX;
    int val3=INT_MIN;
    int val4=INT_MIN;
    int ans=0;
    
    
    for(int i=0;i<A.size();i++)
    {
        val1=min(val1,A[i]-i);
        val2=min(val2,A[i]+i);
        val3=max(val3,A[i]-i);
        val4=max(val4,A[i]+i);
        
        ans=max(val4-val2,val3-val1);
    }
    
    return ans;
      
    ```

27. Find duplicate in an array [Hare & tortoise method] - Something is duplicating & array values can act as valid indices
    ```
      int slow = nums[0];
        int fast = nums[0];

        slow = nums[slow];
        fast = nums[nums[fast]];
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[nums[fast]];
        }

        slow = nums[0];
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }

        return slow;
    ```

28. Partitions
     ```
       if (A < 3)
            return 0;

        int totalSum = 0, sum = 0, ans = 0;

        for (int num : B)
            totalSum += num;

        if (totalSum % 3 != 0)
            return 0;

        int x = totalSum / 3;

        int twiceX = x * 2;

        int xCount = 0;

        for (int idx = 0; idx < A - 1; idx++) {

            sum += B[idx];

            if (sum == twiceX)
                ans += xCount;

            if (sum == x)
                xCount++;
        }

        return ans;
     ```



29. Missing & repeat number
    ```
      long long n = a.size(); // size of the array

    // Find Sn and S2n:
    long long SN = (n * (n + 1)) / 2;
    long long S2N = (n * (n + 1) * (2 * n + 1)) / 6;

    // Calculate S and S2:
    long long S = 0, S2 = 0;
    for (int i = 0; i < n; i++) {
        S += a[i];
        S2 += (long long)a[i] * (long long)a[i];
    }

    //S-Sn = X-Y:
    long long val1 = S - SN;

    // S2-S2n = X^2-Y^2:
    long long val2 = S2 - S2N;

    //Find X+Y = (X^2-Y^2)/(X-Y):
    val2 = val2 / val1;

    //Find X and Y: X = ((X+Y)+(X-Y))/2 and Y = X-(X-Y),
    // Here, X-Y = val1 and X+Y = val2:
    long long x = (val1 + val2) / 2;
    long long y = x - val1;

    return {(int)x, (int)y};
    ```

    ```
      // xor method
        
    int n = a.size(); // size of the array

    int xr = 0;

    //Step 1: Find XOR of all elements:
    for (int i = 0; i < n; i++) {
        xr = xr ^ a[i];
        xr = xr ^ (i + 1);
    }

    //Step 2: Find the differentiating bit number:
    int number = (xr & ~(xr - 1));

    //Step 3: Group the numbers:
    int zero = 0;
    int one = 0;
    for (int i = 0; i < n; i++) {
        //part of 1 group:
        if ((a[i] & number) != 0) {
            one = one ^ a[i];
        }
        //part of 0 group:
        else {
            zero = zero ^ a[i];
        }
    }

    for (int i = 1; i <= n; i++) {
        //part of 1 group:
        if ((i & number) != 0) {
            one = one ^ i;
        }
        //part of 0 group:
        else {
            zero = zero ^ i;
        }
    }

    // Last step: Identify the numbers:
    int cnt = 0;
    for (int i = 0; i < n; i++) {
        if (a[i] == zero) cnt++;
    }

    if (cnt == 2) return {zero, one};
    return {one, zero};
    }




    ```

30. Max area of triangle 
     ```
       int Solution::solve(vector<string>& A) {
    int rows = A.size(), cols = A[0].size();
    map<char, int> maxRow[cols], minRow[cols], maxCol, minCol;

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            maxRow[j][A[i][j]] = max(maxRow[j][A[i][j]], i);
            if (minRow[j].count(A[i][j])) {
                minRow[j][A[i][j]] = min(minRow[j][A[i][j]], i);
            } else {
                minRow[j][A[i][j]] = i;
            }
            maxCol[A[i][j]] = max(maxCol[A[i][j]], j);
            if (minCol.count(A[i][j])) {
                minCol[A[i][j]] = min(minCol[A[i][j]], j);
            } else {
                minCol[A[i][j]] = j;
            }
        }
    }
    int ans = 0;
    for (int col = 0; col < cols;
         col++) // let's check that in which column, the base of triangle can
                // lie so as to maximise the area
    {
        int maxBase, maxHeight;

        if (maxRow[col].count('r') and maxRow[col].count('g') and
            maxCol.count('b')) {
            maxBase = max(abs(maxRow[col]['r'] - minRow[col]['g']),
                          abs(minRow[col]['r'] - maxRow[col]['g'])) +
                      1;
            maxHeight = max(abs(col - maxCol['b']), abs(col - minCol['b'])) + 1;
            ans = max(ans, maxBase * maxHeight);
        }

        if (maxRow[col].count('g') and maxRow[col].count('b') and
            maxCol.count('r')) {
            maxBase = max(abs(maxRow[col]['g'] - minRow[col]['b']),
                          abs(minRow[col]['g'] - maxRow[col]['b'])) +
                      1;
            maxHeight = max(abs(col - maxCol['r']), abs(col - minCol['r'])) + 1;
            ans = max(ans, maxBase * maxHeight);
        }

        if (maxRow[col].count('b') and maxRow[col].count('r') and
            maxCol.count('g')) {
            maxBase = max(abs(maxRow[col]['b'] - minRow[col]['r']),
                          abs(minRow[col]['b'] - maxRow[col]['r'])) +
                      1;
            maxHeight = max(abs(col - maxCol['g']), abs(col - minCol['g'])) + 1;
            ans = max(ans, maxBase * maxHeight);
        }
    }
    return ceil(ans / 2.0);
    }
     ```

31. Set intersecrtion

     ```
           static bool cmp(vector<int>& a, vector<int>& b) {
        if (a[1] != b[1]) {
            // Sort by endpoint in ascending order
            return a[1] < b[1];
        }
        // If endpoints are equal, sort by startpoint in descending order
        return a[0] > b[0];
        }
        int intersectionSizeTwo(vector<vector<int>>& intervals) {
        int n = intervals.size();
        int res = 2; // minimum 2 sized SET should be there

        sort(intervals.begin(), intervals.end(), cmp);
        // Ex: [[1,3],[3,7],[5,7],[7,8]]
        // After sort : [[1,3],[5,7],[3,7],[7,8]]
        int highestValInSet = intervals[0][1]; // max value of first interval
        int secondHighestValueInSet =
            intervals[0][1] - 1; // second max value from first interval

        for (int i = 1; i < n; i++) {
            int start = intervals[i][0], end = intervals[i][1];

            if (start >
                highestValInSet) { // means ther is no common between curr
                                   // interval and intervals before this
                // add 2 new values
                // end - 1 first and then end to make the SET SORTED
                res += 2;
                highestValInSet = intervals[i][1];
                secondHighestValueInSet = intervals[i][1] - 1;
            } else if (start > secondHighestValueInSet &&
                       start <= highestValInSet) { // atleast 1 value from
                                                   // current interval matches
                                                   // with previosu sets
                // just add 1 max value
                res += 1;
                secondHighestValueInSet =
                    highestValInSet; // now second max becomes previous max
                highestValInSet =
                    intervals[i][1]; // new max for current interval
            }
        }

        return res;
      ```

32. Max unsorted array 

     ```
       int n = arr.size();
    int l = -1, r = -1;
    for (int i = 0; i < n - 1; i++) {
        if (arr[i] > arr[i + 1]) {
            l = i;
            break;
        }
    }
    if (l == -1) {
        return {-1};
    }
    for (int i = n - 1; i > 0; i--) {
        if (arr[i] < arr[i - 1]) {
            r = i;
            break;
        }
    }
    int mini = 1e9;
    int maxi = -1e9;
    for (int i = l; i <= r; i++) {
        mini = min(arr[i], mini);
        maxi = max(maxi, arr[i]);
    }
    for (int i = 0; i < l; i++) {
        if (arr[i] > mini) {
            l = i;
            break;
        }
    }

    for (int i = n - 1; i > r; i--) {
        if (arr[i] < maxi) {
            r = i;
            break;
        }
    }
    return {l, r};

     ```
33. Merge intervals

    ```
     bool static cmp(Interval i1, Interval i2) { return i2.start > i1.start; }

     vector<Interval> Solution::insert(vector<Interval>& intervals,
                                  Interval newInterval) {

    intervals.push_back(newInterval);

    sort(intervals.begin(), intervals.end(), cmp);

    vector<Interval> ans;

    ans.push_back(intervals[0]);

    for (int i = 1; i < intervals.size(); i++) {

        if (ans.back().end >= intervals[i].start) {

            ans.back().end = max(ans.back().end, intervals[i].end);

        }

        else
            ans.push_back(intervals[i]);
    }

    return ans;
    }

    ```
34. Set matrix zero https://www.youtube.com/watch?v=coOCVuBx7YA
    
```
int row = A.size();
int col = A[0].size();
int x = 0;
int y = 0;
for (int i = 0; i < col; i++) {
    if (A[0][i] == 0) {
        x = 1;
    }
}
for (int i = 0; i < row; i++) {
    if (A[i][0] == 0) {
        y = 1;
    }
}
for (int i = 1; i < row; i++) {
    for (int j = 1; j < col; j++) {
        if (A[i][j] == 0) {
            A[0][j] = 0;
            A[i][0] = 0;
        }
    }
}
for (int i = 1; i < col; i++) {
    if (A[0][i] == 0) {
        for (int j = 0; j < row; j++) {
            A[j][i] = 0;
        }
    }
}
for (int i = 0; i < row; i++) {
    if (A[i][0] == 0) {
        for (int j = 0; j < col; j++) {
            A[i][j] = 0;
        }
    }
}
if (x == 1) {
    for (int i = 0; i < col; i++) {
        A[0][i] = 0;
    }
}
if (y == 1) {
    for (int i = 0; i < row; i++) {
        A[i][0] = 0;
    }
}
```
35. Make array elements equal 
    
```
   // Method 1
sort(begin(A), end(A));
int n = A.size();

for (int i = 1; i < n; i++) {
    if ((A[i] - A[i - 1]) % B != 0)
        return 0;
}
if ((A[n - 1] - A[0]) > 2 * B)
    return 0;

return 1;

// Method 2

int max = A[0];
int min = A[0];
for (int i = 0; i < A.length; i++) {
    if (A[i] > max)
        max = A[i];
    if (A[i] < min)
        min = A[i];
}
return max - min == B * 2 ? 1 : 0;

 ```
36. Max sum square submatrix https://www.youtube.com/watch?v=WxjYE4_agbo

    ``` 
    //Brute Method
      int row = A.size();
     int col = A[0].size();
     int max_sum = INT_MIN;

    for (int i = 0; i < row; i++) {
    for (int j = 0; j < col; j++) {
        if (i + B <= row && j + B <= col) {
            int sum = 0;
            for (int k = i; k < i + B; k++)

            {
                for (int l = j; l < j + B; l++) {
                    sum += A[k][l];
                }
            }
            max_sum = max(max_sum, sum);
        }
    }
    }
    return max_sum;

    
    ```
    ```
    int dp[row + 1][col + 1];

    for (int i = 1; i <= row; i++) {
        for (int j = 1; j <= col; j++) {
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1] - dp[i - 1][j - 1] +
                       A[i - 1][j - 1];
        }
    }

    for (int i = 1; i <= row; i++) {
        int sum = 0;
        for (int j = 1; j <= col; j++) {
            if (i - B >= 0 && j - B >= 0) {
                sum = dp[i][j] - dp[i - B][j] - dp[i][j - B] + dp[i - B][j - B];
                max_sum = max(max_sum, sum);
            }
        }
    }
    return max_sum;

    ```
37. Peak element
    ```
     int maxi = A[0];
    vector<bool> greater(n, true);
    for (int p = 1; p < n - 1; p++) {
        if (A[p] <= maxi)
            greater[p] = false;
        else
            maxi = A[p];
    }

    int mini = A[n - 1];
    for (int p = n - 2; p > 0; p--) {
        if (A[p] < mini) {
            if (greater[p])
                return 1;
            mini = A[p];
        }
    }

    return 0;
    ```
    ```
      vector<int> greater(n,arr[n-1]);
    for(int i=n-2; i>=0; i--)
        greater[i] = min(greater[i+1], arr[i]);
    for(int i=1; i<n-1; i++) {
        if(arr[i] >= arr[i-1] && arr[i] <= greater[i+1])
            return arr[i];
        arr[i] = max(arr[i-1], arr[i]);
    }
    return -1;
    ```

    ```
     //One pass
       int n = A.size();
    int maxi = A[0];
    int mini = A[2];
    int j = 1;
    for(int i = 1; i<n-1; i++){
        if(A[i] > maxi){
            maxi = A[i];
            mini = A[i+1];
            j = i+1;
            while(j<n){
                if(A[i]<mini){
                    mini = min(A[j+1], mini);
                    maxi = max(maxi, A[j]);
                    j++;
                }
                else{
                    break;
                }
            }
            i = j;
        }
    }
    if(j == n){
        return 1;
    }
    else{
        return 0;
    }
    ```
38. Move zeroes to end
    ```
      int i=0;
    int n=A.size();
    for(int j=0;j<n;j++){
        if(A[j]!=0){
            swap(A[j],A[i]);
            i++;
        }
    }
    return A;
    ```

39. Kth row of pascal 

     ```
       vector<int>ans;
    int prev = 1;
     ans.push_back(prev);
 
    for (int i = 1; i <= N; i++) {
        int curr = (prev * (N - i + 1)) / i;
        ans.push_back(curr);
        prev = curr;
    }
    return ans;
     ```
40. Pascal triangle

    ```
     vector<vector<int>>ans;
   
    for(int i=0;i<A;i++)
    {
        vector<int>temp;
        int curr=1;
        for(int j=1;j<=i;j++)
        {
            temp.push_back(curr);
            curr = curr *(i-j+1)/j;
           
        }
        temp.push_back(curr);
        ans.push_back(temp);
    }
    return ans;

    ```
    ```
       vector<vector<int>> vect;
        vect.push_back({1});        // The first row content 

        for (int i = 1; i < n; i++){
            // Temproary vector to fetch i-th row contents
            vector<int> a;
            a.push_back(1);         // The first value of the row 
            for(int j = 1; j <= i-1; j++){
                a.push_back(vect[i-1][j-1]+vect[i-1][j]);
            }
            a.push_back(1);         // The last value of the row
            vect.push_back(a);      // Updation in main vector
        }
     return vect;   
    ```
      
41. Largest number
    
    ```
      static bool cmp(int& a, int& b)

    {

        string x = to_string(a) + to_string(b);

        string y = to_string(b) + to_string(a);

        return x > y;
    }

    string Solution::largestNumber(const vector<int>& A) {
        vector<int> ans;

        int cnt = 0;

        for (int i = 0; i < A.size(); i++)

        {

            if (A[i] == 0)

                cnt++;
        }

        if (cnt == A.size())

            return "0";

        for (int i = 0; i < A.size(); i++)

        {

            ans.push_back(A[i]);
        }

        sort(ans.begin(), ans.end(), cmp);

        string res;

        for (auto i : ans) {

            res += to_string(i);
        }

        return res;
    }
    ```
42. Rotate array 

     ```
       void Solution::rotate(vector<vector<int>> & matrix) {
        reverse(matrix.begin(), matrix.end());
        for (int i = 0; i < matrix.size(); i++) {
            for (int j = i + 1; j < matrix[i].size(); j++)
                swap(matrix[i][j], matrix[j][i]);
        }
    }
     ```

43. Next permutation
     ```
      vector<int> res;
    int start = 1;
    int end = B;

    for (int i = 0.i < A.size(); i++) {
        if (A[i] == 'I') {
            res.push_back(start);
            start++;
        } else {
            res.push_back(end);
            end--;
        }
    }
    return res;
     ```
 44. Anti diagonal traversal of array
     
     ```
       int n=A.size();
    int row;
    int col;
      vector<vector<int> > ans;
    
    for(int i=0;i<n;i++)
    {
        row=0;
        col=i;
        vector<int> result;
        while(row<n&&col>=0)
        {
            result.push_back(A[row][col]);
            row++;
            col--;
        }
        ans.push_back(result);
    }
    for(int i=1;i<n;i++)
    {
        col=n-1;
        row=i;
        vector<int> result;
        while(row<n&&col>=0)
        {
            result.push_back(A[row][col]);
            row++;
            col--;
        }
        ans.push_back(result);
        
    }
    return ans;
    ```
45. Triplets with sum in range
     
     ```
       int i=0;
    int j=A.size()-1;
    
    sort(A.begin(),A.end());
    
    while(j-i>=2)
    {
        int m=(j+i)/2;
        double s=stof(A[i])+stof(A[j])+stof(A[m]);
        if(s>2)
        j--;
        else if(s<1)
        i++;
        else
        return 1;
        
    }
    return 0;
     ```
46. Balanced arrays https://www.youtube.com/watch?v=jcAsP5z41EM 
    
     ```
      int n=A.size();
    vector<int> v(n, 0);
    
    for(int i=n-1;i>=0;i--)
    {
        v[i]=A[i];
        if(i+2<n)
        v[i]+=v[i+2];
    }
    int ans=0;
    int odc=0;
    int evc=0;
  
    for(int i=0;i<n;i++)
    {
        int odd=0;
        int even=0;
        if(i%2==0)
        {
            if(i+1<n)
            odd+=v[i+1];
            if(i+2<n)
            even+=v[i+2];
        }
        else
        {
             if(i+1<n)
            even+=v[i+1];
            if(i+2<n)
            odd+=v[i+2];
            
        }
        
        
        
        if(odd+odc==even+evc)
        {ans++;
        }
        
        if(i%2)
        evc+=A[i];
        else
        odc+=A[i];
    }
    //cout<<ans;
    return ans;

     ```
47. N/3 repeat number
    
    ```
       int cnt1=0;
    int ele1;
    int cnt2=0;
    int ele2;
    
    for(int i=0;i<A.size();i++)
    {
        if(cnt1==0&&A[i]!=ele2)
        {
            cnt1++;
            ele1=A[i];
            
        }
        else if(cnt2==0&&A[i]!=ele1)
        {
            cnt2++;
            ele2=A[i];
            
        }
        else if(ele1==A[i])
        cnt1++;
        else if(ele2==A[i])
        cnt2++;
        else
        {
            cnt1--;
            cnt2--;
        }
        
        
    }
    int n=A.size();
    int count1=0,count2=0;
    for(int i=0;i<A.size();i++){
        if(A[i]==ele1){
            count1++;
        }
        if(A[i]==ele2){
            count2++;
        }
    }
    if(count1>(n/3)){
        return ele1;
    }
    if(count2>(n/3)){
        return ele2;
    
    }
    return -1;  
    ```
48. Majority element N/ 2 (If more than n/2 then only one element)
    
    ```
    int cnt=0;

    for(int i=0;i<n;i++)
    {
     if(cnt==0)
      {cnt=1;
       ele=A[i];
      }
    else if( ele==A[i])
    cnt++;
    else
    cnt--;
    

    }
    
    ```


49. Max gap
    
   ```
    int mina = nums[0], maxa = nums[0], n = nums.size();

        for (int x : nums) {
            mina = min(mina, x);
            maxa = max(maxa, x);
        }

        if (mina == maxa)
            return 0;

        int bucketSize = ceil(static_cast<double>(maxa - mina) / (n - 1));
        vector<int> minBucket(n, INT_MAX);
        vector<int> maxBucket(n, INT_MIN);

        for (int x : nums) {
            int idx = (x - mina) / bucketSize;
            minBucket[idx] = min(x, minBucket[idx]);
            maxBucket[idx] = max(x, maxBucket[idx]);
        }

        int maxGap = bucketSize;
        int previous = maxBucket[0];

        for (int i = 1; i < n; i++) {
            if (minBucket[i] == INT_MAX)
                continue;
            maxGap = max(maxGap, minBucket[i] - previous);
            previous = maxBucket[i];
        }

        return maxGap;
   ```

50. Subarray with given sum 
    ```
     int i=0,j=0;
        int sum=0;
        vector<int>ans;
        
        while(j<n)
        {
            sum+=arr[j];
            
            while(i<j&&sum>s)
            {
                sum-=arr[i];
                i++;
            }
            
            if(sum==s)
            {
                ans.push_back(i+1);
                ans.push_back(j+1);
                return ans;
                
            }
            j++;
            
            
        }
        ans.push_back(-1);
        return ans;
    ```
51. Count triplets
    ```
     int c=0;
	   sort(arr,arr+n);
	    
	    for(int i=n-1;i>=2;i--)
	    {int j=0;
	    int k=i-1;
	    while(j<k)
	    {
	        if(arr[i]==arr[j]+arr[k])
	       { c++;
	       j++;
	       k--;
	       }
	       else if(arr[i]>arr[j]+arr[k])
	       j++;
	       else
	       k--;
	    
	    }
	    }
	    return c;
      ```

52. Rearange array alternatively 
    ```
      	int min_ind=0;
    	int i=0;
    	int max_ind=n-1;
    	int z=arr[n-1]+1;
    	for(i=0;i<n;i++)
    	{if(i%2==0)
    	   { arr[i]=arr[i]+(arr[max_ind]%z)*z;
    	    max_ind--;
    	    }
    	else
    	{arr[i]=arr[i]+(arr[min_ind]%z)*z;
    	    min_ind++;
    	    
    	}
    	}
    	for(i=0;i<n;i++)
    	arr[i]=arr[i]/z;
    	 
    ```

53. Merge sorted arrays without extra space 
    ```
      int x=n-1;
    int y=0;
    while(x>=0&&y<m)
    {
        if(arr1[x]>=arr2[y])
        {
            swap(arr1[x],arr2[y]);
            x--;
            y++;
        }
        else
        break;
    }
        
        sort(arr1,arr1+n);
        sort(arr2,arr2+m);// code here 
    ```

54  Zig zag fashion

    ```
       void zigZag(int arr[], int n) {
        for(int i=0;i<n;i++)
        {
            if(i%2==1)
            {
                if(arr[i]<arr[i-1])
                swap(arr[i],arr[i-1]);
                
                if(arr[i]<arr[i+1])
                swap(arr[i],arr[i+1]);
                
            }
        }
       
    }
    ```

55. Stock sell & buy Local minima & local maxima
    ```
      vector<vector<int>>ans;
        if (n == 1)
        return ans;
 
    // Traverse through given price array
    int i = 0;
    while (i < n - 1) {
 
        // Find Local Minima
        // Note that the limit is (n-2) as we are
        // comparing present element to the next element
        while ((i < n - 1) && (price[i + 1] <= price[i]))
            i++;
 
        // If we reached the end, break
        // as no further solution possible
        if (i == n - 1)
            break;
 
        // Store the index of minima
        int buy = i++;
 
        // Find Local Maxima
        // Note that the limit is (n-1) as we are
        // comparing to previous element
        while ((i < n) && (price[i] >= price[i - 1]))
            i++;
 
        // Store the index of maxima
        int sell = i - 1;
        vector<int>v;
        v.push_back(buy);
        v.push_back(sell);
        
        ans.push_back(v);
    ```
56. chocolate distribution problem 

        ```
          long long ans=INT_MAX;
        if(n<m)
        return -1;
        sort(a.begin(),a.end());
        
        for(int i=0;i+m-1<n;i++)
        {
            ans=min(ans,a[i+m-1]-a[i]);
        }
        return ans;

        ```
57. Minimum platforms https://www.youtube.com/watch?v=dxVcMDI7vyI

        ```
          sort(arr,arr+n);
    	sort(dep,dep+n);
    	int i=1,j=0;
    	int res=0;
    	int plat=1;
    	 while(i<n&&j<n)
    	 {if(arr[i]<=dep[j])
    	 {
    	     i++;
    	     plat++;
    	 }
    	 
    	 else if(arr[i]>dep[j])
    	 {
    	     plat--;
    	     j++;
    	 }
    	    res=max(res,plat); 
    	     
    	     
    	     
    	 }
    	 return res;
        ```
58. Number of pairs 
       
``` 
long long int N_Y[1005];
memset(N_Y, 0, sizeof(N_Y));

sort(Y, Y + N);
for (long long int i = 0; i < N; i++) {
    N_Y[Y[i]]++;
}
long long int c = 0;
for (int i = 0; i < M; i++) {
    if (X[i] == 0)
        continue;

    else if (X[i] == 1)
        c += N_Y[0];
    else {
        int* idx = upper_bound(Y, Y + N, X[i]);
        c += N_Y[0] + N_Y[1];
        c = c + ((Y + N) - idx);
        if (X[i] == 2)
            c = c - (N_Y[3] + N_Y[4]);
        if (X[i] == 3)
            c += N_Y[2];
    }
    // cout<<c<<"\n";
}
return c;
```
59. Count inversions

    ```
      void merge(long long arr[],long long  low,long long  mid, long long high)
    {
        long long left=low;
        long long right=mid+1;
        vector<long long>temp;
        
        while(left<=mid&&right<=high)
        {
            if(arr[left]<=arr[right])
            {temp.push_back(arr[left]);
            left++;
            }
            else
            {
               count+=(mid-left+1);
                temp.push_back(arr[right]);
                right++;
                
            }
            
        }
        while(left<=mid)
        temp.push_back(arr[left++]);
        while(right<=high)
        temp.push_back(arr[right++]);
        
        for(long long i=low;i<=high;i++)
        {
            arr[i]=temp[i-low];
        }
        
    }
    
    void mergesort(long long arr[],long long low,long long high)
    {
        if(low>=high)
        return;
        long long mid=(low+high)/2;
        mergesort(arr,low,mid);
        mergesort(arr,mid+1,high);
        merge(arr,low,mid,high);
    }
    
    long long int inversionCount(long long arr[], long long N)
    {
        count=0;
        mergesort(arr,0,N-1);
        return count;
    }```

60. Merge without extra space
    
    ```
      {

    int x = n - 1;
    int y = 0;
    while (x >= 0 && y < m) {
        if (arr1[x] >= arr2[y]) {
            swap(arr1[x], arr2[y]);
            x--;
            y++;
        } else
            break;
    }

    sort(arr1, arr1 + n);
    sort(arr2, arr2 + m); // code here
     }
    ```

61. Sort 0,1,2
   ```
     void sort012(int a[], int n) {
    int l = 0, mid = 0, high = n - 1;
    while (mid <= high) {
        switch (a[mid]) {
        case 0:
            swap(a[l++], a[mid++]);
            break;

        case 1:
            mid++;
            break;

        case 2:
            swap(a[mid], a[high--]);
            break;
        }
    }
}
```
62. No consecutive one https://www.youtube.com/watch?v=H7tshfFTSvw

    ```
      	ll countStrings(int n) {
	    int zeroEnd=1;
	    int oneEnd=1;
	    int sum=zeroEnd+oneEnd;
	    if(n==1)
	    return sum;
	    for(int i=2;i<=n;i++)
	    {
	        
	        oneEnd=zeroEnd%1000000007;
	        zeroEnd=sum%1000000007;
	        sum=(zeroEnd+oneEnd)%1000000007;
	    }
	    return sum;
	}
    ```
63. Closet sum to zero 
    ```
      sort(arr, arr + n);
    int low = 0;
    int high = n - 1;
    int ans = INT_MAX;
    while (low < high) {
        int sum = arr[low] + arr[high];
        if (sum == 0)
            return 0;
        if ((abs(sum) <= abs(ans))) {
            if (abs(sum) == abs(ans)) {
                ans = max(ans, sum);
            } else {
                ans = sum;
            }

            if (sum < 0) {
                low++;
            } else {
                high--;
            }
        } else {
            if (sum < 0) {
                low++;
            } else {
                high--;
            }
        }
    }
    return ans;
    ```

64. Odd even jump 

    ```
      int n = arr.size();

        vector<bool> oddJumpPossibleFrom(n, false);
        vector<bool> evenJumpPossibleFrom(n, false);
        oddJumpPossibleFrom[n - 1] = true;
        evenJumpPossibleFrom[n - 1] = true;
        int totalGoodStartingPoints = 1;
        map<int, int> elementToIdxMap;
        // sorted on basis of value
        elementToIdxMap[arr[n - 1]] = n - 1;
        for (int i = n - 2; i >= 0; i--) {
            auto greaterElementForOddJump = elementToIdxMap.lower_bound(arr[i]);
            auto smallerElementForEvenJump =
                elementToIdxMap.upper_bound(arr[i]);
            if (greaterElementForOddJump != elementToIdxMap.end()) {
                oddJumpPossibleFrom[i] =
                    evenJumpPossibleFrom[greaterElementForOddJump->second];
            }
            if (smallerElementForEvenJump != elementToIdxMap.begin()) {
                smallerElementForEvenJump--;
                evenJumpPossibleFrom[i] =
                    oddJumpPossibleFrom[smallerElementForEvenJump->second];
            }
            if (oddJumpPossibleFrom[i]) {
                totalGoodStartingPoints++;
            }
            elementToIdxMap[arr[i]] = i;
        }
        return totalGoodStartingPoints;
    ```
65. Intersection of two arrays 
    ```
      //We can also make use of sets 
      vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        int l1 = nums1.size(), l2 = nums2.size(), i = 0, j = 0;
        vector<int> ans;
        sort(nums1.begin(), nums1.end());
        sort(nums2.begin(), nums2.end());
        while (i < l1 && j < l2) {
            if (nums1[i] < nums2[j])
                i++;
            else if (nums1[i] == nums2[j]) {
                ans.push_back(nums1[i]);
                i++;
                j++;
            } else if (nums1[i] > nums2[j])
                j++;
        }
        ans.erase(unique(ans.begin(), ans.end()), ans.end());
        return ans;
    }
    ```
66. Missing number
      Approach 1 - Sum of series 
      Approach 2 - XOR
      Approach 3 - Sort
     ```
     int missingNumber(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        int l   = 0;
        int r   = n-1;
        int result = n;
        
        while(l <= r) {
            int mid = l + (r-l)/2;
            if(nums[mid] > mid) {
                result = mid;
                r = mid-1;
            } else {
                l = mid+1;
            }
        }
        
        return result;
        
    }
     ```

 67. Kth missing positive number
     ```
        int findKthPositive(vector<int>& arr, int k) {
        vector<int> ans;
        int c = 1;
        int i = 0;
        while (ans.size() < k) {
            if (i >= arr.size() || c < arr[i])
                ans.push_back(c);
            else {
                i++;
            }
            c++;
          }
          return ans[k - 1];
         }
     ```   

68. Find nth digit   
    ```
      int findNthDigit(int n) {
        int total=0;
        int base=9;
        long long digit=1;
        while(total+base*digit<n)
        {
            total+=base*digit;
            digit++;
            base*=10;
        }
        n-=total;
        long long num = pow(10, (digit - 1)) + (n - 1) / digit;
        long long index = (n - 1) % digit;

        return to_string(num)[index] - '0';
        
    }
    ``` 
69. Product of array except itself
    ```
      vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n);
        vector<int> left_Product(n);
        vector<int> right_Product(n);
        left_Product[0] = 1;
        for(int i=1; i<n; i++){
            left_Product[i] = left_Product[i-1] * nums[i-1];
        }
        right_Product[n-1] = 1;
        for(int i=n-2; i>=0; i--){
            right_Product[i] = right_Product[i+1] * nums[i+1];
        }
        for(int i=0; i<n; i++){
            ans[i] = left_Product[i] * right_Product[i];
        }
        return ans;

        
    }
    ```
70. Bulb Switcher 2
    ```
      int flipLights(int n, int presses) {
       if(presses==0)return 1;
        if(n==1)return 2;
        if(n==2)return presses==1?3:4;
        if(presses==1)return 4;
        if(presses==2)return 7;
        return 8;
        
    }
    ```
