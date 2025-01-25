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
       ```
         public List<List<String>> groupAnagrams(String[] strs) {
        HashMap<String, List<String>> mp = new HashMap<>();
        List<List<String>> ans = new ArrayList<>();
        ;

        for (int i = 0; i < strs.length; i++) {
            char[] charArray = strs[i].toCharArray();
            Arrays.sort(charArray);
            String key = new String(charArray);
            if (!mp.containsKey(key)) {
                mp.put(key, new ArrayList<>());
            }
            mp.get(key).add(strs[i]);
        }

        ans.addAll(mp.values());
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

17.  Maximum non negative subarray sum

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

   18. Factorial of a large number https://www.youtube.com/watch?v=O3fwYjcMV_M

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
  19. Spiral order matrix traversal 

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
20.   Part II
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
     vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[0] < b[0];
        });
         vector<vector<int>> merged;
        vector<int> prev = intervals[0];

        for (int i = 1; i < intervals.size(); ++i) {
            vector<int> interval = intervals[i];
            if (interval[0] <= prev[1]) {
                prev[1] = max(prev[1], interval[1]);
            } else {
                merged.push_back(prev);
                prev = interval;
            }
        }

        merged.push_back(prev);
        return merged;        

        
    }
   ```

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
    ```
       vector<vector<int>> generate(int numRows) {
        if (numRows == 0) return {};
        if (numRows == 1) return {{1}};
        
        vector<vector<int>> prevRows = generate(numRows - 1);
        vector<int> newRow(numRows, 1);
        
        for (int i = 1; i < numRows - 1; i++) {
            newRow[i] = prevRows.back()[i - 1] + prevRows.back()[i];
        }
        
        prevRows.push_back(newRow);
        return prevRows;
        
    }
    ```
    ```
       vector<vector<int>> result;
        vector<int> prevRow;
        
        for (int i = 0; i < numRows; i++) {
            vector<int> currentRow(i + 1, 1);
            
            for (int j = 1; j < i; j++) {
                currentRow[j] = prevRow[j - 1] + prevRow[j];
            }
            
            result.push_back(currentRow);
            prevRow = currentRow;
        }
        
        return result;
        
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
     ```
       int low=0;
        int high=arr.length-1;
        while(low<=high){
            int mid=low+(high-low)/2;
            if(arr[mid]-(mid+1)>=k)
                high=mid-1;
            else
                low=mid+1;
        }
        return low+k;
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
    ```
        int n = nums.length;
        int[] ans = new int[n];
        ans[0] = 1;

        for (int i = 1; i < n; i++) {
            ans[i] = ans[i - 1] * nums[i - 1];
        }

        int rightProduct = 1;
        for (int i = n - 1; i >= 0; i--) {
            ans[i] = ans[i] * rightProduct;
            rightProduct *= nums[i];
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
71. Find smallest common element in all rows 
    ```
      int smallestCommonElement(vector<vector<int>>& mat) 
    {
        int n=mat.size();
        int m=mat[0].size();
        unordered_map<int,int>mp;
        unordered_set<int>s;
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
            {
                s.insert(mat[i][j]);
            }
            for(auto it : s)
            {
                mp[it]++;
            }
            s.clear();
        }
        for(auto it : mp)
        {
            if(it.second==n)
            return it.first;
        }
        return -1;
        
    }
    };
    ```
72. Integer to english words
     ```
       string numberToWords(int n) {
        vector<int> numbers = {1000000000, 1000000, 1000, 100, 90, 80, 70, 60,
                               50,         40,      30,   20,  19, 18, 17, 16,
                               15,         14,      13,   12,  11, 10, 9,  8,
                               7,          6,       5,    4,   3,  2,  1};
        vector<string> words = {"Billion",  "Million",  "Thousand",  "Hundred",
                                "Ninety",   "Eighty",   "Seventy",   "Sixty",
                                "Fifty",    "Forty",    "Thirty",    "Twenty",
                                "Nineteen", "Eighteen", "Seventeen", "Sixteen",
                                "Fifteen",  "Fourteen", "Thirteen",  "Twelve",
                                "Eleven",   "Ten",      "Nine",      "Eight",
                                "Seven",    "Six",      "Five",      "Four",
                                "Three",    "Two",      "One"};

        string result;

        for (int i = 0; i < 31; i++) {
            if (num >= numbers[i]) {
                if (num >= 100)
                    result +=
                        numberToWords(num / numbers[i]) + " " + words[i] + " ";
                else
                    result += words[i] + " ";

                num %= numbers[i];

                if (num == 0)
                    break;
            }
        }

        return result.empty() ? "Zero" : result.substr(0, result.size() - 1);
    }
     ```
73. Merge strings alternatively 
    ```
      string mergeAlternately(string word1, string word2) {
        int n = word1.length();
        int m = word2.length();
        string ans = "";
        int i = 0, j = 0;
        while (i < n && j < m) {
            ans += word1[i];
            i++;
            ans += word2[j];
            j++;
        }
        while (i < n) {
            ans += word1[i];
            i++;
        }
        while (j < m) {
            ans += word2[j];
            j++;
        }
        return ans;
    }
    ```
74. Longest consecutive sequence
    ```
      nt longestConsecutive(vector<int>& nums) {
        int n = nums.size();
        if(n==0 || n==1) return n;

        
        unordered_set<int> st;
        for(int i : nums) st.insert(i);
        int ans = 1;
        for(int i : st){
            if(st.find(i-1) == st.end()){ 
                int count = 0;
                while(st.find(i++) != st.end()){
                    count++;
                }
                ans = max(ans, count);
            }
        }
        return ans;
        
    }
    ```
75.  Rotate array 

    ```
       void rotate(vector<int>& nums, int k) {
         int n = nums.size();
        k = k % n; 
        std::reverse(nums.begin(), nums.end());           
        std::reverse(nums.begin(), nums.begin() + k);     
        std::reverse(nums.begin() + k, nums.end());   
        
    }
    ```
76. Merge intervals
    ```
      vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[0] < b[0];
        });
         vector<vector<int>> merged;
        vector<int> prev = intervals[0];

        for (int i = 1; i < intervals.size(); ++i) {
            vector<int> interval = intervals[i];
            if (interval[0] <= prev[1]) {
                prev[1] = max(prev[1], interval[1]);
            } else {
                merged.push_back(prev);
                prev = interval;
            }
        }

        merged.push_back(prev);
        return merged;        

        
    }
    ```
77. Max sum pair in an array 
    ```
      int findMax(int number) {
        int maxDigit = INT_MIN;
        while (number > 0) {
            maxDigit = max(maxDigit, number % 10);
            number /= 10;
        }
        return maxDigit;
    }
    int maxSum(vector<int>& nums) {
        unordered_map<int, vector<int>> mp;
        for (auto& num : nums) {
            int digit = findMax(num);
            if (mp.find(digit) == mp.end()) {
                mp[digit] = {num};
            } else {
                mp[digit].push_back(num);
            }
        }
        int answer = -1;
        for (auto itr = mp.begin(); itr != mp.end(); itr++) {
            vector<int> curr = itr->second;
            sort(curr.begin(), curr.end());
            if (curr.size() == 1)
                continue;
            answer = max(answer, curr[curr.size() - 1] + curr[curr.size() - 2]);
        }
        return answer;
    }
    ```
78. Reverse words in a string 
    ```
      string reverseWords(string s) {
        reverse(s.begin(), s.end());
        int n = s.size();
        int left = 0;
        int right = 0;
        int i = 0;
        while (i < n) {
            while (i < n && s[i] == ' ')
                i++;
            if (i == n)
                break;
            while (i < n && s[i] != ' ') {
                s[right++] = s[i++];
            }
            reverse(s.begin() + left, s.begin() + right);
            s[right++] = ' ';
            left = right;
            i++;
        }
        s.resize(right - 1);
        return s;
    }
    ```
79. Diagonal traverse 2

    ```
           vector<int> findDiagonalOrder(vector<vector<int>>& nums) {
        unordered_map<int, vector<int>> mp;

        for (int row = nums.size() - 1; row >= 0; row--) {
            for (int col = 0; col < nums[row].size(); col++) {

                mp[row + col].push_back(nums[row][col]);
            }
        }

        vector<int> result;
        int diagonal = 0;

        while (mp.find(diagonal) != mp.end()) {

            for (int& num : mp[diagonal]) {
                result.push_back(num);
            }
            diagonal++;
        }

        return result;
        }
     ```
     ```
     //BFS 
       queue<pair<int, int>> que;
        que.push({0, 0});
        vector<int> result;
        
        while (!que.empty()) {
            auto [row, col] = que.front();
            que.pop();
            result.push_back(nums[row][col]);
            
            if (col == 0 && row + 1 < nums.size()) {
                que.push({row + 1, col});
            }
            
            if (col + 1 < nums[row].size()) {
                que.push({row, col + 1});
            }
        }
        
        return result;
     ```
80.Diagonal Traverse

   ```
       class Solution {
    public int[] findDiagonalOrder(int[][] mat) {
        int m = mat.length;
        int n = mat[0].length;

        Map<Integer, List<Integer>> map = new TreeMap<>();

        List<Integer> result = new ArrayList<>();

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                map.putIfAbsent(i + j, new ArrayList<>());
                map.get(i + j).add(mat[i][j]);
            }
        }

        boolean flip = true;
        for (Map.Entry<Integer, List<Integer>> entry : map.entrySet()) {
            List<Integer> diagonal = entry.getValue();

            if (flip) {

                Collections.reverse(diagonal);
            }

            result.addAll(diagonal);

            flip = !flip;
        }

        int[] resultArray = new int[result.size()];
        for (int i = 0; i < result.size(); i++) {
            resultArray[i] = result.get(i);
        }

        return resultArray;

    }
   ```
80. Exam room 
     ```
       class ExamRoom {
       private:
	set<int> seats;
	int capacity;

       public:

	ExamRoom(int N)
	{
		capacity = N;
	}

	int seat()
	{
		int dist = 0;
		int curr = 0;

		if (!seats.empty()) {
			auto itr = seats.begin();
            
            //calculate distance at the begining
			dist = *itr;

			if (dist == 0) {
				itr++;
			}
            
            //calculate distance in between
			while (itr != seats.end()) {
                int mid_dist = (*itr - *prev(itr)) / 2;
				if (dist < mid_dist) {
					dist = mid_dist;
					curr = *prev(itr) + dist;
				}
				itr++;
			}
            
            //calculate distance at the end
			if (dist < ((capacity - 1) - *(seats.rbegin()))) {
				curr = capacity - 1;
			}
		}

		return *(seats.insert(curr).first);
	}

	void leave(int p)
	{
		seats.erase(p);
	}
       };
     ```
81. Best time to buy and sell stock
     ```
        int maxProfit(vector<int>& prices) {
        int buy = prices[0];
        int profit = 0;
        for (int i = 1; i < prices.size(); i++) {
            if (prices[i] < buy) {
                buy = prices[i];
            } else if (prices[i] - buy > profit) {
                profit = prices[i] - buy;
            }
        }
        return profit;
        
        
    }
     ```
82. Song shuffler
     ```
         class SongShuffler {
    private:
        std::vector<std::string> songs; // Array of songs
        int playedIndex;                // Index to track the played songs
        int playIndex;    // Index to track the current song to be played
        int k;            // The number of recent songs to track
        std::rand random; // Random number generator

        // Helper function to swap two elements in the song list
        void swap(int i, int j) {
            std::string temp = songs[i];
            songs[i] = songs[j];
            songs[j] = temp;
        }

    public:
        // Constructor to initialize the playlist and k
        SongShuffler(std::vector<std::string>& songList, int k) {
            songs = songList;
            this->k = k;
            playedIndex = 0;
            playIndex = 0;
            std::srand(std::time(0)); // Seed the random number generator
        }

        // Function to play a shuffled song
        std::string playShuffleSong() {
            // Generate a random index between playIndex and the length of the
            // song list
            int nextSong = std::rand() % (songs.size() - playIndex) + playIndex;
            std::string ret = songs[nextSong];

            // Swap the song at playIndex with the selected random song
            swap(playIndex, nextSong);

            // Update the playIndex
            if (playIndex < k)
                playIndex++;

            // If playIndex equals k, reset and adjust playedIndex
            if (playIndex == k) {
                swap(playedIndex, playIndex);
                playedIndex++;
                if (playedIndex == k)
                    playedIndex = 0;
            }

            return ret;
        }
    };

     ```
83. Max product of 3 numbers
    ```
      int l = nums.size(); 
        sort(nums.begin(), nums.end()); 
        int withNeg = nums[0] * nums[1] * nums[l-1]; 
        int withoutNeg = nums[l-1] * nums[l-2] * nums[l-3]; 
        return max(withNeg, withoutNeg); 
    ```
    ```
      public int maximumProduct(int[] nums) {
        int max1 = Integer.MIN_VALUE, max2 = Integer.MIN_VALUE, max3 = Integer.MIN_VALUE;
        int min1 = Integer.MAX_VALUE, min2 = Integer.MAX_VALUE;
        
        for (int num : nums) {
  
            if (num > max1) {
                max3 = max2;
                max2 = max1;
                max1 = num;
            } else if (num > max2) {
                max3 = max2;
                max2 = num;
            } else if (num > max3) {
                max3 = num;
            }
            

            if (num < min1) {
                min2 = min1;
                min1 = num;
            } else if (num < min2) {
                min2 = num;
            }
        }
        
   
        return Math.max(max1 * max2 * max3, min1 * min2 * max1);
        
    }
    ```
85. Largest plus sign 
    ```
      int[][] inputGrid = new int[n][n];
        for (int[] row : inputGrid) {
            Arrays.fill(row, 1);
        }
        for (int[] mine : mines) {

            inputGrid[mine[0]][mine[1]] = 0;
        }

        int[][] lTR = new int[n][n];

        for (int i = 0; i < n; i++) {
            int runningSum = 0;
            for (int j = 0; j < n; j++) {

                if (inputGrid[i][j] == 0) {
                    runningSum = 0;
                } else {
                    runningSum = inputGrid[i][j] + runningSum;
                }

                lTR[i][j] = runningSum;
            }
        }

        int[][] rTL = new int[n][n];

        for (int i = 0; i < n; i++) {
            int runningSum = 0;
            for (int j = n - 1; j >= 0; j--) {
                if (inputGrid[i][j] == 0) {
                    runningSum = 0;
                } else {
                    runningSum = inputGrid[i][j] + runningSum;
                }
                rTL[i][j] = runningSum;
            }
        }

        int[][] tTB = new int[n][n];

        for (int i = 0; i < n; i++) {
            int runningSum = 0;
            for (int j = 0; j < n; j++) {
                if (inputGrid[j][i] == 0) {
                    runningSum = 0;
                } else {
                    runningSum = inputGrid[j][i] + runningSum;
                }

                tTB[j][i] = runningSum;
            }
        }

        int[][] bTT = new int[n][n];

        for (int i = 0; i < n; i++) {
            int runningSum = 0;
            for (int j = n - 1; j >= 0; j--) {
                if (inputGrid[j][i] == 0) {
                    runningSum = 0;
                } else {
                    runningSum = inputGrid[j][i] + runningSum;
                }

                bTT[j][i] = runningSum;
            }
        }
        int ans = 0;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                int minLen = Math.min(Math.min(lTR[i][j], rTL[i][j]), Math.min(bTT[i][j], tTB[i][j]));
                ans = Math.max(ans, minLen);
            }
        }
        return ans;
    }
    ```
    ```
      vector<vector<int>> inputGrid(n, vector<int>(n, 1));

        for (auto m : mines) {
            inputGrid[m[0]][m[1]] = 0;
        }

        vector<vector<int>> lTR(n, vector<int>(n, 0)); // Left-to-right
        vector<vector<int>> rTL(n, vector<int>(n, 0)); // Right-to-left
        vector<vector<int>> tTB(n, vector<int>(n, 0)); // Top-to-bottom
        vector<vector<int>> bTT(n, vector<int>(n, 0)); // Bottom-to-top

        for (int i = 0; i < n; i++) {
            int runningSum = 0;
            for (int j = 0; j < n; j++) {

                if (inputGrid[i][j] == 0) {
                    runningSum = 0;
                } else {
                    runningSum = inputGrid[i][j] + runningSum;
                }

                lTR[i][j] = runningSum;
            }
        }

        for (int i = 0; i < n; i++) {
            int runningSum = 0;
            for (int j = n - 1; j >= 0; j--) {
                if (inputGrid[i][j] == 0) {
                    runningSum = 0;
                } else {
                    runningSum = inputGrid[i][j] + runningSum;
                }
                rTL[i][j] = runningSum;
            }
        }

        for (int i = 0; i < n; i++) {
            int runningSum = 0;
            for (int j = 0; j < n; j++) {
                if (inputGrid[j][i] == 0) {
                    runningSum = 0;
                } else {
                    runningSum = inputGrid[j][i] + runningSum;
                }

                tTB[j][i] = runningSum;
            }
        }

        for (int i = 0; i < n; i++) {
            int runningSum = 0;
            for (int j = n - 1; j >= 0; j--) {
                if (inputGrid[j][i] == 0) {
                    runningSum = 0;
                } else {
                    runningSum = inputGrid[j][i] + runningSum;
                }

                bTT[j][i] = runningSum;
            }
        }
        int ans = 0;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                int minLen =
                    min(min(lTR[i][j], rTL[i][j]), min(bTT[i][j], tTB[i][j]));
                ans = max(ans, minLen);
            }
        }
        return ans;
    ```
86. Destroy sequential targets
     ```
        int destroyTargets(vector<int>& nums, int space) {
        map<int, int> mp;
        int high = INT_MIN;

        for (int i = 0; i < nums.size(); i++) {
            mp[nums[i] % space]++;
            high = max(high, mp[nums[i] % space]);
        }
        int ans = INT_MAX;
        for (int i = 0; i < nums.size(); i++) {
            if (high == mp[nums[i] % space])
                
            ans = min(ans, nums[i]);
        }
        return ans;
    }
     ```
    ```
       public int destroyTargets(int[] nums, int space) {
        Map<Integer, Integer> freqMap = new HashMap<>();
        int high = Integer.MIN_VALUE;

        for (int num : nums) {
            int mod = num % space;
            freqMap.put(mod, freqMap.getOrDefault(mod, 0) + 1);
            high = Math.max(high, freqMap.get(mod));
        }

        int ans = Integer.MAX_VALUE;

        for (int num : nums) {
            if (freqMap.get(num % space) == high) {
                ans = Math.min(ans, num);
            }
        }

        return ans;

    }
    ```
87. No. of substrings with fixed ratio 
    ```
       long long fixedRatio(string s, int num1, int num2) {
        long long result = 0;
        vector<int> count(2, 0); 
        unordered_map<long long, int> map; 
        map[0] = 1;

        for (char c : s) {
          
            ++count[c - '0'];
            //key=(num2×count[0])−(num1×count[1])
            //When two subarrays (prefixes) have the same key, the difference between them (a subarray) has the same 0-to-1 ratio as num1:num2.


            long long key = (long long)(num2) * count[0] 
                          - (long long)(num1) * count[1];

            result += map[key];

   
            ++map[key];
        }

        return result;
        
    }
    ```
    ```
      long result = 0;

        int[] count = new int[2];

        Map<Long, Integer> map = new HashMap<>();
        map.put(0L, 1);

        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);

            ++count[c - '0'];

            long key = (long) num2 * count[0] - (long) num1 * count[1];

            result += map.getOrDefault(key, 0);

            map.put(key, map.getOrDefault(key, 0) + 1);
        }

        return result;
    ```
88. Make strings anti palindrome 
     ```
        public String makeAntiPalindrome(String s) {
        char[] chars = s.toCharArray();
        Arrays.sort(chars);
        int n = chars.length;
        for (int i = n / 2 - 1, k = n - 1 - i; i >= 0; i--) {
            int j = n - 1 - i;
            if (chars[j] == chars[i]) {
                while (k < n && chars[k] == chars[i]) k++;
                if (k < n) swap(chars, j, k);
                else return "-1";
            } else break;
        }
        return String.valueOf(chars);
    }

    private void swap(char[] chars, int i, int j) {
        char t = chars[i];
        chars[i] = chars[j];
        chars[j] = t;
    }
     ```
89. Insert Delete GetRandom O(1)
    ```
        std::unordered_map<int, int> val_to_index;
    std::vector<int> values;
    RandomizedSet() {}

    bool insert(int val) {

        if (val_to_index.find(val) != val_to_index.end())
            return false;
        values.push_back(val);
        val_to_index[val] = values.size() - 1; // Store index in the map
        return true;
    }

    bool remove(int val) {
        if (val_to_index.find(val) == val_to_index.end())
            return false;

        int index_to_remove = val_to_index[val];
        int last_val = values.back();

      
        values[index_to_remove] = last_val;
        val_to_index[last_val] = index_to_remove;

     
        values.pop_back();
        val_to_index.erase(val); 
        return true;
    }

    int getRandom() {
        int random_index = rand() % values.size();
        return values[random_index];
    }
    ```
90. Zig zag string 
    ```
       string convert(string s, int numRows) {

    string ans;
    int n = s.length();
    vector<string> rows(min(numRows, n)); // Use vector of strings to store rows
    int i = 0;

    while (i < n) {
        // Fill downwards
        for (int index = 0; index < numRows && i < n; index++) {
            rows[index] += s[i++];
        }
        // Fill diagonally upwards
        for (int index = numRows - 2; index > 0 && i < n; index--) {
            rows[index] += s[i++];
        }
    }

    // Combine all rows into the final answer
    for (const string& row : rows) {
        ans += row;
    }

    return ans;
    }
    ```
91. Pow(x,n)
     ```
        double binaryExp(double x, long long n) {
        if (n == 0) {
            return 1;
        }

        if (n < 0) {
            n = -1 * n;
            x = 1.0 / x;
        }

        double result = 1;
        while (n) {

            if (n % 2 == 1) {
                result = result * x;
                n -= 1;
            }

            x = x * x;
            n = n / 2;
        }
        return result;
    }

    double myPow(double x, int n) { return binaryExp(x, (long long)n); }
     ```
    ```
       double binaryExp(double x, long n) {
        if (n == 0) {
            return 1;
        }

        if (n < 0) {
            n = -1 * n;
            x = 1.0 / x;
        }

        double result = 1;
        while (n!=0) {

            if (n % 2 == 1) {
                result = result * x;
                n -= 1;
            }

            x = x * x;
            n = n / 2;
        }
        return result;
    }


    public double myPow(double x, int n) {
        return binaryExp(x, (long)n);
        
    }
    ```
92. Random Pick weight 
     ```
        private int[] prefixSums;
    private Random random;

    public Solution(int[] w) {
        for (int i = 1; i < w.length; i++) {
            w[i] += w[i - 1];
        }
        this.prefixSums = w;
        this.random = new Random();
        
    }
    
    public int pickIndex() {
        int hi = prefixSums[prefixSums.length - 1];
        int index = random.nextInt(hi) + 1;

       
        int lower = Arrays.binarySearch(prefixSums, index);
        if (lower < 0) {
            lower = -lower - 1; 
        }
        return lower;
        
    }
     ```
93. Subarray sum equals k
    ```
       public int subarraySum(int[] nums, int k) {

        HashMap<Integer, Integer> map = new HashMap<>();
        int sum = 0, ans = 0;
        map.put(0, 1);

        for (int num : nums) {
            sum += num;
            int find = sum - k;

            if (map.containsKey(find)) {
                ans += map.get(find);
            }

            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }

        return ans;

    }
    ```
94. Nested List Weight Sum
    ```
      int DFS(vector<NestedInteger>& nestedList, int depth){
            int n = (int)nestedList.size();
            int sum = 0;
            for(int i=0;i<n;i++){
                if(nestedList[i].isInteger()){
                    sum += nestedList[i].getInteger()*depth;
                }
                else{
                    sum += DFS(nestedList[i].getList(),depth+1);
                }
            }
            return sum;
        }
    public:
        int depthSum(vector<NestedInteger>& nestedList) {
            return DFS(nestedList, 1);
        }
    ```
```
    private int dfs(List<NestedInteger> nestedList, int depth) {
        int sum = 0;
        for (NestedInteger ni : nestedList) {
            if (ni.isInteger()) {
                sum += ni.getInteger() * depth;
            } else {
                sum += dfs(ni.getList(), depth + 1);
            }
        }
        return sum;
    }
    public int depthSum(List<NestedInteger> nestedList) {
        return dfs(nestedList, 1);
        
    } 
```
95.Dot product of two sparse vector
   ```
      private int[] array;

    SparseVector(int[] nums) {
        array = nums;
    }

    public int dotProduct(SparseVector vec) {
        int result = 0;

        for (int i = 0; i < array.length; i++) {
            result += array[i] * vec.array[i];
        }
        return result;
    }
   ```
   ```
      Map<Integer, Integer> mapping;      

    SparseVector(int[] nums) {
        mapping = new HashMap<>();
        for (int i = 0; i < nums.length; ++i) {
            if (nums[i] != 0) {
                mapping.put(i, nums[i]);        
            }
        }
    }

    public int dotProduct(SparseVector vec) {        
        int result = 0;

      for (Integer i : this.mapping.keySet()) {
            if (vec.mapping.containsKey(i)) {
                result += this.mapping.get(i) * vec.mapping.get(i);
            }
        }
        return result;
    }
   ```
   ```
      I got asked this question on FB on-site. I immediately came up with hashset solution but the interviewer asked to solve it with something else other than dictionary, then I came up with the stack based solution more like Approach 3 keeping the non-zero indices and popping stacks and comparing if indices equal to add to total, otherwise ignore.
   ```
  ```
     private List<int[]> indexValueList;

    SparseVector(int[] nums) {
        indexValueList = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {
                indexValueList.add(new int[]{i, nums[i]});
            }
        }
    }

    // Compute dot product using two pointers
    public int dotProduct(SparseVector vec) {
        List<int[]> list1 = this.indexValueList;
        List<int[]> list2 = vec.indexValueList;
        int i = 0, j = 0, result = 0;

        while (i < list1.size() && j < list2.size()) {
            int index1 = list1.get(i)[0];
            int index2 = list2.get(j)[0];

            if (index1 == index2) {
                result += list1.get(i)[1] * list2.get(j)[1];
                i++;
                j++;
            } else if (index1 < index2) {
                i++;
            } else {
                j++;
            }
        }

        return result;
    }
  ```
  ```
    //WHEN ONLY ONE SPARSE
     import java.util.ArrayList;
import java.util.List;

class SparseVector {
    // A list of index-value pairs representing the sparse vector
    List<int[]> indexValueList;

    // Constructor to initialize the sparse vector
    SparseVector(int[] nums) {
        indexValueList = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {
                indexValueList.add(new int[]{i, nums[i]}); // Store index and value
            }
        }
    }

    // Dot product of sparse vector with a dense vector using binary search
    public int dotProductSparseToDenseWithBinarySearch(SparseVector sparseVec, int[] denseVec) {
        List<int[]> sparseList = sparseVec.indexValueList; // Sparse list of the input vector
        int result = 0;

        // Iterate through the non-zero elements of the sparse vector
        for (int[] pair : sparseList) {
            int index = pair[0]; // Index of the non-zero element
            int value = pair[1]; // Value at the index

            // Binary search to check if the index exists in the dense vector
            if (binarySearchDenseVector(denseVec.length, index)) {
                result += value * denseVec[index];
            }
        }

        return result;
    }

    // Helper method to perform binary search on the dense vector
    private boolean binarySearchDenseVector(int length, int targetIndex) {
        // Dense vector has implicit indices [0, 1, 2, ..., length-1]
        int left = 0, right = length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (mid == targetIndex) {
                return true;
            } else if (mid < targetIndex) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        return false; // Target index not found
    }
}

// Test case to validate the implementation
public class Main {
    public static void main(String[] args) {
        int[] nums1 = {1, 0, 0, 2, 3}; // Sparse vector 1
        int[] denseVec = {1, 2, 3, 4, 5}; // Dense vector

        SparseVector vec1 = new SparseVector(nums1);

        // Calculate the dot product using binary search
        int result = vec1.dotProductSparseToDenseWithBinarySearch(vec1, denseVec);
        System.out.println("Dot Product: " + result); // Output: 24 (1*1 + 2*4 + 3*5)
    }
}

  ```
96. Building with ocean view
    ```
      vector<int> findBuildings(vector<int>& heights) {

        vector<int> ans;
        int n = heights.size();

        int maxRight = 0;

        for (int i = n - 1; i >= 0; i--) {

            if (heights[i] > maxRight) {
                ans.push_back(i);
                maxRight = heights[i];
            }
        }

        reverse(ans.begin(), ans.end());

        return ans;
    }
    ```
97. Two sum
    ```
       HashMap<Integer, Integer> freq = new HashMap<>();
        int[] ans;

        for (int i = 0; i < nums.length; i++) {
            freq.put(nums[i], i);

        }

        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (freq.containsKey(target - nums[i]) && freq.get(target - nums[i]) != i) {
                return new int[] { i, freq.get(complement) };
            }
        }
        return new int[] {};
    ```
98. Palindrome number
    ```
      public boolean isPalindrome(int x) {
        if (x < 0) {
            return false;
        }

        int n = x;
        long s = 0;
        while (n != 0) {
            s = (s * 10 + n % 10);
            n = n / 10;
        }
        return s == x;

    }
    ```
99. String to integer(atoi)
    ```
      public int myAtoi(String s) {
        int len = s.length();
        double num = 0;
        int i = 0;
        while (i < len && s.charAt(i) == ' ') {
            i++;
        }
        boolean positive = i < len && s.charAt(i) == '+';
        boolean negative = i < len && s.charAt(i) == '-';
        if (positive == true)
            i++;
        if (negative == true)
            i++;

        while (i < len && s.charAt(i) >= '0' && s.charAt(i) <= '9') {
            num = num * 10 + (s.charAt(i) - '0');
            i++;
        }
        num = negative ? -num : num;

        num = (num > Integer.MAX_VALUE) ? Integer.MAX_VALUE : num;
        num = (num < Integer.MIN_VALUE) ? Integer.MIN_VALUE : num;

        return (int) num;

    }
    ```
100. Reverse integer
     ```
        public int reverse(int x) {
        long s = 0;

        while (x!=0) {
            s = s * 10 + x % 10;

            x = x / 10;
        }
        if (s > Integer.MAX_VALUE || s < Integer.MIN_VALUE)
            return 0;
        return (int)s;

    }
     ```
101. Count and say
     ```
         String solve(int n) {
        if (n == 1) {
            return "1";
        }

        String say = solve(n - 1);
        StringBuilder result = new StringBuilder();

        for (int i = 0; i < say.length(); i++) {
            char c = say.charAt(i);
            int count = 1;

            while (i + 1 < say.length() && say.charAt(i) == say.charAt(i + 1)) {
                count++;
                i++;
            }

            result.append(count).append(c);
        }

        return result.toString();
    }

    public String countAndSay(int n) {
        return solve(n);
    }
     ```
102. Sort colors
     ```
         public void sortColors(int[] nums) {
        int zero = 0, one = 0, two = nums.length - 1;

        while (one <= two) {
            if (nums[one] == 0) {
                swap(nums, zero, one);
                zero++;
                one++;
            } else if (nums[one] == 2) {
                swap(nums, one, two);
                two--;
            } else {
                one++;
            }
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
     ```
103. Valid number
      ```
        

          public boolean isNumber(String s) {
        s = s.trim();
        if (s.isEmpty())
            return false;
        boolean hasNumber = false;
        boolean hasDot = false;
        boolean hasExp = false;
        int n = s.length();

        for (int i = 0; i < n; i++) {
            char c = s.charAt(i);

            if (c >= '0' && c <= '9') {
                hasNumber = true;
            } else if (c == '.') {
                if (hasDot || hasExp)
                    return false; 
                hasDot = true;
            } else if (c == 'e' || c == 'E') {
                if (!hasNumber || hasExp)
                    return false; 
                hasExp = true;
                hasNumber = false;
            } else if (c == '+' || c == '-') {
                if (i > 0 && s.charAt(i - 1) != 'e' && s.charAt(i - 1) != 'E')
                    return false;
            } else {
                return false; 
            }
        }

        return hasNumber;
          
      ```
104. Multiply strings
     ```
          public String multiply(String num1, String num2) {
        if (num1.equals("0") || num2.equals("0")) {
            return "0";
        }
        int m = num1.length(), n = num2.length();
        int[] result = new int[m + n];
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                int mul = (num1.charAt(i) - '0') * (num2.charAt(j) - '0');
                int sum = mul + result[i + j + 1];

                result[i + j + 1] = sum % 10; 
                result[i + j] += sum / 10; 
            }
        }
        StringBuilder sb = new StringBuilder();
        for (int num : result) {
            if (!(sb.length() == 0 && num == 0)) {
                sb.append(num);
            }
        }

        return sb.toString();

    }
           
   
     ``` 
105. Maximum Swap
     ```
       public int maximumSwap(int num) {
        
        String s = Integer.toString(num); // Convert num to string
        int n = s.length();

    
        int[] maxRight = new int[n];

       
        maxRight[n - 1] = n - 1;

     
        for (int i = n - 2; i >= 0; i--) {
            int rightMaxIdx = maxRight[i + 1];
            char rightMaxElement = s.charAt(rightMaxIdx);


            if (s.charAt(i) > rightMaxElement) {
                maxRight[i] = i;
            } else {
                maxRight[i] = rightMaxIdx;
            }
        }

      
        for (int i = 0; i < n; i++) {
            int maxRightIdx = maxRight[i];
            char maxRightElement = s.charAt(maxRightIdx);

          
            if (s.charAt(i) < maxRightElement) {
                char[] charArray = s.toCharArray();
                char temp = charArray[i];
                charArray[i] = charArray[maxRightIdx];
                charArray[maxRightIdx] = temp;


                return Integer.parseInt(new String(charArray));
            }
        }

        return num;
    
       
     ```

     ```
       String s = Integer.toString(num); 
        int n = s.length();

        int[] maxRight = new int[10]; 
        for (int i = 0; i < 10; i++) {
            maxRight[i] = -1;
        }

        for (int i = 0; i < n; i++) {
            int idx = s.charAt(i) - '0'; 
            maxRight[idx] = i;
        }


        for (int i = 0; i < n; i++) {
            int currDigit = s.charAt(i) - '0';
            

            for (int digit = 9; digit > currDigit; digit--) {
                if (maxRight[digit] > i) {
              
                    char[] charArray = s.toCharArray();
                    char temp = charArray[i];
                    charArray[i] = charArray[maxRight[digit]];
                    charArray[maxRight[digit]] = temp;
                    

                    return Integer.parseInt(new String(charArray));
                }
            }
        }

        return num;
     ```
106. Missing Range
     ```
       public List<List<Integer>> findMissingRanges(int[] nums, int lower, int upper) {
        int start = lower;

        List<List<Integer>> ans = new ArrayList<>();

        int n = nums.length;
        if (n == 0) {
            ans.add(List.of(lower, upper));
            return ans;
        }

        for (int i = 0; i <= n; i++) {
            int end = (i < n ? nums[i] - 1 : upper);
            if (start <= end) {

                ans.add(List.of(start, end));
            }
            start = (i < n ? nums[i] + 1 : upper + 1);

        }
       
     ```
107. Toeplitz Matrix
     ```
       public boolean isToeplitzMatrix(int[][] matrix) {

        for (int i = 0; i < matrix.length - 1; i++) {
            for (int j = 0; j < matrix[0].length - 1; j++) {
                if (matrix[i][j] != matrix[i + 1][j + 1])
                    return false;
            }
        }
        return true;

    
     ```
108. Move zeroes
     ```
         public void moveZeroes(int[] nums) {
        if (nums.length == 0 || nums.length == 1)
            return;

        int l = 0;
        int h = 0;

        while (h < nums.length) {
            if (nums[h] != 0) {

                int temp = nums[l];
                nums[l] = nums[h];
                nums[h] = temp;
                l++;
            }
            h++;
        }

    
     ```
109. Random Pick Index
   ```
      class Solution {

    private int[] nums; 
    private Random rand; 
    public Solution(int[] nums) {
        this.nums = nums;
        this.rand = new Random();
    }
    

    public int pick(int target) {
        int result = -1; 
        int count = 0;   


        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == target) {
                count++;
              
                if (rand.nextInt(count) == 0) {//Reservoir Sampling of processimng each element is 1/i and here of getting each element is 1/count
                    result = i;
                }
            }
        }

        return result;  
    }
}
   ```
110. Majority element
      ```
        int majorityElement(vector<int>& nums) {
        int count = 0;
        int m;

        for (int i = 0; i < nums.size(); i++) {
            if (count == 0) {
                m = nums[i];
                count++;
            } else if (m == nums[i])
                count++;
            else

                count--;
        }
        return m;
    
      
      ```
111. Group shifted strings
   ```
        string key(string& s) {
        string t;
        int n = s.length();
        for (int i = 1; i < n; i++) {
            int diff = s[i] - s[i - 1];
            if (diff < 0)
                diff += 26;
            t += diff + ',';
        }
        t += '.';
        return t;
    }
    vector<vector<string>> groupStrings(vector<string>& strings) {
        unordered_map<string, vector<string>> mp;
        for (string s : strings)
            mp[key(s)].push_back(s);
        vector<vector<string>> groups;
        for (auto m : mp) {
            vector<string> group = m.second;
            sort(group.begin(), group.end());
            groups.push_back(group);
        }
        return groups;
    }
   ```
112. Continuous Subarray Sum
      ```
          bool checkSubarraySum(vector<int>& nums, int k) {

        int n = nums.size();

        unordered_map<int, int> mp;

        mp[0] = -1;

        int sum = 0;

        for (int i = 0; i < n; i++) {
            sum += nums[i];

            int remainder = sum % k;

            if (mp.find(remainder) != mp.end()) {

                if (i - mp[remainder] >= 2)//length more than two
                    return true;

            } else {
                mp[remainder] = i;
            }
        }
        return false;
    
      ```
113. Goat Latin
      ```
         string toGoatLatin(string sentence) {
        deque<string> a;
        string ans = "";
        string trill = "";
        string s;
        int cnt = 0;
        for (int i = 0; i < sentence.size() + 1; i++) {
            if (sentence[i] != ' ') {
                s += sentence[i];
            } else {
                cnt++;
                a.push_back(s);
                s = "";
            }
        }
        s = s.substr(0, s.size() - 1);
        a.push_back(s);
        for (auto it : a) {
            trill = trill + 'a';
            if (it[0] == 'a' || it[0] == 'e' || it[0] == 'i' || it[0] == 'o' ||
                it[0] == 'u' || it[0] == 'A' || it[0] == 'E' || it[0] == 'I' ||
                it[0] == 'O' || it[0] == 'U') {
                it += "ma" + trill;
            } else {
                it = it.substr(1, it.size()) + it[0] + "ma" + trill;
            }

            ans += it + ' ';
        }
        ans = ans.substr(0, ans.size() - 1);
        return ans;
    
      ```
      ```
           unordered_set<char> vowels = {'a', 'e', 'i', 'o', 'u',
                                      'A', 'E', 'I', 'O', 'U'};
        string result;
        string word;
        int wordIndex = 1;

        stringstream ss(sentence); // Use a stringstream to split words
        while (ss >> word) {       // Extract words one by one
            if (vowels.count(word[0])) {

                word += "ma";
            } else {

                word = word.substr(1) + word[0] + "ma";
            }

            word += string(wordIndex, 'a');
            wordIndex++;

            result += word + " ";
        }


        if (!result.empty()) {
            result.pop_back();
        }

        return result;
    
      ```
114. Can place flowers
     ```
       bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        int count = 0;
        for (int i = 0; i < flowerbed.size(); i++) {
            if (flowerbed[i] == 0) {

                bool emptyLeftPlot = (i == 0) || (flowerbed[i - 1] == 0);
                bool emptyRightPlot =
                    (i == flowerbed.size() - 1) || (flowerbed[i + 1] == 0);

                if (emptyLeftPlot && emptyRightPlot) {
                    flowerbed[i] = 1;
                    count++;
                    if (count >= n) {
                        return true;
                    }
                }
            }
        }
        return count >= n;
    
     ```
115. Maximum size subarray sum equals k
     ```
        public int maxSubArrayLen(int[] nums, int k) {

        int sum = 0;
        int n = nums.length;
        int ans = 0;

        HashMap<Integer, Integer> mp = new HashMap<>();
        for (int i = 0; i < n; i++) {
            sum += nums[i];
            if (sum == k) {
                ans = Math.max(ans, i + 1);
            }
            int curr = sum - k;
            if (mp.containsKey(curr)) {
                ans = Math.max(ans, i - mp.get(curr));
            }
            if (!mp.containsKey(sum)) {
                mp.put(sum, i);
            }

        }

        return ans;

    
     ```
116. Plus One
      ```
    
         //Approach 1
         int n = digits.length;
         int c = 1;

        for (int i = n - 1; i >= 0; i--) {
        int s = digits[i] + c;

        c = s / 10;

        digits[i] = s % 10;

        }

        if (c != 0) {
        int[] ans = new int[n + 1];
        ans[0] = c;
        for (int i = 0; i < n; i++)
        ans[i + 1] = digits[i];
        return ans;

        } else
        return digits;

        /Approach 2
        for (int i = n - 1; i >= 0; i--) {

            digits[i] += 1;

            if (digits[i] < 10) {
                return digits;
            }

            digits[i] = 0;
        }
        int[] result = new int[n + 1];
        result[0] = 1;
        return result;
      ```

117. Minimum Number of Changes to Make Binary String Beautiful

    ```
       public int minChanges(String s) {
        int minChangesRequired = 0;

        for (int i = 0; i < s.length(); i += 2) {

            if (s.charAt(i) != s.charAt(i + 1)) {
                minChangesRequired++;
            }
        }
        return minChangesRequired;

    
    ```
118. Product of Two Run-Length Encoded Arrays

    ```
        vector<vector<int>> findRLEArray(vector<vector<int>>& encoded1,
                                     vector<vector<int>>& encoded2) {
        vector<vector<int>> ans;
        int i = 0, j = 0;

        while (i < encoded1.size() && j < encoded2.size()) {
            int minFreq = min(encoded1[i][1], encoded2[j][1]);
            int product = encoded1[i][0] * encoded2[j][0];

            if (ans.size() && ans.back()[0] == product)
                ans.back()[1] += minFreq;
            else
                ans.push_back({product, minFreq});

        
            if (encoded1[i][1] < encoded2[j][1]) {
                encoded2[j][1] -= minFreq;
                i++;
            } else if (encoded1[i][1] > encoded2[j][1]) {
                encoded1[i][1] -= minFreq;
                j++;
            } else {
                i++;
                j++;
            }
        }
        return ans;
    
    ```
119. Remove element
       ```
           int removeElement(vector<int>& nums, int val) {
        int i = 0;
        for (int j = 0; j < nums.size(); j++) {
            if (nums[j] != val) {
                nums[i] = nums[j];
                i++;
            }
        }
        return i;
        
        }
       ```
       ```
          int i = 0;
        int n = nums.size();
        while (i < n) {
            if (nums[i] == val) {
                nums[i] = nums[n - 1];
               
                n--;
            } else {
                i++;
            }
        }
        return n;
       ```
120. Strobogrammatic number
       ```
           public boolean isStrobogrammatic(String num) {
        Map<Character, Character> rotatedDigits = new HashMap<>(
                Map.of('0', '0', '1', '1', '6', '9', '8', '8', '9', '6'));

        for (int left = 0, right = num.length() - 1; left <= right; left++, right--) {
            char leftChar = num.charAt(left);
            char rightChar = num.charAt(right);
            if (!rotatedDigits.containsKey(leftChar) || rotatedDigits.get(leftChar) != rightChar) {
                return false;
            }
        }
        return true;

       }
       ```
121. Unique Length-3 Palindromic Subsequences
     ```
       int countPalindromicSubsequence(string s) {
        vector<int> first = vector(26, -1);
        vector<int> last = vector(26, -1);

        for (int i = 0; i < s.size(); i++) {
            int curr = s[i] - 'a';
            if (first[curr] == -1) {
                first[curr] = i;
            }

            last[curr] = i;
        }

        int ans = 0;
        for (int i = 0; i < 26; i++) {
            if (first[i] == -1) {
                continue;
            }

            unordered_set<char> between;
            for (int j = first[i] + 1; j < last[i]; j++) {
                between.insert(s[j]);
            }

            ans += between.size();
        }

        return ans;
        }
     ```
122. Design Tic Tac Toe
      ```
         class TicTacToe {
       private:
       vector<int> rowJudge;
       vector<int> colJudge;
       int diag, anti;
       int total;

       public:
        TicTacToe(int n) : total(n), rowJudge(n), colJudge(n), diag(0), anti(0) {}

       int move(int row, int col, int player) {
        int add = player == 1 ? 1 : -1;
        diag += row == col ? add : 0;
        anti += row == total - col - 1 ? add : 0;
        rowJudge[row] += add;
        colJudge[col] += add;
        if (abs(rowJudge[row]) == total || abs(colJudge[col]) == total ||
            abs(diag) == total || abs(anti) == total)
            return player;
        return 0;
        }
        };
      ```
123. Add binary
     ```
        string addBinary(string a, string b) {
        int i=a.length()-1;
        int j=b.length()-1;
        int c=0;
        string res;
        while(i>=0||j>=0||c)
        {
            if (i >= 0) c += a[i--] - '0';
            if (j >= 0) c += b[j--] - '0';
            res += c % 2 + '0';
            c /= 2;
            


        }
        reverse(begin(res), end(res));
        return res;  
        
      }
     ```
124. Minimum Number of Operations to Move All Balls to Each Box
    ```
      vector<int> minOperations(string boxes) {
        int n = boxes.size();

        vector<int> answer(n, 0);

        int cumValue = 0;
        int cumValueSum = 0;
        for (int i = 0; i < n; i++) {
            answer[i] = cumValueSum;

            cumValue += boxes[i] == '0' ? 0 : 1;

            cumValueSum += cumValue;
        }

        cumValue = 0;
        cumValueSum = 0;

        for (int i = n - 1; i >= 0; i--) {
            answer[i] += cumValueSum;

            cumValue += boxes[i] == '0' ? 0 : 1;

            cumValueSum += cumValue;
        }

        return answer;
    }
    ```
124. Divide two integers
      ```
          int divide(int dividend, int divisor) {
        if (dividend == divisor) return 1;

        unsigned int ans = 0;
        int sign = 1;


        if ((dividend < 0 && divisor > 0) || (dividend > 0 && divisor < 0))
            sign = -1;

    
        long n = abs((long)dividend);
        long d = abs((long)divisor);


        while (n >= d) {
            int count = 0;
            while (n > (d << (count + 1)))
                count++;
            n -= d << count;
            ans += 1 << count;
        }


        if (ans == (1 << 31) && sign == 1) return INT_MAX;

        if(sign==1)
        return ans;
        return -ans;
        
       }  
    
      ```
125.   Integer to Roman
        
       ```
         string intToRoman(int num) {
        vector<string> romanSymbols = {"M",  "CM", "D",  "CD", "C",  "XC", "L",
                                       "XL", "X",  "IX", "V",  "IV", "I"};
        vector<int> values = {1000, 900, 500, 400, 100, 90, 50,
                              40,   10,  9,   5,   4,   1};
        string ans = "";
        for (int i = 0; i < 13; i++) {
            while (num >= values[i]) {
                ans += romanSymbols[i];
                num -= values[i];
            }
        }
        return ans;
        }
       ```
125. Shifting letters 2
      ```
        string shiftingLetters(string s, vector<vector<int>>& shifts) {
        int n = s.length();
        vector<int> diff(n, 0);
        for (const auto& shift : shifts) {
            int start = shift[0];
            int end = shift[1];
            int direction = shift[2];

            if (direction == 1) {
                diff[start] += 1;
                if (end + 1 < n)
                    diff[end + 1] -= 1;
            } else {
                diff[start] -= 1;
                if (end + 1 < n)
                    diff[end + 1] += 1;
            }
        }
         for (int i = 1; i < n; ++i) {
            diff[i] += diff[i - 1]; 
        }
        for (int i = 0; i < n; ++i) {
            int shift = diff[i] % 26; 
            if (shift < 0)
                shift += 26;
            
           
            s[i] = ((s[i] - 'a' + shift) % 26) + 'a';
            
        }
        
        return s;
       }
      ```
126. Happy number
     ```
         bool isHappy(int n) {
        if(n==1 || n==7) return true;
        else if(n<10) return false;
        else{
            int sum=0;
            while(n>0){
                int temp=n%10;
                sum+= temp*temp;
                n=n/10;
            }
            return isHappy(sum);
        }
        
      }
     ```
126. Contains Duplicate
      ```
          public boolean containsDuplicate(int[] nums) {
        HashSet<Integer> set = new HashSet<>();
        for (int i = 0; i < nums.length; i++) {
                if (set.contains(nums[i])) {
                    return true;
                }
                set.add(nums[i]);
        }
        return false;
        
      }
      ```
127. Majority element 2
     ```
        ector<int> majorityElement(vector<int>& nums) {
        int e1 = 0, e2 = 0; // Elements
    int c1 = 0, c2 = 0; // Counts
    int n = nums.size();

    // First pass: Find potential candidates
    for (int num : nums) {
        if (num == e1) {
            c1++;
        } else if (num == e2) {
            c2++;
        } else if (c1 == 0) {
            e1 = num;
            c1 = 1;
        } else if (c2 == 0) {
            e2 = num;
            c2 = 1;
        } else {
            c1--;
            c2--;
        }
    }


    c1 = 0;
    c2 = 0;
    for (int num : nums) {
        if (num == e1) {
            c1++;
        } else if (num == e2) {
            c2++;
        }
    }

    vector<int> result;
    if (c1 > n / 3) {
        result.push_back(e1);
    }
    if (c2 > n / 3) {
        result.push_back(e2);
    }

    return result;
        
       }
    ```
128. Valid Anagram 
       ```
           bool isAnagram(string s, string t) {
        if (s.length() != t.length())
            return false;
        unordered_map<char, int> count;

        for (auto x : s) {
            count[x]++;
        }

        for (auto x : t) {
            count[x]--;
        }

        for (auto x : count) {
            if (x.second != 0) {
                return false;
            }
        }

        return true;
        }
       ```
129. Integer to english words
      ```
         string numberToWords(int num) {
        if (num == 0)
            return "Zero";

        string bigString[] = {"Thousand", "Million", "Billion"};
        string result = numberToWordsHelper(num % 1000);
        num /= 1000;

        for (int i = 0; i < 3; ++i) {
            if (num > 0 && num % 1000 > 0) {
                result = numberToWordsHelper(num % 1000) + bigString[i] + " " +
                         result;
            }
            num /= 1000;
        }

        return result.empty() ? result : result.substr(0, result.size() - 1);
      }
       string numberToWordsHelper(int num) {
        string digitString[] = {"Zero", "One", "Two",   "Three", "Four",
                                "Five", "Six", "Seven", "Eight", "Nine"};
        string teenString[] = {"Ten",      "Eleven",  "Twelve",  "Thirteen",
                               "Fourteen", "Fifteen", "Sixteen", "Seventeen",
                               "Eighteen", "Nineteen"};
        string tenString[] = {"",      "",      "Twenty",  "Thirty", "Forty",
                              "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};

        string result = "";
        if (num > 99) {
            result += digitString[num / 100] + " Hundred ";
        }
        num %= 100;
        if (num < 20 && num > 9) {
            result += teenString[num - 10] + " ";
        } else {
            if (num >= 20) {
                result += tenString[num / 10] + " ";
            }
            num %= 10;
            if (num > 0) {
                result += digitString[num] + " ";
            }
         }

        return result;
        }
      ```
130. Minimum time difference
      ```
          public int findMinDifference(List<String> timePoints) {
        int mini = Integer.MAX_VALUE;
        List<Integer> minutes = new ArrayList<>();
         for (String val : timePoints) {
            int hr = 10 * (val.charAt(0) - '0') + (val.charAt(1) - '0'); 
            int min = 10 * (val.charAt(3) - '0') + (val.charAt(4) - '0'); 
            minutes.add(hr * 60 + min); 
        }
        Collections.sort(minutes);

        for (int i = 1; i < minutes.size(); i++) {
            mini = Math.min(mini, minutes.get(i) - minutes.get(i - 1));
        }
         int n = minutes.size();
        mini = Math.min(mini, 1440 - (minutes.get(n - 1) - minutes.get(0)));
        return mini;
        
       }
      ```  
131.  Find Score of an Array After Marking All Elements

       ```
          long long findScore(vector<int>& nums) {
        int n = nums.size();
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> minHeap;  

    
        for (int i = 0; i < n; i++) {
            minHeap.push({nums[i], i});
        }

        long long score = 0;  

        // Process elements in min-heap
        while (!minHeap.empty()) {
            auto curr = minHeap.top();
            minHeap.pop();
            int num = curr.first;
            int idx = curr.second;

            if (nums[idx] != -1) {  
                score += num;
                nums[idx] = -1;  
                if (idx > 0 && nums[idx - 1] != -1) {
                    nums[idx - 1] = -1;  
                }
                if (idx < n - 1 && nums[idx + 1] != -1) {
                    nums[idx + 1] = -1;  
                }
            }
        }

        return score;
        
       }
       ```
       ```
          long long findScore(vector<int>& nums) {
        long long score = 0;
        int n = nums.size();
        deque<int> q;

        for (int i = 0; i < n; i++) {

            if (!q.empty() && nums[i] >= q.back()) {
                bool skip = false;

                while (!q.empty()) {
                    int add = q.back();
                    q.pop_back();
                    if (!skip) {
                        score += add;
                    }
                    skip = !skip;
                }
                continue;
            }

            q.push_back(nums[i]);
        }


        bool skip = false;
        while (!q.empty()) {
            int add = q.back();
            q.pop_back();
            if (!skip) {
                score += add;
            }
            skip = !skip;
        }

        return score;
    }
         
       ```


     

      
 







