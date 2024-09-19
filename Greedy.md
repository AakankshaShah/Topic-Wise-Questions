# Greedy Alogorithm 

## Questions

1. Fractional knapsack
    ```
        Calculate the ratio (profit/weight) for each item.
        Sort all the items in decreasing order of the ratio.
        Initialize res = 0, curr_cap = given_cap.
        Do the following for every item i in the sorted order:
        If the weight of the current item is less than or equal to the remaining capacity then add the value of that item into the result
       Else add the current item as much as we can and break out of the loop.
    ```
2. Minimum no of jumps || Jump Game 2 || Jump Game https://www.youtube.com/watch?v=Yan0cv2cLy8

    ```
      //If possible or not 
        
         int goal=n-1;


        for(int i=n-1;i>=0;i--)
        {
            if(i+arr[i]>=goal)
            goal=i;
        }


        return goal==0;



    int n = arr.size();
        if (n <= 1)
            return 0;

        if (arr[0] == 0)
            return -1;

        int maxReach = arr[0];
        int step = arr[0];
        int jump = 1;

        int i = 1;
        for (i = 1; i < n; i++) {
            if (i == n - 1)
                return jump;

            maxReach = max(maxReach, i + arr[i]);
            step--;
            if (step == 0) {
                jump++;
                if (i >= maxReach)
                    return -1;
                step = maxReach - i;
            }
        }

        return -1;



     // DP approach 
        
         vector<int> dp(nums.size(), INT_MAX);
        dp[0] = 0;
        for(int i = 0; i < nums.size(); i++) {
            for(int k = i+1; k < nums[i] + i + 1 && k < nums.size(); k++) {
                dp[k] = min(1 + dp[i], dp[k]);
            }
        }
        return dp[nums.size() - 1];
  
     ```

3. Sum of fibonacci numbers 

       ```
           int Solution::fibsum(int n) {
   
         vector<int>fb;
        fb.push_back(1);
       fb.push_back(1);
   
        while(fb.back()<n)
        {
        fb.push_back(fb.back()+fb[fb.size()-2]);
       }
   
       set<int>s;
   
       for(auto &x:fb)
        {
        s.insert(x);
        }
   
        int cnt=0;
   
        while(n!=0)
        {
        auto it = s.upper_bound(n);
        it--;
        cnt++;
        n=n-(*it);
        }
       return cnt;
   
       }
       ```
4. Minimum additions to make valid string 

       ```
        int addMinimum(string s) {
        int n = s.size();
        int cnt = 0;

        for (int i = 0; i < n; i++) {
            if (i + 2 < n && s.substr(i, 3) == "abc")
                i += 2;

            else if (i + 1 < n &&
                     (s.substr(i, 2) == "ab" || s.substr(i, 2) == "bc" ||
                      s.substr(i, 2) == "ac")) {
                cnt++;
                i++;
            }

            else {
                cnt += 2;
            }
        }
        return cnt;
        }
       ```
5. Furthest building you can reach 
   ```
     //Recursion
     //TLE 
      int solve(int idx, int b, int l, vector<int>& heights, int n) {
        if (idx == n - 1)
            return 0;

        if (heights[idx] >= heights[idx + 1]) {
            return 1 + solve(idx + 1, b, l, heights, n);
        } else {
            int byBricks = 0;
            int byLadder = 0;
            int diff = heights[idx + 1] - heights[idx];

            if (b >= diff)
                byBricks = 1 + solve(idx+1, b - diff, l, heights, n);
            if (l > 0)
                byLadder = 1 + solve(idx+1, b, l - 1, heights, n);
            return max(byBricks, byLadder);
        }
    }
    int furthestBuilding(vector<int>& heights, int bricks, int ladders) {
        int n = heights.size();
        return solve(0, bricks, ladders, heights, n);
    }

   ```
   ```
    //Efficient approach
     int furthestBuilding(vector<int> & heights, int bricks,
                             int ladders){int n = heights.size();
    priority_queue<int> maxHeap;

    for (int i = 0; i < n - 1; i++) {
        if (heights[i + 1] <= heights[i])
            continue;

        int diff = heights[i + 1] - heights[i];
        if (diff <= bricks) {
            bricks -= diff;
            maxHeap.push(diff);
        } else if (ladders > 0) {
            if (!maxHeap.empty()) {
                int maxPast = maxHeap.top();
                if (diff < maxPast) {
                    maxHeap.pop();
                    bricks += maxPast;
                    bricks -= diff;
                    maxHeap.push(diff);
                }
            }
            ladders--;
        } else {
            return i;
        }
    }
    return n - 1;
    }
   ```
4. Minimum time to complete all tasks
   ```
     static bool compareTasks(const vector<int>& t1, const vector<int>& t2) {
        return t1[1] < t2[1];
    }
    int findMinimumTime(vector<vector<int>>& tasks) {
        // sort(tasks.begin(), tasks.end(),
        //  [](const auto& t1, const auto& t2) { return t1[1] < t2[1]; });
        sort(tasks.begin(), tasks.end(), compareTasks);
        const int MaxTime = 2001;
        bool onComputer[2001] = {0};
        for (int i = 0; i < tasks.size(); i++) {
            int s = tasks[i][0];
            int e = tasks[i][1];
            int d = tasks[i][2];
            for (int j = s; j <= e; j++) {
                if (onComputer[j])
                    d--;
            }
            for (int k = e; d > 0; k--) {
                if (onComputer[k] == false) {
                    onComputer[k] = true;
                    d--;
                }
            }
        }
        int countTimeOn = 0;
        for (int i = 1; i <= 2000; i++)
            countTimeOn += onComputer[i];
        return countTimeOn;
    }
   ```
5. Min time to make ropes colorful 
   ```
      nt minCost(string colors, vector<int>& neededTime) {
        int ans = 0;
        int n = colors.size();

        for (int i = 0; i < n;) {
            int j = i;
            int max_index = i;
            int max_cal = neededTime[i];

            while (j < n && colors[j] == colors[i]) {
                if (neededTime[j] > max_cal) {
                    max_cal = neededTime[j];
                    max_index = j;
                }
                j++;
            }

            int sum = 0;
            for (int k = i; k < j; k++) {
                if (k != max_index)
                    sum += neededTime[k];
            }
            ans += sum;

            i = j;
        }

        return ans;
    }
   ```
6. Minimum Difference Between Largest and Smallest Value in Three Moves
    ```
        int minDifference(vector<int>& nums) {
         sort(begin(nums), end(nums));
        int n = nums.size();
        if(n <= 4) {
            return 0;
        }
        int result = INT_MAX;
        result = min(result, nums[n-4] - nums[0]);
        result = min(result, nums[n-1] - nums[3]);
        result = min(result, nums[n-3] - nums[1]);
        result = min(result, nums[n-2] - nums[2]);

        return result;
        
    }
    ```
9. Campus bikes
   ```
         class Solution {
    public:
    struct node {
        int dist, worker, bike;
    };

    static bool myfunc(node& a, node& b) {
        if (a.dist != b.dist)
            return a.dist < b.dist;
        else {
            if (a.worker != b.worker)
                return a.worker < b.worker;
            else
                return a.bike < b.bike;
        }
    }

    vector<int> assignBikes(vector<vector<int>>& workers,
                            vector<vector<int>>& bikes) {
        vector<node> nodes;
        int m = workers.size(), n = bikes.size();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int dist = abs(workers[i][1] - bikes[j][1]) +
                           abs(workers[i][0] - bikes[j][0]);
                node cur = {dist, i, j};
                nodes.push_back(cur);
            }
        }

        sort(nodes.begin(), nodes.end(), myfunc);

        vector<int> result(m, -1);
        unordered_set<int> marked;

        for (int i = 0; i < nodes.size(); i++) {
            if (result[nodes[i].worker] == -1 &&
                marked.find(nodes[i].bike) == marked.end()) {
                result[nodes[i].worker] = nodes[i].bike;
                marked.insert(nodes[i].bike);
            }
        }

        return result;
    }
     };

   ```
10. Largest number
    ```
       
    string largestNumber(vector<int>& nums) {
        vector<string> array;
        for (int num : nums) {
            array.push_back(to_string(num));
        }

        sort(array.begin(), array.end(), [](const string& a, const string& b) {
            return (b + a) < (a + b);
        });

        // Handle the case where the largest number is "0"
        if (array[0] == "0") {
            return "0";
        }

        // Build the largest number from the sorted array
        string largest;
        for (const string& num : array) {
            largest += num;
        }

        return largest;
    }
    ```
11. Divide Array Into Arrays With Max Difference
    ```
         vector<vector<int>> divideArray(vector<int>& nums, int k) {
         sort(nums.begin(), nums.end());
        int n=nums.size();
        vector<vector<int>> ans(n/3);
        for(int i=0; i<n/3; i++){
            if (nums[3*i+2]-nums[3*i]>k) return {};
            ans[i]={nums[3*i], nums[3*i+1], nums[3*i+2] };
        }
        return ans;  
        
    }
    ```
12. Wiggle sort
    ```
        void wiggleSort(vector<int>& nums) {
         bool inc = true;
        for (size_t i = 0; i < nums.size() - 1; ++i) {
            if (inc != (nums[i]<=nums[i+1])) {
                std::swap(nums[i], nums[i+1]);
            }
            inc = !inc;
        }
        
    }
    ```
13. Wiggle sort 2
     ```
         void wiggleSort(vector<int>& nums) {
        int n = nums.size();
        vector<int> nums1(nums);
        sort(nums1.begin(), nums1.end());
        int i = n-1;
        int j = 0;
        int k = i/2 + 1;
        while(i >= 0)
        {
            if(i % 2 == 1)
            {
                nums[i] = nums1[k];
                k++;
            }
            else
            {
                nums[i] = nums1[j];
                j++;
            }
            i--;
        }
    }
     ```
14. Patching array
    ```
         int minPatches(vector<int>& nums, int n) {
         long maxReach = 0;
        int patch     = 0;
        int i         = 0;

        while(maxReach < n) {
            if(i < nums.size() && nums[i] <= maxReach+1) {
               maxReach += nums[i];
               i++;
            } else {
               maxReach += (maxReach + 1);
               patch++;
            }
        }
        return patch;
        
    }
    ```
15. Rearrange string k distnace apart
     ```
        string rearrangeString(string str, int k) {
        if (k == 0)
            return str;
        unordered_map<char, int> dict;
        for (char ch : str)
            dict[ch]++;
        int left = (int)str.size();
        priority_queue<pair<int, char>> pq;
        for (auto it = dict.begin(); it != dict.end(); it++) {
            pq.push(make_pair(it->second, it->first));
        }
        string res;

        while (!pq.empty()) {
            vector<pair<int, char>> cache;
            int count = min(k, left);
            for (int i = 0; i < count; i++) {
                if (pq.empty())
                    return "";
                auto tmp = pq.top();
                pq.pop();
                res.push_back(tmp.second);
                if (--tmp.first > 0)
                    cache.push_back(tmp);
                left--;
            }
            for (auto p : cache) {
                pq.push(p);
            }
        }
        return res;
    }
     ```
16. Maximum Distance in Arrays
     ```
         int maxDistance(vector<vector<int>>& arrays) {
        int minVal = arrays[0][0];
        int maxVal = arrays[0].back();
        int maxDistance = 0;

        for (int i = 1; i < arrays.size(); i++) {
            int currentMin = arrays[i][0];
            int currentMax = arrays[i].back();

            maxDistance = max(maxDistance, abs(currentMax - minVal));
            maxDistance = max(maxDistance, abs(maxVal - currentMin));

            minVal = min(minVal, currentMin);
            maxVal = max(maxVal, currentMax);
        }

        return maxDistance;
        
    }
     ```
 17. Divide array in sets of k consecutive num
      ```
           bool isPossibleDivide(vector<int>& nums, int k) {
        if(nums.size() % k != 0)return false;
        map<int,int> umap;
        for(int i=0; i<nums.size(); i++){
            umap[nums[i]]++;
        }
        int start = 0;
        int curr;
        while(umap.size()>0){
            curr = umap.begin()->first;
            for(int i=0; i<k; i++){
                if(umap[curr+i] == 0)return false;
                --umap[curr+i];
                if(umap[curr+i] == 0)umap.erase(curr+i);
            }
        }
        return true;
        
    }
      ```
