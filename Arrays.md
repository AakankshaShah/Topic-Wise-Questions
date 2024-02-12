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

14. Missing & repeat number
    ```
      long long n = a.size(); // size of the array

    // Find Sn and S2n:
    long long SN = (n * (n + 1)) / 2;
    long long S2N = (n * (n + 1) * (2 * n + 1)) / 6;

    // Calculate S and S2:
    long long S = 0, S2 = 0;
    for (int i = 0; i < n; i++) {
        S += a[i];
        S2 += (long long)a[i] * (long long)a[i];
    }

    //S-Sn = X-Y:
    long long val1 = S - SN;

    // S2-S2n = X^2-Y^2:
    long long val2 = S2 - S2N;

    //Find X+Y = (X^2-Y^2)/(X-Y):
    val2 = val2 / val1;

    //Find X and Y: X = ((X+Y)+(X-Y))/2 and Y = X-(X-Y),
    // Here, X-Y = val1 and X+Y = val2:
    long long x = (val1 + val2) / 2;
    long long y = x - val1;

    return {(int)x, (int)y};
    ```

    ```
      // xor method
        
    int n = a.size(); // size of the array

    int xr = 0;

    //Step 1: Find XOR of all elements:
    for (int i = 0; i < n; i++) {
        xr = xr ^ a[i];
        xr = xr ^ (i + 1);
    }

    //Step 2: Find the differentiating bit number:
    int number = (xr & ~(xr - 1));

    //Step 3: Group the numbers:
    int zero = 0;
    int one = 0;
    for (int i = 0; i < n; i++) {
        //part of 1 group:
        if ((a[i] & number) != 0) {
            one = one ^ a[i];
        }
        //part of 0 group:
        else {
            zero = zero ^ a[i];
        }
    }

    for (int i = 1; i <= n; i++) {
        //part of 1 group:
        if ((i & number) != 0) {
            one = one ^ i;
        }
        //part of 0 group:
        else {
            zero = zero ^ i;
        }
    }

    // Last step: Identify the numbers:
    int cnt = 0;
    for (int i = 0; i < n; i++) {
        if (a[i] == zero) cnt++;
    }

    if (cnt == 2) return {zero, one};
    return {one, zero};
    }




    ```

15. Max area of triangle 
     ```
       int Solution::solve(vector<string>& A) {
    int rows = A.size(), cols = A[0].size();
    map<char, int> maxRow[cols], minRow[cols], maxCol, minCol;

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            maxRow[j][A[i][j]] = max(maxRow[j][A[i][j]], i);
            if (minRow[j].count(A[i][j])) {
                minRow[j][A[i][j]] = min(minRow[j][A[i][j]], i);
            } else {
                minRow[j][A[i][j]] = i;
            }
            maxCol[A[i][j]] = max(maxCol[A[i][j]], j);
            if (minCol.count(A[i][j])) {
                minCol[A[i][j]] = min(minCol[A[i][j]], j);
            } else {
                minCol[A[i][j]] = j;
            }
        }
    }
    int ans = 0;
    for (int col = 0; col < cols;
         col++) // let's check that in which column, the base of triangle can
                // lie so as to maximise the area
    {
        int maxBase, maxHeight;

        if (maxRow[col].count('r') and maxRow[col].count('g') and
            maxCol.count('b')) {
            maxBase = max(abs(maxRow[col]['r'] - minRow[col]['g']),
                          abs(minRow[col]['r'] - maxRow[col]['g'])) +
                      1;
            maxHeight = max(abs(col - maxCol['b']), abs(col - minCol['b'])) + 1;
            ans = max(ans, maxBase * maxHeight);
        }

        if (maxRow[col].count('g') and maxRow[col].count('b') and
            maxCol.count('r')) {
            maxBase = max(abs(maxRow[col]['g'] - minRow[col]['b']),
                          abs(minRow[col]['g'] - maxRow[col]['b'])) +
                      1;
            maxHeight = max(abs(col - maxCol['r']), abs(col - minCol['r'])) + 1;
            ans = max(ans, maxBase * maxHeight);
        }

        if (maxRow[col].count('b') and maxRow[col].count('r') and
            maxCol.count('g')) {
            maxBase = max(abs(maxRow[col]['b'] - minRow[col]['r']),
                          abs(minRow[col]['b'] - maxRow[col]['r'])) +
                      1;
            maxHeight = max(abs(col - maxCol['g']), abs(col - minCol['g'])) + 1;
            ans = max(ans, maxBase * maxHeight);
        }
    }
    return ceil(ans / 2.0);
    }
     ```

16. Set intersecrtion

     ```
           static bool cmp(vector<int>& a, vector<int>& b) {
        if (a[1] != b[1]) {
            // Sort by endpoint in ascending order
            return a[1] < b[1];
        }
        // If endpoints are equal, sort by startpoint in descending order
        return a[0] > b[0];
        }
        int intersectionSizeTwo(vector<vector<int>>& intervals) {
        int n = intervals.size();
        int res = 2; // minimum 2 sized SET should be there

        sort(intervals.begin(), intervals.end(), cmp);
        // Ex: [[1,3],[3,7],[5,7],[7,8]]
        // After sort : [[1,3],[5,7],[3,7],[7,8]]
        int highestValInSet = intervals[0][1]; // max value of first interval
        int secondHighestValueInSet =
            intervals[0][1] - 1; // second max value from first interval

        for (int i = 1; i < n; i++) {
            int start = intervals[i][0], end = intervals[i][1];

            if (start >
                highestValInSet) { // means ther is no common between curr
                                   // interval and intervals before this
                // add 2 new values
                // end - 1 first and then end to make the SET SORTED
                res += 2;
                highestValInSet = intervals[i][1];
                secondHighestValueInSet = intervals[i][1] - 1;
            } else if (start > secondHighestValueInSet &&
                       start <= highestValInSet) { // atleast 1 value from
                                                   // current interval matches
                                                   // with previosu sets
                // just add 1 max value
                res += 1;
                secondHighestValueInSet =
                    highestValInSet; // now second max becomes previous max
                highestValInSet =
                    intervals[i][1]; // new max for current interval
            }
        }

        return res;
      ```

17. Max unsorted array 

     ```
       int n = arr.size();
    int l = -1, r = -1;
    for (int i = 0; i < n - 1; i++) {
        if (arr[i] > arr[i + 1]) {
            l = i;
            break;
        }
    }
    if (l == -1) {
        return {-1};
    }
    for (int i = n - 1; i > 0; i--) {
        if (arr[i] < arr[i - 1]) {
            r = i;
            break;
        }
    }
    int mini = 1e9;
    int maxi = -1e9;
    for (int i = l; i <= r; i++) {
        mini = min(arr[i], mini);
        maxi = max(maxi, arr[i]);
    }
    for (int i = 0; i < l; i++) {
        if (arr[i] > mini) {
            l = i;
            break;
        }
    }

    for (int i = n - 1; i > r; i--) {
        if (arr[i] < maxi) {
            r = i;
            break;
        }
    }
    return {l, r};

     ```
   

        

