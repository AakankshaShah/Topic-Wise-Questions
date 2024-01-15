# Greedy Alogorithm 

## Questions

1. Fractional knapsack
    ```
        Calculate the ratio (profit/weight) for each item.
        Sort all the items in decreasing order of the ratio.
        Initialize res = 0, curr_cap = given_cap.
        Do the following for every item i in the sorted order:
        If the weight of the current item is less than or equal to the remaining capacity then add the value of that item into the result
       Else add the current item as much as we can and break out of the loop.
    ```
2. Minimum no of jumps || Jump Game 2 || Jump Game https://www.youtube.com/watch?v=Yan0cv2cLy8

    ```
      //If possible or not 
        
         int goal=n-1;


        for(int i=n-1;i>=0;i--)
        {
            if(i+arr[i]>=goal)
            goal=i;
        }


        return goal==0;



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

    3. Sum of fibonacci numbers 

       ```
           int Solution::fibsum(int n) {
   
         vector<int>fb;
        fb.push_back(1);
       fb.push_back(1);
   
        while(fb.back()<n)
        {
        fb.push_back(fb.back()+fb[fb.size()-2]);
       }
   
       set<int>s;
   
       for(auto &x:fb)
        {
        s.insert(x);
        }
   
        int cnt=0;
   
        while(n!=0)
        {
        auto it = s.upper_bound(n);
        it--;
        cnt++;
        n=n-(*it);
        }
       return cnt;
   
       }
       ```
