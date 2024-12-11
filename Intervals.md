
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
2. Merge Interval
    ```
       public int[][] merge(int[][] intervals) {
        if (intervals == null || intervals.length == 0) {
            return new int[0][];
        }
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));

        List<int[]> merged = new ArrayList<>();
        int s = intervals[0][0];
        int e = intervals[0][1];
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] <= e) {

                e = Math.max(e, intervals[i][1]);
            } else {

                merged.add(new int[] { s, e });
                s = intervals[i][0];
                e = intervals[i][1];
            }
        }
        merged.add(new int[] { s, e });

        return merged.toArray(new int[merged.size()][]);

    }
    ```
