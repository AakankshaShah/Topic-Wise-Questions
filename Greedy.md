# Grredy Alogorithm 

## Questions

1. Fractional knapsack
2. Minimum no of jumps || Jump Game 2 || Jump Game

    ```
       int n = arr.size();
        if (n <= 1)
            return 0;

        if (arr[0] == 0)
            return -1;

        int maxReach = arr[0];
        int step = arr[0];
        int jump = 1;

        int i = 1;
        for (i = 1; i < n; i++) {
            if (i == n - 1)
                return jump;

            maxReach = max(maxReach, i + arr[i]);
            step--;
            if (step == 0) {
                jump++;
                if (i >= maxReach)
                    return -1;
                step = maxReach - i;
            }
        }

        return -1;



     // DP approach 
        
         vector<int> dp(nums.size(), INT_MAX);
        dp[0] = 0;
        for(int i = 0; i < nums.size(); i++) {
            for(int k = i+1; k < nums[i] + i + 1 && k < nums.size(); k++) {
                dp[k] = min(1 + dp[i], dp[k]);
            }
        }
        return dp[nums.size() - 1];
  
     ```

    3.
