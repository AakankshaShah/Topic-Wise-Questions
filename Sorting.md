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
int n = intervals.size();
        if(n <= 1)
            return n;
        //why sorting ?
        /*
            We want to handle meetings based on their starting time.
            It ensures that comparison done in priority_queue
            will be definitely correct.
            We won't have to worry about the starting time of
            meetings already running (i.e. meetings that are in priority_queue)
        */
        sort(begin(intervals), end(intervals), sortComp);

        priority_queue<Interval, vector<Interval>, Compare> pq;
        pq.push(intervals[0]); //minimum one room required

        for(int i = 1; i<n; i++) {
            /*if I have to organise a meeting, 
              I will look for the one which ends first
              So that if there is no clash, same room can be occupied
            */

            Interval top = pq.top();
            Interval curr = intervals[i];

            //clash
            if(top.end > curr.start) {
                pq.push(curr);
            } else {
                pq.pop(); //That meeting ends
                pq.push(curr); //I will use same room
            }
        }

        return (int) pq.size();
```
Leetcode - 452 : 
Leetcode - 2446 :  
   

