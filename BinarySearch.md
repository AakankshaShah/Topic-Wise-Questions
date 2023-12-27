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


7. Count of smaller number than self*** https://www.youtube.com/watch?v=_sA1xI4XK0c
```
class Solution {
public:
 void merge(vector<int> &count, vector<pair<int, int>> &v, int left, int mid, int right){
        vector<pair<int,int>> temp(right - left + 1);
        int i=left;
        int j=mid+1;
        int k=0;
        
        while(i<=mid && j<=right){
            
            if(v[i].first <= v[j].first){
                temp[k++] = v[j++];
            }
            
            else{
                
                count[v[i].second] += right-j+1;
                temp[k++] = v[i++];
                
            }
            
        }
        
        
        while(i<=mid){
            temp[k++] = v[i++];
        }
        
        while(j<=right){
            temp[k++] = v[j++];
        }
        
        for(int i=left;i<=right;i++){
            v[i] = temp[i-left]; 
        }
        
    }
    
    void mergeSort(vector<int> &count, vector<pair<int,int>> &v, int left, int right){
        
        
        if(left<right){
            int mid = left + (right-left)/2;
            mergeSort(count, v, left, mid);
            mergeSort(count, v, mid+1, right);
            merge(count, v, left, mid, right);
        }
        
    }
    vector<int> countSmaller(vector<int>& nums) {
        int n = nums.size();
        vector<pair<int,int>> v(n);
        for(int i=0;i<n;i++){
            pair<int, int> p;
            p.first = nums[i];
            p.second = i;
            v[i] = p;
        }
        
        
        vector<int> count(n, 0);
        mergeSort(count, v, 0, n-1);
        return count;
        
    }
};
```


8. ***Split Array largest sum / Aggressive Cow / Minimum number of pages https://leetcode.com/problems/split-array-largest-sum/submissions/ 

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
