
1. Employee free time
  ```
    vector<Interval> employeeFreeTime(vector<vector<Interval>> schedule) {
        vector<pair<int, int>> v; // to store the intervals independent of the workers
        for(vector<Interval> t1 : schedule){
            for(Interval t2 : t1)
                v.push_back({t2.start,t2.end});
        }
        sort(v.begin(), v.end()); // sort the intervals based on the start time

        vector<Interval> res;  // resultant vector

        int maxtl = v[0].second;   // maxtl = max end time till now
        for(int i=1;i<v.size();i++){
            if(maxtl < v[i].first){  // if start time is greater than maxtl, we can have a break
                Interval* temp = new Interval();
                temp->start = maxtl;  // break time start
                temp->end = v[i].first;  // break time end
                res.push_back(*temp); // push the break in the result 
            }
            maxtl = max(maxtl , v[i].second);  // update the maxtl
        }
        return res;
        
    }
  ```
