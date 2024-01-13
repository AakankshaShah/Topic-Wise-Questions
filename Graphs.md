# Graphs 

## Theory 
https://www.youtube.com/watch?v=s7zE4Nmc2Fg&list=PL5DyztRVgtRVLwNWS7Rpp4qzVVHJalt22&index=1&t=24s

## Questions

### DFS

1. https://www.interviewbit.com/problems/path-with-good-nodes/

   ```
     int recurse(int node,int target,vector<int>arr[], vector<int>&vis, vector<int>&A)
   {
    target=target-A[node-1];
    if(arr[node].size()==1)
    {
        if(target>=0)
        return 1;
        else 
        return 0;
    }
    
    vis[node]=1;
    int count=0;
    
    for(int child : arr[node])
    {
        if(vis[child]==0)
        {
            count += recurse(child, target, arr, vis, A);
        }
    }
    
    
    return count;
    }

   
   ```
