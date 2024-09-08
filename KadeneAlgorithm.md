# Kadene's algorithm 

## Theory 

https://www.geeksforgeeks.org/largest-sum-contiguous-subarray/

## Questions

1. Maximum subarray
2.  Flip
    ```
      vector<int>a;
    bool flag=true;
    vector<int>ans;
    
    for(int i=0;i<A.length();i++)
    {
        if(A[i]=='1')
        a.push_back(-1);
        else
       { a.push_back(1);
       flag=false;
       }
    }
    if(flag)
    return ans;
    
    
    int max_ans=0;
    int initialp=0;
    int finalp=0;
    int maxs=0;
    ans.resize(2);
    
    for(int i=0;i<a.size();i++)
    {
        maxs+=a[i];
        
        if(max_ans<maxs)
        {
            max_ans=maxs;
            finalp=i;
            ans[0]=initialp+1;
            ans[1]=finalp+1;
        }
        
        if(maxs<0)
        {
            maxs=0;
            initialp=i+1;
        }
    }
    return ans;
    ```
3. Best time to buy and sell stock 
    ```
      int maxProfit(vector<int>& prices) {
        int buy = prices[0];
        int profit = 0;
        for (int i = 1; i < prices.size(); i++) {
            if (prices[i] < buy) {
                buy = prices[i];
            } else if (prices[i] - buy > profit) {
                profit = prices[i] - buy;
            }
        }
        return profit;
        
        
    }
    ```
4. Max Product Subarray
    ```
      int maxProduct(vector<int>& nums) {
        int maxi = INT_MIN;
        int prod=1;

        for(int i=0;i<nums.size();i++)
        {
          prod*=nums[i];
          maxi=max(prod,maxi);
          if(prod==0)
           prod=1;
        }
        prod=1;
        for(int i=nums.size()-1;i>=0;i--)
        {
          prod*=nums[i];

          maxi=max(prod,maxi);
          if(prod==0)
           prod=1;
        }
        return maxi;
        
    }
    ```
