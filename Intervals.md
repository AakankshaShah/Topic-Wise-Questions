
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
3. Interval List Intersection
   ```
       vector<vector<int>> intervalIntersection(vector<vector<int>>& A,
                                             vector<vector<int>>& B) {
        vector<vector<int>> ans;

        if (A.size() == 0)
            return ans;
        if (B.size() == 0)
            return ans;
        for (int i = 0, j = 0; i < A.size() && j < B.size();) {
            int lo = max(A[i][0], B[j][0]), hi = min(A[i][1], B[j][1]);
            if (lo <= hi)
                ans.push_back({lo, hi});
            if (hi == A[i][1])
                i++;
            else
                j++;
        }
        return ans;
    }
   ```
4. Insert interval
    ```
       public int[][] insert(int[][] intervals, int[] newInterval) {
         List<int[]> result = new ArrayList<>();
    int i = 0;
    int n = intervals.length;

    
    while (i < n && intervals[i][1] < newInterval[0]) {
        result.add(intervals[i]);
        i++;
    }
    while (i < n && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = Math.min(intervals[i][0], newInterval[0]);
        newInterval[1] = Math.max(intervals[i][1], newInterval[1]);
        i++;
    }
    result.add(newInterval);
     while (i < n) {
        result.add(intervals[i]);
        i++;
    }


    return result.toArray(new int[result.size()][]);
        
    }
    ```
