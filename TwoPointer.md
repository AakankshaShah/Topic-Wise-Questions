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
