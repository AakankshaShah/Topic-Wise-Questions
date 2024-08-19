# Sorting


## Theory 

## Questions

1. Count inversions** https://www.youtube.com/watch?v=AseUmwVNaoY

```
int merge(vector<int> &arr, int low, int mid, int high) {
    vector<int> temp; // temporary array
    int left = low;      // starting index of left half of arr
    int right = mid + 1;   // starting index of right half of arr

    //Modification 1: cnt variable to count the pairs:
    int cnt = 0;

    //storing elements in the temporary array in a sorted manner//

    while (left <= mid && right <= high) {
        if (arr[left] <= arr[right]) {
            temp.push_back(arr[left]);
            left++;
        }
        else {
            temp.push_back(arr[right]);
            cnt += (mid - left + 1); //Modification 2
            right++;
        }
    }

    // if elements on the left half are still left //

    while (left <= mid) {
        temp.push_back(arr[left]);
        left++;
    }

    //  if elements on the right half are still left //
    while (right <= high) {
        temp.push_back(arr[right]);
        right++;
    }

    // transfering all elements from temporary to arr //
    for (int i = low; i <= high; i++) {
        arr[i] = temp[i - low];
    }

    return cnt; // Modification 3
}

int mergeSort(vector<int> &arr, int low, int high) {
    int cnt = 0;
    if (low >= high) return cnt;
    int mid = (low + high) / 2 ;
    cnt += mergeSort(arr, low, mid);  // left half
    cnt += mergeSort(arr, mid + 1, high); // right half
    cnt += merge(arr, low, mid, high);  // merging sorted halves
    return cnt;
}

int numberOfInversions(vector<int>&a, int n) {

    // Count the number of pairs:
    return mergeSort(a, 0, n - 1);
}

int main()
{
    vector<int> a = {5, 4, 3, 2, 1};
    int n = 5;
    int cnt = numberOfInversions(a, n);
    cout << "The number of inversions are: "
         << cnt << endl;
    return 0;
}
```

2.  Count of smaller number than self*** https://www.youtube.com/watch?v=_sA1xI4XK0c
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


3. Reverse pairs
```
void merge(vector<int> &arr, long long low, long long mid, long long high) {
    vector<long long> temp; // temporary array
    long long left = low;      // starting index of left half of arr
    long long right = mid + 1;   // starting index of right half of arr

    //storing elements in the temporary array in a sorted manner//

    while (left <= mid && right <= high) {
        if (arr[left] <= arr[right]) {
            temp.push_back(arr[left]);
            left++;
        }
        else {
            temp.push_back(arr[right]);
            right++;
        }
    }

    // if elements on the left half are still left //

    while (left <= mid) {
        temp.push_back(arr[left]);
        left++;
    }

    //  if elements on the right half are still left //
    while (right <= high) {
        temp.push_back(arr[right]);
        right++;
    }

    // transfering all elements from temporary to arr //
    for (long long i = low; i <= high; i++) {
        arr[i] = temp[i - low];
    }
}

long long countPairs(vector<int> &arr, long long  low, long long mid, long long high) {
   long long right = mid + 1;
 long long cnt = 0;
    for (long long i = low; i <= mid; i++) {
        while (right <= high && arr[i] > (long long)2 * arr[right]) right++;
        cnt += (right - (mid + 1));
    }
    return cnt;
}

long long mergeSort(vector<int> &arr, long long low, long long high) {
   long long cnt = 0;
    if (low >= high) return cnt;
    long long mid = (low + high) / 2 ;
    cnt += mergeSort(arr, low, mid);  // left half
    cnt += mergeSort(arr, mid + 1, high); // right half
    cnt += countPairs(arr, low, mid, high); //Modification
    merge(arr, low, mid, high);  // merging sorted halves
    return cnt;
}



    
    
    
    
    
    int reversePairs(vector<int>& nums) 

    {int n=nums.size();
        return mergeSort(nums, 0, n - 1);
        
    }
```


4. Non overlapping interval https://www.youtube.com/watch?v=0TYKyTwGOAs

    1. Sort start time 
    2. Sort end time
     ```
      sort(begin(intervals), end(intervals));
        int n = intervals.size(); 
        
        int count = 0;
        vector<int>L=intervals[0];
        int i=1;
        
        
        while(i < n) {
            int laste=L[1];
            int curr_s=intervals[i][0];
            int curr_e=intervals[i][1];


            if(curr_s>=laste)
            {
                L=intervals[i];
                i++;

            }
            else if(curr_e>=laste)
            {
                count++;
                i++;
                
            }
            else if(curr_e<laste)
            {
                L=intervals[i];
                i++;
                count++;
                
            }

          
        }

        return count;
     ```
     Leetcode - 252 : 
     Leetcode - 253 : 
      ```
      int n = intervals.size();
        if (n <= 1)
            return n;

        vector<int> startTime(n);
        vector<int> endTime(n);

        int i = 0;
        for (int i0; i < n; i++) {
            startTime[i] = intervals[i][0];
            endTime[i] = intervals[i][1];
            i++;
        }

        sort(begin(startTime), end(startTime));
        sort(begin(endTime), end(endTime));

        i = 0;
        int j = 0;
        int count = 0;

        while (i < n) {
            if (startTime[i] < endTime[j]) {
                count++; // we need one more room
            } else {
                j++; // previous room can be used which is over
            }
            i++;
        }

      ```


      ```
        //Heap
        // min heap to store end-times
        std::priority_queue<int, vector<int>, greater<int>> pq;
       
        sort(intervals.begin(), intervals.end());
       
        pq.push(intervals[0][1]);
        
        for (int i=1; i<intervals.size(); ++i)
        {
            // if next interval starts after min-end time
            if (intervals[i][0] >= pq.top())
            {
                // room can be reused
                // pop that end-time and add new end-time
                pq.pop();
            }
            
            pq.push(intervals[i][1]);
        }
        // size of priority queue is the min number of meeting rooms required
        return pq.size();
    ```
    Leetcode - 452 : 
    Leetcode - 2446 :  

5. Wave array 

    ```
     std::sort(A.begin(),A.end());
    int i = 1;
    while(i < A.size())
    {
        if(i%2 == 1) std::swap(A.at(i-1),A.at(i));
        i += 2;
    }
    return A;
   ```

6. Max distance 
   ```
     int n = A.size(), maxDist = 0, minPos = INT_MAX;

    vector<pair<int, int>> nums;

    for (int i = 0; i < n; i++)

    {

        nums.push_back(make_pair(A[i], i));
    }

    sort(nums.begin(), nums.end());

    minPos = nums[0].second;

    for (int i = 1; i < n; i++)

    {

        if (nums[i].second > minPos)

        {

            maxDist = max(maxDist, (nums[i].second - minPos));
        }

        minPos = min(minPos, nums[i].second);
    }

    return maxDist;
   ```
   ```
       int maxIndexDiff(int arr[], int n) 
    { 
        if(n==1){
            return 0;
        }
        int maxDiff; 
        int i, j; 
        
        int *LMin = new int[n];
        int *RMax = new int[n];
      
        //Constructing LMin[] such that LMin[i] stores the minimum value 
        //from (arr[0], arr[1], ... arr[i]).
        LMin[0] = arr[0]; 
        for (i = 1; i < n; ++i) 
            LMin[i] = min(arr[i], LMin[i-1]); 
      
        //Constructing RMax[] such that RMax[j] stores the maximum value 
        //from (arr[j], arr[j+1], ..arr[n-1]).
        RMax[n-1] = arr[n-1]; 
        for (j = n-2; j >= 0; --j) 
            RMax[j] = max(arr[j], RMax[j+1]); 
         
        i = 0, j = 0, maxDiff = -1; 
        //Traversing both arrays from left to right to find optimum j-i.
        //This process is similar to merge() of MergeSort.
        while (j < n && i < n) 
        { 
            if (LMin[i] <= RMax[j]) 
            { 
                //Updating the maximum difference.
                maxDiff = max(maxDiff, j-i); 
                j = j + 1; 
            } 
            else
                i = i+1; 
        } 
        //returning the maximum difference.
        return maxDiff; 
    }
   ```
7. Sort array of 0s,1s,2s
   ```
     int l = 0, mid = 0, high = n - 1;
    while (mid <= high) {
    switch (a[mid]) {
    case 0:
        swap(a[l++], a[mid++]);
        break;

    case 1:
        mid++;
        break;

    case 2:
        swap(a[mid], a[high--]);
        break;
    }
    }
   ```
8. Merge two sorted arrays 
    ```
       void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i=m;
        int j=0;
        for(i=m;i<n+m;i++){
            nums1[i]=nums2[j++];
        }
        int gap = ceil((n+m)/2.0);
        while(gap){
        i=0;
        j=gap;
        while(j<n+m){
            if(nums1[i]>nums1[j])
              swap(nums1[i],nums1[j]);

            i++;
            j++;  
        }
          if(gap==1) break;
          gap=ceil(gap/2.0);
        }
        
    }
      
    ```
9. 3Sum smaller
    ```
      void twoSumSmaller(const vector<int>& nums, int target, int first,
                       int& count) {
        int second = first + 1;
        int third = size(nums) - 1;

        while (second < third) {
            int sum = nums[first] + nums[second] + nums[third];
            if (sum < target) {
                count += (third - second);
                ++second;
            } else
                --third;
        }
    }

    int threeSumSmaller(vector<int>& nums, int target) {
        int count = 0;
        sort(begin(nums), end(nums));

        for (int first = 0; first < size(nums); first++)
            twoSumSmaller(nums, target, first, count);

       
    ```

