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

22.









 




