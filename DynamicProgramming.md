# Dynamic Programming

## Theory
https://www.youtube.com/watch?v=nqowUJzG-iM&list=PL_z_8CaSLPWekqhdCPmFohncHwz8TY2Go&index=1

## Questions

1. 0/1 Knapsack 
   * Subset Sum
   * Equal Sum Parition
   * Count of Subset Sum
   * Minimum Subset Sum Diff
   * No of subset with given difference 
   * Target Sum
   

```
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
                dp[i][j]=max(dp[i-1][j],dp[i-1][j-wt[i-1]]);

                else
                dp[i][j]=dp[i-1][j];
            }
        }
        return dp[n][w];
```

2. Unbounded Knapsack

