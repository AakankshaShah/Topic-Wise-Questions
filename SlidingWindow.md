# Sliding window

## Theory 
https://www.youtube.com/watch?v=EHCGAZBbB88&list=PL_z_8CaSLPWeM8BDJmIYDaoQ5zuwyxnfj

Fixed sliding window [K given]

```
int i=0;j=0;

while(j<size)
{
calculations

if(j-i+1<k)
j++;

else if (j-i+1==k)
{
calculations
i++;
j++;

}

}
```


Variable length sliding window [ K to find]

```
int i=0;j=0;

while(j<size)
{
calculations

ifcondition_not reached c<k)
j++;

else if (condition reached c==k)
{
calculations
j++;

}

else if(c>k)
{
i++;
j++;
}
```

## Questions


1. Max in all all subarrays of size k
2. Max of distant subarray of size k*
3. Max sum in a subarray of size k
4. Count occurrence of anagrams**
5. Largest subarray of sum k - Variation If contains negative numbers
6. Largest substring with k unique characters
7. Longest substring with no repitions*
    ```
      int lengthOfLongestSubstring(string s) {

        int i = 0;
        int j = 0;
        unordered_map<char, int> m;
        int ans = 0;
        int n = s.length();

        while (j < n) {

            m[s[j]]++;

            if (m.size() == j - i + 1) {
                ans = max(ans, j - i + 1);
            }

            else if (m.size() < j - i + 1) {
                while (m.size() <
                       j - i +
                           1) // so till the duplicates are removed completely
                {
                    m[s[i]]--;        // remove the duplicates
                    if (m[s[i]] == 0) // if the frequency becomes zero
                    {
                        m.erase(s[i]); // delete it completely
                    }
                    i++; // go for next element
                }
            }
            j++;
        }

        return ans;
    }
    ```
8. Pick toys*
    ```
       int totalFruit(vector<int>& fruits) {

        unordered_map<int, int> m;
        int ans = 0;

        int i = 0;
        int j = 0;

        int n = fruits.size();
        if (n == 0)
            ans = 0;
        else
            ans = 1;

        while (j < n) {
            m[fruits[j]]++;
            if (m.size() <= 2) {
                ans = max(ans, j - i + 1);
                j++;
            } else {
                while (m.size() > 2) {
                    m[fruits[i]]--;
                    if (m[fruits[i]] == 0)
                        m.erase(fruits[i]);

                    i++;
                }
                j++;
            }
        }
        return ans;
    }
    ```
9. Minimum Window Substring***

```
 unordered_map<char,int>m;

        for(int i=0;i<t.length();i++)
        {
            m[t[i]]++;

        }

        
        int count=m.size();
        int ans=s.length()+1;
        int n=s.length();
        int s1=-1;
   
        string sans="";


if(t.length()>s.length())
return sans;

   int i=0;int j=0;  
   cout<<"count"<<count<<"\n";   
   while(j<n)
        {
            if(m.find(s[j])!=m.end())
           {m[s[j]]--;

            if(m[s[j]]==0)
            count--;
           }
            cout<<"count"<<count<<j<<"\n";   

            while(count==0)
            {
                if(ans>j-i+1)
                {
                    ans=j-i+1;
                    s1=i;
                }

                if(m.find(s[i])!=m.end())
                {
                    m[s[i]]++;
                    if(m[s[i]]>0)
                    count++;
                }
                i++;


                
            }
            j++;
    
        }

        if(ans==s.length()+1)
        return sans;
        else
        return s.substr(s1,ans);
```

10. Longest repeating character replacement https://leetcode.com/problems/longest-repeating-character-replacement/
     ```
       int characterReplacement(string s, int k) {

        int i = 0, j = 0;

        int n = s.length();
        int ans = 0;
        int maxf = 0;

        unordered_map<char, int> m;

        for (int j = 0; j < n; j++) {
            m[s[j]]++;
            maxf = max(maxf, m[s[j]]);

            if (j - i + 1 - maxf > k) {
                m[s[i]]--;
                i++;
            } else {
                ans = max(ans, j - i + 1);
            }
        }
        return ans;
    }
     ```
11. Max consecutive ones https://leetcode.com/problems/max-consecutive-ones-iii/
```

        while(right < n) {
            if(nums[right]==0) {
                count++;
            }
            
            while(count > k) {
                if(nums[left]==0) {
                    count--;
                }
                left++;
            }
            ans = max(ans, right-left+1);
            right++;
        }
```

12. Pick from any side  ||Max points from cards 

    ```
       nt maxScore(vector<int>& cardPoints, int k) {
        int ans = 0;
        int s = 0;
        int n = cardPoints.size();

        for (int i = 0; i < k; i++) {
            s += cardPoints[i];
        }

        int j = n - 1;
        int ls = s;
        int rs = 0;
        for (int i = k - 1; i >= 0; i--) {
            ls -= cardPoints[i];
            rs += cardPoints[j];
            j--;
            s = max(s, ls + rs);
        }
        return s;
    }
     ```
     ```
      int sum = 0;
        int maxi = 0;

        for (int i = 0; i < B; i++)
            sum += A[i];

        int r = A.size() - 1;
        maxi = sum;

        for (int i = B - 1; i >= 0; i--) {
            sum -= A[i];

            sum += A[r];

            maxi = max(maxi, sum);

            r--;
        }

        return maxi;
     ```
13. Subarray with given sum 

    ```
      int i=0,j=0;
        int sum=0;
        vector<int>ans;
        
        while(j<n)
        {
            sum+=arr[j];
            
            while(i<j&&sum>s)
            {
                sum-=arr[i];
                i++;
            }
            
            if(sum==s)
            {
                ans.push_back(i+1);
                ans.push_back(j+1);
                return ans;
                
            }
            j++;
            
            
        }
        ans.push_back(-1);
        return ans;
    ```
14. Repeated dna sequence 
    ```
        vector<string> findRepeatedDnaSequences(string s) {
        int n = s.length();

        if (n < 10)
            return {};

        unordered_map<string, int> mp;
        for (int i = 0; i <= n - 10; i++) {
            string tmp = s.substr(i, 10);
            mp[tmp]++;
        }

        vector<string> ans;
        for (auto i : mp) {
            if (i.second > 1)
                ans.push_back(i.first);
        }

        return ans;
    }
    ```
15. Minimize size  subarray sum
     ```
         int minSubArrayLen(int target, vector<int>& nums) {
        int ans = INT_MAX;
        int s = 0;
        int i = 0, j = 0;
        while (j < nums.size()) {
            s += nums[j];
            while (s >= target) {
                ans = min(ans, j - i + 1);
                s -= nums[i];
                i++;
            }
            j++;
        }
        if (ans == INT_MAX)
            return 0;

        return ans;
    }
     ```
16. Sliding window maximum
     ```
       vector<int> maxSlidingWindow(vector<int>& nums, int k) {

        vector<int> ans;
        int i = 0;
        int j = 0;
        deque<int> q;

        while (j < nums.size()) {
            while (!q.empty() && nums[q.back()] < nums[j])
                q.pop_back();

            q.push_back(j);
            if (j - i + 1 < k)
                j++;
            else if (j - i + 1 == k) {
                ans.push_back(nums[q.front()]);
                if (q.front() == i) {
                    q.pop_front();
                }
                i++;
                j++;
            }
        }
        return ans;
    }
     ```
17. Longest substring with at least char repeating k times **
    ```

        int longestSubstring(string s, int k) {
        int j = 0;
        unordered_map<char, int> mp;
        int freq[26] = {0};
        int n = s.length();
        int ans = 0;

        while (j < s.length()) {
            mp[s[j]]++;
            j++;
        }
        int unique = mp.size();

        for (int i = 1; i <= unique; i++) {
            memset(freq, 0, sizeof(freq));
            int st = 0, e = 0;
            int c = 0, ck = 0;

            while (e < n) {
                if (c <= i) {
                    int ind = s[e] - 'a';
                    if (freq[ind] == 0)
                        c++;
                    freq[ind]++;
                    if (freq[ind] == k)
                        ck++;
                    e++;

                } else {
                    int ind = s[st] - 'a';
                    if (freq[ind] == k)
                        ck--;
                    freq[ind]--;
                    if (freq[ind] == 0)
                        c--;
                    st++;
                }
                if (c == i && ck == i) {
                    ans = max(ans, e - st);
                }
            }
        }
        return ans;
    }
     ```
18. Max avg subarray I
    ```
        double findMaxAverage(vector<int>& nums, int k) {
        int i = 0;
        int j = 0;
        double s = 0.0;
        double ans = INT_MIN;

        while (j < nums.size()) {
            s += nums[j];
            if(j-i+1<k)
            j++;
            else if (j - i + 1 == k) {
                ans = max(ans, (double)s / k);
                s -= nums[i];
                i++;
                j++;
            }
       
        }
            return ans;
        }
     ```
19. Length of longest subarray with atmost k freq  
     ```
         int maxSubarrayLength(vector<int>& nums, int k) {
        int ans = 0;
        unordered_map<int, int> mp;
        int i = 0, j = 0;

        while (j < nums.size()) {
            mp[nums[j]]++;
            if (mp[nums[j]] <= k) {
                ans = max(ans, j - i + 1);
            } else {
                while (mp[nums[j]] > k) {
                    mp[nums[i]]--;
                    i++;
                }
            }
            j++;
        }
        return ans;
    }
     ```
20. Max number of vowels
      ```
         int maxVowels(string s, int k) {
        int c = 0;
        int ans = 0;
        int j = 0;
        int i = 0;
        while (j < s.size()) {
            if (s[j] == 'a' || s[j] == 'e' || s[j] == 'i' || s[j] == 'o' ||
                s[j] == 'u')
                c++;
            if (j - i + 1 < k)
                j++;
            else if (j - i + 1 == k) {
                ans = max(ans, c);
                if (s[i] == 'a' || s[i] == 'e' || s[i] == 'i' || s[i] == 'o' ||
                    s[i] == 'u')
                    c--;
                i++;
                j++;
            }
        }
        return ans;
    }
       ``` 

21. Frequency of most frequent element **
      ```
          int bsearch(vector<int>& nums, int target, int k, vector<long long>& prefix) {
        long long t = nums[target];
        long long l = 0;
        long r = target;
        long long best = target;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            int op = t * (target - mid + 1) -
                     (prefix[target] - (mid > 0 ? prefix[mid - 1] : 0));

            if (op > k)
                l = mid + 1;
            else {
                best = mid;
                r = mid - 1;
            }
        }
        return target - best + 1;
    }
    int maxFrequency(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        vector<long long> prefix(n, 0);
        prefix[0] = nums[0];
        for (int i = 1; i < n; i++) {
            prefix[i] = prefix[i - 1] + nums[i];
        }
        int ans = 0;
        for (int target = 0; target < n; target++) {
            ans = max(ans, bsearch(nums, target, k, prefix));
        }
        return ans;
    }
       ```
       ```
        //Sliding Window
         long long total = 0;
    int maxFrequency(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        int l = 0;
        int r = 0;
        int ans = 0;
        while (r < n) {
            int t = nums[r];
            total += nums[r];
            while ((long long)t * (r - l + 1) - total > k) {
                total -= nums[l];
                l++;
            }

            ans = max(ans, r - l + 1);
            r++;
        }
        return ans;
    }
       ```
22. Contains duplicates II
     ```
       bool containsNearbyDuplicate(vector<int>& nums, int k) {
        bool ans;
        unordered_map<int, int> m;
        int j = 0;
        int i = 0;

        while (j < nums.size()) {
            m[nums[j]]++;
            if (m[nums[j]] > 1) {
                return true;
            }
            if (j - i == k) {
                m[nums[i]]--;
                if (m[nums[i] == 0])
                    m.erase(nums[i]);
                i++;
            }

            j++;
        }
        return false;
    }
      ```
23. Find all anagrams in a string 

         ```
           bool is_check(vector<int>& mp1, vector<int>& mp2) {
        for (int i = 0; i < 26; i++) {
            if (mp1[i] != mp2[i])
                return false;
        }

        return true;
        }
        vector<int> findAnagrams(string s, string p) {
        vector<int> ans;
        int i = 0;
        int j = 0;
        vector<int> mp(26, 0);

        vector<int> mp1(26, 0);
        for (int i = 0; i < p.length(); i++)
            mp[p[i] - 'a']++;
        i = 0;

        while (j < s.length()) {
            mp1[s[j] - 'a']++;

            if (j - i + 1 == p.length()) {
                if (is_check(mp, mp1))
                    ans.push_back(i);
                mp1[s[i] - 'a']--;
                i++;
            }
            j++;
        }
        return ans;
        }
        ```

24. Binary subarray with sum 
      ```
         int numSubarraysWithSum(vector<int>& nums, int goal) {
        unordered_map<int, int> m;
        int ans = 0;
        int curr = 0;

        for (int i = 0; i < nums.size(); i++) {
            curr += nums[i];

            if (curr == goal)
                ans++;

            if (m.find(curr - goal) != m.end()) {
                ans += m[curr - goal];
            }

            if (m.find(curr) != m.end())
                m[curr]++;
            else
                m[curr] = 1;
        }
        return ans;
    }
      ```
25. Minimum number of k bit flips || Minimum Operations to Make Binary Array Elements Equal to One I
     ```
       //Approach 1
       int minKBitFlips(vector<int>& nums, int k) {
        int n = nums.size();
        int flips = 0;
        int flipCountforIfromPast = 0;
        vector<bool> isFlipped(n, false);

        for (int i = 0; i < n; i++) {
            if (i >= k && isFlipped[i - k] == true) {
                flipCountforIfromPast--;
            }
            if (flipCountforIfromPast % 2 == nums[i]) {
                // flip at index i

                if (i + k > n)
                    return -1;
                flipCountforIfromPast++;
                flips++;
                isFlipped[i] = true;
            }
        }

        return flips;
    }
     ```
     ```
       //Approach 2 Remove extra space
          int n = nums.size();
        int flips = 0;
        int flipCountforIfromPast = 0;
       // vector<bool> isFlipped(n, false);

        for (int i = 0; i < n; i++) {
            if (i >= k && nums[i-k]==5) {
                flipCountforIfromPast--;
            }
            if (flipCountforIfromPast % 2 == nums[i]) {
                // flip at index i

                if (i + k > n)
                    return -1;
                flipCountforIfromPast++;
                flips++;
                nums[i]=5;
               // isFlipped[i] = true;
            }
        }

        return flips;
    }
      
     ```
     ```
       //Approach 3
         int n = nums.length;

        int flips = 0;
        Deque<Integer> flipQue = new LinkedList<>();
        int flipCountFromPastForCurri = 0;

        for (int i = 0; i < n; i++) {
            if (i >= k) {
                flipCountFromPastForCurri -= flipQue.pollFirst();
            }

            if (flipCountFromPastForCurri % 2 == nums[i]) {
                if (i + k > n) {
                    return -1;
                }
                flipCountFromPastForCurri++;
                flipQue.addLast(1);
                flips++;
            } else {
                flipQue.addLast(0);
            }
        }

        return flips;
     ```
26. Bulb Switcher - No. of perfect square 

27. Grumpy Bookstore owner**
     ```
         int maxSatisfied(vector<int>& customers, vector<int>& grumpy, int minutes) {
        int n = customers.size();
        int unsat = 0;
        for (int i = 0; i < minutes; i++) {
            unsat += customers[i] * grumpy[i];
        }

        int maxUnsat = unsat;
         int i = 0;
        int j = minutes;

        while(j < n) {
            unsat += customers[j] * grumpy[j];            
            unsat -= customers[i] * grumpy[i];           
            
            maxUnsat = max(maxUnsat, unsat);       
            i++;
            j++;
        }

        int totalCustomers = maxUnsat;

        // Calculate total satisfied customers
        for (int i = 0; i < n; i++) {
            totalCustomers += customers[i] * (1 - grumpy[i]);
        }

        return totalCustomers;
    }
     ```
28. Count number of nice subarrays**

    ```
    int numberOfSubarrays(vector<int>& nums, int k) {

        int ansCnt = 0;
        int cnt = 0;
        int i = 0, j = 0;

        while (j < nums.size()) {
            if (nums[j] % 2 != 0) {
                k--;
                cnt = 0;
            }

            while (k == 0) {
                if (nums[i] % 2 != 0) {
                    k++;
                }
                cnt++;
                i++;
            }

            ansCnt += cnt;
            j++;
        }

        return ansCnt;
    }
     ```
29. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit
     ```
        int longestSubarray(vector<int>& nums, int limit) {

        int l = 0;
        int r = 0;

        multiset<int, greater<int>> ms;
        int longest = 0;

        while (r < nums.size()) {
            ms.insert(nums[r]);
            int largest = *ms.begin();
            int smallest = *ms.rbegin();
            while (abs(largest - smallest) > limit) {
                ms.erase(ms.find(nums[l]));

                l++;
                largest = *ms.begin();
                smallest = *ms.rbegin();
            }
            longest = max(longest, r - l + 1);
            r++;
        }
        return longest;
    }
     ```
30. Permutation in a string 
     ```
       bool checkInclusion(string s1, string s2) {

        int n1 = s1.length();
        int n2 = s2.length();

        unordered_map<char, int> m;
        for (int i = 0; i < n1; i++) {
            m[s1[i]]++;
        }

        int count = m.size();

        int i = 0;
        int j = 0;
        while (j < n2)

        {
            if (m.find(s2[j]) != m.end()) {
                m[s2[j]]--;
                if (m[s2[j]] == 0)
                    count--;
            }

            if (j - i + 1 == n1) {
                if (count == 0)
                    return true;

                if (m.find(s2[i]) != m.end()) {
                    m[s2[i]]++;

                    if (m[s2[i]] == 1)
                        count++;
                }
                i++;
            }

            j++;
        }
        return false;
    }
     ```
31. Longest Nice Subarray
    ```
      int longestNiceSubarray(vector<int>& nums) {
        long long num = 0;
        int i = 0;
        int j = 0;
        int ans = INT_MIN;
        while (j < nums.size()) {
            if ((num & nums[j]) == 0) {
                num |= nums[j];
                ans = max(j - i + 1, ans);
                j++;

            }

            else {
                while ((num & nums[j]) != 0) {
                    num ^= nums[i];
                    i++;
                }
            }
        }
        return ans;
    }
    ```
32. Find median in a data stream 
    ```
      class MedianFinder {
    public:
    priority_queue<int> leftHeap;
    priority_queue<int, vector<int>, greater<int>> rightHeap;

    MedianFinder() {}

    void addNum(int num) {
        if (leftHeap.size() == 0 || num < leftHeap.top()) {
            leftHeap.push(num);

        } else {
            rightHeap.push(num);
        }
        if (leftHeap.size() > rightHeap.size() + 1) {
            rightHeap.push(leftHeap.top());
            leftHeap.pop();
        } else if (rightHeap.size() > leftHeap.size()) {
            leftHeap.push(rightHeap.top());
            rightHeap.pop();
        }
    }

    double findMedian() {
        if (rightHeap.size() < leftHeap.size()) {
            return (double)leftHeap.top();
        } else {
            return ((double)(leftHeap.top() + rightHeap.top())) / 2;
        }
    }
    };

    ```
33. Longest substring with atmost two distinct characters
    ```
      int lengthOfLongestSubstringTwoDistinct(string s) 
    {
        int i=0;
        int j=0;
        int n =s.length();
        int ans=0;
        unordered_map<char,int>m;
        while(j<n)
        {
            m[s[j]]++;
            while(m.size()>2)
            {
                m[s[i]]--;
                if(m[s[i]]==0)
                m.erase(s[i]);
                i++;
            }
            ans=max(ans,j-i+1);
            j++;

        }
        return ans;
        
    }
    ```

34. Count no of substring with dominant ones 
    ```
    ```
35. Sliding window median
    ```
      vector<double> medianSlidingWindow(vector<int>& nums, int k) {
        vector<double> res;
        vector<long long> med;
        for (int i = 0; i < k; i++)
            med.insert(lower_bound(med.begin(), med.end(), nums[i]), nums[i]);
        if (k % 2 == 0)
            res.push_back((double)(med[k / 2] + med[k / 2 - 1]) / 2);
        else
            res.push_back((double)med[k / 2]);

        for (int i = k; i < nums.size(); i++) {
            med.erase(lower_bound(med.begin(), med.end(), nums[i - k]));
            med.insert(lower_bound(med.begin(), med.end(), nums[i]), nums[i]);
            if (k % 2 == 0)
                res.push_back((double)(med[k / 2] + med[k / 2 - 1]) / 2);
            else
                res.push_back((double)med[k / 2]);
        }
        return res;
    }
    ```
36. Max consecutive ones II
     ```
        int result = 0;
        int current = 0;
        int prev = -1;
        for (int num: nums) {
            if (num == 1) {
                current++;
                result = max(result, current + prev + 1);
            } else {
                prev = current;
                result = max(result, current + 1);
                current = 0;
            }
        }
        return result;
     ```
36. Minimum window subsequence
    ```
      string minWindow(string s1, string s2) {

        int right = 0, strRight = 0;

        int result = -1;
        int start = -1;

        while (right < s1.size()) {
            if (s1[right] == s2[strRight])
                ++strRight;

            if (strRight == s2.size()) {
                int i = right;
                --strRight;
                while (strRight >= 0) {
                    if (s2[strRight] == s1[i])
                        --strRight;
                    --i;
                }
                int dist = right - i;
                if (result == -1 || dist < result) {
                    result = dist;
                    start = i + 1;
                }
                right = i + 1;
                strRight = 0;
            }
            ++right;
        }

        return result == -1 ? "" : s1.substr(start, result);
    }
    ```
   37. New 21 game

      ```
        double new21Game(int n, int k, int maxPts) {

        vector<double> P(n + 1);
        P[0] = 1;

        for (int i = 1; i <= n; i++) {

            for (int j = 1; j <= maxPts; j++) {

                if (i - j >= 0 && i - j < k) {

                    // Probability of score j = 1/maxPts
                    // Remaining points = (i-j);
                    // So,  P[i] = Probability of j * Probability of remaining
                    // i.e. P[i] = 1/maxPts * P[i-j]
                    // Or, P[i] = P[i-j]/maxPts;

                    P[i] += P[i - j] / maxPts;
                }
            }
            }
            return accumulate(P.begin() + k, P.end(), 0.0);
           }
      ```  
38. Max value of equation
    ```
      priority_queue<vector<int>> pq;
        pq.push({v[0][1]-v[0][0],v[0][0]});
        int ans = INT_MIN,sum;
        for(int i = 1; i < v.size(); i++){
            sum = v[i][0]+v[i][1];
            while(!pq.empty() && v[i][0]-pq.top()[1]>k)pq.pop();
            if(!pq.empty()){
                ans = max(ans,sum+pq.top()[0]);
            }
            pq.push({v[i][1]-v[i][0],v[i][0]});
        }
        return ans;
        
    }
    ```
39. Minimum consecutive cards pick up 
     ```
       set<int> s;
        int j = 0, i = 0, ans = INT_MAX;
        int n = cards.size();
        while (j < n) {
            if (!s.insert(cards[j]).second) {
                if (ans > s.size() + 1)
                    ans = s.size() + 1;
                s.erase(cards[i]);
                i++;
            } else
                j++;
        }
        return ans == INT_MAX ? -1 : ans;
     ```
40. Minimum Adjacent Swaps for K Consecutive Ones

   ```
     
    int minMoves(vector<int>& nums, int k) {
        vector<int> ones;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] == 1) {
                ones.push_back(i);
            }
        }

        vector<long long> prefSum(ones.size());
        prefSum[0] = ones[0];
        for (int i = 1; i < ones.size(); i++) {
            prefSum[i] = prefSum[i - 1] + ones[i];
        }

        long long minMoves = INT_MAX;
        long long left = 0;
        int right = k - 1;

        while (right < ones.size()) {
            long long mid = (left + right) / 2;
            long long radius = mid - left;
            long long excessMoves;
            long long total = 0;

            long long lsum = (mid == 0 ? 0 : prefSum[mid - 1]) -
                       (left == 0 ? 0 : prefSum[left - 1]);
            long long rsum = prefSum[right] - prefSum[mid];

            if (k % 2 == 0) {
                excessMoves = radius * (radius + 1) + (radius + 1);
                total = rsum - lsum - ones[mid] - excessMoves;
            } else {
                excessMoves = radius * (radius + 1);
                total = rsum - lsum - excessMoves;
            }

            minMoves = min(minMoves, total);

            left++;
            right++;
        }

        return minMoves;
    }

   ```
41. Continuous subarray 
    ```
       typedef long long ll;
    long long continuousSubarrays(vector<int>& nums) {
        ll totalSubarrays = 0;
        int i = 0, j = 0;
        multiset<int> ms;
        while (j < nums.size()) {
            while (!ms.empty()) {
                auto lastElement = ms.rbegin();
                auto firstElement = ms.begin();
                if (abs(*lastElement - nums[j]) <= 2 &&
                    abs(*firstElement - nums[j]) <= 2) {
                    break;
                }
                auto erasingElement = ms.find(nums[i]);
                ms.erase(erasingElement);
                i++;
            }
            ms.insert(nums[j]);
            totalSubarrays += j - i + 1;
            j++;
        }
        return totalSubarrays;
    }
      
    
    ```
    
42. Count subarrays with score less than k 
     ```
        long long countSubarrays(vector<int>& nums, long long k) {
        int i = 0, j = 0, n = nums.size();
        long long ans = 0, sum = 0;
        while(j<n)
        {
            sum += nums[j];
            while(sum*(j-i+1) >= k) sum -= nums[i++];
            ans += j-i+1;
            j++;
        }
        return ans;
        
    }
     ```
43. Number of unique flavours after sharing k candies 
    ```
      nt shareCandies(vector<int>& candies, int k) {
        unordered_map<int, int> cnt;
        int remain = 0;
        int n = candies.size();
        for (int i = k; i < n; i++) {
            cnt[candies[i]]++;
            if (cnt[candies[i]] == 1)
                remain++;
        }

        int ans = remain;
        for (int i = 0; i < n - k; i++) {

            cnt[candies[i + k]]--;
            if (cnt[candies[i + k]] == 0)
                remain--;

            cnt[candies[i]]++;
            if (cnt[candies[i]] == 1)
                remain++;

            ans = max<int>(remain, ans);
        }

        return ans;
    }
    ```
44. Longest substring that occurs thrice
