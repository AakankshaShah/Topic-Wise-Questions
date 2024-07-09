1.  kth smallest element
 priority_queue<int>pq;
        for(int i=l;i<=r;i++){
            pq.push(arr[i]);
            if(pq.size()>k) pq.pop();
        }
        return pq.top();

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
