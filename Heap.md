1.  kth smallest element
``` 
priority_queue<int>pq;
        for(int i=l;i<=r;i++){
            pq.push(arr[i]);
            if(pq.size()>k) pq.pop();
        }
        return pq.top();
```

 2. Car fleet https://www.youtube.com/watch?v=P99yS9jLr6o

     ```
          priority_queue<vector<double>> pq;

        for (int i = 0; i < position.size(); i++) {
            double t = (double)(target - position[i]) / speed[i];
            pq.push({(double)position[i], (double)speed[i], t});
        }
        if (pq.size() == 0)
            return 0;
        int fleet = 0;

        while (true) {
            if (pq.size() == 1) {
                fleet++;
                break;
            }
            auto ahead = pq.top();
            pq.pop();
            auto behind = pq.top();
            pq.pop();
            if (ahead[2] >= behind[2]) {
                pq.push(ahead);
            } else {
                fleet++;
                pq.push(behind);
            }
        }
        return fleet;
     ```
     ```
        //Chatgpt solution
         vector<pair<int, double>> cars;
        
        for (int i = 0; i < position.size(); i++) {
            double t = (double)(target - position[i]) / speed[i];
            cars.push_back({position[i], t});
        }
        
        // Sort cars by their starting positions in descending order
        sort(cars.begin(), cars.end(), [](const pair<int, double>& a, const pair<int, double>& b) {
            return a.first > b.first;
        });
        
        int fleets = 0;
        double maxTime = 0.0;
        
        for (const auto& car : cars) {
            if (car.second > maxTime) {
                maxTime = car.second;
                fleets++;
            }
        }
        
        return fleets;
     ```
 4. kth closest elements
     ```
       vector<int> findClosestElements(vector<int>& arr, int k, int x) {
        vector<int> ans(k);
        priority_queue<pair<int, int>> pq;

        for (int i = 0; i < arr.size(); i++) {
            int a = abs(arr[i] - x);
            pq.push({a, arr[i]});
        }

        while (pq.size() != k)
            pq.pop();
        int i = k - 1;
        while (!pq.empty()) {
            ans[i--] = pq.top().second;
            pq.pop();
        }
        sort(ans.begin(), ans.end());
        return ans;
    }
     ```     
   ```
     vector<int> ans(k);
        priority_queue<pair<int, int>> pq;

        for (int i = 0; i < arr.size(); i++) {
            int diff = abs(arr[i] - x);
            pq.push({diff, arr[i]});
            if (pq.size() > k) {
                pq.pop();
            }
        }

        while (!pq.empty()) {
            ans[--k] = pq.top().second;
            pq.pop();
        }

        sort(ans.begin(), ans.end());
        return ans;
    }
``` 
5. Meeting Rooms II
     ```
        int minMeetingRooms(vector<vector<int>>& intervals) {
        std::priority_queue<int, vector<int>, greater<int>> pq;

        sort(intervals.begin(), intervals.end());
        pq.push(intervals[0][1]);
        for (int i = 1; i < intervals.size(); ++i) {
            if (intervals[i][0] >= pq.top()) {

                pq.pop();
            }

            pq.push(intervals[i][1]);
        }
        return pq.size();
    }
     ```
6. Top K frequent elements
    ```
       vector<int> topKFrequent(vector<int>& nums, int k) {
        priority_queue<pair<int, int>, vector<pair<int, int>>,
                       greater<pair<int, int>>>
            pq;
        vector<int> ans;
        int n = nums.size();
        unordered_map<int, int> mpp;

        for (int i = 0; i < n; i++) {
            mpp[nums[i]]++;
        }

        for (auto it : mpp) {
            pq.push({it.second, it.first});
            if (pq.size() > k) {
                pq.pop();
            }
        }

        while (!pq.empty()) {
            ans.push_back(pq.top().second);
            pq.pop();
        }

        return ans;
    }
    ```
7. Sort characters by frequency 
    ```
       string frequencySort(string s) {

        priority_queue<pair<int, char>> pq;
        string ans;
        int n = s.size();
        unordered_map<char, int> mpp;

        for (int i = 0; i < n; i++) {
            mpp[s[i]]++;
        }

        for (auto it : mpp) {
            pq.push({it.second, it.first});
        }

        while (!pq.empty()) {
            char t = pq.top().second;
            for (int i = 0; i < pq.top().first; i++)
                ans += t;
            pq.pop();
        }

        return ans;
    }
    ```
8. Relative Ranks
    ```
      vector<string> findRelativeRanks(vector<int>& score) {
         priority_queue<tuple<int, int>> pq; 
         vector<string> answer(score.size(), "");
          for (int idx = 0; idx < score.size(); idx++) {
            pq.push(make_tuple(score[idx], idx));
        }
        int place = 1;
        while (pq.size()) {
            tuple<int, int> athelete = pq.top();
            pq.pop();
            if (place == 1)
                answer[get<1>(athelete)] = "Gold Medal";
            else if (place == 2)
                answer[get<1>(athelete)] = "Silver Medal";
            else if (place == 3)
                answer[get<1>(athelete)] = "Bronze Medal";
            else {
                answer[get<1>(athelete)] = to_string(place);
            }
               place++;
        }
        return answer;
        
    }
    ```
7. Task Scheduler
   ```
      int leastInterval(vector<char>& tasks, int n) {
        vector<int> mp(26, 0);
        for (auto& ch : tasks) {
            mp[ch - 'A']++;
        }

        priority_queue<int> pq;
        for (int i = 0; i < 26; i++) {
            if (mp[i] > 0) {
                pq.push(mp[i]);
            }
        }

        int time = 0;
        while (!pq.empty()) {
            vector<int> temp;

            for (int i = 1; i <= n + 1; i++) {
                if (!pq.empty()) {
                    int freq = pq.top();
                    pq.pop();
                    freq--;
                    temp.push_back(freq);
                }
            }
            for (auto& f : temp) {
                if (f > 0) {
                    pq.push(f);
                }
            }

            if (pq.empty()) {
                time += temp.size();
            } else {
                time += n + 1;
            }
        }
        return time;
    }
   ```
  ```
    //Greedy
      vector<int>mp(26,0);
        for(int i=0;i<tasks.size();i++)
        mp[tasks[i]-'A']++;

       sort(begin(mp),end(mp));

        int maxfreq=mp[25];
        int gs=maxfreq-1;
        int idealSpots=gs*n;

        for(int i=24;i>=0;i--)
        {
            idealSpots-=min(mp[i],gs);
        }

        if(idealSpots>0)
        {
            return tasks.size()+idealSpots;
        }
        else
        return tasks.size();

  ```
8. K frequent elements 
   ```
       static bool cmp(pair<int, string> i, pair<int, string> j) {
        if (i.first != j.first)
            return i.first > j.first;
        else {
            return i.second < j.second;
        }
    }
    vector<string> topKFrequent(vector<string>& words, int k) {
        vector<string> ans;
        unordered_map<string, int> m;
        deque<pair<int, string>> dq;

        for (int i = 0; i < words.size(); i++) {
            m[words[i]]++;
        }
        for (auto it : m)
            dq.push_back({it.second, it.first});

        sort(dq.begin(), dq.end(), cmp);
        while (k--) {
            string s = dq.front().second;
            dq.pop_front();
            ans.push_back(s);
        }
        return ans;
    }
   ```
9. Reorganize string 

   ```
    string reorganizeString(string s) {
        vector<int> count(26, 0);
        int n = s.length();

        for (int i = 0; i < s.length(); i++) {
            count[s[i] - 'a']++;

            if (count[s[i] - 'a'] > (n + 1) / 2)
                return "";
        }

        priority_queue<P, vector<P>> pq;

        for (char i = 'a'; i <= 'z'; i++) {
            if (count[i - 'a'] > 0)
                pq.push({count[i - 'a'], i});
        }
        string result = "";

        while (pq.size() >= 2) {

            auto P1 = pq.top();
            pq.pop();

            auto P2 = pq.top();
            pq.pop();

            result.push_back(P1.second);
            result.push_back(P2.second);

            P1.first--;

            if (P1.first > 0)
                pq.push(P1);

            P2.first--;

            if (P2.first > 0)
                pq.push(P2);
        }

        if (!pq.empty()) {
            result.push_back(pq.top().second);
        }

        return result;
    }
   ```
10. Car Pooling 
     ```
        bool carPooling(vector<vector<int>>& trips, int capacity) {
        vector<int> mp(1001, 0);

        for (int i = 0; i < trips.size(); i++) {
            mp[trips[i][1]] += trips[i][0];
            mp[trips[i][2]] -= trips[i][0];
        }
        int s = 0;
        for (int i = 0; i < 1001; i++) {
            s += mp[i];
            if (s > capacity)
                return false;
        }
        return true;
    }
     ```
11. Kth smallest Prime fraction 
     ```
       vector<int> kthSmallestPrimeFraction(vector<int>& arr, int k) {
        int n = arr.size();
        priority_queue<pair<double,pair<int,int>>> pq;

        for(int i=0;i<n;i++){
            for(int j=i+1;j<n;j++){
                double x = arr[i]/(arr[j]*1.0);
                
                if(pq.size() == k){
                    if(x < pq.top().first){
                        pq.pop();
                        pq.push({x,{arr[i],arr[j]}});
                    }
                }
                else{
                    pq.push({x,{arr[i],arr[j]}});
                }
            }
        }

        return {pq.top().second.first,pq.top().second.second};
        
    }
     ```
   ```
     //Approach 2
       priority_queue<V, vector<V>, greater<V>> pq;
        for (int i = 0; i < n; i++)
            pq.push({1.0 * arr[i] / arr.back(), (double)(i), (double)(n - 1)});
        int smallest = 1;

        while (smallest < k) {
            V vec = pq.top();
            pq.pop();

            int i = vec[1];
            int j = vec[2] - 1;

            pq.push({1.0 * arr[i] / arr[j], (double)(i), (double)(j)});
            smallest++;
        }
        V vec = pq.top();
        int i = vec[1];
        int j = vec[2];
        return {arr[i], arr[j]};
    }

   ```
12. Minimum cost to hire k workers 
     ```
       int n = quality.size();
        vector<double> manager_ratio(n);
        double ans = DBL_MAX;

        for (int i = 0; i < quality.size(); i++) {
            manager_ratio[i] = ((double)wage[i] / quality[i]);
        }
        for (int i = 0; i < n; i++) {
            double mr = manager_ratio[i];
            priority_queue<double> pq;

            for (int i = 0; i < n; i++) {
                if (mr >= manager_ratio[i]) {

                    pq.push((double)mr * quality[i]);
                }
                if (pq.size() > k)
                    pq.pop();
            }
            if (pq.size() < k)
                continue;
            double d = 0;
            while (pq.size()) {
                d += pq.top();
                pq.pop();
            }
            ans = min(ans, d);
        }
        return ans;
    }
     ```
   ```
    //Approach 2
        int n = quality.size();

        double result = DBL_MAX; //std::numeric_limits<double>::max()
        //maximum representable finite floating-point (double) number

        vector<pair<double, int>> worker_ratio(n);
        for(int worker = 0; worker < n; worker++) {
            worker_ratio[worker] = make_pair((double)min_wage[worker]/quality[worker], quality[worker]);
        }

        sort(begin(worker_ratio), end(worker_ratio));

        for(int manager = k-1; manager < n; manager++) {
            
            double managerRatio = worker_ratio[manager].first;

            vector<double> group;
            for(int worker = 0; worker <= manager; worker++) {
                double worker_wage = worker_ratio[worker].second * managerRatio;
                group.push_back(worker_wage);
            }

            priority_queue<double, vector<double>> pq;
            double sum = 0;

            for(double &wage : group) {
                sum += wage;
                pq.push(wage);

                if(pq.size() > k) {
                    sum -= pq.top();
                    pq.pop();
                }
            }

            result = min(result, sum);

        }

        return result;
   ```
   ```
    \\Best approach 
       int n = quality.size();
        vector<pair<double, int>> worker_ratio(n);
        for (int worker = 0; worker < n; worker++) {
            worker_ratio[worker] = make_pair(
                (double)wage[worker] / quality[worker], quality[worker]);
        }
        sort(begin(worker_ratio), end(worker_ratio));

        priority_queue<int, vector<int>> pq;

        double sum_quality = 0;
        for (int i = 0; i < k; i++) {
            pq.push(worker_ratio[i].second); // push all qualities in max-heap
            sum_quality += worker_ratio[i].second; // Find sum of qualities
        }

        double managerRatio = worker_ratio[k - 1].first;
        double result = managerRatio * sum_quality;

        for (int manager = k; manager < n; manager++) {

            managerRatio = worker_ratio[manager].first;

            pq.push(
                worker_ratio[manager].second); // push all qualities in max-heap
            sum_quality += worker_ratio[manager].second; // Find sum of
                                                         // qualities

            if (pq.size() > k) {
                sum_quality -= pq.top();
                pq.pop();
            }

            result = min(result, managerRatio * sum_quality);
        }

        return result;

   ```
13. Maximum subsequence score
    ```
     //DP
     long long solve(vector<int>& nums1, vector<int>& nums2, int sum, int minE,
                    int count, int k, int i, int n) {
        if (count == k)
            return sum * minE;
        if (i >= n)
            return 0;
        int take_i = solve(nums1, nums2, sum + nums1[i], min(minE, nums2[i]),
                           count + 1, k, i + 1, n);
    int not_take=solve(nums1,nums2,sum,minE,count,k,i+1,n);
    return max(take_i, not_take);
    }
    long long maxScore(vector<int>& nums1, vector<int>& nums2, int k) {
        int sum = 0;
        int minE = INT_MAX;
        int count = 0;
        int i = 0;
        int n = nums1.size();
        return solve(nums1, nums2, sum, minE, count, k, i, n);
    }
    ```
   ```
   \\Optimal 
     static bool cmp(pair<int,int> p,pair<int,int>q)
    {
        return p.first>q.first;
    }
    long long maxScore(vector<int>& nums1, vector<int>& nums2, int k) {
        int n=nums1.size();
        vector<pair<int,int>> v;
        for(int i=0;i<n;i++)
        {
            v.push_back({nums2[i],nums1[i]});
        }
        sort(v.begin(),v.end(),cmp);
        priority_queue<int,vector<int>,greater<int>> pq;
        long long kSum;
        for(int i=0;i<k;i++)
        {
            kSum+=v[i].second;
            pq.push(v[i].second);
        }
        long long res=kSum*v[k-1].first;
        for(int i=k;i<n;i++)
        {
            kSum-=pq.top();
            pq.pop();
            kSum+=v[i].second;
            pq.push(v[i].second);
            res=max(res,kSum*v[i].first);

        }
        return res;
    }
   ```
14. Single threaded CPU
    ```
     vector<int> getOrder(vector<vector<int>>& tasks) {
        vector<int> ans;
        int n = tasks.size();
        vector<array<int, 3>> v;

        for (int i = 0; i < n; i++) {
            int st = tasks[i][0];
            int pt = tasks[i][1];
            v.push_back({st, pt, i});
        }
        sort(v.begin(), v.end());
        long long curr_time = 0;
        int idx = 0;
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;
        while (idx < n || !pq.empty()) {
            if (pq.empty() && curr_time < v[idx][0]) {
                curr_time = v[idx][0];
            }
            while (idx < n && v[idx][0] <= curr_time) {
                pq.push({v[idx][1], v[idx][2]});
                idx++;
            }
            pair<int, int> curr_task = pq.top();
            pq.pop();
            curr_time += curr_task.first;
            ans.push_back(curr_task.second);
        }
        return ans;
    }
    ```
15. Meeting rooms III
   ```
     int mostBooked(int n, vector<vector<int>>& meetings) {
        sort(meetings.begin(), meetings.end());

        int m = meetings.size();
        vector<int> room_meeting_count(n, 0);
        priority_queue<int, vector<int>, greater<int>> available;
        priority_queue<pair<long long, int>, vector<pair<long long, int>>,
                       greater<pair<long long, int>>>
            busy;

        for (int i = 0; i < n; ++i) {
            available.push(i);
        }

        for (const auto& meeting : meetings) {
            int start = meeting[0], end = meeting[1];
            while (busy.size() > 0 && busy.top().first <= start) {
                available.push(busy.top().second);
                busy.pop();
            }

            if (available.size() > 0) {
                int top = available.top();
                room_meeting_count[top]++;
                available.pop();
                busy.push({end, top});
                continue;
            }
            // no available rooms, will have to wait for occupied room to get
            // empty. earliest room that will be available.
            auto top = busy.top();
            int available_time = top.first, index = top.second;
            busy.pop();
            room_meeting_count[index]++;
            // the end time of current meeting will be
            // (end time of meeting that finishes earliest)+ duration of current
            // meeting
            busy.push({top.first + end - start, index});
        }

        return max_element(room_meeting_count.begin(),
                           room_meeting_count.end()) -
               room_meeting_count.begin();
    }
   ```
16. Kth Largest element in an array 
    ```
       int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int, vector<int>, greater<int>> pq;

        for (int i = 0; i < nums.size(); i++) {
            pq.push(nums[i]);
            if (pq.size() > k)
                pq.pop();
        }

        return pq.top();
    }
    ```
17. Kth Largest element in a stream
    ```
     class KthLargest {
    public:
    priority_queue<int, vector<int>, greater<int>> pq;
    int K;

    KthLargest(int k, vector<int>& nums) {
        K = k;
        for (int i = 0; i < nums.size(); i++) {
            pq.push(nums[i]);
            if (pq.size() > K)
                pq.pop();
        }
    }

    int add(int val) {
        pq.push(val);
        if (pq.size() > K)
            pq.pop();
        return pq.top();
    }
    };

    ```
18. IPO
   ```
    int findMaximizedCapital(int k, int w, vector<int>& profits,
                             vector<int>& capital) {
        int n = profits.size();
        vector<pair<int, int>> vec;
        for (int i = 0; i < n; i++) {
            vec.push_back({capital[i], profits[i]});
        }
        sort(vec.begin(), vec.end());

        int i = 0;
        priority_queue<int> pq;

        while (k--) {
            while (i < n && vec[i].first <= w) {
                pq.push(vec[i].second);
                i++;
            }
            if (pq.empty())
                break;
            w += pq.top();
            pq.pop();
        }
        return w;
    }
   ```
19. Seat Manager
   ```
     class SeatManager {
public:
int last;
    std::priority_queue<int, std::vector<int>, std::greater<int>> pq;
    SeatManager(int n) {
        last=0;
        
    }
    
    int reserve() {
         if (pq.empty()) {
            return ++last;
        } else {
            int seat = pq.top();
            pq.pop();
            return seat;
        }
        
    }
    
    void unreserve(int seatNumber) {
         if (seatNumber == last) {
            --last;
        } else {
            pq.push(seatNumber);
        }
        
    }
};
   ```
20.  Find Subsequence of Length K With the Largest Sum
   ```
    vector<int> maxSubsequence(vector<int>& nums, int k) {
         priority_queue<pair<int,int> , vector<pair<int,int>>,greater<pair<int,int>> > pq;
        for (int i = 0; i < nums.size(); i++) {
        pq.push({nums[i],i});
        if (pq.size() > k)
            pq.pop();
    }
     priority_queue<pair<int,int> , vector<pair<int,int>>,greater<pair<int,int>> > temp;

        while(!pq.empty()){
            temp.push({pq.top().second,pq.top().first});
            pq.pop();
        }  
         vector<int> ans;
        while(!temp.empty()){
            ans.push_back(temp.top().second);
            temp.pop();
        }

        return ans;
    }
   ```
21. Min number of refueling stops 
    ```
      int minRefuelStops(int target, int startFuel,
                       vector<vector<int>>& stations) {
         int i = 0, res;
        priority_queue<int>pq;
        for (res = 0; startFuel < target; res++) {
            while (i < stations.size() && stations[i][0] <= startFuel)
                pq.push(stations[i++][1]);
            if (pq.empty()) return -1;
            startFuel += pq.top(), pq.pop();
        }
        return res;
        
                       }
    ```
22. Minimum operations to halve array sum 
    ```
      int halveArray(vector<int>& nums) {
        priority_queue<double> Q;

        double sum = 0;

        for (auto ele : nums) {
            sum += ele;
            Q.push(ele);
        }

        double half = sum / 2;

        int count = 0;
        while (sum > half) {
            double max = Q.top();
            Q.pop();

            max /= 2;
            Q.push(max);
            count++;

            sum -= max;
        }

        return count;
    }
    ```
23. Min operations to make prefix sum non negative
     ```
       int makePrefSumNonNegative(vector<int>& nums) {
        long long sum=0;
        priority_queue<int, vector<int>, greater<int>> pq;
        for (int i : nums) {
            pq.push(i);
            if ((sum += i) < 0)
                sum -= pq.top(), pq.pop();
        }
        return nums.size() - pq.size();
    }
     ```
24. Find k pairs with smallest sums
     ```
       vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
        vector<vector<int>> ans;

       
        auto comp = [](const vector<int>& a, const vector<int>& b) {
            return a[0] > b[0];
        };
        priority_queue<vector<int>, vector<vector<int>>, decltype(comp)> pq(comp);

      
        for (int i = 0; i < nums1.size() && i < k; i++) {
            pq.push({nums1[i] + nums2[0], i, 0});
        }

        // Extract the k smallest pairs
        while (!pq.empty() && k > 0) {
            vector<int> t = pq.top();
            pq.pop();

            int i = t[1];
            int j = t[2];

            // Add the current pair to the answer list
            ans.push_back({nums1[i], nums2[j]});

            // If possible, add the next pair in the current row to the heap
            if (j + 1 < nums2.size()) {
                pq.push({nums1[i] + nums2[j + 1], i, j + 1});
            }

            k--;
        }

        return ans;
        
    }
     ```
25. Path with maximum probability

    ```
           double maxProbability(int n, vector<vector<int>>& edges,
                          vector<double>& succProb, int start_node,
                          int end_node) {
        vector<vector<pair<int, double>>> adj(n);
        vector<double> prob(n, 0.0);
        prob[start_node] = 1.0;
        for (int i = 0; i < edges.size(); i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            double w = succProb[i];

            adj[u].push_back({v, w});
            adj[v].push_back({u, w});
        }
        priority_queue<pair<double, int>> pq;
        pq.push({1.0, start_node});
        while (!pq.empty()) {
            double currProb = pq.top().first;
            int node = pq.top().second;
            pq.pop();

            if (node == end_node)
                return currProb;

            for (auto& neighbour : adj[node]) {
                int adjNode = neighbour.first;
                double neighbourProb = neighbour.second;

                if (currProb * neighbourProb > prob[adjNode]) {
                    prob[adjNode] = currProb * neighbourProb;
                    pq.push({prob[adjNode], adjNode});
                }
            }
        }
        return 0.0;
        }
    ```
26. Minimum Cost to Make at Least One Valid Path in a Grid
     ```
       int minCost(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();

        map<pl, vector<pll>> graph;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (j < m - 1)
                    graph[{i, j}].push_back(
                        {{i, j + 1}, grid[i][j] == 1 ? 0 : 1});
                if (i < n - 1)
                    graph[{i, j}].push_back(
                        {{i + 1, j}, grid[i][j] == 3 ? 0 : 1});
                if (j > 0)
                    graph[{i, j}].push_back(
                        {{i, j - 1}, grid[i][j] == 2 ? 0 : 1});
                if (i > 0)
                    graph[{i, j}].push_back(
                        {{i - 1, j}, grid[i][j] == 4 ? 0 : 1});
            }
        }
        priority_queue<plp, vector<plp>, greater<plp>> q;
        q.push({0, {0, 0}});
        vector<vector<int>> dist(n, vector<int>(m, 1e9));
        dist[0][0] = 0;
        while (!q.empty()) {
            auto node = q.top().second;
            int distance = q.top().first;
            q.pop();
            if (distance != dist[node.first][node.second])
                continue;

            for (auto& it : graph[node]) {
                auto adj = it.first;
                int weight = it.second;
                int x = adj.first;
                int y = adj.second;

                if (weight + distance < dist[x][y]) {
                    dist[x][y] = weight + distance;
                    q.push({weight + distance, adj});
                }
            }
        }
        return dist[n - 1][m - 1] == 1e9 ? -1 : dist[n - 1][m - 1];
    }
     ```
27. kth smallest element in a sorted matrix
     ```
       int kthSmallest(vector<vector<int>>& matrix, int k) {
        int n = matrix.size();
        priority_queue<vector<int>, vector<vector<int>>, greater<vector<int>>>
            pq;
        for (int i = 0; i < n; i++) {
            pq.push({matrix[i][0], i, 0});
        }
        int result = 0;

        for (int i = 0; i < k; i++) {
            auto elem = pq.top();
            pq.pop();
            result = elem[0];
            int row = elem[1];
            int col = elem[2];

            if (col + 1 < n) {
                pq.push({matrix[row][col + 1], row, col + 1});
            }
        }

        return result;
    }
     ```
     ```
      //Binary search
        int check(vector<vector<int>>& matrix, int mid) {
        int count = 0;
        int n = matrix.size();
        int row = 0;
        int col = n - 1;

        while (row < n && col >= 0) {

            if (matrix[row][col] <= mid) {

                count += (col + 1);
                row++;
            } else {

                col--;
            }
        }
        return count;
    }

    int kthSmallest(std::vector<std::vector<int>>& matrix, int k) {
        int n = matrix.size();
        int low = matrix[0][0];
        int high = matrix[n - 1][n - 1];
        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (check(matrix, mid) < k) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }

        return low;
    }
  
     ```
28. Stock price fluctuations 
     ```
       map<int, int> value;     // price,count
    map<int, int> timeStock; // timestamp,price
    StockPrice() {}

    void update(int timestamp, int price) {
        auto it = timeStock.find(timestamp);
        if (it != timeStock.end()) {
            int price = it->second;
            value[price]--;
            if (value[price] == 0)
                value.erase(price);
        }
        timeStock[timestamp] = price;
        value[price]++;
    }

    int current() {
        auto it = timeStock.end();
        it--;
        return it->second;
    }

    int maximum() {
        auto it = value.end();
        it--;
        return it->first;
    }

    int minimum() {
        auto it = value.begin();
        return it->first;
    }
     ```
29. Last Stone weight 

     ```
           int lastStoneWeight(vector<int>& stones) {
        priority_queue<int> pq;
        for (int i = 0; i < stones.size(); i++) {
            pq.push(stones[i]);
        }
        while (pq.size() > 1) {
            int first = pq.top();
            pq.pop();
            int second = pq.top();
            pq.pop();
            int x = first - second;
            if (x > 0) {
                pq.push(x);
            }
        }
        if (pq.size() > 0)
            return pq.top();
        else
            return 0;
         }
     ```
30. Max score from removing stones
     ```
       int maximumScore(int a, int b, int c) {
        int ans = 0;
        priority_queue<int> pq;
        pq.push(a);
        pq.push(b);
        pq.push(c);
        while (pq.top() > 0) {
            int first;
            if (pq.top() > 0) {
                first = pq.top();
                pq.pop();
            }
            int second;

            if (pq.top() > 0) {
                second = pq.top();
                pq.pop();
            }

            else {
                break;
            }
            pq.push(first - 1);
            pq.push(second - 1);
            ans++;
        }
        return ans;
    }
     ```
31. Minimum Path Cost in a Hidden Grid
     ```
       class Solution {
     public:
    vector<vector<int>> dxys{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    vector<char> dirs{'U', 'D', 'L', 'R'};
    vector<char> reverseDirs{'D', 'U', 'R', 'L'};
    int findShortestPath(GridMaster& master) {
        vector<vector<int>> grid(200, vector<int>(200, -1));
        grid[99][99] = 0;
        vector<int> target(2, -100);
        fillGrid(master, 99, 99, grid, target);

        vector<vector<bool>> visited(200, vector<bool>(200, false));
        priority_queue<vector<int>, vector<vector<int>>, greater<>> pq;
        pq.push({0, 99, 99});

        while (!pq.empty()) {
            auto cur = pq.top();
            pq.pop();
            int cost = cur[0], row = cur[1], col = cur[2];

            if (row == target[0] && col == target[1]) {
                return cost;
            }

            if (visited[row][col]) {
                continue;
            }

            visited[row][col] = true;

            for (const auto& dxy : dxys) {
                int nextRow = row + dxy[0];
                int nextCol = col + dxy[1];
                if (nextRow < 0 || nextRow >= 200 || nextCol < 0 ||
                    nextCol >= 200 || visited[nextRow][nextCol] ||
                    grid[nextRow][nextCol] == -1) {
                    continue;
                }
                int nextCost = cost + grid[nextRow][nextCol];
                pq.push({nextCost, nextRow, nextCol});
            }
        }
        return -1;
    }
    void fillGrid(GridMaster& master, int row, int col,
                  vector<vector<int>>& grid, vector<int>& target) {
        if (master.isTarget()) {
            target[0] = row;
            target[1] = col;
        }

        for (int i = 0; i < 4; i++) {
            char ch = dirs[i];
            auto dxy = dxys[i];
            int nr = row + dxy[0];
            int nc = col + dxy[1];

            if (master.canMove(ch) && grid[nr][nc] == -1) {
                int val = master.move(ch);
                grid[nr][nc] = val;
                fillGrid(master, nr, nc, grid, target);
                master.move(reverseDirs[i]);
            }
        }
    }
    };
     ```
32. Shortest Path in a Hidden Grid

      ```
        class Solution {
     public:
    vector<vector<int>> grid;
    bool foundDest;
    map<pair<int, int>, char> directions = {
        {{0, 1}, 'R'}, {{-1, 0}, 'U'}, {{1, 0}, 'D'}, {{0, -1}, 'L'}};
    map<char, char> rev = {{'U', 'D'}, {'L', 'R'}, {'D', 'U'}, {'R', 'L'}};
    int findShortestPath(GridMaster& master) {
        grid.resize(1001, vector<int>(1001, -2));
        if (!mapPaths(master)) {
            return -1;
        }
        return getShortestPath();
    }
    bool mapPaths(GridMaster& master) {
        int start = 500;
        foundDest = false;
        grid[start][start] = -1;
        dfs(master, {500, 500});
        return foundDest;
    }
    void dfs(GridMaster& master, pair<int, int> curr) {
        if (master.isTarget()) {
            grid[curr.first][curr.second] = 2;
            foundDest = true;
        }
        for (auto& dir : directions) {
            int newX = curr.first + dir.first.first;
            int newY = curr.second + dir.first.second;
            if (grid[newX][newY] != -2) {
                continue;
            } else {
                if (master.canMove(dir.second)) {
                    grid[newX][newY] = 1;
                    master.move(dir.second);
                    dfs(master, {newX, newY});
                    master.move(rev[dir.second]);
                } else {
                    grid[newX][newY] = 0;
                }
            }
        }
    }
    int getShortestPath() {
        deque<pair<int, int>> q;
        set<pair<int, int>> visited;
        q.push_back({500, 500});
        visited.insert({500, 500});
        int dist = 0;
        while (!q.empty()) {
            int size = q.size();
            dist++;
            while (size-- > 0) {
                auto curr = q.front();
                q.pop_front();
                for (auto& dir : directions) {
                    int newX = curr.first + dir.first.first;
                    int newY = curr.second + dir.first.second;
                    if (grid[newX][newY] <= 0) {
                        continue;
                    }
                    if (grid[newX][newY] == 2) {
                        return dist;
                    }
                    pair<int, int> next = {newX, newY};
                    if (visited.count(next)) {
                        continue;
                    }
                    q.push_back(next);
                    visited.insert(next);
                }
            }
        }
        return -1;
    }
    };
      ```
33. Minimum obstacle removal to reach corner
    ```
      bool isValid(int i, int j, vector<vector<int>>& grid) {
        return (i >= 0 && j >= 0 && i < grid.size() && j < grid[0].size());
    }
    int X[4] = {0, 0, 1, -1};
    int Y[4] = {1, -1, 0, 0};
    int minimumObstacles(vector<vector<int>>& grid) {
        int n = grid.size(), m = grid[0].size();

        deque<pair<int, int>> dq;

        vector<int> level(m * n, 1e6);

        vector<vector<int>> mp(n, vector<int>(m, 0));

        int k = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                mp[i][j] = k++;
            }
        }

        dq.push_back({0, 0});
        level[mp[0][0]] = 0;
        while (!dq.empty()) {
            auto curr = dq.front();
            int x = curr.first, y = curr.second;
            dq.pop_front();
            if (x == n - 1 && y == m - 1)
                return level[mp[n - 1][m - 1]];

            for (int i = 0; i < 4; ++i) {
                int newx = x + X[i], newy = y + Y[i];
                if (isValid(newx, newy, grid)) {
                    if (level[mp[x][y]] + grid[newx][newy] <
                        level[mp[newx][newy]]) {

                        level[mp[newx][newy]] =
                            level[mp[x][y]] + grid[newx][newy];

                        if (grid[newx][newy] == 1) {
                            dq.push_back({newx, newy});
                        } else {
                            dq.push_front({newx, newy});
                        }
                    }
                }
            }
        }

        return level[mp[n - 1][m - 1]];
    }
    ```
34. No of restricted path from first to last node
    ```
       const int MOD = 1e9 + 7;
    int dfs(int node, int end, vector<int>& dis,
            unordered_map<int, vector<pair<int, int>>>& adj, vector<int>& dp) {
        if (node == end) {
            return dp[node] = 1; // when we reach the final destination we
                                 // retrun 1 which signifies a path found
        }

        if (dp[node] != -1)
            return dp[node];

        int count = 0;

        for (auto u : adj[node]) {
            if (dis[node] > dis[u.first]) { // we only move forward when
                                            // condition is satisfied
                int path = dfs(u.first, end, dis, adj, dp);
                count = (count + path) %
                        MOD; // Update count with modular arithmetic.
            }
        }

        return dp[node] = count;
    }
    int countRestrictedPaths(int n, vector<vector<int>>& edges) {
        unordered_map<int, vector<pair<int, int>>> adj;

        for (int i = 0; i < edges.size(); i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            int wt = edges[i][2];

            adj[u].push_back({v, wt});
            adj[v].push_back({u, wt}); // Assuming an undirected graph
        }

        vector<int> dis(n + 1, INT_MAX);
        dis[n] = 0;

        priority_queue<pair<int, int>, vector<pair<int, int>>,
                       greater<pair<int, int>>>
            pq;
        pq.push({0, n});

        while (!pq.empty()) {
            int dist = pq.top().first;
            int node = pq.top().second;
            pq.pop();

            for (auto u : adj[node]) {
                int adjNode = u.first;
                int adjDist = u.second;

                if (dis[adjNode] > dist + adjDist) {
                    dis[adjNode] = dist + adjDist;
                    pq.push({dis[adjNode], adjNode});
                }
            }
        }
        // calling dfs from node 1 to last node
        // this will return no of restricated path
        vector<int> dp(n + 1, -1); // dp array
        int i = 1;                 // starting from node 1
        return dfs(i, n, dis, adj, dp);
    }
    ```

   
         


 
35. Split Array into Consecutive Subsequences
     ```
         bool isPossible(vector<int>& nums) {
         unordered_map<int,int>m,endWith;
        for(int val:nums){
            m[val]++;
        }
        
        int subseq=0;
        for(int val:nums){
            if(m[val]==0)continue;
            m[val]--;
            if(endWith[val-1]>0){
                endWith[val-1]--;
                endWith[val]++;
            }
            else if(m[val+1]&&m[val+2]){
                m[val+1]--;
                m[val+2]--;
                endWith[val+2]++;
            }
            else{
                return false;
            }
        }
        return true;
        
    }
     ```
  
