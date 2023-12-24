# Queue

##Questions

1. Shortest Subarray with Sum at Least K** https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/

```
deque<pair<long long,int>>dq;
int ans=INT_MAX;
long long prefix_sum=0;


dq.push_back({0,-1});


for(int i=0;i<nums.size();i++)
{
    prefix_sum+=nums[i];

    while(!dq.empty()&&prefix_sum<=dq.back().first)
    dq.pop_back();


    long checker= prefix_sum-k;


    while(!dq.empty()&&checker>=dq.front().first)
    {
        ans=min(ans,i-dq.front().second);
        dq.pop_front();
    }
    dq.push_back({prefix_sum,i});
}

if(ans==INT_MAX)
return -1;
return ans;
```

2. First non repeating char in a stream 
