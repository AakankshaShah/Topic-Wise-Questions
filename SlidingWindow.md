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
8. Pick toys*
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
