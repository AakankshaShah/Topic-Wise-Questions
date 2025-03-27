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
   ```
      private void solve(int index, String digits, List<String> ans, StringBuilder temp, HashMap<Character, String> mp) {
        if (index >= digits.length()) {
            ans.add(temp.toString());
            return;
        }

        char digit = digits.charAt(index);
        String letters = mp.get(digit);

        for (char letter : letters.toCharArray()) {
            temp.append(letter);
            solve(index + 1, digits, ans, temp, mp);
            temp.deleteCharAt(temp.length() - 1);
        }
    }

    public List<String> letterCombinations(String digits) {

        if (digits.isEmpty()) {
            return new ArrayList<>();
        }

        HashMap<Character, String> mp = new HashMap<>();
        mp.put('2', "abc");
        mp.put('3', "def");
        mp.put('4', "ghi");
        mp.put('5', "jkl");
        mp.put('6', "mno");
        mp.put('7', "pqrs");
        mp.put('8', "tuv");
        mp.put('9', "wxyz");

        List<String> ans = new ArrayList<>();
        StringBuilder temp = new StringBuilder(); 

        solve(0, digits, ans, temp, mp); 
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
16. Expression add operators
    ```
        void solve(vector<string>& ans, string& path, string& num, int target,
               int pos, long long curr, long long prev) {
        if (pos == num.size()) {
            if (curr == target) {
                ans.push_back(path);
            }
            return;
        }

        for (int i = pos; i < num.size(); ++i) {
            if (i != pos && num[pos] == '0')
                break;

            string part = num.substr(pos, i - pos + 1);
            long long n = stoll(part);

            if (pos == 0) {
                path += part;
                solve(ans, path, num, target, i + 1, n, n);
                path.erase(path.size() - part.size());
            } else {
                path += '+' + part;
                solve(ans, path, num, target, i + 1, curr + n, n);
                path.erase(path.size() - part.size() - 1);

                path += '-' + part;
                solve(ans, path, num, target, i + 1, curr - n, -n);
                path.erase(path.size() - part.size() - 1);

                path += '*' + part;
                solve(ans, path, num, target, i + 1, curr - prev + prev * n,
                      prev * n);
                path.erase(path.size() - part.size() - 1);
            }
        }
    }
    vector<string> addOperators(string num, int target) {
         vector<string> ans;
        if (num.empty())
            return ans;

        string path;
        solve(ans, path, num, target, 0, 0, 0);
        return ans;
        
    }
    ```
    ```
       private void solve(List<String> ans, StringBuilder path, String num, int target,
                       int pos, long curr, long latest) {
       
        if (pos == num.length()) {
            if (curr == target) {
                ans.add(path.toString()); 
            }
            return;
        }

        for (int i = pos; i < num.length(); ++i) {
           
            if (i != pos && num.charAt(pos) == '0') break;

            String part = num.substring(pos, i + 1);  
            long n = Long.parseLong(part);  

            if (pos == 0) {
              
                path.append(part);
                solve(ans, path, num, target, i + 1, n, n);
                path.delete(path.length() - part.length(), path.length());  
            } else {
              
                path.append('+').append(part);
                solve(ans, path, num, target, i + 1, curr + n, n);
                path.delete(path.length() - part.length() - 1, path.length());  

                // Add "-" operator
                path.append('-').append(part);
                solve(ans, path, num, target, i + 1, curr - n, -n);
                path.delete(path.length() - part.length() - 1, path.length());  

                // Add "*" operator
                path.append('*').append(part);
                solve(ans, path, num, target, i + 1, curr - latest + latest * n, latest * n);
                path.delete(path.length() - part.length() - 1, path.length());  
            }
        }
    }

    public List<String> addOperators(String num, int target) {
        List<String> ans = new ArrayList<>();
        if (num.isEmpty()) return ans;

        StringBuilder path = new StringBuilder();
        solve(ans, path, num, target, 0, 0, 0);  // Start the recursive backtracking
        return ans;
    }
    ```
17. Beautiful arrangement
     ```
       void helper(vector<int> nums, int start, int& ans) {
        if (start == nums.size()) {
            ans++;
            return;
        }
        for (int i = start; i < nums.size(); i++) {
            swap(nums[start], nums[i]);
            if (nums[start] % (start + 1) == 0 ||
                (start + 1) % nums[start] == 0)
                helper(nums, start + 1, ans);
        }
    }

    int countArrangement(int n) {
        vector<int> nums;
        int ans = 0;
        for (int i = 1; i <= n; i++) {
            nums.push_back(i);
        }
        helper(nums, 0, ans);
        return ans;
    }
     ```
18. Next closest time
    ```
        string nextClosestTime(string time) {
         set<char> digits;
        string out = time;

        for (const auto& digit : time) {
            if (digit != ':') digits.insert(digit);
        }

        // Parse time right to left
        for (auto i = 4; i >= 0; i--) {
            if (i == 2) continue; // skip colon

            auto currTime = i > 2 ? out.substr(3, 2):out.substr(0,2);
            const auto maxTime = i > 2 ? "60":"24";
            const auto idx = i > 2 ? i-3:i;

            auto it = ++digits.find(out[i]);
            if (it == digits.end()) {
                out[i] = *(digits.begin());
            } else {
                currTime[idx] = *it;
                if (currTime >= maxTime) {
                    out[i] = *(digits.begin());
                } else {
                    out[i] = *it;
                    return out;
                }
            }

        }

        return out;
        
    }
    ```
19. Smallest string starting from root
     ```
         string smallestFromLeaf(TreeNode* root) {
        string s = "", result = "";
        helper(root, s, result);
        return result;
        
    }
    void helper(TreeNode* node, string& s, string& result) {

        if (node == NULL) return;

 
        s.push_back(node->val + 'a');
        if ((node->left == NULL) && (node->right == NULL)) {
            string t = s;
            reverse(t.begin(), t.end());
            if (result.empty() || (t < result)) result = t;
        }
        else {
            helper(node->left, s, result);
            helper(node->right, s, result);
        }
        s.pop_back();
    }
     ```
21. Letter Tiles Possibilities
     ```
         set<string> result;

    void solve(string& tiles, string s, map<char, int>& count) {
        if (!s.empty()) {
            result.insert(s);
        }
        for (auto& [ch, cnt] : count) {
            if (cnt > 0) {
                s.push_back(ch);
                count[ch]--;
                solve(tiles, s, count);
                s.pop_back();
                count[ch]++;
            }
        }
    }

    int numTilePossibilities(string tiles) {
        map<char, int> count;
        for (char ch : tiles) {
            count[ch]++;
        }
        solve(tiles, "", count);
        return result.size();
    }
     ```
22. Restore ip address
     ```
         bool isValid(const string& segment) {
        int m = segment.length();
        if (m > 3)
            return false;
        if (segment[0] == '0')
            return m == 1; // Leading zeros are only valid for "0"
        return stoi(segment) <= 255;
    }

    void sum(const string& s, string current, vector<string>& result,
             int count) {
       
        if (count == 4) {
            if (s.empty()) { // Ensure the entire string is used
                result.push_back(current);
            }
            return;
        }

        for (int i = 1; i <= 3 && i <= s.length(); i++) {
            string segment = s.substr(0, i);
            if (isValid(segment)) {
               
                string newCurrent =
                    count == 0 ? segment : current + "." + segment;
                sum(s.substr(i), newCurrent, result, count + 1);
            }
        }
    }

    vector<string> restoreIpAddresses(const string& s) {
        vector<string> result;
        if (s.length() >= 4 && s.length() <= 12) {
            sum(s, "", result, 0);
        }
        return result;
    }
     ```
23. Flip Game 2
    ```
        bool canWin(string curState) {
        int sz = curState.size();
        if (sz < 2)
            return false;
        for (int i = 0; i + 1 < sz; i++) {
            if (curState.substr(i, 2) == "++") {
                string nextStr =
                    curState.substr(0, i) + "--" + curState.substr(i + 2);
                if (!canWin(nextStr))
                    return true;
            }
        }
        return false;
    }
    ```
24. Genralize abbreviation
     ```
         void backtrack(int idx, const string& word, string& cur_combi,
                   vector<string>& res, const int& n) {
        if (idx >= n) {
            res.push_back(cur_combi);
            return;
        }

        bool last_char_is_digit =
            cur_combi.empty() ? false : isdigit(cur_combi.back());
        if (!last_char_is_digit) {
            for (int i = idx; i < n; ++i) {
                int cur_len = i - idx + 1;
                cur_combi += to_string(cur_len);

                backtrack(i + 1, word, cur_combi, res, n);

                cur_combi.pop_back();
                // If the len is greater than 10, we need to pop back twice.
                if (cur_len >= 10)
                    cur_combi.pop_back();
            }
        }

        // Add index of current char into our cur_combi and carry on to the next
        // index.
        cur_combi += word[idx];
        backtrack(idx + 1, word, cur_combi, res, n);
        cur_combi.pop_back();
    }
    vector<string> generateAbbreviations(string word) {
        vector<string> res;
        string cur_combi = "";
        backtrack(0, word, cur_combi, res, word.length());
        return res;
    }
     ```
25. Brace expansion

   ```
        int j;
    vector<char> adj[50];
    vector<string> ans;
    void dfs(int level, string temp) {
        if (level == j) {
            ans.push_back(temp);
            return;
        }
        for (int i = 0; i < adj[level].size(); i++) {
            temp += adj[level][i];
            dfs(level + 1, temp);
            temp.pop_back();
        }
    }
    vector<string> expand(string s) {
        int n = s.size();
        int j = 0;
        stack<char> st;
        for (int i = 0; i < n; i++) {
            if (s[i] == ',')
                continue;
            if (s[i] == '{')
                st.push('{');
            else if (s[i] == '}') {
                st.pop();
                j++;
            } else if ((s[i] >= 'a' || s[i] <= 'z') && st.size()) {
                adj[j].push_back(s[i]);
            }

            else if ((s[i] >= 'a' || s[i] <= 'z') && st.size() == 0) {
                adj[j].push_back(s[i]);
                j++;
            }
        }
        this->j = j;
        dfs(0, "");
        sort(ans.begin(), ans.end());
        return ans;
    }   
   ```
26. Subset II
    ```
         void solve(vector<vector<int>>& ans, vector<int>& nums, int start, int end,
               vector<int> temp) {
        ans.push_back(temp);
        for (int i = start; i < end; i++) {
            if (i > start && nums[i] == nums[i - 1])
                continue;
            temp.push_back(nums[i]);
            solve(ans, nums, i + 1, end, temp);
            temp.pop_back();
        }
    }

    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<vector<int>> ans;
        vector<int> temp;
        sort(nums.begin(), nums.end());
        int n = nums.size();
        solve(ans, nums, 0, n, temp);

        return ans;
    }
    ```
27. Partition to k equal sum subsets
     ```
        bool backtrack(int inx, vector<int>& nums, vector<int>& sums,
                   int targetSum) {
        if (inx < 0)
            return true;
        for (int i = 0; i < sums.size(); i++) {
            if (sums[i] + nums[inx] > targetSum)
                continue;

            sums[i] += nums[inx];
            if (backtrack(inx - 1, nums, sums, targetSum))
                return true;
            sums[i] -= nums[inx];
            if (sums[i] == 0)
                break;
        }
        return false;
    }
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        int sum = 0;
        int n = nums.size();
  
        for (int i = 0; i < n; i++) {
            sum += nums[i];
           
        }

        if (sum % k != 0)
            return false;

        int targetSum = sum / k;
        sort(nums.begin(), nums.end());
        vector<int> sums(k, 0);
        return backtrack(n - 1, nums, sums, targetSum);
    }
     ```
    ```
        boolean backtrack(int inx, int[] nums, int[] sums,
            int targetSum) {
        if (inx < 0)
            return true;
        for (int i = 0; i < sums.length; i++) {
            if (sums[i] + nums[inx] > targetSum)
                continue;

            sums[i] += nums[inx];
            if (backtrack(inx - 1, nums, sums, targetSum))
                return true;
            sums[i] -= nums[inx];
            if (sums[i] == 0)
                break;
        }
        return false;
    }

    public boolean canPartitionKSubsets(int[] nums, int k) {
        int sum = 0;
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            sum += nums[i];

        }

        if (sum % k != 0)
            return false;

        int targetSum = sum / k;
        Arrays.sort(nums);
        int[] sums = new int[k];
        return backtrack(n - 1, nums, sums, targetSum);

    }
    ```
28. Path Sum 2
    ```
          public void solve(TreeNode root, int targetSum, List<Integer> temp, List<List<Integer>> ans, int sum) {
        if (root == null)
            return;
        temp.add(root.val);
        sum += root.val;

        if (root.left == null && root.right == null && sum == targetSum) {
            ans.add(new ArrayList<>(temp));
         
        }
        if (root.left != null) {
            solve(root.left, targetSum, temp, ans, sum);
        }
        if (root.right != null) {
            solve(root.right, targetSum, temp, ans, sum);
        }
        temp.remove(temp.size() - 1);

    }

    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {

        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        int s = 0;

        solve(root, targetSum, temp, ans, s);
        return ans;

    }
    ```
29. Word Ladder 2
     ```
       vector<vector<string>> findLadders(string beginWord, string endWord,
                                       vector<string>& wordList) {
        unordered_set<string> wordSet(wordList.begin(), wordList.end());

        if (!wordSet.count(endWord))
            return {};

        bfs(beginWord, endWord, wordSet);

        currPath = {endWord};
        backtrack(endWord, beginWord);

        return shortestPaths;
    }
    unordered_map<string, vector<string>> adjList;
    vector<string> currPath;
    vector<vector<string>> shortestPaths;
    vector<string> findNeighbors(string& word,
                                 unordered_set<string>& wordList) {
        vector<string> neighbors;
        string originalWord = word;

        for (int i = 0; i < word.size(); i++) {
            char originalChar = word[i];

            for (char c = 'a'; c <= 'z'; c++) {
                word[i] = c;

                if (c == originalChar || !wordList.count(word))
                    continue;

                neighbors.push_back(word);
            }

            word[i] = originalChar;
        }

        return neighbors;
    }
    void bfs(string& beginWord, string& endWord,
             unordered_set<string>& wordList) {
        queue<string> q;
        q.push(beginWord);

        unordered_map<string, bool> isEnqueued;
        isEnqueued[beginWord] = true;

        while (!q.empty()) {
            int size = q.size();
            vector<string> visitedInThisLevel;

            for (int i = 0; i < size; i++) {
                string currentWord = q.front();
                q.pop();

                vector<string> neighbors = findNeighbors(currentWord, wordList);
                for (string& neighbor : neighbors) {
                    adjList[neighbor].push_back(currentWord);

                    if (!isEnqueued[neighbor]) {
                        q.push(neighbor);
                        isEnqueued[neighbor] = true;
                    }

                    visitedInThisLevel.push_back(neighbor);
                }
            }

            for (string& word : visitedInThisLevel) {
                wordList.erase(word);
            }
        }
    }
    void backtrack(string& currentWord, string& beginWord) {
        if (currentWord == beginWord) {
            shortestPaths.push_back(
                vector<string>(currPath.rbegin(), currPath.rend()));
            return;
        }

        for (string& predecessor : adjList[currentWord]) {
            currPath.push_back(predecessor);
            backtrack(predecessor, beginWord);
            currPath.pop_back();
        }
    }
     ```
30. Combinations
      ```
          void solve(int s, int e, int k, vector<vector<int>>& ans,
               vector<int>& temp) {
        if (temp.size() == k) {
            ans.push_back(temp);
            return;
        }
        if (s > e)
            return;
        for (int i = s; i <= e; i++) {
            temp.push_back(i);
            solve(i + 1, e, k, ans, temp);
            temp.pop_back();
        }
    }

    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> ans;
        vector<int> temp;
        solve(1, n, k, ans, temp);
        return ans;
    }
      ```
31. Word Break 2
     ```
       void solve(const string& s, const set<string>& st, int start, int end,
               vector<string>& ans, string temp) {
        if (start > end) {
            ans.push_back(temp);
            return;
        }

        for (int i = start + 1; i <= end + 1; i++) {
            string s_temp = s.substr(start, i - start);
            if (st.find(s_temp) != st.end()) {
                string new_temp = temp;
                if (!new_temp.empty()) {
                    new_temp += " ";
                }
                new_temp += s_temp;
                solve(s, st, i, end, ans, new_temp);
            }
        }
    }

    vector<string> wordBreak(string s, vector<string>& wordDict) {
        set<string> st(wordDict.begin(), wordDict.end());
        vector<string> ans;
        solve(s, st, 0, s.length() - 1, ans, "");
        return ans;
    }
     ```
32. 24 Game
      ```
   
    vector<double> fill(double a, double b) {

        vector<double> v = {a + b, a - b, b - a, a * b};
        if (a != 0)
            v.push_back(b / a);
        if (b != 0)
            v.push_back(a / b);

        return v;
    }
    bool check(vector<double>& newl) {
        if (newl.size() == 1) {

            return abs(newl[0] - 24) <= 0.1;
        }
        for (int i = 0; i < newl.size(); i++) {
            for (int j = i + 1; j < newl.size(); j++) {
                vector<double> newList;
                for (int k = 0; k < newl.size(); k++) {
                    // pushing every element except i , j as we pus operation
                    // result of i,j
                    if (k != j && k != i) {
                        newList.push_back(newl[k]);
                    }
                }

                vector<double> res;

                res = fill(newl[i], newl[j]);
                for (int l = 0; l < res.size(); l++) {

                    newList.push_back(res[l]);

                    if (check(newList))
                        return true;

                    newList.pop_back();
                }
            }
        }
        return false;
       }
       bool judgePoint24(vector<int>& cards) {
        vector<double> newl(cards.begin(), cards.end());
        return check(newl);
       }

      ```
33. Letter Case Permutation
       ```
             void solve(int index, vector<string>& ans, string k) {
        if (index == k.size()) {
            ans.push_back(k);
            return;
        }

        if (isalpha(k[index])) {

            k[index] = tolower(k[index]);
            solve(index + 1, ans, k);

            k[index] = toupper(k[index]);
            solve(index + 1, ans, k);
        } else {

            solve(index + 1, ans, k);
        }
    }

    vector<string> letterCasePermutation(string s) {
        vector<string> ans;
        string k = s;
        solve(0, ans, k);
        return ans;
    }
       ```

