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
