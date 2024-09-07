# Binary Search

## Theory
https://www.youtube.com/watch?v=j7NodO9HIbk&list=PL_z_8CaSLPWeYfhtuKHj-9MpYb6XQJ_f2

## Questions

1.  Search in Rotated Sorted Array
```
int left = 0;
        int right = nums.size() - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] == target) {
                return mid;
            }
            
            if (nums[left] <= nums[mid]) { // Left half is sorted
                if (nums[left] <= target && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            } else { // Right half is sorted
                if (nums[mid] < target && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
        }
        
        return -1; // Target not found
```
2. Find the First and Last Position of Element in Sorted Array
3. Search Insert Position
4. Search a 2d matrix
5. Median of 2 sorted arrays** https://leetcode.com/problems/median-of-two-sorted-arrays/

```
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) 
    {
       
        int n1=nums1.size();
        int n2=nums2.size();
         int l=0;
        int h=n1;

        int left=(n1+n2+1)/2;
        int n=n1+n2;
        
     if(n1>n2)return findMedianSortedArrays(nums2,nums1);

     if(n1==0)
        return n2%2?nums2[n2/2]:(nums2[n2/2]+nums2[n2/2-1])/2.0;
    
    if(n2==0)
        return n1%2?nums1[n1/2]:(nums1[n1/2]+nums1[n1/2-1])/2.0;

       

        while(l<=h)
        { int l1=INT_MIN,l2=INT_MIN,r1=INT_MAX,r2=INT_MAX;

            int m=(l+h)/2;
            int m2=left-m;

            if(m<n1)
           r1=nums1[m];
           if(m2<n2)
           r2=nums2[m2];
            if(m-1>=0)
           l1=nums1[m-1];
           if(m2-1>=0)
           l2=nums2[m2-1];

          
           
           if(l1<=r2&&l2<=r1)
           {
               if(n%2==0)
               return ((double)(max(l1,l2)+min(r1,r2)))/2.0;
               else
               return max(l1,l2);

           }

           else if(l1>r2)
           h=m-1;

           else
           l=m+1;


            
        }

return 0;
        
    }
};
```

6. Longest Increasing subsequence*** https://www.youtube.com/watch?v=on2hvxBXJH4
 ```
 vector<int>v;

        v.push_back(nums[0]);


        for(int i=0;i<nums.size();i++)
        {
            if(nums[i]>v.back())
             v.push_back(nums[i]);
             else
             {
                 int ind=lower_bound(v.begin(),v.end(),nums[i])-v.begin();
                 v[ind]=nums[i];
             }
        }
        return v.size();
```

7. ***Split Array largest sum / Aggressive Cow / Minimum number of pages https://leetcode.com/problems/split-array-largest-sum/submissions/ 

```
int isValid(vector<int>& nums,int n, int k, int mid)
{
    int s=0;
    int c=1;

    for(int i=0;i<n;i++)
    {
        if(s+nums[i]<=mid)
        s+=nums[i];
        else
        {
            c++;
            s=nums[i];

        }

    
    }

return c;

}
    int splitArray(vector<int>& nums, int k) 
    {
        int n=nums.size();
        int l=0;
        int h=0;
        int res=-1;

        for(int i=0;i<n;i++)
        {
          l=max(l,nums[i]);
            h+=nums[i];
        }
while(l<=h)
{
    int mid=(l+h)/2;

    if(isValid(nums,n,k,mid)<=k)
    {
        res=mid;
        h=mid-1;
    }

    else
    l=mid+1;

}

return res;
        
    }
```
8. Number of matching subsequence [ Another approach LCS ]
 ```
vector<vector<int>> charIndexes(26);
        for(int i = 0;i < s.size(); i++){
            charIndexes[s[i]-'a'].push_back(i);
        }
        
        int count = 0;
        
        for(int i = 0;i< words.size();i++){
            bool isSubseq = true;
            int lastCharIndex = -1;
            for(char w : words[i]){
                auto it = upper_bound(charIndexes[w - 'a'].begin(), charIndexes[w - 'a'].end(), lastCharIndex);
                if(it==charIndexes[w - 'a'].end()){
                    isSubseq = false;
                    break;
                }
                else{
                    lastCharIndex = *it;
                }
                
            }
            
            if(isSubseq){
                count++;
            }
            
            
        }
        
        
        return count;
```
9. Koko eating bananas https://leetcode.com/problems/koko-eating-bananas/description/
10. Longest repeating substring ***

```
bool search(int len, string S) {
        unordered_set<string> seen;
        for (int i = 0; i + len <= S.length(); i++) {
            string curr = S.substr(i, len);
            if (seen.find(curr) != seen.end()) {
                return true;
            }
            seen.insert(curr);
        }
        return false;
    }
    int longestRepeatingSubstring(string s) {
         int n = s.length();
        int start = 0;
        int end = n - 1;
        int maxLen = 0;
        while (start <= end) {
            int mid = start + (end - start) / 2;
            if (search(mid, s)) {
                maxLen = mid;
                start = mid + 1;
            } else {
                end = mid - 1;
            }
        }
        return maxLen;
        
    }
```


11. Minimum Cost to Make Array Equal
https://leetcode.com/problems/minimum-cost-to-make-array-equal/description/

```
long long find_cost(int ele,vector<int>& nums, vector<int>& cost)
    {
        long long ans = 0;
        for(int i = 0; i < nums.size(); i++)
        {
            long long a = abs(nums[i] - ele);
            ans = ans + (a * cost[i]);
        }
            
        return ans;
    }
    
    long long minCost(vector<int>& nums, vector<int>& cost) 
    {


long long l=0,r=1e6,n=nums.size(),ans;
        while(l<=r) {
            int m=(l+r)/2;
            long long a1=find_cost(m,nums,cost), a2=find_cost(m+1,nums,cost);
            if(a1<=a2) {
                r=m-1;
            }
            else {
                l=m+1;
            }
        }
        long long a1=find_cost(l,nums,cost), a2=find_cost(l-1,nums,cost);
        return min(a1,a2);
        
    }
```

12. Partition Array Into Two Arrays to Minimize Sum Difference - Meet in middle technique Doubt

13. Latest time to catch a bus 
 ```
sort(buses.begin(), buses.end());
        sort(passengers.begin(), passengers.end());
        int n = buses.size(), m = passengers.size();

        int res = 0;
        for (int i = 0, j = 0; i < n; ++i) {
            int cnt = 0;
            while (j < m && passengers[j] <= buses[i] && cnt < capacity) {
                ++cnt;
                ++j;
            }
            if (i == n - 1) { // the last bus
                if (cnt < capacity) { // still have seats
                    int t = buses[i]; // can be as late as the bus arrive time
                    for (int k = j - 1; k >= 0 && passengers[k] == t; --k, --t);// if that time is already taken
                    res = max(res, t);
                } else { // full of passegers
                    int t = passengers[j - 1] - 1; // should arrive earlier than last passenger aboard
                    for (int k = j - 2; k >= 0 && passengers[k] == t; --k, --t);
                    res = max(res, t);
                }
            }
        }


        
        return res;

```

14. Fair distribution of cookies


    ```
     bool check(int mid,vector<int>& cookies,int k){
        int n=cookies.size();
        int child=1;
        int sum=0;
        for(int i=0;i<n;i++){
            sum+=cookies[i];
            if(mid<cookies[i]) return false;
            if(sum>mid){
                child++;
                sum=cookies[i];
            }
            if(child>k) return false;
        }
        return true;
    }
    int binarySearch(vector<int>& cookies,int k,int sum){
        int start=0;
        int end=sum;
        int ans=0;
        while(start<=end){
            int mid=start+(end-start)/2;
            if(check(mid,cookies,k)){
                ans=mid;
                end=mid-1;
            }
            else{
                start=mid+1;
            }
        }
        return ans;
    }
    int distributeCookies(vector<int>& cookies,int k){
        sort(cookies.begin(),cookies.end());
        int sum=accumulate(cookies.begin(),cookies.end(),0);
        int ans=1e8;
        do{
            ans=min(ans,binarySearch(cookies,k,sum));   
        }
        while(next_permutation(cookies.begin(),cookies.end()));  
        return ans;
    }
    ```
15. Minimum number of days to make m bouquets
    ```
       class Solution {
     public:
    bool isValid(int m, int k, int day, vector<int>& bloomDay) {
        int s = 0;
        int c = 0;
        for (int i = 0; i < bloomDay.size(); i++) {
            if (bloomDay[i] <= day) {
                s++;
                if (s == k) {
                    c++;
                    s = 0;
                }
            } else {
                s = 0;
            }
        }
        if (c >= m)
            return true;
     return false;       
    }
    int minDays(vector<int>& bloomDay, int m, int k) {
        int n = bloomDay.size();
        if ((long long)m * k > n)
            return -1;

        int l = 1;
        int h = *max_element(bloomDay.begin(), bloomDay.end());

        while (l <= h) {
            int mid = (h + l) / 2;
            if (isValid(m, k, mid, bloomDay))
                h=mid-1;
            else
               l=mid+1;
        }
        return l;
    }
    };
    ```
16. Magnetic Force Between Two Balls
     ```
       int isValid(int mid, vector<int> position) {
            int total = 1;
            int curDist = position[0];
            for (int i = 1; i < position.size(); i++) {
                if (position[i] - curDist >= mid) {
                    total++;
                    curDist = position[i];
                }
            }
            return total;
        }
    int maxDistance(vector<int>& position, int m) {
        sort(position.begin(), position.end());
        int n = position.size();
        int l = 0;
        int r = 1e9;


        int curMax = 0;
        while (l <= r) {
            int mid = (l + r) / 2;

            if (isValid(mid, position) >= m) { 
                l = mid + 1;
                curMax = max(curMax, mid);
            } else { // check lower days
                r = mid - 1;
            }
        }
        return curMax;
        
    }
     ```
17. Intersection of two arrays
    ```
           vector<int> ans;
        sort(nums1.begin(),nums1.end());
        sort(nums2.begin(),nums2.end());
        int i=0;
        int j=0;
        while(i<nums1.size() && j<nums2.size())
        {
            if(nums1[i]==nums2[j])
            {
                ans.push_back(nums1[i]);    
                i++;
                j++;
            }
            else if(nums1[i]<nums2[j])
            {
                i++;
            }
            else
            {
                j++;
            }
        }
        return ans;
        
    }
    ```
18. Find Peak element
```
 int n = nums.size();
         if(n==1) return 0;
        if(nums[0] > nums[1]) return 0;
        if(nums[n-1] > nums[n-2]) return n-1;

        int low = 1;
        int high = n-2;

        while(low<=high){
            int mid = (low+high)/2;

            if(nums[mid-1]<nums[mid] && nums[mid]>nums[mid+1]){
                return mid;
            }else if(nums[mid]>nums[mid+1]){
                high = mid-1;
            }else{
                low = mid+1;
            }
        }

        return 0;
```
19. Special Array With X Elements Greater Than or Equal X
```
 int specialArray(vector<int>& nums) {
       
        sort(nums.begin(), nums.end());
        for (int i = 0; i <= 1000; ++i) {

            int ind = lower_bound(nums.begin(), nums.end(), i) - nums.begin();
            int len = nums.size() - ind;
            if (len == i) {
                return i;
            }
        }
        return -1;
    }
```
20. Single Element in a sorted array
    ```
     int singleNonDuplicate(vector<int>& nums) {
        int n=nums.size();
        int low=0;
        int high=n-1;
        int mid;
        if(n==1) return nums[0];
        if(nums[0]!=nums[1]) return nums[0];
        while(low<=high)
        {
            mid=(low+high)/2;
             if (nums[mid] != nums[mid + 1] && nums[mid] != nums[mid - 1]) {
            return nums[mid];
        }
            if(mid%2==0)
            {
                if(nums[mid]!=nums[mid+1])
                {
                    high=mid-1;
                }
                else{
                    low=mid+1;
                }
               
            }
            else{
                if(nums[mid]!=nums[mid-1])
                {
                    high=mid-1;
                }
                else{
                    low=mid+1;
                }
            }
        }
        return nums[mid];
        
    }
    ```
21. Nth Magical Number
   ```
    int nthMagicalNumber(int n, int a, int b) {
        long long start = min(a, b);
        long long end = n * start;
        long long int d = a * b / gcd(a, b);//lcm
        long long int ans = 0;
        while (start <= end) {
            long long mid = start + (end - start) / 2;
            long long c = mid / a + mid / b - mid / d;
            if (c < n) {
                start = mid + 1;
            } else {
                ans = mid;
                end = mid - 1;
            }
        }
        int mod = 1e9 + 7;
        return ans % mod;
    }
   ```
22. Find a smallest divisor given a threshold 
   ```
      int isValid(vector<int>& nums, int mid) {
        int s = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] % mid == 0)
                s += nums[i] / mid;
            else
                s += (nums[i] / mid + 1);
        }
        return s;
    }
    int smallestDivisor(vector<int>& nums, int threshold) {
        int l = 1;
        int h = *max_element(nums.begin(), nums.end());
        int ans = h;

        while (l <= h) {
            int mid = l + (h - l) / 2;
            cout << mid << " " << isValid(nums, mid) << "\n";

            if (isValid(nums, mid) <= threshold) {
                ans = mid;
                h = mid - 1;

            }

            else if (isValid(nums, mid) > threshold)
                l = mid + 1;
        }
        return ans;
    }
   ```
23. Maximum Value at a Given Index in a Bounded Array
    ```
      typedef long long ll;

    ll getSumElements(ll count, ll val) {

        return val * count - (count * (count + 1)) / 2;
    }

    int maxValue(int n, int index, int maxSum) {

        ll left = 1;
        ll right = maxSum;

        ll mid_val;
        int result = 0;

        while (left <= right) {

            mid_val = left + (right - left) / 2;

            ll left_count = min((ll)index, mid_val - 1);

            ll left_sum = getSumElements(left_count, mid_val);

            left_sum += max((ll)0, index - mid_val + 1);

            ll right_count = min((ll)n - index - 1, mid_val - 1);

            ll right_sum = getSumElements(right_count, mid_val);

            right_sum += max((ll)0, n - index - 1 - mid_val + 1);

            if (left_sum + right_sum + mid_val <= maxSum) {
                result = max((ll)result, mid_val);

                left = mid_val + 1;
            } else {
                right = mid_val - 1;
            }
        }

        return result;
    }
    ```
24. Find the uniqueness of median array 
     ```
       int medianOfUniquenessArray(vector<int>& nums) {
        int n = nums.size();
        int l = 1;
        int r = n;
        while (l <= r) {
            int m = (l + r) / 2;
            if (rec1(nums, n, m) > (((long long)n * ((long long)n + 1)) / 2LL - 1) / 2LL) {
                r = m - 1;
            } else {
                l = m + 1;
            }
        }
        return l;
    }

    long long rec1(vector<int>& arr, int n, int k) {
        long long cnt = 0;
        int l = 0;
        int r = 0;
        unordered_map<int, int> map;
        while (r < n) {
            map[arr[r]]++;
            while (map.size() > k) {
                map[arr[l]]--;
                if (map[arr[l]] == 0)
                    map.erase(arr[l]);
                l++;
            }
            cnt += r - l + 1;
            r++;
        }
        return cnt;
    }
     ```
25. Russian doll envelopes
     ```
      //LIS
        static bool comp(vector<int>& a, vector<int>& b) {
        if (a[0] == b[0]) {
            return a[1] > b[1];
        }
        return a[0] < b[0];
    }
    int maxEnvelopes(vector<vector<int>>& envelopes) {
        sort(envelopes.begin(), envelopes.end(), comp);

        int i, j, n = envelopes.size();
        vector<int> lis;

        for (i = 0; i < n; i++) {
            auto it = lower_bound(lis.begin(), lis.end(), envelopes[i][1]);
            if (it == lis.end()) {
                lis.push_back(envelopes[i][1]);
            } else {
                *it = envelopes[i][1];
            }
        }
        return lis.size();
    }
     ```
    26. kth smallest pair distance
         ```
            int smallestDistancePair(std::vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());

        int left = 0;
        int right = nums.back() - nums.front();

        while (left < right) {
            int mid = left + (right - left) / 2;

            if (isSmallPairs(nums, k, mid))
                right = mid;
            else
                left = mid + 1;
        }
        return left;
        }

         private:
        bool isSmallPairs(vector<int>& nums, int k, int mid) {
        int count = 0;
        int left = 0;
        for (int right = 1; right < nums.size(); ++right) {
            while (nums[right] - nums[left] > mid) ++left;
            count += right - left;
        }
        return count >= k;
         }
         ```
