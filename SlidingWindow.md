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
cout<<"start"<<s1<<"\n";
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

12. Pick from any side 
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
17. Longest substring with at least char repeating k times
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

  
