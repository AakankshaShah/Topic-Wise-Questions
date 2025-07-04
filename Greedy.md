# Greedy Alogorithm 

## Questions

1. Fractional knapsack
  
    ```
       bool compare(vector<int>& a, vector<int>& b) {
    double a1 = (1.0 * a[0]) / a[1];
    double b1 = (1.0 * b[0]) / b[1];
    return a1 > b1;
    }

    double fractionalKnapsack(vector<int>& val, vector<int>& wt, int capacity) {
    int n = val.size();
    
    // Create 2D vector to store value and weight
    // items[i][0] = value, items[i][1] = weight
    vector<vector<int>> items(n, vector<int>(2));
    
    for (int i = 0; i < n; i++) {
        items[i][0] = val[i];
        items[i][1] = wt[i];
    }
    
    // Sort items based on value-to-weight ratio in descending order
    sort(items.begin(), items.end(), compare);
    
    double res = 0.0;
    int currentCapacity = capacity;
    
    // Process items in sorted order
    for (int i = 0; i < n; i++) {
        
        // If we can take the entire item
        if (items[i][1] <= currentCapacity) {
            res += items[i][0];
            currentCapacity -= items[i][1];
        }
        
        // Otherwise take a fraction of the item
        else {
            res += (1.0 * items[i][0] / items[i][1]) * currentCapacity;
            
            // Knapsack is full
            break; 
        }
    }
    
    return res;
    }
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
        int step = arr[0];// steps left in current jump
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
     ```
           bool canJump(vector<int>& nums) {
        vector<int> dp(nums.size(), -1);
        return create(nums, 0, dp);
    }
    bool create(vector<int>& nums, int idx, vector<int>& dp) {
        if(idx == nums.size() -1) return true;
        if(nums[idx] == 0) return false;
        
        if(dp[idx] != -1) return dp[idx]; 
        int reach = idx + nums[idx];
        for(int jump=idx + 1; jump <= reach; jump++) {
            if(jump < nums.size() && create(nums, jump, dp)) 
                return dp[idx] = true; 
        }
        
        return dp[idx] = false;
    }
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
   ```
       int minCost(string s, vector<int>& time) {
        int ans = 0;
        if(s.length() == 1) return ans;
        int n = s.length();
        int i=0,j=1;
        while(j<n) {
            if(s[i] == s[j]) {
                if(time[j] > time[i]) {
                    ans += time[i];
                    i = j;
                    j++;
                }
                else{
                    ans += time[j];
                    j++;
                }
            }
            else {
                i=j;
                j++;
            }
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
 17. Divide array in sets of k consecutive num/ Hand of straights
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
18. Minimum dominoe rotation for equal row
    ```
           int minDominoRotations(vector<int>& tops, vector<int>& bottoms) {
        int ans=INT_MAX;
        for (int i=1;i<=6;i++)
        {
            int cntTop=0;
            int cntBottom=0;
            for (int j=0;j<tops.size();j++)
            {
               if (tops[j]!=i && bottoms[j]!=i)
               {
                   cntTop=-1;
                   cntBottom=-1;
                   break;
               }
              else if (tops[j]!=i) cntTop++;
              else if (bottoms[j]!=i) cntBottom++;
              
            }
             if (cntTop!=-1)
               {
                   ans=min(ans,min(cntTop,cntBottom));
               }
        }
     return ans==INT_MAX?-1:ans;
        
    }
    ```
19. Find Valid Matrix Given Row and Column Sums
     ```
         vector<vector<int>> restoreMatrix(vector<int>& rowSum,
                                      vector<int>& colSum) {
        int m = rowSum.size();
        int n = colSum.size();
        vector<vector<int>> vec(m, vector<int>(n));

        int i = 0, j = 0;
        while (i < m && j < n) {
            vec[i][j] = min(rowSum[i], colSum[j]);

            rowSum[i] -= vec[i][j];
            colSum[j] -= vec[i][j];

            if (rowSum[i] == 0)
                i++;
            if (colSum[j] == 0)
                j++;
        }
        return vec;
    }
     ```
20. Longest Palindrome
     ```
         int longestPalindrome(string s) {
        vector<int> v(100, 0);
        int even = 0;
        int odd = 0;
        int om = 0;
        const int m = s.size();
        for (int i = 0; i < m; ++i) {
            ++v[s[i] - 'A'];
            if (v[s[i] - 'A'] % 2 == 1) {
                ++odd;
            } else {
                --odd;
            }
        }
        if (odd > 1) {
            return m - odd + 1;
        }
        return m;
    }
     ```
21. Integer replacement
     ```
        int operate(long long num) {
        if (num == 1)
            return 0;
        if (num == 3)
            return 2;
        if (num % 2 == 0)
            return operate(num / 2) + 1;
        else {
            if ((num + 1) % 4 == 0)
                return operate(num + 1) + 1;
            else
                return operate(num - 1) + 1;
        }
    }
    int integerReplacement(int n) { return operate(n); }
     ```
   22. Gas station

       ```
           int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int n = gas.size();

        int sumGas = accumulate(begin(gas), end(gas), 0);

        int sumCost = accumulate(begin(cost), end(cost), 0);

        if (sumGas < sumCost)
            return -1;

        int total = 0;
        int result = 0;

        for (int i = 0; i < n; i++) {

            total += gas[i] - cost[i];

            if (total < 0) {
                total = 0;
                result = i + 1;
            }
        }
        return result;
         }
        ```
23. Maximum Number of Operations to Move Ones to the End

   ```
       int maxOperations(string s) {
        int n = s.length();
        int cnt = 0;
        int ans = 0;
        int check = 1;
        for (int i = 0; i < n; i++) {
            if (s[i] == '1' && check == 1)
                cnt++;

            if (s[i] == '0' && check == 1)
                check = 0;

            if (s[i] == '1' && check == 0) {
                ans += cnt;
                cnt += 1;
                check = 1;
            }
        }
        if (check == 0)
            ans += cnt;
        return ans;
    }
   ```
24. Candy
     ```
           int candy(vector<int>& ratings) {
        int n = ratings.size();
        
        vector<int> L2R(n, 1);
        vector<int> R2L(n, 1);
        
        //First comparing with only left neighbour
        for(int i = 1; i<n; i++) {
            if(ratings[i] > ratings[i-1])
                L2R[i] = max(L2R[i], L2R[i-1]+1);
        }
        
        //Then comparing with only right neighbour
        for(int i = n-2; i>=0; i--) {
            if(ratings[i] > ratings[i+1])
                R2L[i] = max(R2L[i], R2L[i+1]+1);
        }
        
        
        int result = 0;
        for(int i = 0; i<n; i++) {
            result += max(L2R[i], R2L[i]);
        }
        
        return result;
    }
     ```
     ```
         int candy(vector<int>& ratings) {
        int n = ratings.size();
        vector<int> count(n, 1);
        
        //First comparing with only left neighbour
        for(int i = 1; i<n; i++) {
            if(ratings[i] > ratings[i-1])
                count[i] = max(count[i], count[i-1]+1);
        }
        
        //Then comparing with only right neighbour
        for(int i = n-2; i>=0; i--) {
            if(ratings[i] > ratings[i+1])
                count[i] = max(count[i], count[i+1]+1);
        }
        
        
        return accumulate(begin(count), end(count), 0);
    }
     ```
25. Boats to save people
     ```
         int numRescueBoats(vector<int>& people, int limit) {

        sort(people.begin(), people.end());
        int left = 0;
        int right = people.size() - 1;
        int boats = 0;

    
        while (left <= right) {
           
            if (people[left] + people[right] <= limit) {
                // If they can, move on to the next lightest person
                left++;
            }
            // Move on to the next heaviest person
            right--;

            // Add a boat for the current group of people
            boats++;
        }
        // Return the total number of boats needed
        return boats;
        
    }
     ```
26.  DI string match
      ```
          vector<int> diStringMatch(string s) {
        int n=s.size();
        deque<int> pq;
        for( int i=0;i<=n;i++){
            pq.push_back(i);
        }
        vector<int> v;
        for( int i=0 ; i<n ; i++ ){
            if(s[i] == 'I'){
                v.push_back(pq.front());
                pq.pop_front();
            }
            if( s[i] == 'D' ){
                v.push_back(pq.back());
                pq.pop_back();
            }
        }
        v.push_back(pq.back());
        return v;
        
       }
      ```
27. . Minimum Increment to Make Array Unique
       ```
            int minIncrementForUnique(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int largest = INT_MIN;
        int res = 0;
        for(int i=0; i<nums.size(); i++){
            if(nums[i] > largest){
                largest = nums[i];
            }else{
                res += largest-nums[i]+1;
                largest++;
            }
        }
        return res;
        
    }
       ```
28. Reach End of Array With Max Score
     ```
         long long findMaximumScore(vector<int>& nums) {
        int n = nums.size();
        long long res = 0;
        int x = nums[0], j = 1, i = 0;

        while (j < n - 1) {
            if (nums[i] < nums[j]) {
                res += x * 1LL * (j - i);
                i = j;
                x = nums[i];
            }
            j++;
        }

        return res + x * 1LL * (n - 1 - i);
    }
     ```
29. Bag of Tokens
     ```
         int bagOfTokensScore(vector<int>& tokens, int power) {
        int n = tokens.size();
        sort(tokens.begin(), tokens.end());
        int P=power;

        int currScore = 0;
        int maxScore = 0;
        int l = 0, r = n - 1;

        while (l <= r) {
            if (P >= tokens[l]) {
                currScore++;
                maxScore = max(maxScore, currScore); 
                P -= tokens[l];                    
                l++;

            } else if (currScore >= 1) {
                currScore--;
                P += tokens[r]; // choose largest token
                r--;

            } else {
                // no way further to increase score
                return maxScore;
            }
        }
        return maxScore;
    }
     ```
30. Minimum moves to convert string 
     ```
         int minimumMoves(string s) {
        int i = 0, n = s.length(), count = 0;
        while (i < n) {
            if (s[i] == 'O')
                i++;
            else
                count++, i += 3; // When we find 'X' we increment the count and
                                 // move the pointer by 3 steps
        }
        return count;
    }
     ```
31. Min number of taps to open to water a garden 
    ```
         vector<int> startEnd(n + 1, 0);

        for (int i = 0; i < ranges.size(); i++) {
            int l = max(0, i - ranges[i]);
            int r = min(n, i + ranges[i]);
            startEnd[l] = max(startEnd[l], r);
        }
        int taps = 0;
        int maxEnd = 0;
        int curr = 0;
        for (int i = 0; i <= n; i++) {
            if (i > maxEnd) {
                return -1;
            }
            if (i > curr) {
                taps++;
                curr = maxEnd;
            }
            maxEnd = max(maxEnd, startEnd[i]);
        }

        return taps;
    ```
    ```
        // vector<int> startEnd(n + 1, 0);
        int[] startEnd = new int[n + 1];

        for (int i = 0; i < ranges.length; i++) {
            int l = Math.max(0, i - ranges[i]);
            int r = Math.min(n, i + ranges[i]);
            startEnd[l] = Math.max(startEnd[l], r);
        }
        int taps = 0;
        int maxEnd = 0;
        int curr = 0;
        for (int i = 0; i <= n; i++) {
            if (i > maxEnd) {
                return -1;
            }
            if (i > curr) {
                taps++;
                curr = maxEnd;
            }
            maxEnd = Math.max(maxEnd, startEnd[i]);
        }

        return taps;
    ```
32. Get the max score
    ```
       const int mod = 1e9 + 7;
    int maxSum(vector<int>& nums1, vector<int>& nums2) {
        set<int> s;
        for (int i : nums2) {
            s.insert(i);
        }
        long long int a = 0, b = 0, j = 0, m = nums1.size(), n = nums2.size(),
                      cnt = 0;
        for (int i = 0; i < m; i++) {
            a += nums1[i] % mod;
            if (s.count(nums1[i])) {
                while (j < n && nums2[j] != nums1[i]) {
                    b += nums2[j++] % mod;
                }
                b += nums2[j++] % mod;
                cnt += max(a, b);
                a = 0, b = 0;
            }
        }

        while (j < n) {
            b += nums2[j++] % mod;
        }
        cnt += max(a, b) % mod;
        return cnt % mod;
    }
    ```
    ```
      private static final long mod = (int) 1e9 + 7;

    public int maxSum(int[] nums1, int[] nums2) {
        Set<Integer> set = new HashSet<>();
        for (int num : nums2) {
            set.add(num);
        }
        long a = 0, b = 0, cnt = 0;
        int j = 0, m = nums1.length, n = nums2.length;
        for (int i = 0; i < m; i++) {
            a += nums1[i] % mod;
            if (set.contains(nums1[i])) {
                while (j < n && nums2[j] != nums1[i]) {
                    b += nums2[j++] % mod;
                }
                b += nums2[j++] % mod;
                cnt += Math.max(a, b);
                a = 0;
                b = 0;
            }
        }

        while (j < n) {
            b += nums2[j++] % mod;
        }

        cnt = (cnt + Math.max(a, b)) % mod;
        return (int) cnt;

    }
    ```
33. increasing triplet subsequence

    ```
             int first_num = INT_MAX;
        int second_num = INT_MAX;
        for (int n : nums) {
            if (n <= first_num) {
                first_num = n;
            } else if (n <= second_num) {
                second_num = n;
            } else {
                return true;
            }
        }
        return false;  
    ```
 
34. Minimum Health to Beat Game
```
long long minimumHealth(vector<int>& damage, int armor) {
        int maxDamage = 0;
        long long totalDamage = 0;

        for (auto& d : damage) {
            totalDamage += d;
            maxDamage = max(maxDamage, d);
        }

        return totalDamage - min(armor, maxDamage) + 1;
        
    }
```
