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
2. Find First and Last Position of Element in Sorted Array
3. Search Insert Position
4. Search a 2d matrix
5. Medain of 2 sorted arrays** https://leetcode.com/problems/median-of-two-sorted-arrays/

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


