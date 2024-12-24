# Dynamic Programming

## Theory
https://www.youtube.com/watch?v=nqowUJzG-iM&list=PL_z_8CaSLPWekqhdCPmFohncHwz8TY2Go&index=1

## Questions

1. 0/1 Knapsack 
   * Subset Sum
   * Equal Sum Parition
   * Count of Subset Sum
   * Minimum Subset Sum Diff
   * No of subset with given difference || 1049. Last Stone Weight II
   * Target Sum
   

```
# 0 1 Knapsack
int dp[n+1][w+1];


        for(int i=0;i<=n;i++)
        {
            for(int j=0;j<=w;j++)
            {
                if(j>0&&i==0)
                dp[i][j]=0;

                else if(j==0)
                dp[i][j]=1;

                else if(wt[i-1]<=j)
                dp[i][j]=max(dp[i-1][j],wt[i-1]+dp[i-1][j-wt[i-1]]);

                else
                dp[i][j]=dp[i-1][j];
            }
        }
        return dp[n][w];
```

```
# Target sum
 int sum =0;
    int n=nums.size();
   
   for (int i =0;i<n;i++){
       sum+=nums[i];
   }
   if(target>sum)
       return 0;
   if((target+sum)%2!=0)
       return 0;
   sum =  (sum -target)/2;
     

int dp[n+1][sum+1];

for(int i =0;i<n+1;i++){
dp[i][0] = 1;
}
for (int j =1;j<sum+1;j++){
dp[0][j] = 0;
}
for (int i =1;i<n+1;i++){
        for (int j=0;j<sum+1;j++){
            if (nums[i-1]<=j){
               dp[i][j] =  dp[i-1][j-nums[i-1]] + dp[i-1][j];
            }
            else{
               dp[i][j] = dp[i-1][j];
            }
        }
    }
    
   return dp[n][sum];
```
```
  bool canPartition(vector<int>& nums) {
        int n = nums.size();
        int s = 0;

        for (int i = 0; i < n; i++) {
            s += nums[i];
        }

        if (s % 2 == 1)
            return false;

        s = s / 2;

        bool dp[n + 1][s + 1];

        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= s; j++) {
                if (j > 0 && i == 0)
                    dp[i][j] = false;

                else if (j == 0)
                    dp[i][j] = true;

                else if (nums[i - 1] <= j) {
                    dp[i][j] = dp[i - 1][j - nums[i - 1]] || dp[i - 1][j];
                } else
                    dp[i][j] = dp[i - 1][j];
            }
        }
        return dp[n][s];
    }
```
```
   class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {

        int n = coins.size();
        int dp[n + 1][amount + 1];

        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= amount; j++) {

                if (i == 0&&j>0) {
                    dp[0][i] = INT_MAX - 1;
                } else if (j == 0) {
                    dp[i][j] = 0;

                } else if (i == 1) {
                    dp[i][j] = (j % coins[0] == 0) ? j / coins[0] : INT_MAX - 1;
                } else {
                    if (coins[i - 1] <= j) {
                        dp[i][j] =
                            min(dp[i - 1][j], dp[i][j - coins[i - 1]] + 1);
                    } else {
                        dp[i][j] = dp[i - 1][j];
                    }
                }
            }
        }

        if (dp[n][amount] == INT_MAX - 1)
            return -1;

        return dp[n][amount];
    }
};
```

2. Unbounded Knapsack
   * Coin Change
   * Coin Change II
   * Rod cutting
   * Ribbon cutting  
```
# Unbounded Knapsack
int dp[n+1][w+1];


        for(int i=0;i<=n;i++)
        {
            for(int j=0;j<=w;j++)
            {
                if(j>0&&i==0)
                dp[i][j]=0;

                else if(j==0)
                dp[i][j]=1;

                else if(wt[i-1]<=j)
                dp[i][j]=max(dp[i-1][j],wt[i-1d]+p[i][j-wt[i-1]]);

                else
                dp[i][j]=dp[i-1][j];
            }
        }
        return dp[n][w];
```

3. String DP 
    * Longest Common Subsequence

     ```
        int n=text1.length();
        int m=text2.length();


        int dp[n+1][m+1];


        for(int i=0;i<=n;i++)
        {
            for(int j=0;j<=m;j++)
            {
                if(i==0)
                dp[i][j]=0;

                else if(j==0)
                dp[i][j]=0;

                else if(text1[i-1]==text2[j-1])
                dp[i][j]=dp[i-1][j-1]+1;

                else
                dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
            }
        }
        return dp[n][m];

     ```

   * Longest Common Substring 

     ```
      int n = nums1.size();
        int m = nums2.size();

        int dp[n + 1][m + 1];
        int ans = 0;

        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= m; j++) {
                if (i == 0 || j == 0)
                    dp[i][j] = 0;

                else if (nums1[i - 1] == nums2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                    ans = max(ans, dp[i][j]);

                }

                else
                    dp[i][j] = 0;
            }
        }

        return ans;

     ```
    * Print longest common subsequence 
   
       
    ``` 
    string str1 = "havoc";
    string str2 = "bhvct";
    int m = str1.size();
    int n = str2.size();
    // fill the dp table
    int dp[m + 1][n + 1];
    for(int i = 0; i <= m; i++)
    {
        for(int j = 0; j <= n; j++)
        {
            if(i == 0 || j == 0)
                dp[i][j] = 0;
            else if(str1[i - 1] == str2[j - 1])
                dp[i][j] = dp[i - 1][j - 1] + 1;
            else
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
        }
    }
    string ans;
    int i = m, j = n;
    while(i > 0 && j > 0)
    {
        // If current character in both the strings are same, then current character is part of LCS
        if(str1[i - 1] == str2[j - 1])
        {
            ans.push_back(str1[i-1]);
            i--;
            j--;
        }
        // If current character in X and Y are different & we are moving upwards
        else if(dp[i - 1][j] > dp[i][j - 1])
        {
            i--;
        }
        // If current character in X and Y are different & we are moving leftwards
        else
        {
            j--;
        }
    }
    reverse(ans.begin(), ans.end());
    cout<<ans;

    ```
          
  

      
    * Shortest Common Supersequence 

    X's length + Y's length - LCS

    * Min number of insertion/deletions to make strings equal 

        ```
          int n=word1.length();
        int m=word2.length();

        int dp[n+1][m+1];


        for(int i=0;i<=n;i++)
        {
            for(int j=0;j<=m;j++)
            {
                if(i==0||j==0)
                dp[i][j]=0;

                else if(word1[i-1]==word2[j-1])
                dp[i][j]=dp[i-1][j-1]+1;

                else
                dp[i][j]=max(dp[i-1][j],dp[i][j-1]);

            }
        }

        int a=n-dp[n][m]; //Calculations length of string - LCS
        int b=m-dp[n][m];

        return (a+b);
    

        ```

   * Longest palindromic subsequence 
        
        Reverse given string and LCS

   * Minimum no of deletion in a string to make it palindrome

        String length - longest palindromic subsequence
   
   * Print shortest common supersequence 
     
    ```
    int n = str1.length();
        int m = str2.length();
        int dp[n + 1][m + 1];

        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= m; j++) {
                if (i == 0 || j == 0)
                    dp[i][j] = 0;

                else if (str1[i - 1] == str2[j - 1])
                    dp[i][j] = dp[i - 1][j - 1] + 1;

                else
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
        }

        string ans = "";

        int i = n;
        int j = m;

        while (i > 0 && j > 0) {
            if (str1[i - 1] == str2[j - 1]) {
                ans.push_back(str1[i - 1]);
                i--;
                j--;
            }

            else if (dp[i - 1][j] > dp[i][j - 1]) {
                ans.push_back(str1[i - 1]);
                i--;

            }

            else {
                ans.push_back(str2[j - 1]);
                j--;
            }
        }

        while (i > 0) {
            ans.push_back(str1[i - 1]);
            i--;
        }

        while (j > 0) {
            ans.push_back(str2[j - 1]);
            j--;
        }
        reverse(ans.begin(), ans.end());
        return ans;

     ```
    * Longest repeating subsequence 
        
         LCS with same string with this condition  if (i != j && s1[i - 1] == s2[j - 1])
   

4. Matrix chain multiplication
    ```
     int solve(vector<int>& nums, int i, int j) {
        if (i >=j)
            return 0;

        if (dp[i][j] != -1)
            return dp[i][j];

        int ans ;
        for (int k = i; k <= j; k++) {

            int tans = solve(nums, i, k ) + solve(nums, k + 1, j) +
                       arr[i-1]*arr[k]*arr[j];
            ans = min(ans, tans);
        }
        return dp[i][j] = ans;
    }

    ```


5. Burst Ballons ** https://www.youtube.com/watch?v=Yz4LlDSlkns

     int tans = solve(nums, i, k - 1) + solve(nums, k + 1, j) +
                       nums[i - 1] * nums[k] * nums[j + 1];
    ```
       int dp[301][301];

    int solve(vector<int>& nums, int i, int j) {
        if (i > j)
            return 0;

        if (dp[i][j] != -1)
            return dp[i][j];

        int ans = INT_MIN;
        for (int k = i; k <= j; k++) {

            int tans = solve(nums, i, k - 1) + solve(nums, k + 1, j) +
                       nums[i - 1] * nums[k] * nums[j + 1];
            ans = max(ans, tans);
        }
        return dp[i][j] = ans;
    }

    int maxCoins(vector<int>& nums) {

        nums.insert(nums.begin(), 1);
        nums.insert(nums.end(), 1);

        int n = nums.size();
        memset(dp, -1, sizeof(dp));

        int ans = solve(nums, 1, n - 2);

        return ans;
    }
    ```

6. Palindromic Parition

7. https://leetcode.com/problems/minimum-score-triangulation-of-polygon/description/

8. Evaluate expression to true boolean paratheses

9. Egg dropping 

10. Scrambled strings

11.  Print longest palindromic substring 
     
     ```
       int n = s.length();
        string s1 = s;
        string s2=s;
        reverse(s.begin(), s.end());

        int dp[n + 1][n + 1];
        int ans = 0;
        int pi = 0;
        int pj = 0;

        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= n; j++) {
                if (i == 0 || j == 0)
                    dp[i][j] = 0;

                else if (s1[i - 1] == s[j - 1])
                    dp[i][j] = dp[i - 1][j - 1] + 1;

                else
                    dp[i][j] = 0;

                if (dp[i][j] > ans) {
                    //Check again if palindrome or not 
                    string temp = s2.substr(i - dp[i][j], dp[i][j]);
                    string temp2 = temp;
                    reverse(temp2.begin(), temp2.end());
                    if (temp == temp2)
                    {    ans = dp[i][j];
                    pi = i;
                    pj = j;
                    }
                }
            }
        }
        string result = "";

        for (int i = pi, j = pj; i > 0 && j > 0;) {
            if (dp[i][j] > 0) {
                result.push_back(s1[i - 1]);
                i--;
                j--;
            } else {
                break;
            }
        }
        return result;
       }


      ```
      ```
         public String longestPalindrome(String s) {
        int n = s.length();
        String s1 = s;
        String s2 = new StringBuilder(s).reverse().toString(); // Reversed string

        int[][] dp = new int[n + 1][n + 1];
        int ans = 0;
        int pi = 0;
        int pj = 0;

        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= n; j++) {
                if (i == 0 || j == 0) {
                    dp[i][j] = 0;
                } else if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;

                    if (dp[i][j] > ans) {
                        String temp = s1.substring(i - dp[i][j], i); // Extract substring
                        String temp2 = new StringBuilder(temp).reverse().toString();
                        if (temp.equals(temp2)) {
                            ans = dp[i][j];
                            pi = i;
                            pj = j;
                        }
                    }
                } else {
                    dp[i][j] = 0;
                }
            }
        }

        StringBuilder result = new StringBuilder();
        for (int i = pi, j = pj; i > 0 && j > 0;) {
            if (dp[i][j] > 0) {
                result.append(s1.charAt(i - 1));
                i--;
                j--;
            } else {
                break;
            }
        }

        return result.reverse().toString();

    }
      ```

12.  Wildcard matching***  https://www.youtube.com/watch?v=ZmlQ3vgAOMo

     
```
        int n = s.length(), m = p.length();
        bool v[n + 1][m + 1];

        memset(v, false, sizeof(v));

        for (int i = 0; i <= n; i++) {
            v[i][0] = false;
        }
        v[0][0] = true;

        int st = 0;
        while (st < m && p[st] == '*') {
            v[0][++st] = true;
        }
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (s[i - 1] == p[j - 1] || p[j - 1] == '?') {
                    v[i][j] = v[i - 1][j - 1];
                } else if (p[j - 1] == '*') {
                    v[i][j] = v[i - 1][j] || v[i][j - 1];
                } else {
                    v[i][j] = false;
                }
            }
        }
        return v[n][m];
```


13. Unique paths
14. Decode paths Doubt 

15. Word Break vector<vector<int>> https://leetcode.com/problems/word-break/description/

 ```
  bool solve(string &s,int l, int r, unordered_map<string, bool> &dict, vector<vector<int>> &dp)
    {
           if(l>r)
            return false;
        
        if(dp[l][r]!=-1)
            return dp[l][r];
        
            if(dict[s.substr(l, r-l+1)]==true)
                return true;
      
        bool ans  =false;
        
        for(int k = l ; k<r; ++k)
        {
            ans|=(solve(s, l , k, dict, dp) && solve(s, k+1, r, dict, dp));
        }
        
        return dp[l][r] = ans;
    }



    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_map<string , bool> st;
        
        for(int i = 0 ; i<wordDict.size(); ++i)
            st[wordDict[i]] = true;
        st[""] = true;

        vector<vector<int>> dp(s.length()+1, vector<int>(s.length()+1, -1));
         return solve(s, 0, s.length(), st, dp);
        
    }
 ```
 ```
       bool solve(string& s, unordered_map<string, bool>& st, int idx,
               vector<int>& dp) {
        if (idx == s.size())
            return true;

        if (dp[idx] != -1)
            return dp[idx];

        for (int i = idx + 1; i <= s.size(); i++) {
            string t = s.substr(idx, i - idx);
            if (st.find(t) != st.end() && solve(s, st, i, dp)) {
                return dp[idx] = true;
            }
        }

        return dp[idx] = false;
    }

    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_map<string, bool> st;

        for (const string& word : wordDict)
            st[word] = true;

        vector<int> dp(s.size(), -1);

        return solve(s, st, 0, dp);
    }
 ```

16. House robbery 1 & 2
     ```
        int t[nums.size()+1];

        t[0]=0;
        t[1]=nums[0];

        for(int i=2;i<=nums.size();i++)
        {
            int steal=nums[i-1]+t[i-2];
            int skip=t[i-1];

            t[i]=max(steal,skip);
        }

      return t[nums.size()];

     ```
     ```
      int solve(vector<int>& nums){
        int n = nums.size();
        vector<int>dp(n+1,0);
        dp[0]=0;
        dp[1]=nums[0];
        for(int i = 2 ; i <=n ; i++){
            int incl = dp[i-2]+nums[i-1];
            int excl = dp[i-1];
            dp[i]= max(incl , excl);
        }
        return dp[n];
    }
    int rob(vector<int>& nums) {
        int n = nums.size();
        vector<int>first,second;
        if(n==1){
            return nums[0];
        }
        for(int i = 0 ; i < nums.size() ; i++){
            if(i!=nums.size()-1){
                first.push_back(nums[i]);
            }
            if(i!=0){
                second.push_back(nums[i]);
            }
        }
        return max(solve(first),solve(second)); 
        
    }
     ```
17. Ugly Numbers 2
    
     ```
       int arr[n + 1];

        arr[1] = 1;
        int i2 = 1;
        int i3 = 1;
        int i5 = 1;
        int vi2, vi3, vi5;

        for (int i = 2; i <= n; i++) {
            vi2 = arr[i2] * 2;
            vi3 = arr[i3] * 3;
            vi5 = arr[i5] * 5;
            arr[i] = std::min({vi2, vi3, vi5});

            if (vi2 == arr[i]) {

                i2++;
            }
            if (vi3 == arr[i]) {
                arr[i] = vi3;
                i3++;
            }
            if (vi5 == arr[i]) {
                arr[i] = vi5;
                i5++;
            }
        }

        return arr[n];
     ```

18. Count Palindromic substrings https://www.youtube.com/watch?v=XmSOWnL6T_I

    ```
            int n = s.size();
        bool dp[n][n];
        int cnt = 0;

        for (int g = 0; g < n; g++) { // Traversing by diagonal
            for (int i = 0, j = g; j < n; i++, j++) {
                if (g == 0) dp[i][j] = true;  
                else if (g == 1) { 
                    dp[i][j] = s[i] == s[j];
                }
                else {
                    if (s[i] == s[j] && dp[i + 1][j - 1] == true) dp[i][j] = true;
                    else dp[i][j] = false;
                }
                if (dp[i][j]) cnt++;
            }
        }
        return cnt;
     ```


19. Distinct palindromic substring 

     ```
        int n = s.size();
        bool dp[n][n];
        int cnt = 0;
        set<string> a;

        for (int g = 0; g < n; g++) { // Traversing by diagonal
            for (int i = 0, j = g; j < n; i++, j++) {
                if (g == 0)
                    dp[i][j] = true;
                else if (g == 1) {
                    dp[i][j] = s[i] == s[j];
                } else {
                    if (s[i] == s[j] && dp[i + 1][j - 1] == true)
                        dp[i][j] = true;
                    else
                        dp[i][j] = false;
                }
                if (dp[i][j]) {
                    a.insert(s.substr(i,j-i+1));
                   // cnt++;
                }
            }
        }
        return a.size();

      ```


20. Longest Arithmetic subsequence with given difference https://www.youtube.com/watch?v=OHxwaAbO1A8

     ```
       int longestSubsequence(vector<int>& arr, int difference) 
    {
        int n=arr.size();
        int res=0;
        unordered_map<int,int>mp;

        for(int i=0;i<n;i++)
        {
            //res=max(res,1+solve(arr,difference,i));
            int curr=arr[i];
            int prev=curr-difference;

            int curr_length=mp[prev]+1;
            mp[curr]=mp[prev]+1;
            res=max(res,mp[curr]);
        }
        return res;
    }

     ```



21. Arithmetic slices https://www.youtube.com/watch?v=D7PZZtvHnGo
     ```
      int n = nums.size();
        int curr = 0;
        int total = 0;
        for (int i = 2; i < n; i++) {
            if (nums[i - 1] - nums[i] == nums[i - 2] - nums[i - 1]) {
                curr++;
                total += curr;
            } else
                curr = 0;
        }

        return total;
    ```

22. Decode ways 
    ```
      int dp[101][101];

    bool check(string& s, int i, int j) {
        string temp = s.substr(i, j - i + 1);
        if (temp.size() > 2)

            return false;

        int y = stoi(temp);
        if (y > 26 || y <= 0 || (temp.size() >= 2 && y < 10))

            return false;

        return true;
    }
    int solve(string& s, int i, int j) {

        if (i > j)

            return 0;

        if (i == j)

            return 1;

        if (dp[i][j] != -1)

            return dp[i][j];

        int ans = 0;
        for (int k = i; k < j; k++) {

            if (check(s, i, k)) {
                cout << "DP k+1 j " << dp[k + 1][j];
                if (dp[k + 1][j] != -1) {
                    ans += dp[k + 1][j];
                } else {
                    dp[k + 1][j] = solve(s, k + 1, j);
                    ans += dp[k + 1][j];
                }
            }
        }

        return dp[i][j] = ans;
    }
    int numDecodings(string s) {
        memset(dp, -1, sizeof(dp));
        return solve(s, 0, s.size());
    }
    ```

23. Longest arithemtic subsequence
     ```
      int n;
    int t[1001][1003];

    int solve(vector<int>& nums, int i, int diff) {

        if (i < 0)
            return 0;

        if (t[i][diff + 501] != -1)
            return t[i][diff + 501];

        int ans = 0;

        for (int k = i - 1; k >= 0; k--) {

            if (nums[i] - nums[k] == diff)
                ans = max(ans, 1 + solve(nums, k, diff));
        }

        return t[i][diff + 501] = ans;
    }
    int longestArithSeqLength(vector<int>& nums) {
        n = nums.size();
        if (n <= 2)
            return n;

        memset(t, -1, sizeof(t));

        int result = 0;

        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {

                result = max(result, 2 + solve(nums, i, nums[j] - nums[i]));
            }
        }

        return result;
    }
     ```
    ```
      int longestArithSeqLength(vector<int>& nums) {
        int n = nums.size();

        if (n <= 2)
            return n;

        vector<vector<int>> t(n, vector<int>(1001, 0));
        // t[i][j] = Max AP length till ith index (0 to i) having common
        // difference j

        int result = 0;

        for (int i = 1; i < n; i++) {

            for (int j = 0; j < i; j++) {

                int diff = nums[i] - nums[j] + 500; // to avoid negative diff

                t[i][diff] = t[j][diff] > 0 ? t[j][diff] + 1 : 2;

                result = max(result, t[i][diff]);
            }
        }

        return result;
    }
    ```

24. Min path sum 
    ```
      int minPathSum(vector<vector<int>>& grid) {
        if (grid.empty() || grid[0].empty()) {
            return 0;
        }

        int m = grid.size();
        int n = grid[0].size();
        vector<vector<int>> t(m, vector<int>(n, 0));

        t[0][0] = grid[0][0];

        for (int j = 1; j < n; ++j) {
            t[0][j] = t[0][j - 1] + grid[0][j];
        }

        for (int i = 1; i < m; ++i) {
            t[i][0] = t[i - 1][0] + grid[i][0];
        }

        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                t[i][j] = min(t[i - 1][j], t[i][j - 1]) + grid[i][j];
            }
        }

        return t[m - 1][n - 1];
    }
    ```
25. Palindrome partition
     ```
       bool ispalindrome(string s, int start, int end) {
        while (start < end) {
            if (s[start] != s[end])
                return false;
            start++;
            end--;
        }
        return true;
    }
    int solve(string s, int i, int j) {
        if (i >= j)
            return 0;
        if (ispalindrome(s, i, j))
            return 0;
        int ans = INT_MAX;
        for (int k = i; k < j; k++) {
            ans = min(1 + solve(s, i, k) + solve(s, k + 1, j), ans);
        }
        return ans;
    }
    int minCut(string s) {
        if (s.length() == 0 || s.length() == 1)
            return 0;

        return solve(s, 0, s.length()-1);
    }
     ```
26. Dungeon games
    ```
      int getVal(vector<vector<int>>& mat, vector<vector<int>>& dp, int i = 0,
               int j = 0) {
        int n = mat.size();
        int m = mat[0].size();

        if (i == n || j == m)
            return 1e9;

        if (i == n - 1 and j == m - 1)
            return (mat[i][j] <= 0) ? -mat[i][j] + 1 : 1;

        if (dp[i][j] != 1e9)
            return dp[i][j];

        int IfWeGoRight = getVal(mat, dp, i, j + 1);
        int IfWeGoDown = getVal(mat, dp, i + 1, j);

        int minHealthRequired = min(IfWeGoRight, IfWeGoDown) - mat[i][j];

        dp[i][j] = (minHealthRequired <= 0) ? 1 : minHealthRequired;
        return dp[i][j];
    }
    int calculateMinimumHP(vector<vector<int>>& dungeon) {
        int n = dungeon.size();
        int m = dungeon[0].size();

        vector<vector<int>> dp(n, vector<int>(m, 1e9));

        return getVal(dungeon, dp);
    }
    ```
27. 2 key keyboard
   ```
     int f(int num, int lastcp, int tar, vector<vector<int>> &dp) {
    if (num == tar) return 0;
    if (num + lastcp > tar) return 2000;
    
    if (dp[num][lastcp] != -1) return dp[num][lastcp];
    int cp = 2 + f(num + num, num, tar, dp);
    int p = 1 + f(num + lastcp, lastcp, tar, dp);

    return dp[num][lastcp] = min(cp, p);
    }
    int minSteps(int n) {
        if (n == 1) return 0;
        vector<vector<int>> dp(n + 1, vector<int>(n, -1));
        return f(2, 1, n, dp) + 2;
    }
   ```
28. 4 key keyboard

```
\\Ctrl+ACV (3 key presses) gives 2 times increase
Ctrl+ACVV (4 key presses) gives 3 times increase
Ctrl+ACVVV (5 key pressses) gives 4 times increase
Ctrl+ACVVVV (6 key presses) gives 5 times increase

```

 ```
        int memo[51];
    int dfs(int n) {
        if (n <= 0)
            return 0;
        if (memo[n] != -1)
            return memo[n];
        int ans = 0;
        ans = max(ans, dfs(n - 1) + 1);
        ans = max(ans, dfs(n - 3) * 2);
        ans = max(ans, dfs(n - 4) * 3);
        ans = max(ans, dfs(n - 5) * 4);
        return memo[n] = ans;
    }
    int maxA(int n) {

        memset(memo, -1, sizeof(int) * 51);
        return dfs(n);
    }
  ```
29. Stone Game IV
    ```
      vector<bool> dp(n + 1, false);
        for (int i = 1; i <= n; ++i) {
            for (int k = 1; k * k <= i; ++k) {
                if (!dp[i - k * k]) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[n];
    ```

30. Max number of operations with same score II
    ```
                vector<vector<int>>& dp) {
        if (l >= r) {
            return 0;
        }
        if (dp[l][r] != -1) {
            return dp[l][r];
        }
        int mx = 0;
        if (nums[l] + nums[r] == sum) {
            mx = max(mx, 1 + solve(l + 1, r - 1, sum, nums, dp));
        }
        if (nums[l] + nums[l + 1] == sum) {
            mx = max(mx, 1 + solve(l + 2, r, sum, nums, dp));
        }
        if (nums[r - 1] + nums[r] == sum) {
            mx = max(mx, 1 + solve(l, r - 2, sum, nums, dp));
        }
        return dp[l][r] = mx;
    }

    int maxOperations(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> dp(n + 1, vector<int>(n + 1, -1));
        vector<vector<int>> dp1(n + 1, vector<int>(n + 1, -1));
        vector<vector<int>> dp2(n + 1, vector<int>(n + 1, -1));
        int mx = 1 + solve(1, n - 2, nums[0] + nums[n - 1], nums, dp);
        mx = max(mx, 1 + solve(2, n - 1, nums[0] + nums[1], nums, dp1));
        mx = max(mx, 1 + solve(0, n - 3, nums[n - 1] + nums[n - 2], nums, dp2));
        return mx;
    }
    ```
31. Optimal Partition of string 
    ```
      int partitionString(string s) {
        vector<int> dp(s.size() + 1, 0);
        string curr = "";
        for (int i = 1; i <= s.size(); i++) {
            if (find(curr.begin(), curr.end(), s[i - 1]) != curr.end()) {
                curr = s[i - 1];
                dp[i] = 1 + dp[i - 1];
            } else {
                curr = curr + s[i - 1];
                dp[i] = dp[i - 1];
            }
        }
        return dp[s.size()] + 1;
    }
    ```
32. Minimum Number of Increments on Subarrays to Form a Target Array
    ```
      int minNumberOperations(vector<int>& target) {

        long long result = 0;
        int n=target.size();

        int curr = 0;
        int prev = 0;

        for(int i = 0; i < n; i++) {
            curr = target[i];

            if(abs(curr) > abs(prev)) {
                result += abs(curr - prev);
            }

            prev = curr;
        }

        return result;
        
    }
    ```
33. Minimum Operations to Make Array Equal to Target
    ```
      long long minimumOperations(vector<int>& nums, vector<int>& target) {
        int n = nums.size();

        long long result = 0;

        int curr = 0;
        int prev = 0;

        for (int i = 0; i < n; i++) {
            curr = target[i] - nums[i];

            if ((curr > 0 && prev < 0) || (curr < 0 && prev > 0)) {
                result += abs(curr);
            } else if (abs(curr) > abs(prev)) {
                result += abs(curr - prev);
            }

            prev = curr;
        }

        return result;
    }
    ```
34. Minimum falling path sum
     ```
     //TLE
       int MFS(vector<vector<int>>& A, int row, int col, vector<vector<int>>& t) {
        if (row == A.size() - 1)
            return A[row][col];
        if (t[row][col] != -1)
            return t[row][col];

        int minSum = INT_MAX;

        for (int shift = -1; shift <= 1; shift++) {
            if (col + shift >= 0 && col + shift < A[row].size()) {
                minSum =
                    min(minSum, A[row][col] + MFS(A, row + 1, col + shift, t));
            }
        }
        return t[row][col] = minSum;
    }
    int minFallingPathSum(vector<vector<int>>& A) {
        int m = A.size(); // row
        int n = m;        // column
        vector<vector<int>> t(101, vector<int>(101));
        for (int i = 0; i < 101; i++) {
            for (int j = 0; j < 101; j++) {
                t[i][j] = -1;
            }
        }
        int result = INT_MAX;
        for (int col = 0; col < n; col++) {
            result = min(result, MFS(A, 0, col, t));
        }
        return result;
    }
     ```
     ```
       int minFallingPathSum(vector<vector<int>>& A) {
        int n = A.size();
        vector<int> prev(n);
        for(int col = 0; col<n; col++)
            prev[col] = A[0][col];
        
        for(int row = 1; row<n; row++) {
            vector<int> curr(n);
            for(int col = 0; col<n; col++) {
                curr[col] = A[row][col] + min({prev[max(0, col-1)],  prev[col],  prev[min(n-1, col+1)]});
            }
            prev = curr;
        }
        return *min_element(prev.begin(), prev.end());
    }
     ```
35.  Maximum Number of Points with Cost

      ``` 
         //TLE
         typedef long long ll;
         ll maxPoints(vector<vector<int>>& points) {
        int m = points.size();
        int n = points[0].size();
        vector<ll> prev(n);
        int score = 0;

        for (int col = 0; col < n; col++) {
            prev[col] = points[0][col];
        }

        for (int i = 1; i < m; i++) {
            vector<ll> curr(n);
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < n; k++) {
                    curr[j] = max(curr[j], prev[k] + points[i][j] - abs(k - j));
                }
            }
            prev = curr;
        }
        return *max_element(prev.begin(), prev.end());
        }

      ```

      ```
     typedef long long ll;
      ll maxPoints(vector<vector<int>>& points) {
        int m = points.size(), n = points[0].size();
        vector<ll> prev(n);
        int score = 0;

        for (int col = 0; col < n; col++) {
            prev[col] = points[0][col];
        }

        for (int i = 1; i < m; i++) {
            vector<ll> curr(n);
            auto left = curr, right = curr;
            left[0] = prev[0];
            for (int j = 1; j < n; j++) {
                left[j] =
                    max(prev[j],
                        left[j - 1] - 1); // points[i][j] will be added later
            }

            // Fill right
            right[n - 1] = prev[n - 1];
            for (int j = n - 2; j >= 0; j--) {
                right[j] =
                    max(prev[j],
                        right[j + 1] - 1); // points[i][j] will be added later
            }

            for (int j = 0; j < n; j++)
                curr[j] = points[i][j] +
                          max(left[j], right[j]); // points[i][j] added here

            prev = curr;
        }
        return *max_element(prev.begin(), prev.end());
        }
       ```

36. Edit distance

     ```
      int solve(string& a, string& b, int i, int j) {
        // Base case
        if (i == a.length()) {
            return b.length() - j;
        }
        if (j == b.length()) {
            return a.length() - i;
        }

        int ans = 0;
        if (a[i] == b[j])
            return solve(a, b, i + 1, j + 1);
        else {
            // insert
            int insertans = 1 + solve(a, b, i, j + 1);

            // delete
            int deleteans = 1 + solve(a, b, i + 1, j);

            // replace
            int replaceans = 1 + solve(a, b, i + 1, j + 1);

            ans = min(insertans, min(deleteans, replaceans));
        }
        return ans;
    }
    int minDistance(string word1, string word2) {
        return solve(word1, word2, 0, 0);
    }
     ```

     ```
       int solveMem(string &a,string &b,int i,int j,vector<vector<int>> &dp)
    {
        //Base case
        if(i==a.length())
        {
            return b.length()-j;
        }
        if(j==b.length())
        {
            return a.length()-i;
        }

        if(dp[i][j]!=-1) return dp[i][j];

        int ans=0;
        if(a[i]==b[j]) return solveMem(a,b,i+1,j+1,dp);
        else
        {
            //insert
            int insertans=1+solveMem(a,b,i,j+1,dp);

            //delete
            int deleteans=1+solveMem(a,b,i+1,j,dp);

            //replace
            int replaceans=1+solveMem(a,b,i+1,j+1,dp);

            ans=min(insertans,min(deleteans,replaceans));
        }
        return dp[i][j]=ans;
    }      
    int minDistance(string word1, string word2) {
         int n=word1.length();
         int m=word2.length();

         vector<vector<int>> dp(n,vector<int> (m,-1));
         return solveMem(word1,word2,0,0,dp);
        
    }
     ```
37. Best time to buy & sell stocks

    ```
      int maxProfit(vector<int>& prices) {

        int profit = 0;
        int buy = prices[0];

        for (int i = 1; i < prices.size(); i++) {
            if (buy < prices[i]) {
                profit += prices[i] - buy;
            }
            buy = prices[i];
        }
        return profit;
    }
    ```
38. Palindrome partition
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
  39. Maximal square

      ```
         int findMaxSquare(vector<vector<char>>& matrix, int row, int col, int &maxSideLength) {
        if (row >= matrix.size() || col >= matrix[0].size()) return 0;

        int right = findMaxSquare(matrix, row, col + 1, maxSideLength);
        int diagonal = findMaxSquare(matrix, row + 1, col + 1, maxSideLength);
        int down = findMaxSquare(matrix, row + 1, col, maxSideLength);

        if (matrix[row][col] == '1') {
            int currentSideLength = 1 + min({right, diagonal, down});
            maxSideLength = max(maxSideLength, currentSideLength);
            return currentSideLength;
        }
        return 0;
       }

       int maximalSquare(vector<vector<char>>& matrix) {
        int maxSideLength = 0;
        findMaxSquare(matrix, 0, 0, maxSideLength);
        return maxSideLength * maxSideLength;
        }
       ```


   ```
    int findMaxSquare(vector<vector<char>>& matrix, int row, int col, int &maxSideLength,vector<vector<int>>&memo) {
        if (row >= matrix.size() || col >= matrix[0].size()) return 0;
        if(memo[row][col]!=-1) return memo[row][col];
        int right = findMaxSquare(matrix, row, col + 1, maxSideLength,memo);
        int diagonal = findMaxSquare(matrix, row + 1, col + 1, maxSideLength,memo);
        int down = findMaxSquare(matrix, row + 1, col, maxSideLength,memo);

        if (matrix[row][col] == '1') {
            int currentSideLength = 1 + min({right, diagonal, down});
            memo[row][col] = currentSideLength ;
            maxSideLength = max(maxSideLength, currentSideLength);
            return memo[row][col] ;
        }
        return 0;
    }

    int maximalSquare(vector<vector<char>>& matrix) {
        int maxSideLength = 0;
        vector<vector<int>>memo(matrix.size(),vector<int>(matrix[0].size(),-1));
        findMaxSquare(matrix, 0, 0, maxSideLength,memo);
        return maxSideLength * maxSideLength;
    }
   ```
40. Fibonacci number
     ```
        int fib(int n) {
        if(n<=1)
        return n;
        int dp[n+1];
        dp[0]=0;
        dp[1]=1;
        for(int i=2;i<=n;i++)
        {
            dp[i]=dp[i-1]+dp[i-2];
        }
        return dp[n];
        
    }
     ```
41. Filling bookshelf
     ```
        int n;
    int WIDTH;
    int t[1001][1001];
    int solve(vector<vector<int>>& books, int i, int remainW, int maxHt) {
        if (i >= n) {
            return maxHt;
        }

        if (t[i][remainW] != -1) {
            return t[i][remainW];
        }

        int bookW = books[i][0];
        int bookH = books[i][1];

        int keep = INT_MAX;
        int skip = INT_MAX;

        // keep
        if (bookW <= remainW) { // let's keep it here
            keep = solve(books, i + 1, remainW - bookW, max(maxHt, bookH));
        }

        // skip and put in next shelf
        skip = maxHt + solve(books, i + 1, WIDTH - bookW, bookH);

        return t[i][remainW] = min(keep, skip);
    }
    int minHeightShelves(vector<vector<int>>& books, int shelfWidth) {
        memset(t, -1, sizeof(t));
        n = books.size();
        WIDTH = shelfWidth;

        int remainW = shelfWidth;
        return solve(books, 0, remainW, 0);
    }
     ```
42. Regular expression matching
    ```
        bool isMatch(string text, string pattern) {
        if (pattern.length() == 0) {
            return text.length() == 0;
        }

        bool first_char_matched = false;
        if(text.length() > 0 && (pattern[0] == text[0] || pattern[0] == '.')) {
            first_char_matched = true;
        }
        
        //Best example to understand : s = "aaab", p = "a*b"
        if (pattern.length() >= 2 && pattern[1] == '*') {
            return (isMatch(text, pattern.substr(2)) ||
                    (first_char_matched && isMatch(text.substr(1), pattern)));
        } else {
            return first_char_matched && isMatch(text.substr(1), pattern.substr(1));
        }
    }
    int t[21][21];

    bool solve(int i, int j, string& text, string& pattern) {
        if (j == pattern.length())
            return i == text.length();
            
        if (t[i][j] != -1) {
            return t[i][j];
        }
        
        bool ans = false;

        bool first_match = (i < text.length() &&
                            (pattern[j] == text[i] || pattern[j] == '.'));

        if (j + 1 < pattern.length() && pattern[j + 1] == '*') {
            ans = (solve(i, j + 2, text, pattern) ||
                   (first_match && solve(i + 1, j, text, pattern)));
        } else {
            ans = first_match && solve(i + 1, j + 1, text, pattern);
        }

        return t[i][j] = ans;
    }
    
    bool isMatch(string text, string pattern) {
        memset(t, -1, sizeof(t));
        return solve(0, 0, text, pattern);
    }
    ```
    ```
       int[][] t = new int[21][21]; // Memoization table

    boolean solve(int i, int j, String text, String pattern) {

        if (j == pattern.length()) {
            return i == text.length();
        }

        if (t[i][j] != -1) {
            return t[i][j] == 1;
        }

        boolean ans = false;

        boolean firstMatch = (i < text.length() &&
                (pattern.charAt(j) == text.charAt(i) || pattern.charAt(j) == '.'));

        if (j + 1 < pattern.length() && pattern.charAt(j + 1) == '*') {
            ans = (solve(i, j + 2, text, pattern) ||
                    (firstMatch && solve(i + 1, j, text, pattern)));
        } else {
            ans = firstMatch && solve(i + 1, j + 1, text, pattern);
        }

        t[i][j] = ans ? 1 : 0;
        return ans;
    }

    public boolean isMatch(String s, String p) {

        for (int[] row : t) {
            java.util.Arrays.fill(row, -1);
        }

     
        return solve(0, 0, s, p);
    }
    ```
43. No of unique subarrays

    <img width="797" alt="image" src="https://github.com/user-attachments/assets/a0cb2fa2-a3d2-4727-8630-e4c639b7d380">


     ```
            int numberOfUniqueGoodSubsequences(string binary) {
        bool hasZero = false;
        int endWithZeroes=0,endWithOnes=0,mod=1e9+7;
        
        for(auto b:binary){
            if(b=='1'){
                endWithOnes = (endWithOnes + endWithZeroes + 1)%mod;
            }else{
                hasZero=true;
                endWithZeroes = (endWithOnes + endWithZeroes)%mod;
            }
        }
        
        return (endWithOnes + endWithZeroes + hasZero)%mod;
        
    }
     ```
44. Count number of teams
     ```
       int numTeams(vector<int>& rating) {
         int n = rating.size();

        int teams = 0;

        for(int j = 1; j < n-1; j++) {

            int countSmallerLeft = 0;
            int countLargerLeft  = 0;
            int countSmallerRight = 0;
            int countLargerRight = 0;

            for(int i = 0; i < j; i++) {
                if(rating[i] < rating[j]) {
                    countSmallerLeft++;
                } else if(rating[i] > rating[j]) {
                    countLargerLeft++;
                }
            }

            for(int k = j+1; k < n; k++) {
                if(rating[j] < rating[k]) {
                    countLargerRight++;
                } else if(rating[j] > rating[k]) {
                    countSmallerRight++;
                }
            }

            teams += (countLargerLeft * countSmallerRight) + (countSmallerLeft * countLargerRight);


        }

        return teams;

        
    }
     ```
45. All possible full binary tree
     ```
        unordered_map<int, vector<TreeNode*>> mp;
    vector<TreeNode*> solve(int n) {

        if (n % 2 == 0) // Even nodes can't make Full Binary Tree
            return {};

        if (n == 1) {
            TreeNode* node = new TreeNode(0);
            return {node};
        }

        if (mp.find(n) != mp.end())
            return mp[n];

        vector<TreeNode*> result;

        for (int i = 1; i < n; i += 2) {

            vector<TreeNode*> leftAllFBT = allPossibleFBT(i);
            vector<TreeNode*> rightAllFBT = allPossibleFBT(n - i - 1);

            for (auto& l : leftAllFBT) {

                for (auto& r : rightAllFBT) {

                    TreeNode* root = new TreeNode(0);
                    root->left = l;
                    root->right = r;
                    result.push_back(root);
                }
            }
        }

        return mp[n] = result;
    }

    vector<TreeNode*> allPossibleFBT(int n) { return solve(n); }
     ```
46. Unique binary search tree
    ```
       //Part II
        vector<TreeNode*> solve(int start, int end) {

        if (start > end) {
            return {NULL};
        }

        if (start == end) {
            TreeNode* root = new TreeNode(start);
            return {root};
        }

        vector<TreeNode*> result;
        for (int i = start; i <= end; i++) {

            vector<TreeNode*> leftList = solve(start, i - 1);
            vector<TreeNode*> rightList = solve(i + 1, end);

            for (TreeNode* leftRoot : leftList) {

                for (TreeNode* rightRoot : rightList) {

                    TreeNode* root = new TreeNode(i);
                    root->left = leftRoot;
                    root->right = rightRoot;

                    result.push_back(root);
                }
            }
        }

        return result;
    }

    vector<TreeNode*> generateTrees(int n) { return solve(1, n); }

    ```
   ```
     //Part 1
        For a given number of nodes i (where i >= 2), the function considers each value j (from 1 to i) as the root. It calculates the number of BSTs possible by multiplying:

       The number of BSTs that can be formed with j-1 nodes (i.e., nodes less than j) on the left subtree: sol[j-1].
       The number of BSTs that can be formed with i-j nodes (i.e., nodes greater than j) on the right subtree: sol[i-j].
       So, for each root j, the number of BSTs is sol[j-1] * sol[i-j].
       int numTrees(int n) {
       
        std::vector<int> sol(n + 1, 0);
        sol[0] = sol[1] = 1;

        // Run a loop from 2 to n...
        for (int i = 2; i <= n; i++) {
            // Within the above loop, run a nested loop from 1 to i...
            for (int j = 1; j <= i; j++) {
                // Update the i-th position of the vector by adding the multiplication of the respective index...
                sol[i] += sol[i - j] * sol[j - 1];
            }
        }
        // Return the value of the nth index of the vector to get the solution...
        return sol[n];
    }
   ```
47. Perfect squares
    ```
       int numSquares(int n) {
        std::vector<int> power(201), dp(n + 1);

        for (int i = 0; i < 200; i++) {
            power[i] = (i + 1) * (i + 1);
        }

        for (int i = 1; i <= n; i++) {
            dp[i] = INT_MAX;
            int j = 0;

            while (power[j] <= i) {
                dp[i] = min(dp[i], dp[i - power[j]] + 1);
                j++;
            }
        }

        return dp[n];
    }
    ```
48. Minimum number to eat n oranges
     ```
        int solve(int n) {
       if(n==0)return 0;
        int a = 1+solve(n-1);
        int b = INT_MAX,c=INT_MAX;
        if(n%2==0)
        b = 1+solve(n/2);
        if(n%3==0)
        c = 1+solve(n-(n/3)*2);
        return min(a,min(b,c));
        
    }
    int minDays(int n) { return solve(n); }
     ```
     ```
           {
        if(n==0)return 0;
        if(n==1)return 1;
        if(dp.find(n)!= dp.end()) return dp[n];
  
        int x = n%2 + recurdp(n/2,dp);
        int y = n%3 + recurdp(n/3,dp);

        return dp[n] = 1+min(x,y);
    }
    int minDays(int n) {
        unordered_map<int,int>dp;
        return recurdp(n,dp);
    }
     ```
49. Min cost for tickets
     ```
        int ans = 0;
    int solve(vector<int>& days, vector<int>& costs, int i) {
        int n = days.size();
        if (i >= days.size())
            return 0;
        int c1 = costs[0] + solve(days, costs, i + 1);
        int j = i;
        int md = days[i] + 7;
        while (j < n && days[j] < md)
            j++;
        int c7 = costs[1] + solve(days, costs, j);
        j = i;
        md = days[i] + 30;
        while (j < n && days[j] < md)
            j++;
        int c30 = costs[2] + solve(days, costs, j);
        return min({c1, c7, c30});
    }

    int mincostTickets(vector<int>& days, vector<int>& costs) {

        return solve(days, costs, 0);
    }
     ```
     ```
        int t[366];
    int memoized(vector<int>& days, vector<int>& costs, int& n, int idx) {
        if (idx >= n)
            return 0; // you can't travel, so no cost

        if (t[idx] != -1)
            return t[idx];

        // if i take 1-day pass at idx
        int cost_1 = costs[0] + memoized(days, costs, n, idx + 1);

        // if i take 7-day pass at idx
        int i = idx;
        while (i < n && days[i] < days[idx] + 7) {
            i++;
        }
        int cost_7 = costs[1] + memoized(days, costs, n, i);

        // if i take 30-day pass at idx
        int j = idx;
        while (j < n && days[j] < days[idx] + 30) {
            j++;
        }
        int cost_30 = costs[2] + memoized(days, costs, n, j);

        return t[idx] = min({cost_1, cost_7, cost_30});
    }

    int mincostTickets(vector<int>& days, vector<int>& costs) {
        memset(t, -1, sizeof(t));
        int n = days.size();
        return memoized(days, costs, n, 0);
    }
     ```
     ```
        int bottomUp(vector<int>& days, vector<int>& costs) {
        unordered_set<int> st(begin(days), end(days));
        int last_day = days.back();
        
        vector<int> t(last_day+1, 0);
        //t[i] = min cost to reach till day i of your travel plan
        t[0] = 0; //since, on day 0, we spend cost = 0
        
        for(int i = 1; i<=last_day; i++) {
            if(st.find(i) == st.end()) {
                t[i] = t[i-1]; //skip
                continue;
            }
            //you need to filter those days from 1 to 30 which are not in days vector
            t[i] = INT_MAX;
            
            //I come to i either by taking a 1-day pass from jth day
            int day_1_pass = t[max(0, i-1)] + costs[0];

            //I come to i either by taking a 7-day pass from jth day
            int day_7_pass = t[max(0,i-7)] + costs[1];

            //I come to i either by taking a 30-day pass from jth day
            int day_30_pass = t[max(0, i-30)] + costs[2];
            
            t[i] = min({day_1_pass, day_7_pass, day_30_pass});
        }
        
        return t[last_day]; //minimum cost to travel till my last planned day
    }
    
    int mincostTickets(vector<int>& days, vector<int>& costs) {
        return bottomUp(days, costs);     
    }
     ```
     ```
       class Solution {
    int t[] = new int[366];

    int memoized(int[] days, int[] costs, int n, int idx) {
        if (idx >= n)
            return 0;

        if (t[idx] != -1)
            return t[idx];

        int cost_1 = costs[0] + memoized(days, costs, n, idx + 1);

        int i = idx;
        while (i < n && days[i] < days[idx] + 7) {
            i++;
        }
        int cost_7 = costs[1] + memoized(days, costs, n, i);

        int j = idx;
        while (j < n && days[j] < days[idx] + 30) {
            j++;
        }
        int cost_30 = costs[2] + memoized(days, costs, n, j);

        return t[idx] = Math.min(Math.min(cost_1, cost_7), cost_30);
    }

    public int mincostTickets(int[] days, int[] costs) {
        // memset(t, -1, sizeof(t));
        Arrays.fill(t, -1);
        int n = days.length;
        return memoized(days, costs, n, 0);

    }
     ```
50. Best time to buy sell stocks with cooldown
     ```
        int t[5001][2];
    int maxP(vector<int>& prices, int day, int n, int buy) {
        if (day >= n)
            return 0;

        int profit = 0;
        if (t[day][buy] != -1) {
            return t[day][buy];
        }
        // buy
        if (buy) {
            int consider = maxP(prices, day + 1, n, false) - prices[day];
            int not_consider = maxP(prices, day + 1, n, true);
            profit = max({profit, consider, not_consider});
        } else { // sell
            int consider = maxP(prices, day + 2, n, true) + prices[day];
            int not_consider = maxP(prices, day + 1, n, false);
            profit = max({profit, consider, not_consider});
        }

        return t[day][buy] = profit;
    }
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        memset(t, -1, sizeof(t));
        return maxP(prices, 0, n, true);
    }
     ```
51. Number of Ways to Rearrange Sticks With K Sticks Visible
    ```
         int mod = 1e9 + 7;
    int dp[1001][1001];
    long long f(int n, int k) {
        if (k == 0 || k > n)
            return 0;
        if (n <= 2)
            return 1;
        if (dp[n][k] != -1)
            return dp[n][k];

        ll ans = 0;

        ans = (ans + f(n - 1, k - 1)) %
              mod; // tallest stick at rightmost position
        ans = (ans + (n - 1) * f(n - 1, k)) %
              mod; // non-tallest stick at rightmost position

        return dp[n][k] = ans;
    }
    int rearrangeSticks(int n, int k) {
        memset(dp, -1, sizeof(dp));
        return f(n, k);
    }
    ```
52. Number of LIS
     ```
        int findNumberOfLIS(vector<int>& arr) {
        int n = arr.size();
        vector<int> dp(n, 1);
        vector<int> cnt(n, 1);
        int maxi = 1;

        for (int i = 0; i < n; i++) {
            for (int prev = 0; prev < i; prev++) {
                if (arr[prev] < arr[i] && 1 + dp[prev] > dp[i]) {

                    dp[i] = dp[prev] + 1;
                    cnt[i] = cnt[prev];
                }

                else if (arr[prev] < arr[i] && 1 + dp[prev] == dp[i]) {
                    cnt[i] += cnt[prev];
                }
            }
            maxi = max(maxi, dp[i]);
        }
        int totalCount = 0;
        for (int i = 0; i < n; i++) {
            if (dp[i] == maxi) {
                totalCount += cnt[i];
            }
        }
        return totalCount;
    }
     ```
     ```
       int n = arr.length;
        int[] dp = new int[n];
        int[] cnt = new int[n];
        Arrays.fill(dp, 1);
        Arrays.fill(cnt, 1);

        int maxi = 1;

        for (int i = 0; i < n; i++) {
            for (int prev = 0; prev < i; prev++) {
                if (arr[prev] < arr[i] && 1 + dp[prev] > dp[i]) {

                    dp[i] = dp[prev] + 1;
                    cnt[i] = cnt[prev];
                }

                else if (arr[prev] < arr[i] && 1 + dp[prev] == dp[i]) {
                    cnt[i] += cnt[prev];
                }
            }
            maxi = Math.max(maxi, dp[i]);
        }
        int totalCount = 0;
        for (int i = 0; i < n; i++) {
            if (dp[i] == maxi) {
                totalCount += cnt[i];
            }
        }
        return totalCount;
     ```
53.  Length of the Longest Subsequence That Sums to Target

   ```
     int dp[1001][1001];
    int solve(int ind, int target, vector<int>& nums, int n) {
        if (target < 0)
            return -1e5;
        if (target == 0) {
            return 0;
        }
        if (ind == n)
            return -1e5;
        if (dp[ind][target] != -1)
            return dp[ind][target];
        int nontake = solve(ind + 1, target, nums, n);
        int take = 1 + solve(ind + 1, target - nums[ind], nums, n);
        return dp[ind][target] = max(take, nontake);
    }
    int lengthOfLongestSubsequence(vector<int>& nums, int target) {
        int n = nums.size();
        memset(dp, -1, sizeof(dp));
        int ans = solve(0, target, nums, n);
        if (ans >= -1e5 && ans <= -1e4)
            return -1;
        return ans;
    }
   ```
54. Max score difference in a grid **
    ```
       vector<vector<ll>> dp;
    vector<vector<int>> grid;
    int rows, cols;
    ll MaxScore(int r, int c) {
        if (r == rows - 1 && c == cols - 1)
            return -INF;

        ll& ans = dp[r][c];
        if (ans != -INF)
            return ans;

        for (int rgt = c + 1; rgt < cols; rgt++) {
            ans = max(ans, (ll)grid[r][rgt] - grid[r][c] +
                               max(0LL, MaxScore(r, rgt)));
        }
        for (int dwn = r + 1; dwn < rows; dwn++) {
            ans = max(ans, (ll)grid[dwn][c] - grid[r][c] +
                               max(0LL, MaxScore(dwn, c)));
        }
        dp[r][c]=ans;
        return ans;
    }
    int maxScore(vector<vector<int>>& _grid) {
        grid = _grid;
        rows = grid.size();
        cols = grid[0].size();

        dp.clear();
        dp.resize(rows + 1, vector<ll>(cols + 1, -INF));

        ll ans = -INF;
        for (int r = 0; r < rows; r++)
            for (int c = 0; c < cols; c++)
                ans = max(ans, MaxScore(r, c));
        return ans;
    }
    ```

     




 




