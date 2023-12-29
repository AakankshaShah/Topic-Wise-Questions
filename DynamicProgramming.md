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


 




