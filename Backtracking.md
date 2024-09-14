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
