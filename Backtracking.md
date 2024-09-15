# Backtracking

## Theory

## Questions 

1. Generate Parantheses 
 ```
vector<string> res;
    void findAll(string current, int op, int cl, vector<string>& res, int n) {

        if (current.length() == 2 * n) {
            res.push_back(current);
            return;
        }
        if (op < n)
            findAll(current + "(", op + 1, cl, res, n);
        if (cl < op)
            findAll(current + ")", op, cl + 1, res, n);
    }

    vector<string> generateParenthesis(int n)

    {

        findAll("(", 1, 0, res, n);
        return res;
    }
```
2. Largest Number after k swaps
   ```
   void solve(string& s, string& res, int k, int start) {
    if (k == 0 || start == s.size() - 1) {
        return;
    }

    char max_char = *max_element(s.begin() + start, s.end());
    if (s[start] == max_char) {
        solve(s, res, k, start + 1);
        return;
    }

    for (int i = start + 1; i < s.size(); i++) {
        if (s[i] == max_char && s[i] > s[start]) {

            swap(s[i], s[start]);
            if (s > res) {
                res = s;
            }
            solve(s, res, k - 1, start + 1);

            swap(s[i], s[start]);
        }
    }
   }
   string findMaximumNum(string str, int k) {
    string res = str;
    solve(str, res, k, 0);
    return res;
   }
   ```
   ```
     string swapChars(int a, int b, string s) {
        swap(s[a], s[b]);
        return s;
    }
    void solve(string str, int k, int index, string& max) {
        if (k == 0 || index == str.length() - 1) {
            if (str.compare(max) > 0) {
                max = str;
            }
            return;
        }
        
        // Traverse the string starting from the current index
        for (int i = index + 1; i < str.length(); i++) {
            if (str[index] < str[i]) {
                // Swap characters at index and i
                str = swapChars(i,index,str);
                
                // Recur for the next index with one less swap
                solve(str, k - 1, index + 1, max);
                
                // Backtrack (undo the swap)
                str = swapChars(i,index,str);
            }
        }
        // Recur without making any swap for the next index
        solve(str, k, index + 1, max);
    }
    
   ```
3. Permutations of a string 
    ```
          if (start == S.size() - 1) {
                set.insert(S); // Insert the current permutation into the set
                return;
            }

            // Traverse the string and swap characters
            for (int i = start; i < S.size(); i++) {
                swap(S[start], S[i]);
                solve(S, start + 1, set); // Recur for the next index
                swap(S[start],
                     S[i]); // Backtrack to restore the original string
            }
        }
        vector<string> find_permutation(string S) {
            vector<string> v;
            set<string> set;  // To store unique permutations
            solve(S, 0, set); // Start the recursive process

            // Copy all unique permutations from the set to the result vector
            for (auto i : set) {
                v.push_back(i);
            }

            return v;
        }
    
    ```
4. N Digit numbers with digits in increasing order

  ```
      void solve(string ds, int n, vector<int>& ans) {
            if (n == 0) {
                ans.push_back(stoi(ds));
                return;
            }

            for (char i = '1'; i <= '9'; i++) {
                if (ds.size() == 0 || ds.back() < i) {
                    ds.push_back(i);
                    solve(ds, n - 1, ans);
                    ds.pop_back();
                }
            }
        }
        vector<int> increasingNumbers(int n) {
            vector<int> ans;
            if (n == 1) {
                for (int i = 0; i < 10; i++)
                    ans.push_back(i);
                return ans;
            }
            string ds = "";
            solve(ds, n, ans);
        }
  ```
5. Rat in a maze
   ```
      bool isValid(int i, int j, vector<vector<int>>& mat,
                     vector<vector<bool>>& visited) {

            return (i >= 0 && i < n && j >= 0 && j < n && mat[i][j] == 1 &&
                    !visited[i][j]);
        }

        void solve(vector<vector<int>> & mat, int i, int j, vector<string>& ans,
                   string& s, vector<vector<bool>>& visited) {
            if (i == n - 1 && j == n - 1) {

                ans.push_back(s);
                return;
            }

            visited[i][j] = true;

            for (int k = 0; k < 4; k++) {
                int new_i = i + dx[k];
                int new_j = j + dy[k];
                if (isValid(new_i, new_j, mat, visited)) {
                    s.push_back(p[k][0]);
                    solve(mat, new_i, new_j, ans, s, visited);
                    s.pop_back();
                }
            }

            visited[i][j] = false;//Ek hi path me we cannot come to same point that is why
        }

        vector<string> findPath(vector<vector<int>> & mat) {
            vector<string> ans;
            n = mat.size();

            if (n == 0 || mat[0][0] == 0 || mat[n - 1][n - 1] == 0) {

                return ans;
            }

            string s = "";
            vector<vector<bool>> visited(n, vector<bool>(n, false));
            solve(mat, 0, 0, ans, s, visited);
            return ans;
        }
   ```
6. Palindrome partition

   ```
    bool isPalindrome(string& s) {
        int l = 0;
        int r = s.length() - 1;
        while (l <= r) {
            if (s.at(l) != s.at(r)) {
                return false;
            }
            l++;
            r--;
        }
        return true;
    }
    void sum(string s, vector<string> a, vector<vector<string>>& k) {
        if (s == "" || s.length() == 0) {
            k.push_back(a);
            return;
        }
        for (int i = 1; i <= s.length(); i++) {
            string temp = s.substr(0, i);
            if (!isPalindrome(temp)) {
                continue;
            }
            a.push_back(temp);
            sum(s.substr(i, s.length()), a, k);
            a.pop_back();
        }
        return;
    }

    vector<vector<string>> partition(string s) {
        vector<vector<string>> k;
        sum(s, {}, k);
        return k;
    }
   ```
7. Word Break
   ```
      bool solve(string &s, unordered_map<string, bool> &st, int idx, vector<int> &dp) {
    if (idx == s.size()) 
        return true;

    if (dp[idx] != -1) 
        return dp[idx];

  
    for (int i = idx + 1; i <= s.size(); i++) {
        string t = s.substr(idx, i - idx);  // Extract substring from idx to i-1
        if (st.find(t) != st.end() && solve(s, st, i, dp)) {
            return dp[idx] = true;
        }
    }

    return dp[idx] = false;  // Mark the current idx as false if no valid break
   }

   bool wordBreak(string s, vector<string>& wordDict) {
    unordered_map<string, bool> st;

    // Populate the dictionary set
    for (const string &word : wordDict)
        st[word] = true;

    // DP array for memoization, initialized to -1 (not processed)
    vector<int> dp(s.size(), -1);

    // Start solving from index 0
    return solve(s, st, 0, dp);
   }
   ```
8. Word Break 2
   ```
      bool solve(string &s, unordered_map<string, bool> &st, int idx, vector<int> &dp, vector<string> &result, vector<string> &currentPath) {
    if (idx == s.size()) {
        // If we've reached the end, save the current path (words found so far) into the result
        result.push_back("");
        for (int i = 0; i < currentPath.size(); ++i) {
            result.back() += currentPath[i];
            if (i != currentPath.size() - 1) result.back() += " ";
        }
        return true;
    }

    if (dp[idx] != -1)  // Memoization check
        return dp[idx];

    bool found = false;

    // Try breaking the word from idx to any future index
    for (int i = idx + 1; i <= s.size(); i++) {
        string t = s.substr(idx, i - idx);  // Extract substring from idx to i-1
        if (st.find(t) != st.end()) {
            currentPath.push_back(t);  // Add the current word to the path
            if (solve(s, st, i, dp, result, currentPath)) {
                found = true;
            }
            currentPath.pop_back();  // Backtrack
        }
    }

    return dp[idx] = found;  // Memoize and return the result for this index
    }

    vector<string> wordBreak(string s, vector<string>& wordDict) {
    unordered_map<string, bool> st;

    // Populate the dictionary set
    for (const string &word : wordDict)
        st[word] = true;

    // DP array for memoization, initialized to -1 (not processed)
    vector<int> dp(s.size(), -1);
    
    vector<string> result;       // Stores the final result of word combinations
    vector<string> currentPath;  // Tracks the current path of words during recursion

    solve(s, st, 0, dp, result, currentPath);  // Start solving from index 0
    
    return result;  // Return all possible segmentations
   }
   ```
9. Letter combinations of a phone number
   ```
     void solve(string& digits, string& s, vector<string>& ans, int start,
               unordered_map<int, vector<char>>& m) {
        if (start ==
            digits.size()) { // Base case: if we've processed all digits
            ans.push_back(s);
            return;
        }

        int digit = digits[start] - '0';
        for (char c : m[digit]) {
            s.push_back(c);
            solve(digits, s, ans, start + 1, m);
            s.pop_back();
        }
    }

    vector<string> letterCombinations(string digits) {
        if (digits.empty()) {
            return {};
        }

        // Create the mapping for each digit
        unordered_map<int, vector<char>> m;
        m[2] = {'a', 'b', 'c'};
        m[3] = {'d', 'e', 'f'};
        m[4] = {'g', 'h', 'i'};
        m[5] = {'j', 'k', 'l'};
        m[6] = {'m', 'n', 'o'};
        m[7] = {'p', 'q', 'r', 's'};
        m[8] = {'t', 'u', 'v'};
        m[9] = {'w', 'x', 'y', 'z'};

        vector<string> ans;
        string s = "";
        solve(digits, s, ans, 0, m);

        return ans;
    }
   ```
   10. N queens

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
11. Sudoku solver
    ```
      bool isSafe(int i, int j, char val, vector<vector<char>>& board) {
        for (int k = 0; k < 9; k++) {
            if (board[i][k] == val)
                return false;
            if (board[k][j] == val)
                return false;
            if (board[3 * (i / 3) + k / 3][3 * (j / 3) + k % 3] ==
                val) // checks within submatrix
                return false;
        }
        //  int r = i - i % 3;
        //  int c = j - j % 3;

        // for (int m = r; m < r + 3; m++) {
        //     for (int n = c; n < c + 3; n++) {
        //         if (board[m][n] == val)
        //             return false;
        //     }
        // }

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
13. Android unlock patterns
    ```
      int dfs(int mid_element[10][10], vector<bool>&visited, int curr, int remaining_len){

        // If remaining length is negative, this path is invalid
        if(remaining_len < 0) return 0;

        // If remaining length is zero, we found a valid pattern
        if(remaining_len == 0) return 1;

        visited[curr] = true;

        int count = 0;

        for(int i=1; i<=9; i++){

            // Check if the next key is not visited and meets the conditions
            if(!visited[i] && (!mid_element[curr][i] || visited[mid_element[curr][i]]))
                count += dfs(mid_element, visited, i, remaining_len-1);
        }

        // Backtrack: mark the current key as not visited
        visited[curr] = false;
        
        return count;
    }
    int numberOfPatterns(int m, int n) {
         int mid_element[10][10] = {0};
        mid_element[1][3] = mid_element[3][1] = 2;
        mid_element[1][7] = mid_element[7][1] = 4;
        mid_element[3][9] = mid_element[9][3] = 6;
        mid_element[7][9] = mid_element[9][7] = 8;
        mid_element[1][9] = mid_element[9][1] = mid_element[3][7] = mid_element[7][3] = 
        mid_element[2][8] = mid_element[8][2] = mid_element[4][6] = mid_element[6][4] = 5;

        vector<bool>visited(10, false);

        int count = 0;

        for(int i=m; i<=n; i++){

        // For keys 1 and 2, we multiply the count by 4 because they have symmetries: 1, 3, 7, and 9 are symmetrically opposite, and similarly for 2, 4, 6, and 8.
            count += dfs(mid_element, visited, 1, i-1 )*4;
            count += dfs(mid_element, visited, 2, i-1 )*4;//2,4,6,8
        // For key 5, we don't multiply by 4 because it's in the center and doesn't have symmetrical patterns.
            count += dfs(mid_element, visited, 5, i-1 );
        }

        return count;
        
    }
    ```
14. Combination sums 2
     ```
        void findCombination(int index, int target, vector<int>& ds,
                         vector<vector<int>>& ans, vector<int> arr, int n) {

        if (target == 0) {

            ans.push_back(ds);
            return;
        }

        for (int i = index; i < n; i++) {
            //Generate duplicate conbinations with 1 at 0 and 1 at 1

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
14. Combination sum
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
15. Subsets
    ```
        void solve(vector<vector<int>>& ans, vector<int>& v, vector<int>& nums,
               int start) {

        ans.push_back(v);

        for (int i = start; i < nums.size(); i++) {

            v.push_back(nums[i]);

            solve(ans, v, nums, i + 1);

            v.pop_back();
        }
    }

    vector<vector<int>> subsets(vector<int>& nums) {
        vector<int> v;
        vector<vector<int>> ans;

        solve(ans, v, nums, 0);
        return ans;
    }
    ```

