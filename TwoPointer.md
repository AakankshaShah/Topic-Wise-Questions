# Two pointer approach 

## Questions 

1. is Subsequence

   ```
    int n = s1.length(), m = s2.length();
    int i = 0, j = 0;
    while (i < n && j < m) {
        if (s1[i] == s2[j])
            i++;
        j++;
    ```
2. Sum of square numbers
   ```
     bool judgeSquareSum(int c) {
        if (c < 0) return false;
        int l=0;
        int r=int(sqrt(c));
        
        while(l<=r)
        {
            int s=c-l*l;
            int sq=(int)sqrt(s);
            if(sq*sq==s)
            return true;
            l++;
        }
        return false;
        
    }
   ```
  ```
   bool judgeSquareSum(int c) {
        ll left = 0, right = static_cast<ll>(sqrt(c));
        while(left <= right) {
            if(left * left + right * right == c) return true;
            else if(left * left + right * right > c) right--;
            else left++;
        }

        return false;
    }
  ```
3. K diff pairs in a array
   ```
       int findPairs(vector<int>& nums, int k) {
        sort(nums.begin(),nums.end());
        set<pair<int,int>> ans;

        int i = 0,j=1;
        while(j<nums.size()){
            int diff = nums[j]-nums[i];
            if(diff == k){
                ans.insert({nums[i],nums[j]});
                ++i;
                ++j;
            }
            else if(diff > k ){
                i++;
            }
            else{
                j++;
            }
            if(i == j){
                j++;
            }
        }
        return ans.size();
        
    }
   ``` 
4. Two pointer 
   ```
     int countPairs(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end()); // sort the vector nums
        int count = 0;                  // variable to store the count
        int left = 0;                   // variable to store the left
        int right = nums.size() - 1;    // variable to store the right
        while (left < right) {          // loop until left is less than right
            if (nums[left] + nums[right] <
                target) { // if nums[left] + nums[right] is less than target
                count += right - left; // update the count
                left++;                // increment the left
            } else {                   // else
                right--;               // decrement the right
            }
        }
        return count; // return the count
    }
   ```
5. Shortest unsorted subarray
    ```
       int findUnsortedSubarray(vector<int>& nums) {
        int low = 0, high = nums.size() - 1;
        while (low + 1 < nums.size() && nums[low] <= nums[low + 1])
            low++;
        while (high - 1 >= 0 && nums[high - 1] <= nums[high])
            high--;

        if (low == nums.size() - 1)
            return 0;

        int wMin = INT_MAX, wMax = INT_MIN;
        for (int i = low; i <= high; i++) {
            wMin = min(wMin, nums[i]);
            wMax = max(wMax, nums[i]);
        }

        while (low - 1 >= 0 && nums[low - 1] > wMin)
            low--;
        while (high + 1 <= nums.size() - 1 && nums[high + 1] < wMax)
            high++;

        return high - low + 1;
    }
    ```
6. Squares of sorted arrays
    ```
     public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];
        int left = 0;
        int right = n - 1;

        for (int i = n - 1; i >= 0; i--) {
            int square;
            if (Math.abs(nums[left]) < Math.abs(nums[right])) {
                square = nums[right];
                right--;
            } else {
                square = nums[left];
                left++;
            }
            result[i] = square * square;
        }
        return result;
        
    }
    ```
7. Number of Subsequences That Satisfy the Given Sum Condition
    ```
       int numSubseq(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        int l = 0;
        int n = nums.size();
        int r = n - 1;
        int mod = 1000000007;
        int res = 0;
        vector<int> pows(n, 1);
        for (int i = 1; i < n; ++i)
            pows[i] = pows[i - 1] * 2 % mod;

        while (l <= r) {
            if (nums[l] + nums[r] > target) {
                r--;
            } else {
                res = (res + pows[r - l++]) % mod;
            }
        }

        return res;
    }
    ```
