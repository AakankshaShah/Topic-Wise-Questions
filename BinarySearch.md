# Binary Search

## Theory

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

6. Longest Increasing subsequence
