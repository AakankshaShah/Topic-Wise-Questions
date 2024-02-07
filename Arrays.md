# Arrays

## Questions

1. Circular tour
2.  Maximum non negative subarray sum
     ```
     vector<int> result;
    long long sum = 0;
    int count = 0;
    int maxcount = 0;
    long long  maxsum = 0;
    int index = 0;
    for(int i = 0; i < A.size(); i++)
    {
        if(A[i] >= 0)
        {
            count++;
            sum = sum + A[i];
        }
        else
        {
            count = 0;
            sum = 0;
        }
        if(sum >= maxsum && maxsum != 0 || count >= maxcount && sum >= maxsum)
        {
            maxcount = count;
            maxsum = sum;
            index = i;
        }
    }

    for(int i = index - maxcount+1; i <=index ; i++)
    {
        result.push_back(A[i]);
    }
    return result;
    
     }

     ```
   3. Factorial of a large number https://www.youtube.com/watch?v=O3fwYjcMV_M

   ```
        void multiplier( vector<int> &arr, int &size,int fact)
         { void carry=0;
         for (int i=0;i<size;i++)
         {
        int res=arr[i]*fact;
        res=res+carry;
        arr[i]=res%10;
        carry=res/10;
         }
    
          while(carry>0)
         {
           arr[size++]=carry%10;
          carry=carry/10;
         }
    
        }
       vector<int> factorial(int N){
    
        vector<int> arr(10000,0);
        vector<int> res;
        arr[0] =1;
        int size=1;
        
        
        for(int fact=2;fact<=N;fact++)
        {
            multiplier(arr,size,fact);
        }
        
        
        for(int i=size-1;i>=0;i--)
        {
            res.push_back(arr[i]);
        }
        return res;

        
      }
   ```
  4. Spiral order matrix traversal 

    ```
       int n=A.size();
    int m=A[0].size();
    int left=0;
    int right=m-1;
    int top=0;
    int bottom=n-1;
    vector<int>arr;
    
    while(left<=right&&top<=bottom)
    {
        for(int i=left;i<=right;i++)
        arr.push_back(A[top][i]);
        top++;
        
        for(int i=top;i<=bottom;i++)
        arr.push_back(A[i][right]);
        right--;
        if(top<=bottom)
        {
        for(int i=right;i>=left;i--)
        arr.push_back(A[bottom][i]);
        bottom--;
        }
        if(left<=right)
       {
        for(int i=bottom;i>=top;i--)
        arr.push_back(A[i][left]);
        left++;
       } 
        
        
    }
    return arr;
    ```
    
 5. Pick from both sides! https://www.youtube.com/watch?v=XJZczN4wts0 Another approach 
       ```
         int left_sum[B + 1], right_sum[B + 1], max, i;
        int n = A.size();

        left_sum[0] = right_sum[0] = 0;

        for (i = 1; i < B + 1; ++i)

        {

            left_sum[i] = left_sum[i - 1] + A[i - 1];

            right_sum[i] = right_sum[i - 1] + A[n - i];
        }

        max = left_sum[0] + right_sum[B];

        for (i = 0; i < B + 1; ++i)

        {

            if (left_sum[i] + right_sum[B - i] > max)

            {

                max = left_sum[i] + right_sum[B - i];
            }
        }
       return max;

       ```
       
6. Min steps in infinite grid 

     ```
      int dist= 0;
    for(int i = 0; i<A.size()-1; i++){
        int a = abs(A[i+1] - A[i]);
        int b = abs(B[i+1] - B[i]);
        if(a>=b) dist += a;
        else dist+= b;
    }
    return dist;

   ```
7. Min lights to activate 
    ```
      int ans = 0;
    int n = A.size();
    for(int i=0;i<A.size();){
        int r = min(i+B-1, n-1), l = max(i-B+1, 0);
        bool canLight = false;
        while(r >= l){
            if(A[r] == 1){
                canLight = true;
                break;
            }
            r--;
        }
        if(canLight == false)
            return -1;
        ans++;
        i = r+B;// r is last position so for r+1 farthest is i+B-1 = r+1+B-1 hence r+B
    }
    return ans;
    ```
8. Maximum sum triplet

    ```
     vector<int> suffix(A.size());
        int n = A.size();

        suffix[n - 1] = A[n - 1];

        for (int i = A.size() - 2; i >= 0; i--) {

            suffix[i] = max(suffix[i + 1], A[i]);
        }

        set<int> left;

        int maxSum = 0, sum;

        left.insert(A[0]);

        for (int i = 1; i < A.size(); i++) {

            left.insert(A[i]);

            auto it = left.find(A[i]);

            if (it != left.begin() && suffix[i] != A[i])

                sum = (*--it) + A[i] + suffix[i];

            maxSum = max(sum, maxSum);
        }

        return maxSum;
    

    ```
9. Add 1 to number 

   ```
     int carry =1;
    vector<int>ans;
    
    for(int i=A.size()-1;i>=0;i--)
    {
        int res=A[i]+carry;
        ans.push_back(res%10);
        carry=res/10;
    }
    while(carry>0)
    {
        ans.push_back(carry%10);
        carry=carry/10;
    }
    int i=ans.size()-1;
    while(ans[i]==0) {
        ans.pop_back();
        i--;
    }
    
    reverse(ans.begin(),ans.end());
    return ans;
   ```

10. Max absolute difference https://www.youtube.com/watch?v=KqDIeQ9S5Vc

    ```
      int val1=INT_MAX;
    int val2=INT_MAX;
    int val3=INT_MIN;
    int val4=INT_MIN;
    int ans=0;
    
    
    for(int i=0;i<A.size();i++)
    {
        val1=min(val1,A[i]-i);
        val2=min(val2,A[i]+i);
        val3=max(val3,A[i]-i);
        val4=max(val4,A[i]+i);
        
        ans=max(val4-val2,val3-val1);
    }
    
    return ans;
      
    ```

11. Find duplicate in an array [Hare & tortoise method] - Something is duplicating & array values can act as valid indices
    ```
      int slow = nums[0];
        int fast = nums[0];

        slow = nums[slow];
        fast = nums[nums[fast]];
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[nums[fast]];
        }

        slow = nums[0];
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }

        return slow;
    ```

12. Partitions
     ```
       if (A < 3)
            return 0;

        int totalSum = 0, sum = 0, ans = 0;

        for (int num : B)
            totalSum += num;

        if (totalSum % 3 != 0)
            return 0;

        int x = totalSum / 3;

        int twiceX = x * 2;

        int xCount = 0;

        for (int idx = 0; idx < A - 1; idx++) {

            sum += B[idx];

            if (sum == twiceX)
                ans += xCount;

            if (sum == x)
                xCount++;
        }

        return ans;
     ```
13. First missing positive number
     ```
      int n = A.size();

        for (int i = 0; i < n; i++) {
            int a = A[i];

            if (a >= 1 && a <= n) {
                int pos = a - 1;
                if (A[pos] != A[i]) {
                    swap(A[pos], A[i]);
                    i--;
                }
            }
        }
        for (int i = 0; i < n; i++) {
            if (i + 1 != A[i])
                return i + 1;
        }

        return n + 1;
     ```
   

        

