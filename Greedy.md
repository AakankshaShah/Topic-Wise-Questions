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
