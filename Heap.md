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
       
