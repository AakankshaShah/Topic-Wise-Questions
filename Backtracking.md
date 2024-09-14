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
3. Permuattions of a string 
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
