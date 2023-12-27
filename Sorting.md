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

   

