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
